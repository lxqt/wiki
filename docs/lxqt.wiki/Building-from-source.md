## Introduction

While precompiled binary packages are appropriate most of the time, compiling from source may also make sense, for example, to run the latest development versions when they aren't available for a particular operating system. Here's a guide on how to do this.

If running the latest development snapshots is your intent, please note that you can compile them conveniently on Arch Linux using its [AUR](https://aur.archlinux.org). Corresponding repositories are also available for Debian and openSUSE; see [[Binary packages|Binary-packages]].   

We'll try to be thorough, but you will need basic knowledge of the shell, package management, and compiling with CMake and GNU make.    
The instructions below should work on any operating system in the *nix ecosystem, including current Linux distributions and BSD. For now, instructions are provided only for Arch Linux, Debian/Ubuntu, Fedora, and openSUSE. Additional distributions may be added.
 
For Debian, version 12 ("bookworm") is required to build the 1.4.x release, while Debian 13 is required to build version 2.0 and later.

## Prerequisites

Before compiling, we strongly recommend removing any preexisting LXQt components installed by other means. Updating an installation by following this guide again has not been thoroughly tested, but it should work without issues.   

Next, install the dependencies for compiling LXQt. These include the build environment and the components required by the software we plan to compile.   

### Build environment

A CMake version ≥ 3.1.8 is required; see your distribution's documentation.   

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

A Qt version ≥ 6.6 is now required. Compiling Qt from source is not trivial and is outside the scope of this document.   
If you need to compile Qt, you may want to consult the resources on this topic and optionally tell us about your experience.   

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
apt-get install libkf6guiaddons-dev libkf6idletime-dev libkscreen-dev libkf6windowsystem-dev libkf6solid-dev liblayershellqtinterface-dev
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

To meet all dependencies, compile the components in the order summarized below.

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


Process the groups in this order, but the order of components within each group does not matter.     

All components are compiled using CMake and GNU make.   
We strongly recommend building outside the source tree.   
On some operating systems, such as Arch Linux and Debian, LXQt does not seem to start unless the CMake variable `CMAKE_INSTALL_PREFIX` (explained below) is set to `/usr`; this may need further investigation. Setting it to `/usr` is unlikely to cause harm, so it seems safe to recommend for now.   

### Compiling all components individually

The repositories for all LXQt components, as well as nearly all other components listed above, include a `README.md` file with information on how to install each component.   
This information also applies to compiling the source archives of regular releases.   
The component names in the [list above](#compiling) link to their repositories.   

The general approach is something like
```
mkdir build
cd build
cmake <path to sources> -DCMAKE_INSTALL_PREFIX=/usr
make
make install
```
The `build` directory should be outside the source directory. Run `cmake` and `make` as a regular user; `make install` requires root privileges to write to the FHS. You can acquire these with `sudo`. As shown in [Scripts to build in one step](#scripts-to-build-in-one-step), use `sudo make install`. When compiling components individually, it may be more convenient to open an additional root session and run `make install` there.   

### Scripts to build in one step

In addition to hosting the main issue tracker, the [lxqt repository](https://github.com/lxqt/lxqt) provides scripts to compile the latest checkouts of all components from the Git `master` development branch in one step.   
By default, this repository does not contain source code; instead, it fetches the source through Git submodules (see below).   

`build_all_autotools_projects.sh` handles components configured with Autotools; `build_all_cmake_projects.sh` handles components configured with CMake; and `build_all.sh` runs the other two.   
The only components that rely on Autotools are libfm and libfm-extra from LXDE, which are also needed by LXQt. For now, it is safe to use the precompiled packages that provide these libraries. They are listed under [Miscellaneous](#miscellaneous), so we only need to run `build_all_cmake_projects.sh`. If you want to compile libfm and libfm-extra from source as well, uninstall the binary packages that provide them to avoid file conflicts.   

After building, the scripts use `sudo` to acquire root privileges during installation. `sudo` is listed under [Miscellaneous](#miscellaneous), but may need configuration depending on the system's defaults.
    
First, prepare a local checkout of the repository and its source code with `git`.   
We'll start by cloning
```
git clone https://github.com/lxqt/lxqt.git
```
The repository uses Git submodules for its source code. Initialize and update them with:   
```
cd lxqt
git submodule init
git submodule update --remote --rebase
```
We strongly recommend backing up the `lxqt` directory after this step so you do not need to repeat the procedure if something goes wrong.   

The scripts support several environment variables; see their explanations in the script headers. The most important ones are:
* `LXQT_PREFIX`: Base directory relative to which LXQt is installed. It points to `/usr/local` by default and should be set to `/usr`. This corresponds to CMake's `CMAKE_INSTALL_PREFIX` variable.
* `LIB_SUFFIX`: Path relative to `LXQT_PREFIX` where libraries are stored. Use it to adjust for Linux distributions with paths such as `/usr/lib`, `/usr/lib64`, or `/usr/lib/x86_64-linux-gnu`. This corresponds to CMake's `CMAKE_LIB_SUFFIX` variable.   
Set it only when the auto-detected default does not work as expected.

Finally, launch the build with:
```
LXQT_PREFIX=/usr ./build_all_cmake_projects.sh
```

### Qt5 Support

The script builds only with Qt 6. To style Qt 5-based applications and let them use the LXQt file dialog, install the following packages, usually through your package manager:

* [libqtxdg 3.12.0](https://github.com/lxqt/libqtxdg/releases/tag/3.12.0)
* [libfm-qt 1.4.0](https://github.com/lxqt/libfm-qt/releases/tag/1.4.0)
* [lxqt-qtplugin 1.4.1](https://github.com/lxqt/lxqt-qtplugin/releases/tag/1.4.1)


To compile them, you will also need:
* [lxqt-build-tools 0.13.0](https://github.com/lxqt/lxqt-build-tools/releases/tag/0.13.0)

These four Qt 5-based packages can be installed alongside their Qt 6-based versions; `lxqt-build-tools` is needed only to compile the other three.
