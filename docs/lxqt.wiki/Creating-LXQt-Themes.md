# Structure of an LXQt Theme

## Theme Stylesheets (*.qss)
Themes for LXQt consist of a named directory under `/usr/share/lxqt/themes/` (global) or `$HOME/.local/share/lxqt/themes/` (per-user). Inside these folders are:

- Qt stylesheets, plaintext files ending in `.qss` and containing QSS selectors and theming directives. The stylesheets correspond to the executable name of the component they apply to. If one of the stylesheets is not present, the application in question will simply fall back to the chosen Qt widget style and palette. LXQt themes currently support the following stylesheets:

> * `lxqt-panel.qss` - Styles the `lxqt-panel` and its plugins. **This file is mandatory.**
> * `lxqt-runner.qss` - Styles the `lxqt-runner` dropdown, accessible with `Alt + F2`.
> * `lxqt-notificationd.qss` - Styles the `lxqt-notificationd` notification popups.
> * `lxqt-config.qss` - Styles the `lxqt-config` windows.
> * `lxqt-leave.qss` - Styles the `lxqt-leave` "end session" dialog.

- Resources used by the theme, such as SVG graphics. These are used to provide an icon for the main menu and, for example, customized arrows and other small graphical elements to give your theme a unique look. You may also provide a default wallpaper (see below).

- A `preview.png` image, 100px wide by 32px high. This is usually taken from the panel with the main menu and desktop switcher widgets in view, and provides a quick impression of what the theme looks like.

