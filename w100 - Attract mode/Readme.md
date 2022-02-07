# w100 - Attract mode for Windows
Recipe to cross compile attract mode for Windows with Debian Buster.<br>
We will compile ffmpeg to include only the options we need making the overall executable much smaller.

### Objectives
I. Install Minimum Vanilla Debian Buster<br>
II. Install MXE and prepare Environment <br>
III. System Software Installation<br>
IV. Grub boot loader and reboot<br>
V. Update apt and install basic tools<br>

MXE on Debian Buster
We're going to compile attract mode for windows...

### II. Install MXE and prepare Environment

Install the dependencies...
- apt install autoconf build-essential autopoint bison flex g++-multilib gettext git gperf intltool libgdk-pixbuf2.0-dev libltdl-dev libssl-dev libtool-bin libxml-parser-perl lzip p7zip-full python ruby wget 


Clone the repository 
- git clone https://github.com/mxe/mxe.git

Get the basic compiler up and running
- cd mxe
- make cc


Edit Environment Variables
- export PATH=/root/mxe/usr/bin:$PATH


Edit (/root/mxe/ffmpeg.mk) and replace with the following
```
# This file is part of MXE. See LICENSE.md for licensing information.

PKG             := ffmpeg
$(PKG)_WEBSITE  := https://ffmpeg.org/
$(PKG)_IGNORE   :=
$(PKG)_VERSION  := 4.4
$(PKG)_CHECKSUM := 42093549751b582cf0f338a21a3664f52e0a9fbe0d238d3c992005e493607d0e
$(PKG)_SUBDIR   := $(PKG)-$($(PKG)_VERSION)
$(PKG)_FILE     := $(PKG)-$($(PKG)_VERSION).tar.bz2
$(PKG)_URL      := https://ffmpeg.org/releases/$($(PKG)_FILE)
$(PKG)_DEPS     := cc bzip2 gnutls lame libass libbs2b libcaca \
                   opus sdl2 theora vorbis x264 yasm zlib

define $(PKG)_BUILD
    cd '$(BUILD_DIR)' && '$(SOURCE_DIR)/configure' \
        --cross-prefix='$(TARGET)'- --enable-cross-compile \
        --arch=$(firstword $(subst -, ,$(TARGET))) \
        --target-os=mingw32 --prefix='$(PREFIX)/$(TARGET)' \
        $(if $(BUILD_STATIC), \
            --enable-static --disable-shared , \
            --disable-static --enable-shared ) \
        --x86asmexe='$(TARGET)-yasm' --disable-everything --disable-network --disable-autodetect \
		--disable-ffprobe --enable-decoder=aac*,ac3*,opus,vorbis --enable-demuxer=mov,m4v,matroska \
		--enable-muxer=mp3,mp4 --enable-protocol=file --enable-small --enable-w32threads \
        $($(PKG)_CONFIGURE_OPTS)
    $(MAKE) -C '$(BUILD_DIR)' -j '$(JOBS)'
    $(MAKE) -C '$(BUILD_DIR)' -j 1 install
endef
```

### II. Compile dependency programs

- make ffmpeg sfml libarchive

Compile attractmode with SWF patch
- cd ~/mxe
- git clone https://github.com/mickelson/attract
- cd attract
- wget https://github.com/mickelson/attract/files/7532474/attract-gameswf-gcc11-no-access-control.patch.txt -O Makefile.patch
- patch < Makefile.patch
- make -j3 CROSS=1 TOOLCHAIN=i686-w64-mingw32.static WINDOWS_STATIC=1 WINDOWS_CONSOLE=1

