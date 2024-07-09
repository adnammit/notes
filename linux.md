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
```
