# PSQL

## PSQL WINDOWS - SQL SHELL
* SQL shell is a special shell that will make your life easier. run psql within this shell
* when you start, you'll enter:
  - Server: localhost by default. just click enter
  - Database: there is a database called "postgres" that's created with install
  - Port: enter for default
  - Username: postgres by default
  - if you have a pw stored in pgpass.conf, it'll use that (so make sure it's right)
* remember that you might have to restart the pg server with pg_ctl and that sucks and might not work and you'll have to restart your computer...

## DATA TYPES
* use text over varchar (they're all dynamic arrays so it doesn't matter)
* for primary keys use `serial` over `int` (`id serial primary key not null`)
* for datetimes use `timezonetz` (`datecreated timezonetz not null default NOW()`)


## RUNNING PSQL LOCALLY:
* from your local CLI type:
    ```
        psql -d <dbname> -U <username>
        \c foo;    // connect to database foo
    ```
* data is in ~/Library/Application Support/Postgres[ver]/var by default.


To have launchd start postgresql at login:
```
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
Then to load postgresql now:
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
Or, if you don't want/need launchctl, you can just run:
postgres -D /usr/local/var/postgres
```
* `pg_config --includedir` tells you where the pg_dtconfig is



## PSQL COMMANDS
**remember you have to put a ; after everything**
```
* \? - full list of commands
* \h - help
* \l - list databases
* \c - connect - switch from one db to another
* \d - describe all relations
* \d+ foo - describe relation foo in detail
* \dn - view all schemas
* \dt - describe tables
* \dt+ - with size and description
* explain: defines the query plan for the following command
    explain select * from boats;
* \pset null <null> - displays null as <null>
* \e - open query in vim, nano, or other environment editor
```

## CREATE AND DESIGN A TABLE:
* **CREATE** table
    ```sql
    create table faculty(
        fid int primary key,
        fname char(15),
        age int,
        did int references department(did) deferrable
    );
    ```
* **ALTER** a table:
    ```sql
        -- create fk
        alter table department add foreign key (dhead) references
            faculty(fid) deferrable;
        -- remove column?
        alter table faculty drop age;
        -- add a column
        alter table students add column major char(15);
    ```
* **INSERT** values:
    ```sql
        insert into faculty values(124, 'Jean-Luc', 45, 1005);
        -- insert multiple values
        insert into pirates values
            ('Black Beard', 42),
            ('Anne Bonny', 28),
            ('Calico Jack', 32)
    ```
* **DELETE** a row which satisfies a condition:
    ```sql
        delete from faculty where fname = 'Jean-Luc';
        delete from faculty;    -- empties the table :(
        delete from students where gpa < 2;
        delete from students where name = 'Daniel';
    ```
* **UPDATE** a row:
    ```sql
        update students set gpa = 4 where name = 'Daniel';
    ```
* **NOT NULL**: if you add a not-null column to a table, you can't just add it -- that column will be null for all existing rows which is not permitted
    - to get around this, create the column as null, add values for existing columns and then make the column not null:
    ```sql
        -- conditionally add a column if it doesn't exist to avoid errors
        if (not exists(select 1 from information_schema.columns where table_schema = 'HighSeas' and table_name = 'Pirates' and column_name = 'Age'))
        begin
            alter table HighSeas.Pirates add Age int null;
        end
        update HighSeas.Pirates
        set
        	Age = 32
        where Name = 'Anne Bonny';
        alter table attendance.eventreason alter column displayorder int not null;
    ```


### PRIMARY KEY:
* **primary key** is a distinct value which distinguishes one row of data in a table from all the rest
* you can change the column of data which is the pkey but psql won't let you if each row doesn't have a distinct value.
* change pkey with:
    ```sql
        alter table students add constraint pkey_type primary key (lastname);
    ```
* a pkey can be more than one attribute:
    ```sql
        alter table enrolled add constraint enrolled_pkey
        primary key (sid, cid);
    ```
