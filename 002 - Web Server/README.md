# 002 - Web Server using Busybox's httpd
Recipe to install web server with optional PHP and cgi support on bare metal or a virtual machine.<br>

### Virtual machine settings
- At least 576 megs of RAM
- 4 gig hard drive
- Bridge-mode network interface card for live IP 

### Objectives
I. Install Minimum Vanilla Debian Buster<br>
II. Partition disks options (option A or B) <br>
III. System Software Installation<br>
IV. Grub boot loader and reboot<br>
V. Update apt and install basic tools<br>
VI. Root access via SSH<br>
VII. Speedup Boot and Disable IPv6<br>
VIII. Configure NTP<br>
IX. Edit hosts file<br>
X. Optional, Set Static Addess on Ethernet Interface<br>

### I. Install Minimum Vanilla Debian Buster<br>
Set these options when they first appear
- Set name of box to "BUSTER"
- Add a user called "user"
- Set your time zone



XXX: Run BusyBox httpd Web Server as a Daemon

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

- systemctl enable httpd.service
- service httpd start   

Create `/home/user/index.html` and add the following
```
<html><body>
Test website, works!
</body></html>
```

Check to see if the webserver is running by using your browser and open
`http://192.168.10.70`


- apt install php-cgi

Edit `/etc/php/7.3/cgi/php.ini`
```
cgi.force_redirect = 0
cgi.redirect_status_env ="yes";
```

Create `/etc/httpd.conf` and add the following
```
*.php:/usr/bin/php-cgi7.3
```
- service httpd restart 


Create `/home/user/phpinfo.php` and add the following
```
<?php
phpinfo();
?>
```



