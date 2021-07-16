# 001 - Debian GNU Core Headless Server
Recipe to install Debian as a headless server on bare metal or a virtual machine.<br>

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


### I. Install Minimum Vanilla Debian Buster<br>
Set these options when they first appear
- Set name of box to "BUSTER"
- Add a user called "user"
- Set your time zone

### II. Partition disks options (option A or B)
Option A: Select the default `Guided - use entire disk` option.
- Press Enter then select `All files in one partition (recommended for new users)`
- Select `Finish partitioning and write changes to disk` and press Enter
- Select `Yes` to `Write the changes to disk?` and press Enter.
- Go to part III.

Option B: Select the `Manual` option.
- HARD DRIVE SELECTION
  - Select the hard drive `(sda)` and press Enter
  - You may need to `Create new empty partition table on this device?` Select `Yes`, then press Enter.
- SWAP FILE SETUP
  - Now, highlight the `pri/log    4.3 GB    FREE SPACE` drive and press Enter.
  - Highlight `Create a new partition`, then press Enter.
  - Set the `New partition size:` to `110M`, then press Enter.
  - Select `Primary` for the `Type of the new partition`, then press Enter.
  - Select `Beginning` for the `Location for the new partition`, then press Enter.
  - Press Enter to change the `Use as:` partition setting to `swap area`.
  - Select `Done setting up the partition`, then press Enter.
- SYSTEM PARTITION SETUP
  - Now, highlight the `pri/log    4.2 GB    FREE SPACE` drive and press Enter.
  - Highlight `Create a new partition`, then press Enter.
  - Set the `New partition size:` to `max`, then press Enter.
  - Select `Primary` for the `Type of the new partition`, then press Enter.
  - Highlight `Bootable flag` and press Enter to toggle it to `on`.
  - Select `Done setting up the partition`, then press Enter.
- FINALIZE PARTITION
  - Select `Finish partitioning and write changes to disk` and press Enter
  - Select `Yes` to `Write the changes to disk?` and press Enter.
  - Go to part III.

### III. System Software Installation
- Select `No` for `Scan another CD or DVD?`, then press Enter.
- Select `Yes` for `Use a network mirror?`, then press Enter.
  - You can use the defaults of `United States` and `deb.debian.org` (or the settings for your country)
  - Most people are not using a HTTP proxy so it is safe to press Enter here.
- Select `No` for `Participate in the package usage survey?` then press Enter.
- Use the space bar to set the following options and then `Continue`.
```
[ ] Debian desktop environment
[ ] ... GNOME
[ ] ... Xfce
[ ] ....all the others are unchecked
[ ] web server
[ ] print server
[*] SSH server
[ ] standard system utilities
```

### IV. Grub boot loader and reboot
- Select `Yes` for `Install the GRUB boot loader to the master boot record?`, then press Enter.
- Highlight `/dev/sda` then press Enter.
- Now that the installation is complete, press Enter to `Continue`. The system will reboot.

### V. Update apt and install basic tools
Edit `/etc/apt/sources.list` file<br>
- You'll comment out all references to `deb cdrom:` with a `#`
- You'll add `main contrib non-free` at the end of each `deb` and `deb-src` line.
```
# deb cdrom:[blah blah blah....]
deb http://deb.debian.org/debian/ buster main contrib non-free
deb-src http://deb.debian.org/debian/ buster main contrib non-free
...
```

`apt update`<br>
`apt upgrade`<br>
`apt install net-tools`   <-- install network tools like netstat, ping, etc<br>
`apt install dnsutils`    <-- dig and other dns tools<br>


### VI. Root access via SSH
Edit `(/etc/ssh/sshd_config)` file 
```
ListenAddress 0.0.0.0
PermitRootLogin yes
```

### VII. Speedup Boot and Disable IPv6
Edit `(/etc/default/grub)` file
```
GRUB_TIMEOUT=2
GRUB_CMDLINE_LINUX="ipv6.disable=1 quiet"
```
`update-grub`   <-- applies those changes

### IX. Configure NTP
Edit (/etc/systemd/timesyncd.conf) file to change to Google's time servers
```
NTP=time.google.com
```
- systemctl restart systemd-timesyncd.service    <-- restarts the time service
- systemctl status systemd-timesyncd.service     <-- check the time service status
- timedatectl        

### X. Verify hosts and hostname
Edit (/etc/hostname) to be printsrv
```
printsrv
```

Edit (/etc/hosts) file adding the following to the end
```
127.0.0.1       printsrv
```

II.  Set Static Addess on Ethernet Interface
Set static address by editing (/etc/network/interfaces) file
```
iface eth0 inet static
     address 192.168.10.70
     network 192.168.10.0
     netmask 255.255.255.0
     broadcast 192.168.10.255
     gateway 192.168.10.1
```
