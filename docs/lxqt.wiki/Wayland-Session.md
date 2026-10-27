For an overall Wayland support, LXQt 2.1.0 or later is required, while most components already support Wayland in V2.0.x. To start a LXQt Wayland Session select "LXQt (Wayland)" in your display manager. From tty the session can be started with `startlxqtwayland`.

[lxqt-wayland-session](https://github.com/lxqt/lxqt-wayland-session) has to be installed for enabling "Wayland Settings" in "Session Settings". The panel can choose a specific backend for each compositor to enable workspace switcher, other workspace related menu-items, and more. In LXQt 2.3 the available backends are X11 (xcb), one for kwin_wayland, one for Wayfire and general one for wlroots-compatible compositors.
## Compositors

The compositor can be chosen in "Session Settings" under "Wayland Settings". The supported stacking compositors are labwc, Wayfire and kwin_wayland, while the tiling ones are Hyprland, Niri, River and Sway. For a working taskbar the compositor should support `wlr-foreign-toplevel-management-unstable-v1` protocol. For starting an LXQt session under other compositors, the command `lxqt-session && <logout_command>` has to be added in the autostart setting of the compositor. Forks of the aforementioned compositors should work fine all.

For the supported compositors, minimal default settings based on their default settings are shipped and will be installed in `~/.config/lxqt/wayland/` folder at the first start, except for `kwin_wayland` and `labwc`, where the default configurations are used in `~/.config/`.

!!! note
     Some distributions, notably OpenSuse, may use also different location for labwc config.

GUI configuration tools are available for labwc ([labwc-tweaks](https://github.com/labwc/labwc-tweaks)), niri ([niri-settings](https://github.com/stefonarch/niri-settings)), Wayfire ([wcm](https://github.com/WayfireWM/wcm)) and kwin (Plasma's `systemsettings`. Customized menu entries for LXQt can be installed from [here](https://github.com/lxqt/lxqt-WM-integration-files#kwin_wayland).

> [!NOTE] 
> Consider disabling the shortcut for exiting directly the compositor to avoid accidental killing of the session.

## Disabled Components

Some components cannot be used under Wayland, and an information dialog will be shown if they are launched there. This includes "Shortcut Keys", power key settings in "Power Management", and "Monitor Settings" which works only under kwin_wayland. Instead in "Keyboard and Mouse" some settings are not present - all those settings are handled directly by the compositor now and can be configured in its settings. Some panel widgets are not supported at all (Keyboard State) or not on every compositor (Desktop Switcher, Show Desktop), depending on whether there is a dedicated backend available or not. Some right click menu-items of task-bar buttons are grayed out if unavailable. See the table for an overview (LXQt ≥ 2.3):

<img width="1125" height="819" alt="table-wiki" src="https://github.com/user-attachments/assets/471221f2-7683-48ef-8efd-c33aa1bf9711" />

*) On tiling compositors the desktop is shown on empty workspaces.

ScreenGrab ≥ 3.0 has support for Wayland.

## LXQt Panel's Wayland Backends

In LXQt ≥ 2.3 three Wayland backends are available: kwin_wayland, wayfire and a generic one for wlroots-compatible compositors. The correct backend will be automatically detected for all seven supported compositors.

For special cases the backend to load can be forced manually for any desktop specified in `XDG_CURRENT_DESKTOP` by a comma-separated list in `~/.config/lxqt/panel.conf`:
```ini
[General]
…
preferred_backend=labwc:wlroots, niri:wlroots, xyz:my_own_backend
…
```

## Screen Lockers

Supported/tested screenlockers are `swaylock`, `waylock` and `hyprlock`. On kwin_wayland `loginctl lock-session` has to be set in the input field. Other Wayland screenlockers may work as well.

## Global Shortcuts

Shortcuts are handled exclusively by the compositor. Because of that, applications launched by such shortcuts might not exit decently on logging out if they are running before the session ends. That might result in unsaved settings on logging out. The reason is that their processes are not controlled by LXQt Session.

As a workaround, you can use shortcuts that launch applications through lxqt-session's *D-BUS interface* (with lxqt-session ≥ 2.2.0 or git), which makes them exit gracefully when the session ends. In LXQt ≥ 2.3 a wrapper script `lxqt-qdbus` is available which simplifies the command syntax. An example for labwc:

Instead of

```xml
<action name="Execute" command="featherpad"/>
```
Use

```xml
<action name="Execute" command="qdbus6 org.lxqt.session /LXQtSession execDesktopFile featherpad.desktop"/>
```
Using the wrapper script the application name has to be used:

```xml
<action name="Execute" command="lxqt-qdbus run featherpad"/>
```

The names of desktop files can be found in `/usr/share/applications/`. The full command is:

```bash
qdbus6 org.lxqt.session /LXQtSession execDesktopFile DESKTOP_FILE_NAME_OR_PATH
```
except on Suse/Fedora:
```bash
qdbus-qt6 org.lxqt.session /LXQtSession execDesktopFile DESKTOP_FILE_NAME_OR_PATH
```

Also, a desktop file can be made for any executable file, and its full path can be used in a command like above. But, for a desktop file inside `$XDG_DATA_DIRS/applications/`, the file name is enough.

###  Brightness, Volume, Multimedia Keys (XF86-*)

Please check your compositor configuration if some of them aren't working. Sway, for example, uses `pactl` (part of libpulse) for volume, while labwc and Wayfire use `pamixer` by default, which may not be present in your installation. The already mentioned wrapper script will act directly on the volume plugin on the panel:
```sh
lxqt-qdbus volume up|down|mute
```

## Global Scaling

*The global screen scaling of "LXQt Session Settings" is only for X11 and should not be used on Wayland.* Wayland supports per-monitor scaling, and best results can be achieved in the settings of the compositor, if existing. 
Examples:

Hyprland: `monitor=,preferred,auto,1.2`

Sway: `Output eDP-1 scale 1.2`

With all compositors that support `wlr-output-management protocol`, [kanshi](https://sr.ht/~emersion/kanshi/) or [shikane](https://github.com/hw0lff/shikane) can be configured. With kwin_wayland, KDE's `systemsettings` is the best tool.

## Screensharing

On all compositors except kwin_wayland `xdg-desktop-portal-wlr` will work, make sure `slurp` is installed too.

### kwin_wayland

Three files are needed in order to use the advanced KDE Plasma portal which allows sharing windows, screens and more:

`~/.config/xdg-desktop-portal/portals.conf`:

```ini
 [preferred]
 default=kde
 org.freedesktop.impl.portal.FileChooser=lxqt
```
`~/.local/bin/lxqt-kde-portal_wrapper.sh`:

```bash
#!/bin/sh
export XDG_CURRENT_DESKTOP=KDE

# Execute the KDE desktop portal
[ -x /usr/libexec/xdg-desktop-portal-kde ] && exec /usr/libexec/xdg-desktop-portal-kde 
[ -x /usr/lib/x86_64-linux-gnu/libexec/xdg-desktop-portal-kde ] && exec /usr/lib/x86_64-linux-gnu/libexec/xdg-desktop-portal-kde
exec /usr/lib/xdg-desktop-portal-kde
```
The above file has to be executable:
`chmod +x ~/.local/bin/lxqt-kde-portal_wrapper.sh`

`~/.local/share/dbus-1/services/org.freedesktop.impl.portal.desktop.kde.service`:
```bash
[D-BUS Service]
Name=org.freedesktop.impl.portal.desktop.kde
Exec=/bin/sh -c 'exec ${HOME}/.local/bin/lxqt-kde-portal_wrapper.sh'
```
Reboot is needed usually to make it work. Please note that you need to change or remove `portals.conf` to make screensharing work on other compositors.

## Third party tools (mostly for wlroots, not kwin_wayland)

* `swayidle` Screen blanking/monitor off in combination with `wlopm`
* `swaybg` Background below desktop (splash)
* `lswt` list Wayland toplevels (windows), useful for window rules
* `wlrctl` Command line utility for miscellaneous wlroots extensions
* `wlopm` Turn off monitor(s)
* `wlsunset`,`gammastep`,`hypersunset` Night light (blue light filter)
* `wtype` Sending keystrokes (p.e. in custom command plugin or shortcuts)
* `kanshi` Monitor settings
* `wdisplay` Monitor settings on the fly
* `copyQ` Clipboard manager; *works fine under kwin_wayland* as well as wlroots
* `wvkbd` Virtual keyboard
* `wev` List keypress and mouse events
* `grim` and `slurp` Screenshot
* `wf-recorder ` Screencast
* `wshot` GUI for screenshots

## Issues, Tips and Tricks

### Tooltips and popups (Qt bugs)

* On bottom and right panels some tooltips and some popups align to screen border.

See https://github.com/lxqt/lxqt-panel/issues/2092 for status.

### Generic Icons in Taskbar

Check "Use icons by window class" in taskbar settings.

### Desktop icons under the panel

Read https://github.com/lxqt/pcmanfm-qt/wiki#using-desktop-under-wayland

### Forcing use of Wayland

Applies to electron-based apps like VS Code, Slack, Discord, Spotify, Obsidian, Element (Matrix client) and also to Chrome/Chromium based browsers:

`.config/electron-flags.conf`:
```
--enable-features=UseOzonePlatform
--ozone-platform=wayland
```
* Darktable:`GDK_BACKEND=wayland darktable` (can be added in the `.desktop` file)


### Turn off monitor when idle

LXQt's power manager settings for turning off the monitor will work fine if you use the configuration dialog of version 2.2.2 or later or manually add a line to `~/.config/lxqt/power.conf` for previous versions:

#### kwin_wayland

Make sure you have `kscreen-doctor`. It is provided by `libkscreen` in Arch.

```bash
monitorOffCommand_wayland=kscreen-doctor --dpms off
```
#### Niri

```bash
monitorOffCommand_wayland=niri msg action power-off-monitors
```
#### Hyprland 
```bash
monitorOffCommand_wayland=hyprctl dispatch dpms off
```
To wake up all monitors it needs in `lxqt-hyprland.conf` under "misc":
```css
misc {
    ...
    mouse_move_enables_dpms=true
    key_press_enables_dpms=true
}
```
#### Labwc, others

Make sure `wlopm` is installed; works only for an external monitor. Use `wlopm` to get the monitor name. Examples:
```bash
monitorOffCommand_wayland=wlopm --off HDMI-A-1
monitorOffCommand_wayland=wlopm --off "*"
```
A tool which apparently works with all screens is https://github.com/lilydjwg/dpms-off, but it needs more tests by more users.

The other actions of the LXQt idleness watcher work in all supported compositors, but this action only works with the above commands for now, and you can control it in the right-click menu of the battery tray icon, as in X11. Use `swayidle` or  `hypridle ` with other compositors.


### 3-finger-swipe to switch workspace

Default in niri, can be enabled in Hyprland and Wayfire. For other compositors as kwin and Hyprland a workaround can be used.

`~/.config/libinput-gestures.conf` :

```
# Add this to auto-start in compositor: libinput-gestures-setup start
# https://github.com/bulletmark/libinput-gestures

# sway
gesture: swipe left 3   swaymsg -t command workspace next_on_output
gesture: swipe right 3  swaymsg -t command workspace prev_on_output

# labwc (with C-A-right configured in rc.xml)
gesture: swipe left 3   wtype  -M ctrl -M alt -P right
gesture: swipe right 3 wtype  -M ctrl -M alt -P left

```

### Screenshots with grim/slurp

Although `ScreenGrab` is working (check out its command line options!) you may find useful using them.

Save to executable files `screenshot_full` and `screenshot_area` and set shortcuts like Alt+Print/Ctrl+Print.
```bash
grim /tmp/screen_full_$(date +'%a_%T_.png') &&
 notify-send -a grim -i screengrab -t 2000 "Full screenshot saved"
lximage-qt /tmp/screen_full_$(date +'%a_%T_.png')
```
```bash
grim -g "$(slurp -d)" - | wl-copy -t image/png && notify-send -a grim -i screengrab -t 2000 "Screenshot saved to clipboard"
```

### Action icons in the panel

Custom command plugin can trigger actions for which shortcuts are configured, using `wtype`. Example: Go to workspace right|left.

### Display keyboard layout in panel (2 layouts only)

Add to XKB_DEFAULT_OPTIONS: `grp_led:scroll`, example labwc:
`XKB_DEFAULT_OPTIONS=grp:caps_toggle,grp_led:scroll`

Add 
```bash
if [ "$(cat /sys/class/leds/input3\:\:scrolllock/brightness)" == "1" ]; then echo "US";else echo "DE"; fi
``` 

as command. Repeat every second. `/sys/class/leds/input3` can be different on different OS.

## Running SDDM under Wayland

`/etc/sddm.conf`:

```ini
[General]
...
DisplayServer=wayland


[Wayland]
...
SessionLogFile=/tmp/wayland-session.log
CompositorCommand=labwc

```
SDDM uses `/var/lib/sddm/` as home directory where the compositor should be configured, otherwise default settings will be applied:

`/var/lib/sddm/.config/labwc/rc.xml`:
```xml
<?xml version="1.0"?>
<labwc_config>

<keyboard>
    <keybind key="A-Space">
      <action name="ShowMenu" menu="client-menu" />
    </keybind>
    <keybind key="A-k">
      <action name="Execute">
        <command>wvkbd-mobintl</command>
      </action>
    </keybind>
</keyboard>

  <mouse>
    <default />
  </mouse>
  <libinput>
    <device category="default">
      <naturalScroll>no</naturalScroll>
      <pointerSpeed>0.8</pointerSpeed>
      <accelProfile>adaptive</accelProfile>
      <tap>yes</tap>
      <disableWhileTyping>yes</disableWhileTyping>
    </device>
  </libinput>
</labwc_config>
```



