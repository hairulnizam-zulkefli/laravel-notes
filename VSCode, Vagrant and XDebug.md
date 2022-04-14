# Debugging Laravel with Visual Studio Code and XDebug

Environment: **Ubuntu 18.04, PHP 7.4, Vagrant**

1. SSH to vagrant machine
```
C:\Vagrant\laravel-app> vagrant ssh
```

2. Check PHP version
```
$ php -v
PHP 7.4.28 (cli) (built: Feb 17 2022 16:06:35) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.28, Copyright (c), by Zend Technologies
    with Xdebug v3.1.2, Copyright (c) 2002-2021, by Derick Rethans
```
- If Xdebug is not installed, install from repo
```
$ sudo apt-get install php7.4-xdebug
```
- Or, install from <https://xdebug.org/download>

3. Get host IP address
```
$ netstat -rn | grep "^0.0.0.0 " | cut -d " " -f10
10.0.2.2
```

4. Find xdebug.ini
```
$ php --ini | grep 'xdebug'
/etc/php/7.4/cli/conf.d/99-xdebug.ini
```
Above is the location of Xdebug config for CLI. Edit Xdebug config for FPM.

5. Edit xdebug.ini
```
$ sudo nano /etc/php/7.4/fpm/conf.d/99-xdebug.ini
# OR
$ sudo nano /etc/php/7.4/fpm/conf.d/20-xdebug.ini
```

6. Contents of xdebug.ini
```
zend_extension = xdebug.so
xdebug.mode = debug
xdebug.discover_client_host = true
xdebug.client_host = 10.0.2.2
xdebug.client_port = 9003
xdebug.max_nesting_level = 512
xdebug.start_with_request = yes
xdebug.start_upon_error = yes
xdebug.idekey = VSCODE
xdebug.remote_handler = dbgp
xdebug.log = /tmp/xdebug.log
xdebug.log_level = 7
```
- Log level reference: <https://xdebug.org/docs/all_settings#log_level>

7. Restart PHP
```
$ sudo systemctl restart php7.4-fpm
```