* psql always creates a b tree for the pkey
* a pkey may consist of two values combined together
* remove a pkey:
    ```sql
        alter table foo drop constraint foo_pkey;
    ```

### FOREIGN KEY:
* adding two foreign keys to the table 'enrolled':
    ```sql
        alter table enrolled add foreign key (sid) references student(sid);
        alter table enrolled add foreign key (cid) references class(cid);
    ```

### POPULATE A TABLE BY UPLOADING A FILE
* populate the table by copying data from a .csv or .txt:
    ```sql
        copy zip_codes from '/path/to/csv/ZIP_CODES.txt' DELIMITERS ',' CSV;
    ```



## SQL BASICS

### DISPLAY ORDERING:
* if an order is not specified, the output is in the order it was inserted onto the drive, unless some kind of processing like 'distinct' occurs
* psql is perfectly happy to have duplicate rows unless you use 'distinct'
    - recall that this is a 'bag' -- a set of data with duplicate members

### CORRELATION NAMES:
* **correlation names** allow you to reference the same table twice, and create more complex statements
    ```sql
        select s.sname from sailors s;
    ```




## RELATIONSHIPS BETWEEN TABLES:
* some tables share the same attributes:
    sailors(sid, sname, rating, age)
    reserves(sid, bid, day)
    boats(bid, bname, color)
* show the hierarchy of schemas:
    show seach_path;

## INDEXES:
* indexes should not be used on small tables
* add an index based on a column in the table:
    create index width_idx on rocks(width);





## VIEWS
* create a view
    ```sql
        create view youngSailors as
        select * from sailors where age < 27;
    ```

## STORED PROCEDURES
* create a new sproc:
    ```sql
        if not exists(
            select
                *
            from information_schema.routines as r
            where r.routine_name = 'GetPirateyInfo'
                and r.routine_schema = 'HighSeas'
                and r.routine_type = 'procedure'
        )
        begin
            exec sp_executesql
                n'create procedure HighSeas.GetPirateyInfo as select null;';
        end;
        go

        alter procedure HighSeas.GetPirateyInfo
        as
        begin
            set nocount on;
            set transaction isolation level read uncommitted;
            select
                 t.TripDate
                 ,p.Name
                 ,b.BoatName
            from HighSeas.Pirates p
                inner join HighSeas.Trips t on t.SailorId = s.SailorId
                inner join HighSeas.Boats b on b.BoatId = t.BoatId
            order by t.TripDate desc
        end
        go
    ```





AGGREGATE OPERATORS:
* count
* max
* min
* sum
* avg

COMPARISON OPERATORS:
=, !=, <>, >, <. >=, <=


EXPRESSION OPERATORS:
* we can merge and compare two query expressions using operators
* operators are:
    - union: automatically eliminates duplicates
    - union all: returns a bag set
    - intersect:
    - intersect all:
    - except:
    - except all
* when you use an expression operator the queries must have a matching
  number of columns
* slick use of expression operator:
    select sid from sailors
    except
    select sid from reserves;
    // returns all sailors with no reservations

EFFICIENCY
* "touching disk" - it is 100K slower to pull from disk compared to
  grabbing from memory
    - the more we can grab from memory, the faster our operations
* data is in the order that they were inserted into the database
    - sequential scan: steps through the data in the inserted order
    - seq scan vs materialize: materialize puts the data into memory
* using 'explain' may help figure out if a query is logically sound
    - cost in time listed for first row to last entry
* psql uses B-trees to load ids and pointers to the data for faster searches
* use the 'explain' function to see how psql arranges its algorithms and
  the relative cost of running a command:
    explain select * from foo;
* use the 'analyze' function to update psql's knowledge of a table. For
  instance, if you add a row or change the data, you'll need to run
  analyze before getting an accurate 'explain' query
* for large databases, you can disable sequential scan so a more efficient
  algorithm can do the work:
    set enable_seqscan to False;



