# PCManFM-Qt

## Custom Actions

Custom user actions can be defined and added to the right-click menus (aka. context menus) of files/folders. Each action may be limited to certain file types or certain places, so that it shows up only with those types and inside those places.

For PCManFM-Qt to see a change in custom actions, it needs to be restarted from *LXQt Session Settings* → *Desktop*.

### Action Definition

A custom action is a "desktop entry" inside the directroy `~/.local/share/file-manager/actions`. A desktop entry has a name like "NAME.desktop" ("NAME" is sometimes called its "ID"). It starts with the `[Desktop Entry]` group, which can contain the following keys and their corresponding values:

| Key           | Description |
|---------------|-------------|
| `Type`        | This can be `Action` or `Menu`. The latter creates a sub-menu with more actions. See [Menu Definition](#menu-definition). |
| `Name`        | The label of the action, as it appears in the context menu.<br>Its localized variant can also be added, like `Name[it]=...`. |
| `Icon`        | The name of a theme icon. |
| `Description` | A free description of the action.<br>Defaults to empty. |
| `Enabled`     | A user might define many actions or menus, and choose to only enable some of them from time to time.<br>Defaults to `true`. |
| `Profiles`    | The ordered list of the profiles attached to this action, e.g. `profile-zero;profile-one;` See [Profile Definition](#profile-definition). |

Please note that, if the value of a key is a list, you should put `;` after each element of that list, even when the list has a single element.

### Profile Definition

A profile tells the action what to do. Each profile that is added to the `Profiles` key must be defined in a `[X-Action-Profile profile-id]` group, where "profile-id" is its identifying string, like `[X-Action-Profile profile-zero]`. The group can have the following keys:

| Key              | Description |
|------------------|-------------|
| `Name`           | Not really needed and defaults to empty. It is just a convenience. |
| `Exec`           | The command to execute, possibly with arguments. [Parameters](#parameters_table) are often used with this key. |
| `MimeTypes`      | The MIME type(s) list with which this action appears.<br>Each mimetype may be fully specified (e.g. `audio/mpeg;`), or as a group (e.g. `image/*;`).<br>Mimetypes may be negated (e.g. `audio/*;!audio/mpeg;`).<br>Some of well-known mimetypes include:<br> ● all/all: matches all items<br> ● all/allfiles: matches only files<br> ● inode/directory: matches only directories |
| `Basenames`      | A list of base directory names the selection should match for this profile to appear.<br>The `*` character is accepted as a wildcard.<br>Base names may be negated (e.g. `*;!*.h;`).<br>Defaults to `*;`. |
| `Folders`        | A list of paths the current base directory must be in for the action to appear.<br>A folder path may be negated (e.g. `/data;!/data/resources/secret;`).<br>The `*` character is accepted as a wildcard, replacing any level(s) of subdirectory (e.g. `/music;/video;!*/secret;`).<br>Defaults to `/;`. |
| `SelectionCount` | Whether this profile may be selected depending of the count of the selection.<br>This is a string of the form "{< *or* = *or* >} NUMBER".<br>Examples of valid strings are: `=1`, `>1`, `<10` *(do not forget the `=` sign after the key)*.<br>Defaults to `>0`. |

Except for `Exec`, all the above keys may also appear in an [action](#action-definition).

<a name="parameters_table"></a>List of parameters, used with `Exec` (the most common ones are %f, %F, %u and %U):

| Parameter | Description                                                   | Form       |
|-----------|---------------------------------------------------------------|------------|
| %b        | (first) basename                                              | singular   |
| %B        | space-separated list of basenames                             | plural     |
| %c        | count of selected items                                       | irrelevant |
| %d        | (first) base directory                                        | singular   |
| %D        | space-separated list of base directory of each selected items | plural     |
| %f        | (first) file name                                             | singular   |
| %F        | space-separated list of selected file names                   | plural     |
| %h        | hostname of the (first) URI                                   | irrelevant |
| %m        | mimetype of the (first) selected item                         | singular   |
| %M        | space-separated list of the mimetypes of the selected items   | plural     |
| %n        | username of the (first) URI                                   | irrelevant |
| %s        | scheme of the (first) URI                                     | irrelevant |
| %u        | (first) URI                                                   | singular   |
| %U        | space-separated list of selected URIs                         | plural     |
| %w        | (first) basename without the extension                        | singular   |
| %W        | space-separated list of basenames without their extension     | plural     |
| %x        | (first) extension                                             | singular   |
| %X        | space-separated list of extensions                            | plural     |
| %%        | the "%" character                                             | irrelevant |

---

Let us see the meanings of some of these keys in practice.

Create `~/.local/share/file-manager/actions/root-editor.desktop` as:

```
[Desktop Entry]
Type=Action
Icon=text-editor
Name=In Root Text Editor
Profiles=profile-zero;

[X-Action-Profile profile-zero]
SelectionCount==1
MimeTypes=text/plain;
Exec=lxsudo featherpad %f
Name=Default profile
```

Then restart PCManFM-Qt from *LXQt Session Settings* → *Desktop*. Now, if you right click a single text file, you will see a custom action that says "In Root Text Editor" and will open that file in FeatherPad's root instance. The action does not appear when you right click a selection of multiple text files, because of the line `SelectionCount==1`.

As another example, `~/.local/share/file-manager/actions/set_wallpaper.desktop` sets a right-clicked image as the wallpaper of LXQt desktop:

```
[Desktop Entry]
Type=Action
Name=Set as wallpaper
Icon=user-desktop
Profiles=profile-zero;

[X-Action-Profile profile-zero]
SelectionCount==1
MimeTypes=image/*;
Exec=pcmanfm-qt -w %f
Name=Default profile
```

To have a more flexible and complex custom action, you can make an executable Bash script and put its path plus an appropriate [parameter](#parameters_table) as the value of `Exec`. In most systems, executables could also be called by their names if they are put inside the directory `~/bin`. If that is not the case in your distro, you might want to add that directory to your `PATH` environment variable (consult your distro documentation).

### Menu Definition

A menu action defines a sub-menu with more actions. Its keys are like those of ordinary actions but, instead of `Profiles`, it has `ItemsList`:

| Key         | Description |
|-------------|-------------|
| `ItemsList` | The ordered list of the actions attached to this menu.<br>Each element of this strings list is the ID of an action (its file name minus ".desktop"), e.g. `root-editor;set_wallpaper;` |

For example, if we have the two actions that were defined under the section [Profile Definition](#profile-definition), we could create `~/.local/share/file-manager/actions/misc-actions.desktop` as:

```
[Desktop Entry]
Type=Menu
Icon=unknown
Name=Miscellaneous Actions
ItemsList=root-editor;set_wallpaper;
```

Then, if a text or image file is right clicked, a sub-menu item with the name "Miscellaneous Actions" will appear in its context menu and the relevant action will be in its sub-menu.

The particular sub-menu created above is not useful in itself but shows how to group actions in sub-menus. For example, you can put all actions whose `MimeTypes` key is `inode/directory;` inside a "Folder Actions" sub-menu to have an uncluttered context menu.

If the actions of a sub-menu are shown together (when their `MimeTypes` overlap), their order will be the same as in `ItemsList`.

### Order of Actions

By default, top-level actions appear in alphabetical order (if they can appear together at all) but you can give them a different order by creating `~/.local/share/file-manager/actions/level-zero.directory` as:

```
[Desktop Entry]
ItemsList=action1_id;action2_id;action3_id;...;
```

`ItemsList` is as in [Menu Definition](#menu-definition) but, here, it defines the order of top-level actions, as they appear in the context menu.

### More Examples

Some custom actions are available [here](https://github.com/stefonarch/custom-actions).
