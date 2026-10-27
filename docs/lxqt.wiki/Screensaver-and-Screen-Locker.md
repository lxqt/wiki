## General

LXQt does not provide its own screensaver or screen locker but any arbitrary screensaver/locker can be used.

In LXQt in order to use the checkbox in [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings), lock actions from the leave-menu, panel's application menus or lxqt-runner and also lock actions configured in [lxqt-config-powermanagement](https://github.com/lxqt/lxqt-powermanagement#translations) a screensaver/locker has to be installed.

With LXQt 2.1 and later a custom screensaver can be added in "Session Settings" for both X11 and Wayland sessions.

### Configuration

Locking before suspend and hibernate can be set in  [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings). Sometimes configuring a delay is needed when the screen is visible for a short time after resume.

Locking when idle or on laptop lid closure can be configured in Main menu → Settings → LXQt Settings → Power Management.

## X11

A xdg-screensaver compliant screensaver works automatically under X11, but also other custom screensavers/lockers can be set in "Session Settings".

`xdg-screensaver 1.1.3` supports the following screensavers:

 * XScreenSaver
 * Xautolock
 * Gnome-screensaver
 * Mate-screensaver
 * Cinnamon-screensaver

Note that the latter three will install many GTK+ dependencies.

A list of available screen-lockers can be found [here](https://wiki.archlinux.org/title/List_of_applications/Security#Screen_lockers).

### XScreenSaver 

XScreenSaver can be configured in Main menu → Settings → Screensaver.

### Other Screenlockers using Xautolock

Xautolock provides autolocking when idle and can be configured to use any installed screenlocker.
Please refer to `man xautolock` for more details about possible configurations. Default timeout for _idle_ is 10min.

A basic setup consists in adding in  [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings) under the "Autostart" section a command like `xautolock -locker i3lock`.

A custom screenlocker can be set manually in `~/.config/lxqt/lxqt.conf` in versions prior to LXQt 2.1:

```
[Screensaver]
lock_command=xsecurelock
```
## Wayland

Working screenlockers are `swaylock`, `hyprlock` and `waylock`, not all compositors support all of them.

!!! note
     Under kwin_wayland `loginctl lock-session` has to be used in "Session Settings" to trigger its built-in screensaver. Disabling the screensaver in KDE's `systemsettings` is recommended as both settings will trigger it.







 