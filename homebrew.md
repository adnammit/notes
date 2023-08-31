# Homebrew
* Package manager for MacOS and linux
* brew uses ruby scripts to install packages

## BASICS
```sh
brew update && brew upgrade					# update brew and all packages
brew doctor
brew list									# list all packages managed by brew and all tapped casks
brew remove foo								# remove package foo
brew info $(brew list)						# view caveats for currently installed packages
```
* a **formula** is a package (ruby script) that can be installed with brew
* a **tap** is a repo of packages that can be installed with brew -- when you tap a repo, you expand the available packages
* **cask** is an extension of `brew` that manages GUI apps 

**don't forget to check your caveats!!**
