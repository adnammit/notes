## LINUX-SPECIFIC

```
apt-get 	package manager for debian-linux

apt list --installed	list all installed pkgs

sudo apt-get install foo	install package foo

sudo apt-get install *foo*	install all packages matching ‘*foo*’

sudo apt-get update	resychronize the package index files from the sources listed in /etc/apt/sources.list w/out actually changing installed packages

sudo apt-get upgrade	looks at the updated index of packages and upgrades necessary pkgs

sudo apt-get dist-upgrade	more sophisticated upgrade which intelligently resolves dependencies

apt-cache pkgnames <foo>	list all packages (optionally those matching ‘foo’)

apt-cache search <foo>	display available packages (optionally matching ‘foo’)

apt-cache show netcat	display details about netcat

apt-cache showpkg vsftpd	display dependencies for vsftpd

apt-cache stats	display statistics about the cache

sudo apt-get clean	clear up disk space by removing .deb files

sudo apt-get remove foo	remove package foo but leave config intact

sudo apt-get purge foo	remove package foo including foo’s config files (watch out!)

sudo apt-get autoremove	remove any dependencies that are no longer needed
```