EXAMPLES:
```
select * from sailors;
    // list all entries in the 'sailors' table
select * from sailors where age > 30;
    // returns all entries from sailor database where age is over 30
select * from boats where color = 'red'; //notice that strings need ''
    // returns all entries from boats database where color is red
select * from sailors where rating > 5 and age < 30

select * from sailors limit 3;

select sname from sailors;

select rating, sname from sailors order by rating;
    // display in order
select rating, sname from sailors order by rating desc;
    // descending order
select agent_id, first, last, salary from agent limit 3;

select sname as "good sailor" from sailors where rating > 7;
    // you can name your query columns using the 'as' clause
select sname, rating * 10 as "rating percentage" from sailors;
    // arithmetic!
select 5 * 12;
    // calculator!
select count(*) from language;
    // count of entries in language database
//COMPARE THAT TO:
\dt spy.*;
    // which will display tables and count of tables in the spy database
select max(age) as "oldest", min(age) as "youngest" from sailors
    // aggregate operators
select min(salary), max(salary), avg(salary) from agent;
    // display a single hit for each aggregate
select * from boats, sailors;
    // returns the union of the two tables
select * from sailors, sailors;
    // won't work. you have to use an alias to cheat:
select * from boats b1, boats b2;
    // we just gave 2 correlation names to the 'boats' table
select s.sname, b.bname from sailors s, boats b;
    //
select distinct s.rating from sailors s;
    // only display unique ratings
```




EXERCISES:

`\dt spy.*;`
    // counts how many tables are in the spy schema (there are 11)
for the languagerel table:
    - the foreign keys reference the agent table (agent_id) and the
      language table (lang_id)
    - the  primary key is the combination of the two other id's
      (lang_id, agent_id)
for the skill table:
    contains


RELATIONAL DATABASES
* why study relational databases?
    - way better than having a big chunk of random data -- better
      efficiency and better design
* database technology and theories have been very thoroughly developed
* DBMS: database management system
* schemas:
    - 'public' is the default schema space
* key: one or more attributes whose values uniquely identify the rows in
  the table. example: the key could be an id number, or a combination of
  id and date, etc

* how is a query evaluated?
    - it starts with the 'from' clause -- we have to have a pool to draw
      candidates from
    - once we've got our candidates, the 'where' clause is used to narrow
      down candidates
    - finally, the 'select' clause is evaluated to tell us which columns
      to keep in the query answer
* equivalent queries: order is irrelevant unless there is an 'order by'
  clause
* null: not quite zero. can't be compared to a string

BAGS AND SETS:
* set: one instance of each value
* bag: there may be more than one instance of each value

JOIN:
* join is secretly built in to most queries that we write
    - foriegn keys
* joins in the from clause:
    - inner join (the regular join)
    - cross join - cross product
    - natural join - don't use
    - left outer join
    - right outer join
    - full outer join
    - (the words 'inner' and 'outer' are not needed)
* you can use two tables at a time with join and then pipe join statements
  together for more complex statements
* example:
        select s.sid, s.sname, r.bid
    from sailors s join reserves r on s.sid = r.sid;
    this is a bit more elegant than what we've been writing:
    select s.sid, s.sname, r.bid
    from sailors s, reserves r
    where s.sid = r.sid;
    they produce the same effect though.
* what about
    select * from
    psql will let you enter anything into the 'on' clause which doesn't
    always give you the result you think you should get
* example:
    select m.name, t.name from mission m join team t
    on m.team_id = t.team_id;
    equivalent to the more familiar:
    select m.name, t.name from mission m, team t
    where m.team_id = t.team_id;

* you can concatenate join statements together for more complex queries:

    select b.bame s.sname
    from boats b join reserves r on b.bid = r.bid
    join sailors s join reserves r on s.sid = r.sid;

    select a.agent_id, a.first, a.middle, a.last, l.language
    from agent a join languagerel lr on a.agent_id = lr.agent_id
    join language l on lr.lang_id = l.lang_id;
* does the order of tables effect the efficiency of the query?
    - they are equivalent, regardless of the order they're written
    - the optimizer selects the order, regardless of the order you pick
    - however, if you start with the smallest table, that's best
