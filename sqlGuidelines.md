# FORMATTING AND STYLE GUIDELINES FOR SQL

Independent clauses are at the same level
    - Independent clauses are FROM,WHERE,GROUP BY,HAVING,SELECT,ORDER BY (and a couple others such as PIVOT and ROLLUP for example)
    - Do not put the FROM clause at the end of the statement

Dependent clauses are indented one tab from their independent clause
    - i.e. INNER JOIN, LEFT OUTER JOIN, CROSS APPLY, OUTER APPLY, etc.

Always fully qualify your join syntax (don't use shortcuts)
    - i.e. INNER JOIN not JOIN, LEFT OUTER JOIN not LEFT JOIN, etc.

The ON clause should be on the same line as the join and the table defined in the JOIN should appear first in the ON clause
    - An example:
FROM dbo.Customer c
    INNER JOIN dbo.Account a ON a.CustomerId = c.CustomerId     <-- dbo.Account is the JOIN table therefore it appears first in the join

Objects should always include the schema
    - e.g. dbo.Customer not just Customer
    - Without the schema name, I can change the behavior of your code without modifying schema :)

Avoid three-part naming where possible, i.e. try to avoid this: Billing.dbo.Customer
    - All connections have a connection scope and the three part naming should not be used for database objects in that connection scope
    - Cross DB joins (if absolutely necessary) require the three-part naming
    - Troubleshooting queries and stuff that won't go into production, you're OK to put three part naming, i.e. here in Dev and QA.

Tables should be aliased
    - Generally recommend using lower case, first letter abbreviations from the table name
    - e.g. dbo.LineServiceDetail AS lsd
           dbo.CustomerStatusDetail AS csd
    - Sometimes it is necessary to alias differently for clarity or if doing multiple joins on the same table
    - If in doubt, letter-it-out

Try to avoid cross DB joins
    - Eventually when the databases are scaled out and put onto separate servers all of these cross DB joins will FAIL!
    - P.S. this is coming sooner than you think
    - Essentially, every cross DB join you add is new technical debt
    - This stuff will be dealt with in the new architecture approach and I'm collaborating with Taylor on the best way to move forward

For INNER JOINs, try to limit the number of join conditions to two or less
    - Mostly for readabiliy
    - There are some exceptions but as a rule, try to stick to two
    - Compound join conditions should have their extra conditions on separate lines and indented from the parent JOIN clause
    - For INNER JOINs, putting the conditions on the join or in the WHERE cluase are logically equivalent

For OUTER JOINS, the join conditions are more critical and do what is needed for the sceanrio
    - For these joins, the ON clause and WHERE clause are not logically equivalent like in INNER JOINs
    - Still put the extra conditions on separate lines and indented from the parent JOIN clause

Pretty much all DB objects should favor Pascal casing, including parameters
    - Please avoid camelCasing
        - (virtually all non-Microsoft data platforms use the underscore naming so don't tempt me :) )

SQL standard uses <> to indicate inequality, not !=
    - Please favor <>

That's all for now ... more to come in my final (mythical ?) document
