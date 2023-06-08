# Command Line Your Way to a Great Happiness

## Shells vs Terminal Emulators
* shells are interpreters which take user input and translate the input to execute actions (the language)
	- bash/zsh (Linux)
	- cmd or powershell on Windows
		* cmd is based on DOS but we won't talk about it much bc no one uses it unless they have to
* terminal emulators emulate terminals which are real keyboard/monitor interfaces (the "machine")
	- they provide input and output -- a way of providing instruction and display to/from your computer
	- TEs allow host computer to access another computer through protocols such as SSH

## Powershell vs Bash?
* [comparison](https://www.cbtnuggets.com/blog/certifications/microsoft/powershell-vs-bash-whats-the-difference)
* [another comparison](https://www.techtarget.com/searchitoperations/tip/On-Windows-PowerShell-vs-Bash-comparison-gets-interesting)
* both are CLIs (command language interpreter) used in terminal emulators, however powershell is more than that -- it is also a language
* powershell is optimized for microsoft environments, whereas bash is native to *nix
* powershell is thought of more as a configuration management tool -- used to manage windows environments at scale. both powershell and bash are scripting languages that can used for automation tasks

# bash
* native to linux/unix. bash can be used on Windows via Cygwin or more recently the linux subsystem
* particularly useful for porting *nix code into windows envs
* quicker commands
* treats everything as plain text

## WSL
* the linux subsystem for Windows makes it possible to install multiple Linux distros on a windows machine
* compare to Cygwin, which doesn't have the proper integration with windows

# Powershell
* flexible and powerful; open-source
* Windows mostly, but Powershell Core can be installed on any OS
* designed to interact with .NET structures
* more than a shell: it's a complete scripting environment (Powershell ISE)
* uses lightweight cmdlets and external tools like Windows Management Instrumentation and .NET
* treats everything as objects
* powershell cmdlets have cumbersome names like `Get-ChildItem` that have native aliases to their old DOS commands (`dir`)

## Powershell Core
* much like .NET Core, Powershell Core is made to be cross-platform friendly
* core can be run as a primary shell within linux using the object properties of powershell
* legacy Powershell is based on windows-only .NET framework and the executable name is `powershell.exe`
* Powershell Core is based on is based on cross-platform Core/.NET 5+ and the executable name is `pwsh.exe`

## Powershell ISE
* Powershell ISE is an application/environment used to run, test and debug powershell scripts
* kind of like an IDE for powershell?

## Installing Powershell
* if you already have powershell 5, you can install powershell 7 (with pwsh) side-by-side
* [installing powershell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows)
* [powershell 5 vs 7](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/migrating-from-windows-powershell-51-to-powershell-7?view=powershell-7.3#using-powershell-7-side-by-side-with-windows-powershell-51)

## Powershell Resources
* [how to source functions](https://stackoverflow.com/a/6040725/7898566)
* [do more with modules](https://learn.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-script-module?view=powershell-7.3)

## Powershell Cheat Sheet
* **TODO** add more!

```powershell
# ls
Get-ChildItem

# cd
Set-Location {destination}

# less
Get-Content {file}

# echo "Hello World!"
Write-Host "Hello World!"

Get-Command
```