- Optionally, a `wallpaper.cfg` file which specifies the included desktop background, which the user may choose to apply or not from the theme switcher.
## Color Palettes
LXQt also supports user-specified palettes of 8 colors, which are applied to the base Qt widget style chosen from the theming preferences configurator. Palettes are stored under `/usr/share/lxqt/palettes/` (global) or `$HOME/.local/share/lxqt/palettes/` (per-user). These are standard plaintext files, and the 8 values are specified as HTML hex triplets (i.e., #RRGGBB). 

Keep in mind that some Qt widget styles are less amenable than others to picking up user-defined palette colors. In particular, Fusion and Plastik are rather well-behaved, while Oxygen enforces a shaded gradient and Kvantum will likely ignore them outright.

A sample palette file, that of the base Clearlooks theme, is shown below - note the `[Palette]` header before the actual color definitions:
> ```
> [Palette]
> base_color=#ffffff
> highlight_color=#80a0d0
> highlighted_text_color=#ffffff
> link_color=#0000ee
> link_visited_color=#ffb3f7
> text_color=#1a1a1a
> window_color=#edeceb
> window_text_color=#1a1a1a
> ```

Most of these are self-explanatory. For the rest: `window_color` refers to the "bulk" of an application, such as a file manager's menubars and controls, while `base_color` would govern the display pane with the icon or list view of the filesystem in it; likewise the `text_color` and `window_text_color` definitions.

LXQt themes have the ability to pick up colors from the active palette, which allows for some interesting theming possibilities. For example, this is how Azuma Hazuki's <a href=https://github.com/AzumaHazuki/lxqt-themes-mint-y>Mint-Y LXQt themes collection</a> works: rather than ship nearly two dozen redundant full themes, it consists simply of a light and dark variant along with a set of palettes, from which the themes draw their highlights and accent colors. Switching palettes changes these colors dynamically.

# Writing Themes
## Qt Stylesheet Syntax
The syntax of .qss stylesheets should be immediately familiar to anyone who has worked with CSS, and is covered in great detail <a href=https://doc.qt.io/qt-6/stylesheet-reference.html>here</a>. Aspiring theme creators should be familiar with the <a href=https://doc.qt.io/qt-6/stylesheet-customizing.html#box-model>box model</a> and the <a href=https://doc.qt.io/qt-6/stylesheet-syntax.html>precedence rules</a> of QSS selectors.

Broadly speaking, QSS operates on an "outside-in" model, whereby more general selectors are overridden by more specific ones. The hypothetical case 
>```
>* { 
>        color: #00ff00; 
>        background: #000000; 
>} 
>
>QMenu {
>        color: #ffff00; 
>        background: #000040; 
>}
>``` 
will produce green text on a pure black background everywhere _except_ standard Qt menus, which will have yellow text on a navy-blue background.

## Theming LXQt Applications
LXQt includes many widgets derived from the base Qt classes, which are generally addressed with a `#` to denote that they are identifiers, rather than classes or class families of widgets; for example, `#MainMenu` references the "Start Menu" plugin of the LXQt panel, while `#MainMenu QMenu { some_option: something; }` will theme only those widgets of type QMenu that descend from (that is, which are created by and embedded in) the `#MainMenu` plugin itself. 

**Important: widgets beginning with a `#` will not pass their theming directives down to their descendants.** This is why it is necessary to style, for example, `#TaskBar QToolButton` to alter taskbar buttons

While it is not mandatory to theme any specific widget or plugin in any specific order, there are some best practices to keep in mind for legible and maintainable theme files:

1. Try to theme specific widgets and their descendants close to one another - keep your menu theme directives separate from the ones for the taskbar, clock, etc.
2. Because QSS specifiers have higher priority the more specific they are, it is good practice to place those specifying broader or more general rules closer to the top of the file. Within sections for individual plugins/widgets, it is likewise good to specify the top-level identifier first and then its sub-widgets, their subcontrols and pseudostates, and so forth, under these.
3. However, if you specifically wish to theme different widgets identically, you may further save space by defining them with a comma-separated list of selectors. For example, if you wish your taskbar and desktop switcher buttons to have the same appearance, you can try something like
> ```
> #TaskBar QToolButton, #DesktopSwitch QToolButton {
>         property: value;
>         other_property: other_value;
> }
> ```

## The LXQt Panel (`$PATH/lxqt-panel`)
* The LXQt panel consists of a base executable which draws a horizontal (or vertical) area onto the screen, optionally and by default reserving space for itself so as not to be covered by application windows. Everything we normally associate with a panel - application menu, taskbar, system tray, clock, and so forth - is a plugin, a small self-contained applet which is drawn in a specific position on the panel and which may be interacted with via mouse, keyboard, touch, etc.

## List of widgets themeable in `lxqt-panel.qss`

## The Panel Itself
### LXQtPanel #BackgroundWidget
The groundmass of the panel, which will show below/around any other widgets

## Menus and Submenus
### #MainMenu
The "start menu" button plugin, visible on the panel.
* Add an icon with `qproperty-icon: url(path/to/image.ext);`
* Theme submenus with `#MainMenu QMenu`
* * `#TopLevelMainMenu` lets you explicitly theme _only_ the initial menu popup; useful for faking a Win9x/2000-style banner, as seen <a href=https://github.com/pixelocdguy/windows-classic>here</a>
* Theme the search entry with `#MainMenu QMenu QLineEdit`
* Theme the search results with`#MainMenu ActionView`

### QMenu
Themes standard QMenu child widgets of the panel, such as the right-click menus.
* Has subcontrols `::(item,left-arrow,right-arrow,indicator)`
* * All subcontrols may be in `:(hover,selected,disabled)` pseudostates
* * * The `::indicator` subcontrol also has the radiobutton-like `:exclusive:checked`/`:exclusive:unchecked` and checkbox-like `:non-exclusive:checked`/`:non-exclusive:unchecked` pseudostates. You may theme these with `image: url(path/to/image.ext);` directives.
* `QMenu QToolButton` allows you to theme the _headers_ of menus, which are implemented as QToolButton objects.

## The Taskbar
### #TaskBar
The background of the taskbar. May be set to `background: transparent;` to inherit the color from `LXQtPanel #BackgroundWidget` if desired.

Taskbar buttons are addressed with `#TaskBar QToolButton` and have the following pseudostates:

* `#TaskBar QToolButton:on` is the active state, corresponding to the focused application's button.
* `#TaskBar QToolButton:hover` is the appearance of an inactive task's button when the cursor is on top of it.
* `#TaskBar QToolButton:pressed` is the appearance of a task button when receiving a left click event.
* `#TaskBar QToolButton[urgent="true"]` is the appearance of a task button which has set the standard urgency hint. Note the lack of a leading `:` here.

Logically-valid combinations of these states are possible, such as
* `#TaskBar QToolButton:on:hover`, which is the appearance of the active task button when the cursor is on top of it.

The pseudostates will derive their `padding`, `margin`, etc. values from the base `#TaskBar QToolButton` block, though you may alter these if desired.

## The Virtual Desktop Switcher
### #DesktopSwitch
Most window managers have the ability to implement virtual desktops, and this widget produces a set of buttons to display, track, and allow switching between these. LXQt is designed to be WM-agnostic, so the plugin _should_ work with any compliant window manager's implementation of virtual desktops.

* `#DesktopSwitch QToolButton` works exactly the same as its taskbar variant above, and it is possible and encouraged to declare them both together if you wish your theme to draw them identically; see <a href=https://github.com/lxqt/lxqt/wiki/Creating-LXQt-Themes/#theming-lxqt-applications>here</a> for how.

## The Quicklaunch Toolbar

### #QuickLaunch
The Quicklaunch plugin provides a space for the user to drag-and-drop icons from the main menu, enabling simple one-click access to them.
* `#QuickLaunch QToolButton` will theme the individual app buttons that have been dragged-and-dropped into the launcher dock.

### #QuickLaunchPlaceHolder 

* `#QuickLaunchPlaceHolder` themes the "drop application icons here" area and caption of a default (empty) Quicklaunch toolbar.

## The World Clock

### #WorldClock (deprecates #Clock)
The LXQt WorldClock plugin is able to display time, date, and timezone in a fully-localizable, standards-compliant manner. It also displays a calender when clicked, about which see below.
* `#WorldClock` themes the area of the panel reserved for the clock widget as a whole.
* `#WorldClockContent` specifically themes the caption of the clock widget.

## The Calendar

### QCalendarWidget
* **TODO: Please document the many, many, _many_ options for the QCalendarWidget plugin here.** The <a href=https://github.com/lxqt/lxqt-themes/blob/master/themes/kvantum/lxqt-panel.qss>Kvantum theme</a> may be a good place to look. 

## The LXQt Runner  (`$PATH/lxqt-runner`)

* The LXQt Runner is a self-contained dropdown widget, similar to KDE Plasma's `krunner`, which accepts text input and updates search results in real time. It is accessible by pressing `Alt + F2` (by default) in a running LXQt session.
## List of widgets themeable in `lxqt-runner.qss`
### #panel
* This is the body of the LXQt Runner application
### #closeButton
* Clicking this closes the LXQt runner. Theme with `qproperty-icon: url(path/to/image.ext);`
### #actionButton
* Clicking this displays the logout/shutdown menu. Theme with `qproperty-icon: url(path/to/image.ext);`
### #commandEd
* This is the command-entry area, and takes the same options as a QLineEdit widget.
### #commandList
* The list of filtered items matching what is typed in the #commandEd above.
* Has subcontrol `::item`, which has pseudostates `:alternate,:hover,:selected`.
### QMenu
* This is the logout/shutdown menu displayed when the user clicks the #actionButton widget.
# The LXQt Notification Daemon (`$PATH/lxqt-notificationd`)
* The LXQt Notification Daemon listens for standard FreeDesktop.org notification events and displays transient popups when these occur.
## List of widgets themeable in `lxqt-notificationd.qss`
### Notification
* This is the body of the notification message itself.
### #closeButton
* Appears in the corner of the notification. Style with `qproperty-icon: url(path/to/image.ext);`
### #appLabel
* Caption showing the name of the application that threw the notification in question.
# The LXQt Configurator (`$PATH/lxqt-config`)
* The LXQt Configurator is the central hub for configuring how LXQt operates. It is analagous to Xfce's `xfce4-settings-manager`, GNOME's `gnome-control-center`, KDE Plasma's `systemsettings5`, and so forth.
## List of widgets themeable in `lxqt-config.qss`
* **TODO: To date, no extant LXQt theme contains this stylesheet. Please document the available widgets here as they are discovered.**
# The LXQt Leave Dialog (`$PATH/lxqt-leave`)
* The LXQt leave dialog is a simple window that appears when the user selects Leave from the main menu or otherwise invokes it.
## List of widgets themeable in `lxqt-leave.qss`
### QDialog
* The main body of the Leave Dialog is a QDialog widget.
### QLabel
* Styles the "What would you like the computer to do?" and "Are you sure you want to {logout/shutdown/hibernate}?" captions.
### QAbstractItemView
* The six action icons are contained inside a QAbstractItemView widget.
* * Notably, as of LXQt 1.2.x, `background: #RRGGBB;` and similar _do not work_; to get around this, create a 1x1 image file of the color you would like to apply behind the icons and apply it with `background-image: url(path/to/image.ext);`. You can still theme the captions (`color: #RRGGBB`) and apply borders, margins, etc., but for now the background does not accept simple colors.
### QPushButton
* The yes/no buttons on child dialogs are QPushButton widgets.
### QToolButton
* The "Cancel" button on the main dialog is a QToolButton. It may be wise to theme both together with `QPushButton, QToolButton { ... }`.