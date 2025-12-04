
## General

This document is about advanced and, in some cases, hidden properties or functionalities of PCManFM-Qt.


### GVFS

GVFS is an important optional dependency of PCManFM-Qt (`libfm-qt`). Although PCManFM-Qt does not depend on it explicitly, it provides important functionalities like Trash support. Its name appears in several places of the current document.

It is highly recommended that you install the GVFS packages that you may need from your distro repository. If you do not know what each GVFS package does, install all of them.

### View Modes

PCManFM-Qt has four view modes: icon view, thumbnail view, compact view, and detailed list view. You can choose the default mode in Preferences → Behavior → Default view mode (it is icon view by default).

In addition, there are four toolbar buttons for changing the mode. They do not change the default mode, but you can use them to switch to another mode temporarily. For example, if the default mode is icon view and you need a detailed list temporarily, you could click the "Detailed List" button on toolbar. Also, see [Customizing View Settings](#customizing-view-settings).

### Customizing View Settings

The item sorting, showing of hidden items and view mode can be changed globally. The item sorting and visibility of hidden items can be changed by using the folder's right-click menu; then subsequent windows will reflect the changes. The view mode can be changed permanently only in Preferences (see [View Modes](#view-modes)).

But each folder can also have customized view settings of its own. To customize a folder, check `View → Customized View Settings → Preserve Settings for This Folder` or, more easily, right click the tab and check the same menu item. After that, any change in the view settings of that folder — including the view mode, that can be changed by using view toolbar buttons — will be preserved for it, without affecting other folders.

For example, if you have a folder containing images, you could check the above-mentioned menu item for it and click the "Thumbnail View" toolbar button. Then, that folder will always show big thumbnails of images (their size can be changed in Preferences → Display → Size of thumbnails), while other folders will have the default view mode.

If you want to customize a folder *and* all its sub-folders, you should check "Preserve Settings Recursively from Here" in the same menu instead. With that item checked, any change in view settings of the folder is preserved for its sub-folders too. If you change the view settings of a sub-folder *separately*, the changes will be temporary because it should take its permanent settings from the parent folder.

The above-mentioned menu may also have other items, whose functioning you could easily guess.

The view mode of a customized folder is reflected by its tab icon. The existence of a tab icon is a sign of view customization.

### Mouse Wheel Scrolling

Wheel scrolling over the scroll-bar follows Qt's default behavior. But when the cursor is inside the main view, the behavior depends on the view mode, as explained here.

In the icon and thumbnail views, which support large icons, Qt's default scroll jumps can make it difficult to keep track of items. As a workaround and for user convenience, the scrolling is smooth by default and its speed depends on the icon size. The following table shows the speed inside icon and thumbnail views.

| Icon Size        | Scroll Speed |
|------------------|--------------|
| 256              | half the row |
| ≥ 96 and < 256   | one row      |
| ≥ 64 and < 96    | two rows     |
| < 64             | DE setting   |

Under LXQt, the DE setting can be changed in `Keyboard and Mouse Settings → Mouse → Wheel scroll lines`.

In the detailed list mode, the scroll speed is always that of the DE. In the compact mode, at most one column is scrolled at a time.

If you want the minimum speed, press `Shift` while turning the mouse wheel inside the view.

Because of a Qt bug in updating views with huge numbers of items, an option has been added for disabling smooth scrolling (Preferences → Display → Disable smooth  scrolling).

### Tab DND

Tabs can be reordered by the mouse. In Addition, *if the main view is not split*, they can be detached or moved from one window to another.

When there is more than one tab, to detach a tab, drag and drop it outside all windows of PCManFM-Qt. Then, a new window will be created for the dropped tab.

To move a tab to another window, drag and drop it into that window. If the source window has only one tab, it will disappear after the tab is moved.

### Permanently Deleting Files

If GVFS Trash support is installed, to delete a file permanently — in contrast to moving it into Trash — use `Shift + Delete`.

If you do not want Trash at all, you could uncheck Preferences → Behavior → File operations → Move deleted files to "trash bin" instead of erasing from disk.

### Forward/Backward History

Each tab of PCManFM-Qt has a forward/backward history. To access and use it, right click the "Go Back" and "Go Forward" buttons on the tool bar. You can move through the history by clicking the items of the right click menu.

### Filter Bar

If `View → Toolbars → Permanent filter bar` is checked, you should type inside the filter bar to filter files/folders. That may not be comfortable because you need to focus the filter bar for filtering and refocus the main view after filtering, e.g., by clicking inside them or using shortcuts.

To filter files/folders more easily, *uncheck* the above-mentioned item and type *inside the main view*. Once you type the first letter, the filter bar appears and filtering starts, while the main view keeps the focus.

To remove filtering, just press `Escape` or click the Clear icon of the filter bar (`Ctrl + K` can also be used). If you had selected some files before removing the filter, your selection will be preserved.

Let us call this a "transient filter bar". For the sake of consistency, if the filter bar is transient and *if there is no selection*, the first `Space` that is typed inside the view will only select the *current item*, without starting filtering. The current item may be marked by widget styles in different ways, e.g. it may have a dotted rectangle around its name or a faded line below it. There can only be one current item inside a focused widget. In Qt, the current item is selected by `Space` and deselected by `Ctrl + Space` everywhere; hence making an exception for the first `Space`.

For pasting text into the transient filter bar, press `Shift + Insert` inside the view. Of course, you could also show and focus the filter bar with `Ctrl + I` but `Shift + Insert` guarantees that the focus remains on the view.

!!! note
     Pressing and holding `Backspace` is not a good way of clearing the transient filter bar because the selected file may change. Pressing `Escape` is always the best and easiest way.

### Bulk Renaming

Bulk Renaming is a method of renaming multiple files without getting a prompt dialog for each one.

To rename files in bulk, select them and press `SHIFT + F2` or click `Edit → Bulk Rename`. A dialog will show up, in which you could choose a name that is suffixed by `#`. `#` will be replaced by consecutive numbers, starting from 0 or any number you might choose in the spin-box of the Bulk Rename dialog. You could also put `#` at the start of the name or inside it.

If you confirm the dialog by pressing OK or `Enter`, the selected files will be renamed according to your chosen pattern, while their extensions, if any, will be preserved.

### Changing Folder Icons

The icon of a folder can be changed in the *File Properties* dialog. Show *File Properties* by right clicking the folder and clicking *Properties* or by selecting it and pressing `Alt + Enter/Return`. There is a button on the top left of the `General` page. If you click it, a file dialog will be opened inside your active icon theme, where you could choose a customized icon for that folder.

To remove the customized icon of a folder, open it, show the hidden files inside it and delete the hidden file named `.directory`.

You can also edit the hidden file `.directory` and put any image path in front of `Icon=`. The path can be relative to the current folder.

Icons of files cannot be changed because they show their mime types; they depend on the active icon theme.

### Emblems

The first tab of File Properties dialog has a button for adding an emblem to a file — or a selection of files — and another button for clearing it. Emblems are small icons that appear at the bottom right corner of the main icon and should be chosen from the active icon theme. They require GVFS.

Emblems are user-specific. They are not really added to files but are attributed to them by the current user. So, they are not preserved when files are copied or transferred.

GVFS allows multiple emblems for each file but the File Properties dialog only sets one because the other corners of a file's icon are preserved for other purposes (like the symlink emblem).

Emblems can be used to distinguish certain files visually, especially non-folder files, whose icons cannot be changed.

### Hiding Files/Folders without Renaming Them

A really hidden file has a name that starts with a dot. But, sometimes, you may not want to rename a file to make it hidden, e.g., when that file is used by an app. Also, folders like `lost+found` belong to root and should not be renamed.

Some file managers have a way of making files "virtually hidden". Technically, those files are not hidden but only treated as hidden by the file managers in question.

PCManFM-Qt has the above-mentioned functionality. To hide files without renaming them, create the file `.hidden` inside their parent directory and add to it the names of the files you want to hide in separate lines.

For example, you may create `~/.hidden` as:

```
Public
Templates
Certain File
```

Then, the folders `Public` and `Templates` as well as the file `Certain File` inside your home directory are treated as hidden by PCManFM-Qt and some other file managers.

!!! note
     Editing `.hidden` does not have an immediate effect because `GLib` (which does the file management in PCManFM-Qt) uses cache. It will take effect the next time its containing folder is opened. You could also wait a few seconds and reload the folder.

### Thumbnails

PCManFM-Qt (actually, `libfm-qt`) creates thumbnails of images internally — installation of `qt-imageformats` and [`kimageformats`](https://api.kde.org/frameworks/kimageformats/html/index.html) can extend the range of its supported formats considerably — but it can also use external thumbnailers for other formats or kinds of files.

User-defined thumbnailers can be created inside the directory `~/.local/share/thumbnailers`. For each new thumbnailer to be seen, PCManFM-Qt needs to be restarted from *LXQt Session Settings* → *Desktop*.

The following examples show how to add user-defined thumbnailers for DjVu and PDF files. You can use them as a guide for creating other kinds of thumbnailers.

To have thumbnails for DjVu files, create a file like `~/.local/share/thumbnailers/djvu-djvu.thumbnailer` as,

```
[Thumbnailer Entry]
Exec=ddjvu -format=tiff -page=1 -size=%sx%s %i %o
MimeType=image/vnd.djvu+multipage;image/vnd.djvu;
```

Then restart PCManFM-Qt. Of course, you need `ddjvu`, which belongs to `djvulibre`.

For PDF files, you can install `graphicsmagick` and create `~/.local/share/thumbnailers/graphicsmagick-pdf.thumbnailer` as,

```
[Thumbnailer Entry]
Exec=gm convert -quality 100 -thumbnail %s -flatten %i[0] %o
MimeType=application/pdf;application/x-pdf;image/pdf;application/x-gzpostscript;application/postscript;application/vnd.ms-xpsdocument;
```

Again, restart PCManFM-Qt.

The file can have any name but its extension should be `.thumbnailer`. It should start with `[Thumbnailer Entry]` and have two keys, namely, `Exec` and `MimeType`.

The value of the `MimeType` key tells which file types should have thumbnails.

The value of the `Exec` key tells how thumbnails are created. It is a command-line that depends on the thumbnailing application but it always includes three elements: `%s` for the image size, `%i` for the "input" (the file whose thumbnail should be created), and `%o` for the "output" (the thumbnail itself). You need to be familiar with the thumbnailing application to know how it can create an image with a specific size out of a file. Then replace input, output and image size with `%i`, `%o` and `%s` in its command-line respectively and put it in front of `Exec=`.

In rare cases, you may want to disable a thumbnailer, which is installed as root, without removing its application. To do so, copy it from `/usr/share/thumbnailers/` (or `/usr/local/share/thumbnailers/`) to `~/.local/share/thumbnailers/`, change its `MimeType` line to something like `MimeType=application/nothing;`, and restart PCManFM-Qt.

### "Admin Mode" vs. Root Instance

PCManFM-Qt can be launched as root under X11. The menu-item that does that is `Tools → Open Tab in Root Instance`. The command in `Preferences → Advanced → Switch user command` should be something like this:

```
lxsudo dbus-run-session -- %s
```

But a root instance may not be safe for various reasons. Moreover, no root instance is possible under Wayland. If you only want to transfer files that are owned by root, you could use the admin mode instead. Its menu-item is `Tools → Open Tab in Admin Mode`. The admin mode requires GVFS.

You cannot open files in the admin mode, unless the associated applications support it. Most apps do not, especially Qt apps (however, FeatherPad can open text files in the admin mode if the user has the reading permission). But it can be useful with file transference.

Because the admin mode is provided by GVFS, PCManFM-Qt just calls it, without having control over it. For example, it has a time-out, after which, the root password will be asked again. PCManFM-Qt cannot change that.

### Trusting Executables

By default, executable files are not "trusted", which means that a prompt dialog will be shown if the user (double) clicks them. Desktop entries (files whose real names end with `.desktop`), that are not inside `~/.local/share/applications` or `/usr/share/applications`, may have a special emblem on their icons, showing that they are not trusted. The emblem depends on the icon set but, usually, it is like an exclamation mark. Other kinds of executables do not have this emblem.

You can select the option `Preferences → Behavior → Launch executable files without prompt` to remove the prompt for all executables but, by doing so, you might compromise your system because you might click a suspicious executable unintentionally.

There is a safer way of trusting executables: you could right click them and check "Trust this executable" (or "Trust selected executables" in the case of multiple selection) if you really trust them. If an executable is a desktop entry, its special emblem will disappear.

Like some other important properties, trusting executables requires GVFS. Without GVFS, the trust state will be lost in the next session.

### Profiles

PCManFM-Qt has a `--profile` option, which can be used for having different configurations when needed. For example, this command starts the desktop module with the profile `myProfile`:

```sh
pcmanfm-qt --desktop --profile=myProfile
```

Users usually do not need to set it. It is set to `lxqt` when the desktop module is started by the LXQt Session and is `default` when omitted.

When PCManFM-Qt is started with a profile `xyz`, it (creates and) uses the configuration files inside the folder `~/pcmanfm-qt/xyz/`. In this way, the application can be started with different configurations under different desktop environments. Please note that only the LXQt Session sets it automatically. If you need a specific profile for another DE, you should set it explicitly, e.g., in front of `Exec=` in a desktop entry that you may make for that DE.

### Using Desktop under Wayland

Since its version 2.0.0, PCManFM-Qt's desktop is fully functional under Wayland compositors which implement the `wl-layer-shell` protocol, e.g., KWin and `wlroots`-based compositors like LabWC, Wayfire,…

For avoiding the current problems of multi-screen desktops under Wayland, each screen is given its separate desktop with a separate items layout in version 2.1.0.

Unfortunately, Qt cannot distinguish between the desktop geometry and the *available* desktop geometry under Wayland yet. The latter is the geometry excluding the areas reserved by the window manager for panels, task bars, etc. For this reason, you may need to change margins in `Desktop Preferences → General → Spacing → Margins of work area`, such that the desktop items do not go under the reserved areas.

***

However, if you set up your Wayland desktop, your X11 desktop will also be affected, and conversely. If you switch between X11 and Wayland frequently, you might not want that. PCManFM-Qt's `--profile` option can solve this problem inside an LXQt session:

 1. Make an executable script `lxqt-desktoploader` like this, which uses another profile on Wayland:

```sh
#!/bin/sh

if [ "$XDG_SESSION_TYPE" = "wayland" ]; then
    exec pcmanfm-qt --desktop --profile=lxqtwayland
else
    exec pcmanfm-qt --desktop --profile=lxqt
fi
```

 2. Put `/etc/xdg/autostart/lxqt-desktop.desktop` into the folder `~/.config/autostart/`, and change the value of its `Exec=` to the path of `lxqt-desktoploader`.
 3. Copy the folder `~/.config/pcmanfm-qt/lxqt` as `~/.config/pcmanfm-qt/lxqtwayland` to transfer your settings from X11 to Wayland.
 4. Log out and log in.

Then the X11 and Wayland settings of PCManFM-Qt will be separate from each other and will not interfere with each other.

***

Like in X11, PCManFM-Qt's destop draws the background under Wayland. But, since version 2.1.0, you can also let a Wayland wallpaper/background app draw the background of PCManFM-Qt's desktop. To do so, first set PCManFM-Qt's wallpaper to any transparent image — the simplest transparent image can be an SVG file like

```xml
<svg width="200" version="1.1" xmlns="http://www.w3.org/2000/svg" height="200" viewBox="0 0 200 200"/>

```

Then make sure that the wallpaper app starts *before* PCManFM-Qt's desktop — with a Wayland LXQt session, that will happen automatically if it is auto-started by the Wayland compositor. With this setup, PCManFM-Qt will draw desktop items, while the background will be drawn by the app.

### Custom Actions

Custom actions are explained separately. See [Custom Actions](https://github.com/lxqt/pcmanfm-qt/wiki/custom_actions).
