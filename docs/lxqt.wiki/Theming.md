## LXQt themes
LXQt themes are [qss stylesheets](http://doc.qt.io/qt-4.8/stylesheet.html). They are either located in `/usr/share/lxqt/themes/` or `~/.local/share/lxqt/themes`. So far the following themes are available:
* Ambiance
* Arch Colors
* Clearlooks
* Dark
* Frost
* KDE-Plasma
* Kvantum
* Leech
* Light
* Silver
* System (plain widget theme)
* Valendas


Some discussions about themes can be followed [here](https://github.com/lxde/lxqt/issues/572). 
Please note that using "System" theme with Qt widget styles Breeze, Oxygen or Qtcurve results in uncut text in task manager buttons.

For transparency in some themes a compositor is needed. Only Breeze, Oxygen and Kvantum widget styles support transparency in all panel menus.



If anybody is interested in how the environment would look without a custom theme, create the following files (if \<prefix\> is /usr):

```
    mkdir /usr/share/lxqt/themes/notheme
    touch /usr/share/lxqt/themes/notheme/lxqt-panel.qss
```

Then choose the Notheme in `lxqt-config-appearance`.

## Qt 
Theming Qt is achieved by Qt widget themes. So far the following are available:
* Breeze
* Oxygen
* QtCurve
* GTK+
* Windows
* Fusion
* Kvantum

You can install Qt widget themes using your package manager. `Oxygen` widget theme can cause DND issues from mainmenu to the desktop or the "quicklaunch" panel widget, see [lxqt-panel/issues/1750](https://github.com/lxqt/lxqt-panel/issues/1750).

#### Kvantum

If `Kvantum` as Qt Style is selected to achieve an unique overall look the `System` theme can be selected.

Kvantum appearance can be configured in `Kvantum Manager`.

## Theming the environment 

* Qt Palette colors can be modified in lxqt-config-appearence > Widget Style > Qt Palette. In themes like Frost, Dark and Light palette colors are used at some extent. In LXQt 1.0.0 and later customized palettes can be saved and loaded; they are saved in `~/.local/share/lxqt/palettes/` .
* Window decoration, titlebar and window buttons can be configured in the settings of the [window manager](https://github.com/lxqt/lxqt/wiki/ConfigWindowManagers) used.

For theming Qt5 applications some extra packages are needed, see https://github.com/lxqt/lxqt/wiki/Building-from-source#qt5-support


For some infos about how to create a nice looking environment take a look at this [Manjaro Wiki entry](https://wiki.manjaro.org/index.php/LXQt#Theming).

## Icons and Artwork #
You can find the LXQt Logo done by Caig in [his repo](https://github.com/Caig/LXQt-graphics).

Icon sets can be installed using the package manager of the distribution. Suitable among others are
* Paper, Paper-Mono-Dark
* Papirus, Papirus Dark, Papirus Light
* Breeze, Breeze Dark
* Oxygen
* Norse
* Faenza, Faenza Dark
* Tangerine
* Tango


## Symbolic icons

LXQt's icon engine supports symbolic icons, which change color based on their background colors. For supporting them, `LXQt Appearance Configuration` → `Icon theme` → `Colorize icons based on widget style (palette)` should be checked.

With symbolic icons, for having a dark panel with a light Qt widget style (or conversely),

 * Either a non-symbolic icon set should be assigned to the panel, by using `Configure Panel` → `Styling` → `Override icon theme`;
 * Or the Kvantum widget style should be used and a dark Kvantum theme should be assigned to the panel (so far, only Kvantum can do so).

Otherwise, black symbolic icons might appear on a dark panel (or conversely).

### GTK2/3 Themes

To achieve uniformed look between Qt and  GTK applications installing corresponding gtk themes (example breeze-gtk-theme) is recommended. The GTK themes can be set in lxqt-config-appearance → Widget Style → Set GTK Themes.

Alternatively, Kvantum comes with a few themes that simulate some GTK themes — especially, `KvGnome` and `KvGnomeDark` are very similar to the default GTK themes.

## Official themes

Official themes can be found in the [lxqt/lxqt-themes](https://github.com/lxqt/lxqt-themes) repository and they can be used in any LXQt version just downloading the .zip file and unzip the content of the "themes" directory to `~/.local/share/lxqt/themes/`.

## Community created themes #

If you create a theme and put it on GitHub it is recommended to tag it so GitHub creates a release for it once you are finished. Like this users can see what changes you did in a later version and the chances of your theme being added to a distribution are higher.

* [LXQt-Breeze](https://gitlab.com/M4he/lxqt-theme-breeze) by @Mahe
* [lush](https://github.com/Val67/lush-lxqt-theme) by @Val67
* [openSUSE-default](https://github.com/jubalh/lxqt-theme-openSUSE-default) by @jubalh
* [openSUSE-leaper](https://github.com/jubalh/lxqt-theme-openSUSE-leaper) by @jubalh
* [openSUSE-light](https://github.com/jubalh/lxqt-theme-openSUSE-light) by @jubalh
* [QTStep](https://www.opendesktop.org/p/1211687/) by abgr
* [QTStep Gradient](https://www.opendesktop.org/p/1211693/) by abgr
* [Debian/Ubuntu themes](https://salsa.debian.org/lxqt-team/lxqt-themes-extra) by @agaida and @tsimonq
* [arc dark](https://gitlab.com/isseigx/lxqt-arc-dark-theme) by Gabriel Ibáñez
* [Materia](https://gitlab.com/isseigx/lxqt-materia-theme) by Gabriel Ibáñez
* [win-eleven-dark + sombre-et-rond](https://gitlab.com/Bapt17/lxqt-themes) by Baptiste Huchon

Some of them can be found in the opendesktop.org category [LXQt Themes](https://www.opendesktop.org/browse/cat/446/).

### Fix for invisible text in KDE Plasma Systemsettings with dark themes

Choose both dark theme and color scheme in Plasma Settings and add `QT_QUICK_CONTROLS_STYLE=org.kde.desktop` in Session Configuration > Advanced:

![screen_area_dom_15:54:33_](https://github.com/lxqt/lxqt/assets/10681413/e0acf32c-357b-401a-bb08-bab77156d2a3)

## Press
[pcsuggest](https://www.pcsuggest.com/best-lxqt-themes/) has an article about LXQt themes which also mentions a few other ones.