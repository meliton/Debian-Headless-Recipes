# 003 - File Server using SAMBA
Recipe to install a file server using samba on bare metal or a virtual machine.<br>

### Prerequisites
Follow the instructions for `001 - Debian GNU Core Headless Server` before continuing.<br>
You should provide enough space to store files.<br>
A static address is highly recommended.<br>
#### Creates the following home directories to save files.<br>
  `/home/user1/` <-- default location of user<br>
  `/home/user2/` <-- default location of user2<br>
  `/home/user3/` <-- default location of user3<br>

Assumes IP address is `192.168.10.70`.

### Objectives
I. Add Users to the System<br>
II. Create Service Daemon File<br>
III. System Software Installation<br>
IV. Install PHP and CGI Support (optional from here)<br>
V. Configure PHP for CGI Support<br>
VI. Test PHP<br>
VII. Test CGI

### I. Add Users to the System
`adduser user1`  <-- adds the user1 user... password is `password`<br>
`adduser user2`  <-- adds the user2 user... password is `password`<br>
`adduser user3`  <-- adds the user3 user... password is `password`<br>

### II. Install SAMBA/CIFS
`apt install samba`			<-- creates ability to share<br>
`apt install cifs-utils`	<-- allows automounting shares via fstab<br>
`apt install smbclient`		<-- allows `smbclient -L localhost` command<br>

### III. Create SAMBA Passwords for Users
`smbpasswd -a root`<br>
`smbpasswd -a user1`<br>
`smbpasswd -a user2`<br>
`smbpasswd -a user3`<br>


Edit (/etc/samba/smb.conf) by adding the following information to the end 
```
[user1]
comment = Danger, Admin Share
path = /home/user1
read only = No
valid users = root, user1

[user2]
comment = Danger, Admin Share
path = /home/user1
read only = No
valid users = root, user1

[user3]
comment = Danger, Admin Share
path = /home/user1
read only = No
valid users = root, user1

[user4]
comment = Danger, Admin Share
path = /home/user1
read only = No
valid users = root, user1
```

...In the the [global] section, fix ...maybe...
  workgroup    = HOME          <-- to the right workgroup i.e HOME
  netbios name = FILESERVER    <-- and add this...
  
- testparm           <-- checks samba configuration changes

