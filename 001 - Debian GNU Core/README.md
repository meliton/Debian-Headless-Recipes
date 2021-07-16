# 001 - Debian GNU Core Headless Server
Recipe to install Debian as a headless server. 

### Objectives
I. Install Debian Buster<br>
II. Housecleaning Tasks <br>
II. Set Static Address on Ethernet Interface<br>



### I. Install Minimum Vanilla Debian Buster<br>
** Set these options when they first appear<br>
Set name of box to "BUSTER"<br>
Add a user called "user"<br>

```
==============================================
[ ] Debian desktop environment
[ ] ... GNOME
[ ] ... Xfce
[ ] ....all the others are unchecked
[ ] web server
[ ] print server
[*] SSH server
[ ] standard system utilities
==============================================
```

#### Edit (/etc/apt/sources.list) file and add at the end of each line<br>
```
==============================================
main contrib non-free
==============================================
```

`- apt update`<br>
`- apt upgrade`<br>
`- apt install net-tools`   <-- install network tools like netstat, ping, etc<br>
`- apt install dnsutils`    <-- dig and other dns tools<br>