* remember that the attributes you select have *NOTHING* to do with
  'right' or 'left' -- R or L refer to the order of the tables.
* LEFT (outer) JOIN:
    - includes all data from the left table and all data from the right
      table that matches.
    - if we want to create a query table where the two join tables have
      an uneven number of columns, we can use left join to insert null
      values on the right
    select s.sid, s.sname, r.bid
    from
    sailors s left join reserves r
    on s.sid = r.sid;
* RIGHT (outer) JOIN:
    - includes all data from the right table and all data from the left
      table that matches
    - we get the same result as above if we swap sailors and reserves:
    select s.sid, s.sname, r.bid
    from
    reserves r right join sailors s
    on s.sid = r.sid;
* INNER JOIN VS OUTER JOIN:
    - an inner join includes only matches between tables
    - full outer join will include all matches plus the leftovers from the
      first table and from the second table
* FULL OUTER JOIN:
    - includes all matches between tables and all other rows from both
      tables
* CROSS JOIN:
    - the two queries are equivalent:
    select * from sailors s, boats b;
    select * from sailors s cross join boats b;
* 'USING'
    - 'using(sid)' is pretty much the same as 'on s.sid = r.sid'
    select * from sailors join reserves using (sid);
    - in this example, 'sid' MUST be in both tables so saying 's.sid' is
      redundant and will cause a syntax error
    - this also drops the extra 'sid' column that appears using the 'on'
      clause
* NATURAL JOIN:
    - finds the similarity between tables -- implied "using" or on
    select * from sailors natural join reserves;
    select * from sailors natural join reserves natural join boats;
        **whoa, that's slick!**
    - so why not do this all day long?
    * if two tables have two columns that have the same name (like
      'name' or 'first') natural join will join based on the matching
      attribute names -- example, if you join boats and sailors based
      on 'name', you won't get anything unless a boat and a sailor
      have the same name
    - as schemas change, this small problem could break your software
    - using 'using' is less risky and better practice

VALUE CONSTRUCTOR:
* allows you to create rows or tables on the fly

QUERY OPTIMIZATION:
* the job of the query optimizer is to find the fastest query plan, after
  looking at the costs of various plans. the optimizer considers:
    - how many plans are there? can we enumerate them?
    - how can we estimate the cost of a plan? what units do we use?
    * we can count and compare by counting number of times that we
      touch disk
    - how are queries (and query operators) implemented?
* query optimization steps:
    - translate SQL query into a query tree
    - generate other equivalent trees
    - for each possible tree:
    * select an algorithm for each operator
    * estimate the cost of the plan
    - choose the plan with the lowest estimated cost
* using 'explain' gives an estimate of a potential cost.
    - BUT you have to 'analyze' first to make sure PSQLs stats on the
      table are accurate

INDICES
* what is an index?
    - it is a separate file that holds a B+ tree structure
    - the B+ tree is always balanced (same number of levels on every
      branch)
    - you set the size of the index block when you first define it
    - typically the index holds about 127 key values and 128 pointers
    - the leaves of the tree hold pointers to the rows on the actual
      table
* why?
    - the index file is smaller, faster to search
    - the top couple of tree levels are brought in to main memory when
      the DBMS is fired up
    - the tree is sorted and designed to direct you to the key value as
      fast as possible
* but!
    - they make inserts slower because all the b trees have to be updated
* clustered index: the data records are sorted and blocked together in
  groups based on the index order
* unclustered index: the data records are not sorted into any order
    - terrible for range queries -- you have to keep jumping back to get
      to the next item, rather than just scanning through the table until
      you get to the end or the condition is no longer met
* dense index: one index entry for each entry in the table
* sparse index: one index points to each PAGE or block of data
    - you can get away with this if the data is sorted
* PSQL uses dense and clustered indices
    - an index is UNCLUSTERED until you cluster it
* create an index:
    create index on sailorscopy (age);
