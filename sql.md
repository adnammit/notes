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
* you can view dependencies of views and sprocs by right clicking and selecting "View Dependencies"
* view the contents of a stored procedure by right-clicking on the procedure and going to `Script stored procedure as -> CREATE to -> New Query Editor Window`
	- here you can view the procedure without saving

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
