# 001 - Debian GNU Core Headless Server
Recipe to install Debian as a headless server.<br>

### Virtual machine settings
- At least 576 megs of RAM
- 4 gig hard drive
- Bridge-mode network interface card for live IP 

### Objectives
I. Install Debian Buster<br>
II. Housecleaning Tasks <br>
II. Set Static Address on Ethernet Interface<br>



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

### IV. Grub boot loader settings
- Select `Yes` for `Install the GRUB boot loader to the master boot record?`, then press Enter.
- Highlight `/dev/sda` then press Enter.
- Now that the installation is complete, press Enter to `Continue`. The system will reboot.

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
