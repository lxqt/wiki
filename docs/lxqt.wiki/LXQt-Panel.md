# LXQt Panel

Settings are accessible by right clicking on free space on the panel or its  plugins (except window buttons in task manager) and can be blocked completely checking the last item to prevent accidental changes or removals of plugins.

Plugins can be configured either by right clicking or by Panel Settings → Plugin → icon "..."

## Menu Files

Right click the panel's "Main Menu" or "Fancy Menu" widget and choose Configure → Menu File. The following files belong to LXQt:
* `lxqt-applications-fm.menu`: Used by PCManFM-Qt in `menu://Applications`; not quite relevant to lxqt-panel.
* `lxqt-config.menu`: used by "Configuration Center"; not relevant to lxqt-panel.
* `lxqt-applications.menu`: The default menu.
* `lxqt-applications-compact.menu` Can be used in both Mainmenu and Fancymenu, in Mainmenu it will generate a two-level layout.

> [!Note]
> `lxqt-applications-compact.menu` does not display a probable "Wine" category because `~/.config/menus/applications-merged` does not match "compact". Renaming it to "compact-merged" will fix that.

LXQt 2.3 and later:
* `lxqt-panel-applications.menu` Default menu for Fancymenu.

## Add, modify or hide Menu Entries

* For adding a new menu entry, the directory `~/.local/share/applications/` can be opened in PCManFM-Qt and a new desktop entry can be created in it by using Tools → Create Launcher.
* For modifying an existing menu entry, the application's `.desktop` file can be copied from `/usr/share/applications/` to `~/.local/share/applications/` and then edited. Changes will not be lost with updates.
* For hiding an existing menu entry, the application's `.desktop` file can be copied from `/usr/share/applications/` to `~/.local/share/applications/` and then edited by adding the line `NoDisplay=true`. Changes will not be lost with updates.

See also [lxqt-project.org/blog/2022/06/18/menu-editing-in-lxqt/](https://lxqt-project.org/blog/2022/06/18/menu-editing-in-lxqt/).

## Panel Profiles

By installing [lxqt-panel-profiles](https://codeberg.org/MrReplikant/lxqt-panel-profiles/)  different panel configurations can be backuped and restored. 

## Custom Command Widget

A powerful and versatile plugin which can show outputs from commands as text or image and launch applications.

Some examples:

### Used partition space in /dev/sda1
<img width="102" height="43" alt="harddisk: percentage-free" src="https://github.com/user-attachments/assets/65fbd94b-107f-43f7-bb80-90e26f62aea0" />

```bash
df -h|grep sda1|awk {'print $5'}
```
On left click: "filelight" or similar tools

### Temperature of devices
<img width="136" height="44" alt="41 °C" src="https://github.com/user-attachments/assets/fd413e8f-4109-4281-86f5-99ea75839c77" />

```bash
acpi -t |awk {'print $4'}|sed 's/.0$//' 
```
```bash
cat /sys/class/thermal/thermal_zone0/temp |cut -c1,2
```
Text: `%1°C`
Icon: "temperature-normal"
On left click: "Qps"

### Battery percentage
<img width="94" height="45" alt="battery: percentage" src="https://github.com/user-attachments/assets/8002beab-eafd-49eb-83b1-937e2c4be131" />

```bash
 upower -b |grep percentage|awk {'print $2'}
```
### Battery remaining time
<img width="139" height="45" alt="battery: time left" src="https://github.com/user-attachments/assets/1f538120-1efd-4c93-b9e3-eec817dc3b7a" />

```bash
upower -i /org/freedesktop/UPower/devices/battery_BAT0|grep time|awk {'print $4,$5'}

Icon: "battery"
<img width="94" height="45" alt="battery: percentage" src="https://github.com/user-attachments/assets/c6b76c62-7eea-4ac9-a1d4-ecc3e1811b17" />

```
### Free memory percentage
<img width="96" height="40" alt="memory-free" src="https://github.com/user-attachments/assets/3f957249-258f-4126-971c-e6dba4f72402" />

```bash
 free -mh | grep Mem | awk '{print $4}'
```

### Network speed
![immagine](https://github.com/user-attachments/assets/0c97c1ca-734f-464e-ad37-dd78625dade3)
```bash
ifstat | grep enp2s0f3u2u4 | awk '{print "⬇ "$2" KB/s ⬆ "$3" KB/s"}'
```
(see https://github.com/lxqt/lxqt/discussions/2600)

### Bandwidth/month

![immagine](https://github.com/user-attachments/assets/6f2302da-6a36-4258-85fa-8fede61096b0)
```bash
vnstat -m|grep estim|awk '{print $8 $9}
```

## Plugin Keyboard-Indicator

It is possible to show country flags instead of the  ISO code  for the keyboard layout installing `iso-flags-svg` or `iso-flags-png` if provided by the distribution in use. An independent set can be found [here](https://github.com/hampusborgos/country-flags). The path to the folder containing the flags has to be configured manually, example `/usr/share/iso-flags-svg/country-4x3/%1.svg`. 