* when a table is clustered it is resorted according to the specified
  index
    - if you add data after clustering it gets thrown on the end, the
      added data won't be sorted
    - a table can only be clustered under one index at a time.

EMBEDDED SQL:
* putting databases in our code, and code in our databases
    - connecting a website to a database --> web app!
* SQL can be customized, controlling what is visible to users, and results
  can be updated dynamically
* SQL can be called by a host languege:
    - C/C++, .NET, PHP, Java, Ruby, Python
* SQL statements can refer to host variables (return values) and must
  include a statement to connect to the correct database
* you can code multiple cursors in multiple databases to operate at the
  same time.
* the general procedure for embedding SQL:
    - take some command line args
    - open a connection
    - create a cursor
    - do what you need to do with the cursor data
    * this includes throwing an error if needed
    - close the cursor (so you're not leaking memory)
    - close the connection

CURSORS
* cursors can be declared on a relation or query statement (which
  generates a relation)
* you can open a cursor, repeatedly fetch a tuple and move a tuple
* you can also modify and delete the tuple pointed to by a cursor
* must be able to report data-generated errors
* creating a cursor:
    - open a transation:
    begin;
    - 'begin' will be concluded with 'end transaction;' or 'end;'
    - create the cursor with:
    DECLARE agent_cursor CURSOR for select * from agent;
    - after that you can look at the next result or the previous result:
    fetch next from agent_cursor;
    fetch prior from agent_cursor;
    fetch first from agent_cursor;
    fetch last from agent_cursor;
    - changing your table using a cursor:
    update agent set middle='lotsomoney' where current of
        agent_cursor;
* cursors are ephemeral -- they are only good for the transaction and do
  not persist outside of the transaction session
* transactions: nothing that is done in a transaction is done until the
  transaction is completed -- if an error occurs it kills your transaction
* capabiliies of a cursor vary between DBMSs
* the cursor begins *before* the first item in the selection




STATIC VS DYNAMIC:
* static is faster, more secure- we don't have to analyze every time
* dynamic requires more overhead but it's more flexible

STATIC SQL
* SQL commands are embedded in the program with some kind of special
  delimiters
* need a way to pass parameters to query
* you must know the schema of the result of a select statement in advance

DYNAMIC
* create a string at runtime that represents the query
* analyze and execute the query every time
* need a way to discover the schema of the result

SQL INJECTION: SECURITY
* sanitize your inputs so you don't have unexpected commands on your
  tables

PSQL TIPS:
* psql won't let you join between databases -- some DBSMs will though
* show: see if a setting is on or off
    show enable_seqscan;
* change the setting for one database:
    alter database f15ddb3 set enable_bitmapscan to true;

QUERY OPTIMIZATION: JOIN ALGORITHMS
* simple nested loops join
    - "for every row in table 1, check every row in table 2
    - not very efficient at all
* page-oriented nested loops join
    - "for each page of tuples in table 1, compare each value on the page
      to every value in a page of table 2"
* block nested-loops join
* index nested loops join
    - read first page of table 1 and hit the index to find matching tuples
      in table 2

SQL EXTENSIONS
* GROUP BY:
    select store, max(size) from pricelist group by store;
    // show the largest size each store sells

    select b.color, count(*) from boats b group by color;
    // count the number of boats per color
    - 'null' counts as a group -- if the group by clause is null, that row
      will still be grouped with null
* HAVING:
    select s.rating, avg(s.age) from sailors s group by s.rating
    having count(*) > 1;
    // find the average age for each sailor rating where there is more
    //  than one sailor with that rating

    select store, max(price) from pricelist group by store
    f15ddb58-> having count(distinct beverage) > 3;
    // display the most expensive beverage that each store that has
    //  at least 4 distinct beverages sells

    select store, sum(price)
    from pricelist
    where beverage= 'Coke'
    group by store;
    // what is the cost of purchasing one Coke of each size from
    //  each store?

EQUIVALENCE:
* two queries are equivalent if they are guaranteed to return the same
  answer for any database state (any rows in the table)
* how would you determine if two queries are equivalent?
    - run them. not a definitive answer though -- just might narrow it
      down
    - do they return the same columns? if not, they're not equivalent
    - or: use relational algebra! equivalences
* RELATIONAL ALGEBRA:
    - translate SQL queries to relational algebra and compare them
    - translation comes across as a tree
    - example: where R, S and T are tables
    (R x S) x T == R x (S x T)
    - relational algebra is a set of operators that work on tables/
      relations (including intermediate results) that produce one table/
      relation
    - for n, u and -, it is requred that relations
    - project is not neede (only select) where we are projecting all of
      the fields
    - there's no 'having' in relational algebra -- the grouping operator
      produces aggregates that you can then simply select from.
* tables and relations:
    - database theory began with relational math theory
    - tables are the structure used by the language SQL
    - most DBMS systems use tables as bags of rows
* cross products:
    - Codd simplified or "flattened" the product results:
    sailors X boats is not:
        ((101, 'Rusty', 7, 35),(102,'Interlake', 'red'))
    but rather it is:
        (101, 'Rusty', 7, 35, 102,'Interlake', 'red')
* real-life applications of relational algebra:
    - the algebra used by DBMSs are different.
    - functional algebras need to be as small (efficient) as possible
    - the most efficient and optimized

SUBQUERIES
* a subquery is a query with parentheses around it! boom.
* we do this all the time in relational algebra
* union, except, intersect all use subqueries
* we can use the same correlation names for tables within the subquery
  as we used in the outer query -- the optimizer treats them as two
  different names
* generally you should avoid using subqueries because it's more work than
  using join, say. modern optimizers will get it, but not always
* example:
    select p.first, p.last, p.city
    from (select * from agent a where city = 'Paris') p;
    - note that subqueries need to have correlation names assigned to them
      even if you don't use it.
* scalar query: guaranteed to produce just one row with just one column;
  one single simple value called a scalar
    - ex: aggregates(max, min, count, ave)
* scalar subquery example:
    select * from sailors s where
        s.rating = (select max(rating) from sailors);
    - common in where clauses, join condition, select clause, and having
      clause
        where color = (select color from boats)
    * this works if the query returns one result; otherwise it barfs
* scalar subqueries with constants
    - we can use subqueries to insert constants into our query results
* how to use nonscalar subqueries
    - predicates that work with nonscalar queries
        * exists: returns boolean
        * not exists: return boolean
        * some/any: does any result in the query match to the subquery
        * all
        * in/=some
        * not in
* correlated queries: subqueries can reference tables outside, in another
  query
    select s.sid, s.sname
    from sailors s
    where exists (select * from reserves r where r.sid=s.sid);
    - why? recall that SQL evaluates the 'from' clause and THEN the
      'where' clause -- by the time we get to 'where' we already have s

CORRELATED VS NON-CORRELATED SUBQUERIES
* non-correlated subqueries can be run on their own without any context
  from the outer query -- it will have the same result when run over and
  over again (and only needs to be run once)
* a correlated subquery needs to be run for each iteration of the outer
  query
* you cannot use a correlated subquery in the 'from' clause

## CONSTRAINTS IN SQL
* examples of constraints: primary keys, foreign keys
* 'unique' allows null values for more than one row -- null is a non-value
    - unique implicity creates an index (makes sense -- easier sorting)
* primary key does not allow null values and all values must be unique
* why bother to name constraints?
    - you can modify/drop them more easily
* how do you make sure a value in a column matches a specified list of
  values?
    - use a lookup table!
    - your table contains a foreign key reference to the table, kind of
      like the language table
* deferred constraints:
    - foreign keys can be deferred -- that is, if you try to insert a new
      faculty member who is the head of a new department, you won't be
      able to insert either the faculty member or the new department whose
      references faculty for the head of the department.
    - the solution: defer the change until COMMIT. it's like saying 'ok
      computer, trust me here. it will all make sense before you change
      anything.'
    - how to:
    * begin a transaction
    * 'set all constraints deferred;' // if a restraint can be
        deferred, it will be
    * on commit, all deferred constraints will be deferred or rejected
      (abort)

CONCURRENCY OF TRANSACTIONS
* remember ACID? The Isolation bit is a problem to be solved by
  concurrency
* what is isolation?
    - isolation is achieved when the result of concurrent transactions is
      the same as if they had been run in serial
    - the order doesn't matter from an isolation correctness standpoint
* a SCHEDULE is an interleaving of transaction actions
    - the order of operations within each transaction must be preserved
    - a schedule is SERIAL if its transactions occur as a series of
      complete/uninterrupted transactions, one after the other
    - a COMPLETE schedule means that all transactions commit or abort
* if a schedule is serializable, it is correct (because serial schedules
  are always correct
* isolation levels vary through applications: it matters what order your
  bank transactions post, but it doesn't matter what order your twitter
  feed is in exactly.
* the acyclic precedence graph
    ( serializable ( conflict serializable ( Serial) ) )
    - why don't DBMSs implement this?
    * we need the full schedule to determine how serial or correct
      a schedule is
* then how do DBMSs actually handle things?
    - a transaction must get a LOCK before it can read or update an item
    - we can have multiple transactions operating on the same DB but we
      must lock individual items
    - SHARED lock (S) for reading
    * as long as there's no X lock, you can get an S lock
    - EXCLUSIVE lock (X) for writing
    * to get an X lock on an item, there can be no locks on it.
    - lock info maintained by a LOCK MANAGER
    - if a transaction can't get a lock, it waits in a queue
* locking protocols:
    - strict 2-phase locking protocol tells us when locks can be released
    * locks cannot be released until the transaction ends through
      commit or abort
    - non-strict 2-phase locking protocol: a transaction gets and drops
      locks as needed
    * problem: what if the transaction is aborted? it results in a
      cascading abort scenario where all transactions which viewed
      the dirty data must be aborted also
    - multi-version concurrency control
    - lock scopes:
    * lock tables
    * lock rows
    * lock attribute values
    * lock predicates (age > 25)
    * the last one is difficult to implement
    * increasing granularity --> increasing concurrency
* deadlock
    - all this queueing results in a lot of waiting which can result in
      deadlock, where transactions are waiting for an abort/commit which
      will never happen
    - T1 wants a X lock on A and has a X lock on B
    - T2 has a X lock on B and wants a X lock on A
    - solution: the DBMS must detect this scenario and kill one of the
      requests
    * one transaction is the 'victim' of deadlock
    - strict 2pl can have deadlocks
* phantoms:
    - T1 reads an object, T2 modifies it, T1 reads a different value
    - sometimes this is a problem, sometimes not

DESIGNING A DATABASE
* ER (Entity Relationship) model: provides a conceptual, high level view
  of your schema
* logical design: transform ER to relational schema
* normalization: check for redundancies and related anomalies
* physical db design and tuning
    - get it up and running, fast
* ER vs Relational model:
    - relation model has tables, keys, attributes, etc
    - ER: no tables, just entities and relationships
* UML diagrams can be used to map out database designs
* relationships:
    - something like 'reserves' is represented as a relationship, rather
      than a class object
    - example: skillrel, teamrel, affilliationrel are just "connections"
      between entities like agent, skill, team, affiliation, language
    - if you want to connect an agent to a language, somewhere you have
      to get agent_id and lang_id on the same row together
    - 'has many' versus 'has one' relationships:
    * when designing, consider how the connections are made:
    * does one agent know one language? is each language known by one
      agent? or can each agent and language 'have many' of each other?
    * this is the CARDINALITY of the relationship
    - relationship arrows may have accompanying role names to make it even
      more clear
    * you need role names when you have a RECURSIVE relationship
    * recursive: suppose you have one employee table which relates
      back to itself with a 'reports-to' relationship and has
      subordinate and supervisor roles
    - sometimes you have to decide whether or not an attribute should be
      it's own entity:
    * should 'quarter' be an attribute of 'class', or should it be
      it's own entity?
    * it depends -- there's no right answer.
    * more entities = more overhead
    * having entities allows something like "office" to exist
      independently of "employee"
* MEMORIZE ERD NOTATION AND CARDINALITY -- you will be tested
* turning ERD into a DB:
    - create a table with the appropriate keys and attributes
    - have a multi-valued attribute? make a table for it
    - have a many-to-many relationship set? make a table for it
    - if you have a one-to-many relationship, you can stick it in the
      table that has one of the other table
    * ex: each project has one manager, but each manager has many
      projects. the simplest solution is to reference manager from the
      project table
    * you can create a separate table for a one-to-many relationship
      if you need to avoid the possibilty of null values (a project
      with no manager assigned yet)
* weak entity:
    - dependent on the "strong" entity -- they don't need to exist on
      their own, they are just a somewhat complex add-on to supplement
      the strong entity it's correlated with
    - don't have keys on their own -- they pick up keys from their
      identifying relationship
    * official def: they don't have a key of their own (but in
      practice some of them do)
    - partial key: take the key from the strong entity and concatenate
      it with the weak key to make a partial key
    * eg: 716287321ANDREWS

COMPREHENSIVE AND MATERIAL VIEW
* CRUD (create, read, update and delete) - we need something to help
  manage alterations across tables
* comprehensive view: a combination of data from several tables
    - for example, if you draw from three different tables to create a
      fourth which contains an intersection of the three under certain
      conditions
    - we can create a function and a trigger that will update all tables
      when one is updated
    - does not contain the query that created it -- it's just data
    - on disk
    - in comparison to a materialized view
* materialized view
    - stored on disk
    - populated when it's created. not automatically updating
    - unlike tables, it remembers the query used to create it, and thus
      can be refreshed, which happens on view

TRIGGERS
* what are triggers? they 'watch and tell' -- they don't DO anything with
  the date
* a trigger is tied to a table -- when an insert, deletion, modification
  occurs, a corresponding procedure is called
    - can execute before or after the modification
* a trigger can return a row/record from the old table, the new table,
  or null
* triggers on different tables can call the same procedures
* creating triggers
    create trigger foo on bar

STORED PROCEDURES
* stored procedures are the action in reaction to triggers
* stored procedures live in the database, so it saves a lot of back-and-
  forth
* creating a procedure/function
 create or replace function procjack() returns trigger as $$
     begin
         insert into ajack values (new.agent_id, NEW.first, NEW.middle,
                 NEW.last, NEW.address, NEW.city, NEW.country, NEW.salary,
                 NEW.clearance_id);
         insert into ljack values (NEW.lang_id, NEW.language);
         insert into lrjack values (NEW.lang_id, NEW.agent_id);
         return NEW;
     end;
$$ language plpgsql;


IMPLEMENTATIONS
* comprehensive display + triggers + procedures
    - pro: targeted updates -- we only touch the data that has changed
    - con: difficulty, messy and complex
* embedded code
    - adv: often required to integrate it into software
    - con: complicated, largest overhead
* materialized view + triggers + procedures
    - pro: simple
    - con: updates entire view -- bad for complex queries especially if
      they access normal views

FUZZINESS:
* do you want to allow a user to change an entity's id? how much power
  will be allowed?

NORMALIZATION
* smooshing all the data together is nice sometimes
* but this allows for the possibility of redundancy and/or inconsistancy
  due to functional dependency
* normalization is based on functional dependency -- using it to decompose
  tables
* for instance, if you have a table that is the result of a join on
  sailors and reserves, break them apart, alter them and stick them back
  together again
* how do we know it's successful?
    - lossless
    - all tables in BCNF
    - all FDs preserved
* sometimes you must choose between BCNF and preserving FDs

FINAL:
* Final -- monday 12:30
    - open book
    - no embedded SQL
    - no join algorithms
* equivalences: what is it? can you identify if two things are equivalent?
