# 002 - Web Server using Busybox's httpd
Recipe to install BusyBox httpd Web Server as a daemon with optional PHP and cgi support on bare metal or a virtual machine.<br>

### Prerequisites
Follow the instructions for `001 - Debian GNU Core Headless Server` before continuing.<br>
A static address is highly recommended.<br>
Assumes web server home directory is `/home/user`.<br>
Assumes IP address is `192.168.10.70`.

### Objectives
I. Verify busybox httpd applet<br>
II. Create Service Daemon File<br>
III. System Software Installation<br>
IV. Install PHP and CGI Support (optional from here)<br>

### I. Verify busybox httpd applet
At the terminal, type `busybox` or `busybox --list` and look for `httpd`


### II. Create Service Daemon File
Create `/etc/systemd/system/httpd.service` file with the following
```
[Unit]
Description=Busybox httpd Daemon

[Service]
User=root
Group=root
Type=forking
GuessMainPID=no
ExecStart=/bin/busybox httpd -h /home/user

[Install]
WantedBy=multi-user.target
```

`systemctl enable httpd.service`<br>
`service httpd start`

### III. Create Sample index file for Webserver
Create `/home/user/index.html` and add the following
```
<html><body>
Test website, works!
</body></html>
```

Open a web browser and and check `http://192.168.10.70`

### IV. Install PHP and CGI Support (optional from here)
`apt install php-cgi`

Make note of your php version. Mine is 7.3.

### V. Configure PHP for CGI Support
Edit `/etc/php/7.3/cgi/php.ini` file
```
cgi.force_redirect = 0
cgi.redirect_status_env ="yes";
```

Create `/etc/httpd.conf` and add the following
```
*.php:/usr/bin/php-cgi7.3
```

`service httpd restart `  <-- restarts web service<br>

### VI. Test PHP
Create `/home/user/phpinfo.php` and add the following
```
<?php
phpinfo();
?>
```

Open a web browser and and check `http://192.168.10.70/phpinfo.php`

