The following features are still X11-only in our codebase:

Feature | Component | PoC/WIP/Pull request | Potentially useful tools/libraries
------- | --------- | ------------ |  --------- 
Activate NumLock keyboard modifier | lxqt-session | https://github.com/lxqt/lxqt-session/pull/368 | -
Set keyboard layout | lxqt-session/lxqt-config | https://github.com/lxqt/lxqt-session/pull/368 | -
Display keyboard layout | lxqt-panel's kbindicator | - | -
Global shortcuts | lxqt-globalkeys/qterminal (for drop-down terminals) | - | [org.freedesktop.portal.GlobalShortcuts](https://github.com/flatpak/xdg-desktop-portal/issues/624) protocol, supported by [KDE Plasma 5.27](https://invent.kde.org/plasma/xdg-desktop-portal-kde/-/merge_requests/80)
Screen/display management | lxqt-config-monitor (via libkscreen) |https://github.com/lxqt/lxqt-config/issues/916 | <ul><li>[wdisplay GUI](https://github.com/artizirk/wdisplays)</li><li>[kanshi](https://sr.ht/~emersion/kanshi/)</li><li>[way-displays](https://github.com/alex-courtis/way-displays)</li></ul>Working under kwin_wayland|
colorpicker| panel-plugin | - |<ul><li>[hyprpicker](https://github.com/hyprwm/hyprpicker)</li></ul>

#### Wayland status overview

https://github.com/orgs/lxqt/projects/4/views/2

#### Results from testing already working components

Testing on labwc, wayfire, sway, hyprland, kwin_wayland, niri


 Component | To fix | Issue/WIP/PR     | Notes 
-----------|--------|-----------|-----------
lxqt-panel      |  Plugins not working:  kbindicator (is prevented from adding), colorpicker,mainmenu (on bottom panel)  | https://github.com/lxqt/lxqt-panel/pull/2043| Plugins working: sysstat, cpu monitor, network monitor, Notification Area, worldclock, fancy menu, quicklaunch, mount, spacer, custom command, backlight, volume, showdesktop. 
lxqt-powermanagement| Button settings | - | Info is shown now



