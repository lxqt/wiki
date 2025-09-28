## Overview

The LXQt Project uses again an [Weblate instance](https://translate.lxqt-project.org) for translations,  including desktop files. Potential translators should choose Weblate as the preferred translation tool.

Local translations using QtLinguist are possible downloading and uploading *.ts files using the Weblate platform. 

Direct Pull Requests to Github for *.ts files are still possible but not recommended as they can lead to merge conflicts.

## General approach

Create an account on [translate.lxqt-project.org](https://translate.lxqt-project.org/accounts/login/?next=/) or login with your existing Github, Ubuntu, Fedora or OpenSuse account. It is also possible to add suggestions for strings _without_ having any account.
The LXQt Project is split up in 3 groups:

* [LXQt-Desktop](https://translate.lxqt-project.org/projects/lxqt-desktop): Contains applications and main components.
* [LXQt-Configuration](https://translate.lxqt-project.org/projects/lxqt-configuration): Groups all configuration settings.
* [LXQt-Panel](https://translate.lxqt-project.org/projects/lxqt-panel): Translations for the panel and its plugins.

There can be selected watched projects, preferred languages, email notifications and else in the account's profile settings. 
Please note that for adding language files that are not already present [contacting  administrators](https://translate.lxqt-project.org/contact/) is needed.

### Desktop Entry Files

Those are comprised in distinct components' repositories like the one of PCManFM-Qt where they reside in a folder "translations". Translations are handled by snippets like pcmanfm-qt_de.desktop.yaml comprising nothing but the respective translation. 
All of the `*.desktop.yaml` files are now handled in LXQt Weblate ("Menu Entry").

* `Name=`: Name displayed in the menu, runner and pcmanfm-qt/applications
* `GenericName`: Tooltip in the menu, explanation in runner
* `Comment`: is used as tooltip only if  `GenericName=`   and `GenericName[xy]=` are both absent in `/usr/share/applications/foo.desktop`.

It is still possible to edit desktop entry files forking the repository, using an text editor and making a Pull Request but it is not recommended.

### Panel Desktop Entry Files

The names and descriptions of the panel plugins  are *.desktop.yaml files  in `/lxqt/lxqt-panel/plugin-foo/translations/`.

Translations for menu categories are *.directory.yaml files in https://github.com/lxqt/lxqt-menu-data/tree/master/menu/translations, also on Weblate.

### Credits

Credits for translations (displayed in LXQt-About) can be inserted in [lxqt/lxqt-about/tree/master/translatorsinfo/translators_XY.info](https://github.com/lxqt/lxqt-about/tree/master/translatorsinfo) by forking the and making a PR.

### Language vs. country specific translations

The Qt translation framework allows for distinguishing language from country specific translations which does have some implications that should get considered by translators.

### Glossary

Language specific translations are meant to cover a language independently from a particular country. Corresponding files feature a single double-digit lower-case code referring to the language only like "pt" in `lxqt-panel_pt.ts` for Portuguese.   
Country specific translations are meant to address a language variant that's spoken in a particular country only. Corresponding files feature an additional double-digit code in capital letters referring to a particular country like "BR" in `lxqt-panel_pt_BR.ts` for Brazil.   

Either way the code(s) have to be reflected in the TS files' headers as well like   
`<TS version="2.1" language="pt">`.   
or   
`<TS version="2.1" language="pt_BR">`   
This can get handled by Qt Linguist, too (menu `Edit` - `Translation File Settings`).

### Need for country specific translations

Frequently these aren't needed at all. E. g. German gets spoken in several countries and plenty of differences exist. But these do not affect the technical terms used by LXQt in any way.   
In Portuguese many of the technical terms used by LXQt are different in Brazil or Portugal, though. Corresponding country specific variants are hence needed.

So first of all a translator should think about the need of country specific translations. Questions can be asked in [Discussions[(https://github.com/lxqt/lxqt/discussions) and so be discussed with other native speakers.   
If country specific translations aren't needed a single language specific translation like `de` will do (and the next paragraph doesn't matter any longer).

### Country specific translations vs. system locale

The locale used by a running system as indicated by the output of `$ locale` on Linux will be country specific most of the time like in this example
```
$ locale 
LANG=de_DE.UTF-8
LC_CTYPE="de_DE.UTF-8"
LC_NUMERIC="de_DE.UTF-8"
[...]
```
Note the string `de_*DE*`. The encoding, usually `UTF-8` nowadays, frequently `ISO-8859-1` earlier, doesn't matter in this context.

It is important to note that country specific translations are only used when a system's locale is set to the same country.   
E. g. a Portuguese translation `pt_BR` is only used when a running system's locale is `pt_BR`, too. This translation does *not* get used if the system's locale is `pt_PT` (Portugal).   
This means that no Portuguese translation is used at all should a system's locale be set to `pt_PT` and `pt_BR` be the only Portuguese translation that's available.

Language specific files will always be used as fallback.   
E. g. if two translations `pt_BR` and `pt` are available and a system's locale is set to `pt_PT` translation `pt` will be used.

As a consequence a country agnostic translation like `pt` should be provided even if country specific translations are basically needed as long as not all of them are available yet.   
This should be done in order to have a fallback as depicted above. It's still better to use a translation that eventually doesn't fit the country of a running system's locale than no translation at all resulting in an English GUI.

### Resolving Weblate rebase failures

For most LXQt repositories on GitHub, a [webhook](https://docs.github.com/en/developers/webhooks-and-events/webhooks/about-webhooks) is configured so that the Weblate server receives notifications about new commits of a repository from GitHub.
Weblate will rebase local commits on top of the latest upstream (GitHub) commit.

However, from time to time, rebase failed [for various reasons](https://github.com/lxqt/lxqt/issues/1919).
Rebase failures can be fixed by logging onto the server via SSH and fix commits with the `git` command.
The Weblate documentation also mentions the `wlc` command line tool, but I haven't not tried it out yet.

Here are steps about fixing failures via SSH:

1. `ssh weblate@translate.lxqt-project.org` (ask [stefonarch](https://github.com/stefonarch) or [yan12125](https://github.com/yan12125) for SSH access)
2. `cd data/vcs/<git path>`.
    Here the git path can be checked from the _Repository containing Weblate translations_ field on the _Repository Maintenance_ page on the Weblate website.
    For example, on https://translate.lxqt-project.org/projects/lxqt-configuration/lxqt-admin-user-menu-entry/#repository, it is shown that the git path is `lxqt-configuration/lxqt-admin-user`.
    Note that for [linked components](https://docs.weblate.org/en/latest/vcs.html#weblate-internal-urls), the git path can be different from the URL.
3. Resolve rebase failures (**needs more explanation**)
4. Hit the _Rebase_ button on the _Repository Maintenance_ page, and the Weblate component (as long as linked ones) will be unlocked.

### Refresh the GitHub token for Weblate

1. Login to LXQtBot GitHub account and [regenerate a token](https://github.blog/2021-04-05-behind-githubs-new-authentication-token-formats/)
2. Update `GITHUB_TOKEN = ` line in `~/settings.py` and `oauth_token: ` line in `~/.config/hub`
3. Restart the Weblate service by `sudo systemctl reload celery-weblate.service`