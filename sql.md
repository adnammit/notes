# SQL

## OVERVIEW
* you can view and query databases by opening the Microsoft SQL Server Management Studio program
    - expand `Databases` -- there you can see all the databases used by various apps and teams
    - each project folder contains a `Tables` folder (where the actual data lives) and Stored Procedures (under `Programmability`)


## STORED PROCEDURES
* view the contents of a stored procedure by right-clicking on the procedure and going to `Script stored procedure as -> CREATE to -> New Query Editor Window`
    - here you can view the procedure without saving

## GOTCHAS AND TIPS
* using `NOLOCK` for a query seems like a good idea if you don't want to change any data, so why might that be a bad idea?
    - using `NOLOCK` means another query won't block your access to that data, but if you read that data while the query is changing it, you might get a **dirty read** where your data is suddenly no longer current
* you can view dependencies of views and sprocs by right clicking and selecting "View Dependencies"


## OPTIMIZATION
* if you have the options of using a vw or a sproc, sprocs provide more ways to optimize
* use dynamic joins wherever possible (only join if you have to)
* inner joins are faster than outer joins
* use more `WHERE` conditions to narrow down your pool (such as only recent data, etc)
* enums take longer than bools
* use temporary tables rather than iterating over the same beastly table
* query plans are cached -- if the query parameters are exactly the same, it pulls the existing query out of the cache and uses that one. "optimized" queries sometimes waste time -- the cache may need to be cleared
* don't put `CONST`s at the beginning of a sproc

## QUERY FEATURES
* **LIKE**
    ```sql
        -- select pirates with names ending in "beard" or with names containing "john"
        SELECT *
        FROM pirates
        WHERE 1=1
            AND (name LIKE '%beard' OR name LIKE '%john%')
    ```
* **NULL**
    ```sql
        -- select boats with no trestle
        SELECT *
        FROM boats
        WHERE 1=1
            AND (trestle is null OR trestle = '')

    ```
* **MAX/MIN**
    ```sql
        -- given a table of trips with dates and the boat taken on the trip,
        -- display the latest trip for each boat
        SELECT
            max(tripDate) as LatestVoyage
            ,boatName
        FROM trips
        GROUP BY boatName
    ```
