# WSL
* [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/about) allows you to run a linux environment on your windows machine, without the need for a separate virtual machine or dual booting
* WSL makes it possible to install multiple Linux distros at once
* allows for native execution of bash - compare to Cygwin, which doesn't have the proper integration with windows
* unique users are created for each distro that is installed
* see [best practices for setting up WSL development environment](https://learn.microsoft.com/en-us/windows/wsl/setup/environment) including info for configuring users and resetting passwords
* [Basic WSL commands](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#check-wsl-status)


## Installing, Managing and Updating
* WSL is managed from powershell
```pwsh
# install WSL
wsl --install

# list installed distros
wsl --list --verbose

# list available distros to install
wsl --list --online

# install a distro (-d option as default)
wsl --install -d ubuntu

# set distro as default
wsl --setdefault ubuntu

# update wsl
wsl --update
```
* within a distro, you should periodically update your packages with:
```bash
sudo apt update && sudo apt upgrade
```

## Managing Users
* you'll be signed in as root user by default, but you can/should configure different users with the correct permissions
* commands are performed from within the distro shell (i.e. Ubuntu)
```bash
# list users
eval getent passwd {$(awk '/^UID_MIN/ {print $2}' /etc/login.defs)..$(awk '/^UID_MAX/ {print $2}' /etc/login.defs)} | cut -d: -f1

# list sudoers
getent group sudo | cut -d: -f4

# add user
sudo adduser [username]

# make user sudoer
sudo usermod -a -G sudo [username]

# delete user
sudo deluser [username]
```
* if you need to change the default user for a distro, you'll need to do so from powershell. changing default user will restart the distro
```pwsh
# set default
ubuntu config --default-user <username>
```

## Integrations with WSL
* you can integrate VS Code with WSL to develop in a linux environment -- see [VS Code and WSL](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)
* [setting up git in WSL](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)


## Tips and Tricks
* WSL will hog your memory (`vmmemWSL` in task manager) -- kill the process when not in use:
```pwsh
	wsl --shutdown
```
* Modify your [WSL config file](https://learn.microsoft.com/en-us/windows/wsl/wsl-config) to manage many things including limiting the amount of memory allotted for WSL

