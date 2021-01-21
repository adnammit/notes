# SCHEDULED TASKS
[ms doc on Scheduled Tasks](https://docs.microsoft.com/en-us/windows/win32/taskschd/about-the-task-scheduler)

## BASICS
* a **task** is the scheduled work that is performed by the Task Scheduler
* tasks must contain a **trigger** that is used to start a task
* tasks must contain an **action** that describes the work that the Task Scheduler will perform
* a task is also comprised of:
	- **principals**: the security context in which the task is run
	- **settings**: spec for the Task Scheduler to run the task that are external to the task itself (e.g. priority to other tasks, whether multiple instances can be run, etc)
- **registration info**: task author, description and other metadata
- **data**: additional documentation about the task from the author, like Help

## TRIGGERS
* when all criteria of a trigger are met, execution of a task begins
* triggers can be event or time-based
* a task can have a max of 48 triggers
* **time-based triggers**: tasks are started at specified times
* **event-based triggers**: tasks are started in response to certain system events, like when the system starts up or goes idle, or when a user logs off


