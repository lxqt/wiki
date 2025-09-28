## Introduction

While using precompiled binary packages will be appropriate most of the time compiling from sources may very well make sense, too, e. g. to run latest development versions if these aren't available on a particular operating system. So here's a guide how to achieve this.

If running latest development snapshots is your intent please note that these can be compiled in a very convenient way on Arch Linux using its [AUR](https://aur.archlinux.org) and that corresponding repositories exist in Debian and openSUSE as well, see [[Binary packages|Binary-packages]].   

We'll try to be verbose yet basic knowledge about working on the shell, package management and how to compile using CMake and GNU make is needed.    
The descriptions stated below should work on any operating system of the *ix ecosystem, that is on all current Linux distributions as well as on BSD. For now distinct packages of Arch Linux, Debian / Ubuntu, Fedora and openSUSE are stated only. Additional distributions may be added.
 
As for Debian to build the release 1.4.x Debian version 12 ("bookworm") is mandatory while to build version 2.0 and higher Debian 13 is needed.

## Prerequisites

Before compiling it is strongly recommended to remove any preexisting LXQt and associated components which were installed other than according to this guide (updating by following the guide once more hasn't been thoroughly tested yet but should work without issues).   

Next we need to install the dependencies to compile LXQt. These are the build environment as well as the various components needed by the software we are going to compile.   

### Build environment

A CMake version ≥ 3.1.8 is required, see documentation of your distribution.   

#### Arch Linux

```
pacman --needed -S base-devel cmake git pkgconf
```

#### Debian, Derivatives

```
apt install build-essential cmake git
```

#### Fedora

```
dnf group install c-development
dnf install cmake git
```

#### openSUSE

```
zypper install -t pattern devel_basis
zypper install cmake git
```

### Qt

Qt version ≥6.6 is required now. Compiling Qt from its sources isn't exactly a trivial task and not within the scope of this document.   
So if you need to compile Qt you may want to look into the various sources addressing this topic (and optionally tell us about your experience).   

#### Arch Linux

```
pacman --needed -S qt6-base qt6-svg qt6-tools 
```

#### Debian, Derivatives

```
apt install qt6-base-private-dev libqt6svg6-dev qt6-tools-dev qt6-tools-dev-tools libpolkit-qt6-1-dev qt6-wayland-private-dev
```

#### Fedora

```
dnf install qt6-qtbase-devel qt6-qtsvg-devel qt6-qttools-devel
```

#### openSUSE

```
zypper install libqt6-qtbase-private-headers-devel libqt6-qtsvg-devel libqt6-qttools-devel qt6-waylandclient-private-devel
```

### Qt extensions developed by KDE team (Frameworks, KScreen, Layer-shell-qt)

#### Arch Linux

```
pacman --needed -S kguiaddons libkscreen kidletime kwindowsystem solid layer-shell-qt
```

#### Debian, Derivatives

```
apt-get install libkf6guiaddons-dev libkf6idletime-dev libkf6screen-dev libkf6windowsystem-dev libkf6solid-dev liblayershellqtinterface-dev
```

#### Fedora

```
dnf install kf6-kguiaddons-devel libkscreen-qt6-devel kf6-kidletime-devel kf6-kwindowsystem-devel kf6-solid-devel layer-shell-qt-devel
```

#### openSUSE

```
zypper install kguiaddons-devel libkscreen2-devel kidletime-devel kwindowsystem-devel solid-devel layer-shell-qt
```

### Miscellaneous

#### Arch Linux

```
pacman --needed -S bash libstatgrab alsa-lib libpulse lm_sensors libconfig muparser upower polkit-qt6 sudo xorg-xmessage libxss libxcursor libxcomposite libxcb libxkbcommon-x11  libfm menu-cache gtk-update-icon-cache hicolor-icon-theme xdg-utils xdg-user-dirs oxygen-icons openbox libxslt
```

#### Debian, Derivatives (Ubuntu see below)

```
apt install bash gtk-update-icon-cache hicolor-icon-theme libasound2-dev libconfig-dev libdbusmenu-qt5-dev libexif-dev libfm-dev libjson-glib-dev libmenu-cache-dev libmuparser-dev libpolkit-agent-1-dev  libpulse-dev libsensors-dev libstatgrab-dev libudev-dev libupower-glib-dev libx11-xcb-dev libxcb-composite0-dev libxcb-damage0-dev libxcb-dpms0-dev libxcb-image0-dev libxcb-randr0-dev libxcb-screensaver0-dev libxcb-util0-dev libxcomposite-dev libxcursor-dev libxdamage-dev libxi-dev libxkbcommon-x11-dev libxss-dev libxtst-dev openbox-dev oxygen-icon-theme sudo x11-utils xdg-user-dirs xdg-utils xserver-xorg-input-libinput-dev libproc2-dev
```

#### Ubuntu

Same as Debian but replace `gtk-update-icon-cache` with `libgtk2.0-bin`.

#### Fedora

```
dnf install systemd-devel bash libstatgrab-devel alsa-lib-devel pulseaudio-libs-devel lm_sensors-devel libconfig-devel muParser-devel upower-devel polkit-devel polkit-qt5-1-devel sudo libexif-devel xorg-x11-apps libSM-devel libXScrnSaver-devel libXcursor-devel libXcomposite-devel libxcb-devel xcb-util-devel libxkbcommon-x11-devel dbusmenu-qt5-devel libfm-devel menu-cache-devel gtk-update-icon-cache hicolor-icon-theme xdg-utils xdg-user-dirs oxygen-icon-theme openbox openbox-devel libxslt-devel
```

#### openSUSE

```
zypper install bash libstatgrab-devel alsa-devel libpulse-devel libsensors4-devel libconfig-devel muparser-devel libupower-glib-devel libpolkit-qt6-1-devel sudo libexif-devel xmessage libXss-devel libXcursor-devel libXcomposite-devel libxcb-devel xcb-util-devel libxkbcommon-x11-devel  libfm-devel menu-cache-devel gtk3-tools hicolor-icon-theme xdg-utils xdg-user-dirs oxygen6-icon-theme openbox-devel libxslt-devel
```

## Compiling

### General/Build Order

In order to meet all mutual dependencies the various components have to be compiled in a particular order which is summed up below.

#### 0 (2 packages)
* [lxqt-build-tools](https://github.com/lxqt/lxqt-build-tools)
* [libdbusmenu-lxqt](https://github.com/lxqt/libdbusmenu-lxqt)

#### I (2 packages)
* [libqtxdg](https://github.com/lxqt/libqtxdg)
* [lxqt-menu-data](https://github.com/lxqt/lxqt-menu-data/)

#### II (4 packages)
* [liblxqt](https://github.com/lxqt/liblxqt)
* [libsysstat](https://github.com/lxqt/libsysstat)
* [qtxdg-tools](https://github.com/lxqt/qtxdg-tools)
* [libfm-qt](https://github.com/lxqt/libfm-qt)

#### III (4 packages)

* [lxqt-globalkeys](https://github.com/lxqt/lxqt-globalkeys)
* [lxqt-qtplugin](https://github.com/lxqt/lxqt-qtplugin)
* [lxqt-session](https://github.com/lxqt/lxqt-session)
* [qtermwidget](https://github.com/lxqt/qtermwidget)

#### IV - All remaining packages in any order

* [lxqt-panel](https://github.com/lxqt/lxqt-panel)
* [pcmanfm-qt](https://github.com/lxqt/pcmanfm-qt)
* [qterminal](https://github.com/lxqt/qterminal)
* [lxqt-powermanagement](https://github.com/lxqt/lxqt-powermanagement)
* [lxqt-runner](https://github.com/lxqt/lxqt-runner)
* [lxqt-themes](https://github.com/lxqt/lxqt-themes)
* [lxqt-admin](https://github.com/lxqt/lxqt-admin)
* [lxqt-notificationd](https://github.com/lxqt/lxqt-notificationd)
* [lxqt-about](https://github.com/lxqt/lxqt-about)
* [lxqt-config](https://github.com/lxqt/lxqt-config)
* [lxqt-policykit](https://github.com/lxqt/lxqt-policykit)
* [lxqt-sudo](https://github.com/lxqt/lxqt-sudo)
* [lxqt-openssh-askpass](https://github.com/lxqt/lxqt-openssh-askpass)
* [lxqt-wayland-session](https://github.com/lxqt/lxqt-wayland-session)
* [pavucontrol-qt](https://github.com/lxqt/pavucontrol-qt)
* [xdg-desktop-portal-lxqt](https://github.com/lxqt/xdg-desktop-portal-lxqt)
* [lxqt-archiver](https://github.com/lxqt/lxqt-archiver)
* [screengrab](https://github.com/lxqt/screengrab)
* [lximage-qt](https://github.com/lxqt/lximage-qt)
* [qps](https://github.com/lxqt/qps)
* [obconf-qt](https://github.com/lxqt/obconf-qt)


These groups must be processed in this order but the order of components within the each group does not matter.     

All components are compiled using CMake and GNU make.   
It is strongly advised to do the builds out of source tree.   
At least on some operating systems like Arch Linux and Debian LXQt doesn't seem to start when CMake variable `CMAKE_INSTALL_PREFIX` which is explained below is *not* set to `/usr` which is something we may have to investigate. On the other hand this assignment will hardly ever do harm so it should be safe to recommend setting `/usr` in general for now.   

### Compiling all components individually

The repositories of all components maintained by the LXQt project as well as nearly all others stated above come with a file `README.md` providing information how to install the respective component.   
This information applies to compiling the archive files of regular releases as well.   
The names of all components in the synopsis [above](#compiling) are links to those repositories you may just follow.   

The general approach is something like
```
mkdir build
cd build
cmake <path to sources> -DCMAKE_INSTALL_PREFIX=/usr
make
make install
```
Directory `build` should be outside the directory of the sources. `cmake` and `make` can and should be run by regular users, `make install` requires root privileges in order to write to the FHS. These can be acquired by `sudo`. This is depicted in section [Scripts to build in one step](#scripts-to-build-in-one-step) and mandates an invocation `sudo make install`. But when all components are compiled on their own its probably more convenient to open an additional root session and invoke `make install` there.   

### Scripts to build in one step

Aside from hosting the main issue tracker repository [lxqt](https://github.com/lxqt/lxqt) provides some scripts to compile latest checkouts of the Git `master` development branch of all components in one step.   
By default this repository does not contain any source code. Rather, source code is pulled by Git submodules, see below.   

Script `build_all_autotools_projects.sh` is covering all components where code is configured by the autotools, `build_all_cmake_projects.sh` those where it is configured by CMake, `build_all.sh` is just sourcing the other two.   
The only components relying on autotools are libfm and libfm-extra of LXDE which are needed by LXQt, too. But by now its' safe to use the precompiled packages providing theses libraries. This was reflected by stating them as [prerequisite](#miscellaneous) and means we'll only have to run `build_all_cmake_projects.sh`. If you do want to compile libfm and libfm-extra from the sources as well you have to uninstall the binary packages providing them as file conflicts will result otherwise.   

To install the software after building the scripts will acquire root permissions via `sudo`. It belongs to the [Prerequisites](#miscellaneous) anyway but may need some configuration depending on the defaults of the various systems.
    
First we have to prepare a local checkout of repository and source code by running `git` on the command line.   
We'll start by cloning
```
git clone https://github.com/lxqt/lxqt.git
```
The repository is using Git submodules to include the actual source code. So next we acquire the sources by running   
```
cd lxqt
git submodule init
git submodule update --remote --rebase
```
It is strongly recommended to make a backup of directory `lxqt` prepared that way so as to avoid repeating this procedure should something go wrong.   

The scripts support several environment variables, see explanation in the scripts' headers. The most important ones are
* `LXQT_PREFIX`: Base directory relative to which LXQt is installed. Points to `/usr/local` by default and should be set to `/usr`. Technically it is corresponding with CMake variable `CMAKE_INSTALL_PREFIX`.
* `LIB_SUFFIX`: Path relative to `LXQT_PREFIX` libraries are stored in. Can be used to adjust the path to habits of Linux distributions where different paths like `/usr/lib`, `/usr/lib64` or `/usr/lib/x86_64-linux-gnu` can be found. Technically it is corresponding with CMake variable `CMAKE_LIB_SUFFIX`.   
Should only be set when the auto-detected default doesn't work as expected.

So finally we can launch by running
```
LXQT_PREFIX=/usr ./build_all_cmake_projects.sh
```

### Qt5 Support

The script will build only with Qt6. For styling Qt5-based applications and allowing them to use LXQt file dialog, the following packages are needed, usually provided by your package manager:

* [libqtxdg 3.12.0](https://github.com/lxqt/libqtxdg/releases/tag/3.12.0)
* [libfm-qt 1.4.0](https://github.com/lxqt/libfm-qt/releases/tag/1.4.0)
* [lxqt-qtplugin 1.4.1](https://github.com/lxqt/lxqt-qtplugin/releases/tag/1.4.1)


And if you want to compile them, you will need:
* [lxqt-build-tools 0.13.0](https://github.com/lxqt/lxqt-build-tools/releases/tag/0.13.0)

These four Qt5-based packages can be installed in parallel with their Qt6-based versions, the fourth one being needed only for compiling the others.
