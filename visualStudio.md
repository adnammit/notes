# VISUAL STUDIO

## SHORTCUTS

```
																	   [custom?]
==GENERAL==
C--                         navigate backward
C-shift--                   navigate forward
Alt-<                       go to top of file
Alt->                       go to bottom of file
C-Shift-F                   find in files
C-,                         "fuzzy finder"
alt-w, v                    split pane vertically
Alt-Enter                   display properties
C-k-k                       toggle bookmark on/off current LOC
C-alt-b                     open breakpoints window
C-alt-l                     open solution explorer
C-m-o                       collapse all code blocks
C-m-a                       collapse even more all
C-m-l                       expand all code blocks
C-'                         collapse/expand current selection               *

==EDITING==
C-a                         Edit.LineStart                                  *
C-e                         Edit.LineEnd                                    *
C-h                         Edit.SelectAll                                  *
C-X                         Edit.LineDelete                                 *
C-up                        Edit.JumpUp                                     * [extension]
C-down                      Edit.JumpDown                                   * [extension]
C-shift-up                  Edit.JumpSelectUp                               * [extension]
C-shift-down                Edit.JumpSelectDown                             * [extension]
C-/                         Edit.CommentSelection                           *
C-?                         Edit.UncommentSelection                         *
C-d                         Edit.SelectCurrentWord
C-alt-click                 insert cursor


==DEBUG==
C-shift-B                   build
C-=                         go to current debug statement                   *
F12 or C-click              go to definition
C-F12                       go to declaration/implementation
Shift-F12                   find all references
C-k C-t                     view call hierarchy
C-alt-p                     attach to process

==FORMATTING==
C-k C-f                     fix indentation in selection
C-k C-d                     fix indentation in entire document

==EDITING==


```

## VOCABULARY
* **solution**
	- a solution is a container for one or more related projects as well as build information, VS window settings and any misc files that aren't associated with any particular project
	- solution files end with `.sln` or `.suo`
	- solutions are not intended to be edited by hand
* **project**
	- a project is a self-contained unit which includes all the source code files, icons, images, data files, config and compiler settings, etc. which are used to compile an executable, library or website.
	- projects are defined in an XML file and has an extension of `.csproj`, `.vbproj` or `.vcxproj`
	- the project file contains the virtual folder hierarchy and paths to all the items in the project
	- the project file also contains the build settings
* **reference** vs a **project reference**:
	- sometimes you'll see a reference to an executable (like a dll). Don't do this unless you need to do it -- if the executable is from another package, it might not have been built yet so it won't be there -- the build will fail.
	- It is preferred to link to a project so the solution can build it. The exception is that you might need to reference an executable when using a third party dll or references to `System`
* **Solution Explorer**:
	- the SE allows you to view the solution, contained projects, and their associated items
	- solutions have the VS icon next to them, projects have a boxed [C#] icon and source code has a C# icon
* **Code Lens**:
	- you may see a statement like "4 References" in small, low-chroma print above a declaration
	- clicking the statement will show you references to this code without leaving the editor


## DEBUGGING
* to debug a project that takes args, highlight the project in the Solution Explorer and hit `Alt-Enter` to open properties
	- under "Debug" you can fill in your command line arguments
* using `Go to definition` and `Go to declaration/implementation` to get to the bottom of things:
	- so you're browsing through code and you see something like `ServiceManager.UpdateCustomerStatusDetail()` and you want to know what happens in there
	- highlight `UpdateCustomerStatusDetail` and hit `F12` to go to the definition of the function.
	- well huh, that didn't really work -- you ended up in the declaration of the method -- in the Interface `IServiceManager`
	- what you really wanted was the _implementation_ of the method -- highlight the method name again either in the interface or in the original calling code and hit `C-F12` to go to the actual method body in `ServiceManager::UpdateCustomerStatusDetail()`
	- when you use `Go to implementation` you might get multiple options -- makes sense, because interfaces can be implemented by multiple classes. you'll need to choose which class is correct


## ATTACHING A PROCESS
* in the VS menu, click `Debug -> Attach to Process...` (or `C-Alt-P`)
* make sure "Show processes from all users" is checked
* look for processes called `w3wp.exe` and look at the usernames for the processes (things like `IIS APPPOOL\CustomerInfo`) -- this will tell you which app each process is associated with
