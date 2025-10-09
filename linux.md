# Linux

## Server Admin Quick Reference

```bash
# general server stats
uptime


# check apache server status
sudo systemctl status apache2
sudo apachectl -V


# apache config:
/etc/apache2/apache2.conf
# php config for apache:
/etc/php/5.6/apache2/php.ini


# check php version -- this is CLI version, not what's being used by apache necessarily
php -v
php -i


# centos error logs
/var/log/httpd/error_log
# ubuntu error logs
/var/log/apache2/error.log


# SERVICES
# view all service enabled/disabled status (will it restart on server boot)
sudo systemctl list-unit-files --type=service
# enable a service
sudo systemctl enable <service_name>
# view service status (is it running)
sudo systemctl list-units --type=service
sudo systemctl status <service_name>

```
