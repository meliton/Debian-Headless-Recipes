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
II. Install SAMBA/CIFS<br>
III. Create SAMBA Passwords for Users<br>
IV. Configure SAMBA<br>
V. Test SAMBA from Windows 7/10<br>


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

### IV. Configure SAMBA
Edit `/etc/samba/smb.conf` by adding the following information to the end 
```
[user1]
comment = Danger, Admin Share
path = /home/user1
read only = No
valid users = root, user1

[user2]
comment = Danger, Admin Share
path = /home/user2
read only = No
valid users = root, user2

[user3]
comment = Danger, Admin Share
path = /home/user3
read only = No
valid users = root, user3
```

...In the the [global] section, fix ...maybe...<br>
`workgroup    = HOME`      <-- to the right workgroup i.e HOME<br>
`netbios name = BUSTER`    <-- and add this...<br>
  
`testparm`           <-- checks samba configuration changes<br>

### V. Test SAMBA from Windows 7/10
From a Windows 7 or 10 computer...



