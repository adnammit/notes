# SQL

## SSMS
* script a sproc without going through the tree
	- `exec sp_helptext 'Attendance.GetDisciplinaryAction'`
	- this will print to grid, so click the Text column to highlight the script, copy/paste to a new window
* view table columns w/out clickity clicking
	- `exec sp_columns varchar50list`
* shortcuts
	```sql
		Ctrl-R      -- show/hide the results window
		Ctrl-t      -- results to text
		Ctrl-d      -- results to grid
	```
* curious what queries are being run? Use the XEvent Profiler (Standard) and add some handy filters
	- `database_name Like EmployeeManagement`
	- `statement <> exec sp_reset_connection`


## STORED PROCEDURES
* view the contents of a stored procedure by right-clicking on the procedure and going to `Script stored procedure as -> CREATE to -> New Query Editor Window`
	- here you can view the procedure without saving


## GOTCHAS AND TIPS
* using `NOLOCK` for a query seems like a good idea if you don't want to change any data, so why might that be a bad idea?
	- using `NOLOCK` means another query won't block your access to that data, but if you read that data while the query is changing it, you might get a **dirty read** where your data is suddenly no longer current
* consider `NOLOCK` vs `SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED`
	- `NOLOCK` sets just one table to read w/out locking
	- the latter sets the state of the entire transaction
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



## EXAMINE QUERIES USING EXTENDED EVENTS

* check to see if you have perms to alter event data:
	```sql
		SELECT HAS_PERMS_BY_NAME(null, null, 'ALTER ANY EVENT SESSION');
	```

```sql

IF EXISTS (SELECT *
	FROM sys.server_event_sessions    -- If Microsoft SQL Server.
	WHERE name = 'testSession')
BEGIN
DROP EVENT SESSION testSession
	  ON SERVER;
END
go

CREATE EVENT SESSION [testSession]
	ON SERVER
	ADD EVENT sqlserver.sql_statement_completed(
		WHERE ([sqlserver].[i_like_sql_unicode_string]([sqlserver].[sql_text],N'%SELECT%HAVING%')))
	ADD TARGET package0.event_file(
		SET filename=N'C:\Temp\testSession.xel') -- change to corresponding output file
		WITH (
			MAX_MEMORY=4096 KB
			,EVENT_RETENTION_MODE=ALLOW_MULTIPLE_EVENT_LOSS
			,MAX_DISPATCH_LATENCY=3 SECONDS
			,MAX_EVENT_SIZE=0 KB
			,MEMORY_PARTITION_MODE=NONE
			,TRACK_CAUSALITY=OFF
			,STARTUP_STATE=OFF
		)
GO



```
