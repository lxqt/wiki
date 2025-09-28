

#### Autostart Applications on Login

Launching applications at the beginning of LXQt sessions is implemented according to the to the XDG [Autostart Specification](https://www.freedesktop.org/wiki/Specifications/autostart-spec/).   

Basically this means suitable desktop entry files have to be placed in `~/.config/autostart/`.   
These files can be handled manually or from configuration dialogue [LXQt Session Settings](https://github.com/lxqt/lxqt-session/blob/master/README.md#lxqt-session-settings) (binary `lxqt-config-session`) which belongs to [lxqt-session](https://github.com/lxqt/lxqt-session).   

For an explanation of terms "Global Autostart" and "LXQt Autostart" see section [Autostart](https://github.com/lxqt/lxqt-session/blob/master/README.md#autostart) in the repository's `README.md`.   
Note this distinction is currently suffering from a problem addressed in issue [lxqt#746](https://github.com/lxqt/lxqt/issues/746).   

#### LXQt Modules

Under "Session settings" it is possible to start/stop and enable/disable single modules. Any application which contains `X-LXQt-Module=true` in its `.desktop` file in `/usr/share/applications` or `~/.local/share/applications` will be handled as a module. This can be useful for applications like compositors (compton or picom) and similar.

#### Default Applications

The default browser, email client and file manager can be set by using File Associations dialog (`lxqt-config-file-associations`) → Default Applications.

LXQt's command-line tool for manipulating MIME type associations and default applications is `qtxdg-mat`, provided by [qtxdg-tools](https://github.com/lxqt/qtxdg-tools). It can also set the default terminal. See `qtxdg-mat --help` for details.

NOTE: The default terminal used by PCManFM-Qt should be set in its Preferences dialog → Advanced.

#### LXQt Filedialog in GTK Applications

Requires [xdg-desktop-portal-lxqt](https://github.com/lxqt/xdg-desktop-portal-lxqt/).

* Firefox version 98 and higher:  Open in the address bar `about:config`, search for "portal" and set both `widget.use-xdg-desktop-portal.file-picker` and `widget.use-xdg-desktop-portal.mime-handler`  from `2` to `1`. 
* Thunderbird: Preferences > Configuration Editor `widget.use-xdg-desktop-portal` has to be set to `1`. 
* Some other GTK applications can be started with `GTK_USE_PORTAL=1 application`

A general use of `GTK_USE_PORTAL=1` in `~/.profile` or `/etc/profile` can lead to issues and
 is not recommended. 

If multiple xdg-desktop-portals are installed  to use the LXQt filedialog in a LXQt session the file
`~/.config/xdg-desktop-portal/lxqt-portals.conf` has to be created containing:

```
[preferred]
default=lxqt
```
For details see https://flatpak.github.io/xdg-desktop-portal/docs/portals.conf.html


### LXQt Panel

Settings are accessible by right click on free space on the panel or its  plugins (except window buttons in task manager) and can be blocked completely checking the last item to prevent accidental changes or removals of plugins.

Plugins can be configured either by right click or by Panel Settings → Plugin → icon "..."

#### Plugin Keyboard-Indicator

It is possible to show country flags instead of the  ISO code  for the keyboard layout installing `iso-flags-svg` or `iso-flags-png` if provided by the distribution in use. An independent set can be found [here](https://github.com/hampusborgos/country-flags). The path to the folder containing the flags has to be configured manually, example `/usr/share/iso-flags-svg/country-4x3/%1.svg`. 

#### Add or modify Main Menu Entries

* To  modify an existing menu entry  the application's `.desktop` file can be copied from `/usr/share/applications/` to `~/.local/share/applications/` and then edited. Changes will not be lost on updates.
* To add a new menu entry `~/.local/share/applications/` can be opened in PcManFM-qt and by Tools → Create Launcher a new entry can be configured.

### Power Profiles

With LXQt ≥ 2.2, if `power-profiles-daemon` is installed, the available profiles could be changed by right clicking the battery icon.

### LibreOffice / OpenOffice vs. environment variable SAL_USE_VCLPLUGIN

Both office suites do not use one of the well-known widget toolkits like GTK or Qt but stick with their custom "Visual Component*s* Library (VCL)". Integrating with the former is handled by components called `plugins`.   

A choice among the available plugins is made by setting environment variable `SAL_USE_VCLPLUGIN` which is accepting values `gen` ("generic", hence no use of plugins / native VCL UI), `gtk3`, `gtk4`, `kf6` (KDE), `qt5` and `qt6`. 
System defaults are set by files `/etc/profile.d/libreoffice-<flavour>.<shell>`. Alternatively in LXQt the variable can be set in the session configuration dialogue [LXQt Session Settings](https://github.com/lxqt/lxqt-session#lxqt-session-settings) under "Advanced".
By default it is not set this way but if set it supersedes other means to tweak the variable.

Note the support of the Qt ecosystem is rather poor. Unlike in GTK there's no interaction with the widget toolkit itself but with KDE libraries in an outdated version only. In particular it is currently not possible to use the generic Qt file chooser dialogues with LibreOffice or OpenOffice.   
Also, enabling a certain plugin does not necessarily mean the corresponding backends are available which rather have to be installed independently.   
