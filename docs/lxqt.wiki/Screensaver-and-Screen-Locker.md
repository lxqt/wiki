LXQt does not provide its own screensaver or screen locker but any arbitrary screensaver/locker can be used.

In LXQt in order to use the checkbox in [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings), lock actions from the leave-menu or lxqt-runner and lock actions  configured in [lxqt-config-powermanagement](https://github.com/lxqt/lxqt-powermanagement#translations) a xdg-screensaver compliant screensaver is needed under X11.

From LXQt 2.1 on a custom screensaver can be added in "Session Settings" for both X11 and Wayland sessions

```xdg-screensaver 1.1.3``` supports the following screensavers:

 * XScreenSaver
 * Xautolock
 * Gnome-screensaver
 * Mate-screensaver
 * Cinnamon-screensaver

Note that the latter three will install many GTK+ dependencies.

A list of available screen-lockers can be found [here](https://wiki.archlinux.org/title/List_of_applications/Security#Screen_lockers).

#### Wayland

Available screenlockers are swaylock, hyrplock and waylock, not all compositors support all of them.
Under kwin `loginctl lock-session` has to be used in "Session Settings" to trigger its screensaver.

### Configuration

Locking before suspend and hibernate can be set in  [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings). Sometimes configuring a delay is needed when the screen is visible for a short time after resume.

Locking when idle or on laptop lid closure can be configured in Main menu → Settings → LXQt Settings → Power Management.


#### XScreenSaver 

XScreenSaver can be configured in Main menu → Settings → Screensaver.

#### Other Screenlockers using Xautolock

Xautolock provides autolocking when idle and can be configured to use any installed screenlocker.
Please refer to ```man xautolock``` for more details about possible configurations. Default timeout for _idle_ is 10min.

A basic setup consists in adding in  [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings) under the "Autostart" section a command like  ```xautolock -locker i3lock```.

A custom screenlocker can be set manually in `~/.config/lxqt/lxqt.conf` in versions prior to LXQt 2.1:

```
[Screensaver]
lock_command=xsecurelock
```




 