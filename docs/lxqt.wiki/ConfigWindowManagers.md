## Window managers (X11)

## General

LXQt does not provide its own window manager but any arbitrary window manager can be used, most notably Openbox, KWin (the window manager of KDE), and Xfwm4 (the window manager of Xfce).   

The current window manager is stored in section `[General]` of file `session.conf`, see [ConfigGeneral](ConfigGeneral#general).
```
[General]
[...]
window_manager=xfwm4
[...]
```

It can be chosen in configuration dialogue [LXQt Session Settings](https://github.com/lxde/lxqt-session#lxqt-session-settings) of [lxqt-session](https://github.com/lxde/lxqt-session).

#### Known Window Managers

Listed in `/usr/share/lxqt/windowmanagers.conf` shipped by [LXQt Session](https://github.com/lxqt/lxqt-session/blob/master/config/windowmanagers.conf) and if installed are shown in _Session Settings > Window Manager_. Window manager not listed can be set by their path.

#### Keyboard Shortcuts

Window manager related shortcuts (raise/lower, maximize/minimize, desktop and window switching and else) are handled by their configuration and not by [lxqt-globalkeys](https://github.com/lxqt/lxqt-globalkeys). Therefore conflicts are possible, some of them will be notified at session startup. Avoiding identical shortcuts is recommended.

**Note**: Assigning Super_L ("Windows" or "Meta" key) for opening the main menu in the panel or switching keyboard layout disables the use of this key as a modifier, disabling eventually some of the default keybindings in the window manager in use.

#### Compositor

For desktop effects, shadows, transparency and else a compositor has to be enabled. Kwin and Xfwm4 provide their own compositor, for openbox or other Window Managers 'picom' (active fork of compton) can be used.

A configuration tool for picom is: [picom.conf](https://github.com/redtide/picom-conf/).


### Openbox

Configuration is handled by a configuration file [rc.xml](http://openbox.org/wiki/Help:Configuration), the user specific copy of which is normally found in `~/.config/openbox/`.   

**Note**: Prior to version 0.17 LXQt was using a specific copy of this file` ~/.config/openbox/lxqt-rc.xml` and the project is maintaining a GUI [ObConf-Qt](https://github.com/lxde/obconf-qt) to edit this file. Users using openbox and upgrading to version 0.17 can copy the deprecated file to `~/.config/openbox/rc.xml` in order to preserve their customization. 

#### Shortcuts

Keyboard shortcuts can only be changed, added or removed editing directly `~/.config/openbox/rc.xml`

### KWin

The KWin configuration is stored in `~/.config/kwinrc`. Note that the KDE Plasma concepts for window management differ from Xorg de-facto standard concepts. Therefore the integration with LXQt is not always straight forward.

The only GUI to configure KWin in LXQt sessions is KDE's [System Settings](https://userbase.kde.org/System_Settings). Unfortunately this represents a drawback of using KWin as part of KDE Plasma is needed to get this to work reasonably in LXQt sessions. A minimal working setup to configure KWin can be achieved by installing `systemsettings kitemmodels plasma-workspace`.

An essential custom menu entry for kwin can be installed from [here](https://github.com/lxqt/lxqt-WM-integration-files#kwin_x11).

#### Shortcuts
KDE stores keyboard shortcuts in the `~/.config/kglobalshortcutsrc` configuration file. The GUI can be installed separately, but requires the complete `plasma-desktop` package.

**Note**: Kwin version 6.*  does not start `kglobalacceld` automatically anymore, adding  `/usr/lib/kglobalacceld` in LXQt `Session Settings > Autostart` is needed.

#### Issues

##### Show Desktop

KWin does not distinguish PcManFm-Qt windows from the desktop which is drawn by PcManFm-Qt. Eventually open file manager windows will therefore not be minimized but only blurred. To have "Show Desktop" fully working following steps are necessary:
* In `lxqt-globalshortcuts` "Show Desktop" keybinding is disabled
* No "Show Desktop" plugin on any panel
* In _KDE Systemsettings > Window Managment > KWin Scripts_ "MinimizeAll" is activated
* In _KDE Systemsettings > Shortcuts_ searching for "MinimizeAll" a shortcut is set


`~/.config/kglobalshortcutsrc`

```
...
MinimizeAll=Alt+D,none,MinimizeAll
...

```
With `xdotool` a "Show Desktop" button on the panel can be created by adding the "Custom Command" plugin, set `xdotool key "alt+d"` (if shortcut is Alt D) as left click command, icon `desktop.svg` and unset all other settings.

##### Font of titlebar

The settings in `~/.config/kdeglobals` are not respected outside of Plasma. The only way to change the titlebar font is using [breeze-enhanced](https://aur.archlinux.org/packages/breeze-enhanced-git) instead of breeze in KDE settings.

### Xfwm4

Three tools are provided to configure this window manager.   
* Window Manager: Basic settings. Binary `xfwm4-settings`, desktop entry file `xfce-wm-settings.desktop`.
* Window Manager Tweaks: Additional / advanced settings. Binary `xfwm4-tweaks-settings`, desktop entry file `xfce-wmtweaks-settings.desktop`.
* Workspaces: Configures the virtual desktops. Binary `xfwm4-workspace-settings`, desktop entry file `xfce-workspaces-settings.desktop`.

**Note**: All desktop entry files mentioned above have by default key `OnlyShowIn=XFCE;` and will thus not be visible in LXQt sessions including the panel's main menu. In order to use these desktop entry files in LXQt sessions remove this line either in the system copy of the files, normally in `/usr/share/applications/`, or in a user specific copy which has to be placed in `~/.local/share/applications/` most of the time. The latter is the preferred solution as it won't be overwritten by updates:
 ```
 mkdir ~/.local/share/applications
 cp /usr/share/applications/xfce-w* ~/.local/share/applications

Those files ready for use can be installed from [here](https://github.com/lxqt/lxqt-WM-integration-files#xfwm4).
```
#### Shortcuts

Shortcuts can be modified in _Preferences > Window Manager >  Keyboard_ , binary `xfwm4-settings`.

### Other Window Manager


A comprehensive list can be found in the [Arch Wiki](https://wiki.archlinux.org/title/Window_manager#List_of_window_managers).



  





