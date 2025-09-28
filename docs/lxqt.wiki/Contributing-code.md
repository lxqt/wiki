Welcome! Whether you just want to contribute a few patches, or be an active developer on LXQt, your help is greatly appreciated. LXQt is exclusively a volunteer effort. Finding people willing to contribute isn't always easy. That is why every bit of help can make huge differences.

Let's get you started.

## Development environment

Due to its modular approach, you do not have to have every bit of LXQt compiled and installed by hand in order to work on it. As long as you have up-to-date git-version of [lxqt-build-tools](https://github.com/lxqt/lxqt-build-tools), [libqtxdg](https://github.com/lxqt/libqtxdg), [liblxqt](https://github.com/lxqt/liblxqt) and [lxqt-qtplugin](https://github.com/lxqt/lxqt-qtplugin) you can likely use a prepackaged version of LXQt as your day-to-day Desktop Environment. The [Arch Linux AUR](https://aur.archlinux.org/packages/lxqt-desktop-git/) makes this very easy and is the preferred way for LXQt developers.

[KDevelop](https://www.kdevelop.org/) is an excellent Qt-based IDE for C++ and due to it being a KDE project, integrates really well with Qt projects.
However, you will find that you do not necessarily need a full-blown IDE to contribute. Several developers work on LXQt off their favourite text editor - FeatherPad, JuffEd, Kate, or even Vim.

Say for example you want to contribute to [lxqt-runner](https://github.com/lxqt/lxqt-runner). It is as simple as cloning the repository, then running:

* `mkdir build && cd build`
* `cmake ..`
* `make`

You can then run your self-compiled runner with `./lxqt-runner`. No installation necessary.

## Getting your changes merged

We use [git](http://git-scm.com/). Our repositories are on [GitHub](https://github.com/lxqt). A basic understanding of git is required in order to contribute - there's plenty of tutorials on the web, look around!

Still with our earlier lxqt-runner example, let's say you want to implement a new feature.

Your first step should be to see if there is an [existing issue](https://github.com/lxqt/lxqt/issues) for it. If there is, make sure to leave a note that you want to work on it.
Once you have written the code for it, you will have to [fork the repository](https://help.github.com/articles/fork-a-repo/), commit your changes and push them to your fork.
Your next step will then be to [file a pull request](https://help.github.com/articles/using-pull-requests/) ("PR") with the LXQt repository ("upstream").
There, your changes will be reviewed by a developer and you may be asked to change or fix a few things. You do so by continuing to commit (or modifying existing commits) on your fork, and then running `git push` as you normally would - the pull request will be automatically updated. (Note that if you modified existing commits in your PR, you will have to run `git push --force`.)

Eventually, the developer will recognize your PR as "Good To Merge" ("GTM"). Depending on the size of the changes, it will then be merged straight away or it will require the input of more developers.

If you regularly contribute good code and are interested in joining the developer team, you will eventually get commit access. Keep at it!

### New committer
Once you got commit access don't run loose.
Take care to not break something. In case of small changes you can just push them, but if you are not totally sure then rather file a PR and get at least one "GTM" from another developer. For big changes maybe even wait for three *GTMs*.

Nobody is perfect, so in case you made a wrong commit which you would like to revert, do a:

```
git reset --hard <hash>
git push --force
```

Where *\<hash\>* is the last good commit.
Forced pushes can be dangerous when you are working in a team. **Only** do this immediately after your commit, so chances are small someone already made another commit on top of your wrong one. You can break the tree if you don't take care. Don't do this.

## Clean commits

To help getting your changes merged, please make sure your commits are clean!

* The surrounding code style should be respected. We also use 4 spaces for indentation everywhere. No mixed indents, no trailing whitespaces, and nothing unreadable!
* In most cases, the code should have no debug statements left over from testing and no commented out functionality.
* The commits themselves should be clean! Each commit should be working independently, add one feature/fix per commit and the commit messages should be [descriptive of the change and properly spelled/capitalized](http://chris.beams.io/posts/git-commit/). This is important for [bisecting bugs](http://git-scm.com/docs/git-bisect) and reading the log pleasantly.
 * **Tip**: Learn, use and abuse [`git rebase --interactive`](https://help.github.com/articles/about-git-rebase/). It will help you edit, clean and squash your existing commits. There are lots of [excellent tutorials](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase/) on how to use it.
* Make sure to set your [`git config --global user.name`](https://help.github.com/articles/setting-your-username-in-git/) and [`user.email`](https://help.github.com/articles/setting-your-email-in-git/) so that your real name and email can be available in the commit logs.

<!--## Notes for usage of specific features
 * We **don't use Qt's [QStringLiteral](http://doc.qt.io/qt-5/qstring.html#QStringLiteral) in plugins**, because QString objects constructed this way can "leak out of plugin boundaries" and cause application crash (Qt < 5.6) or unnecessary memory usage (Qt >= 5.6). See [QTBUG-49061](https://bugreports.qt.io/browse/QTBUG-49061), [Qt discussion](http://lists.qt-project.org/pipermail/development/2015-November/023681.html), [our discussion](https://github.com/lxde/lxqt-panel/pull/256).
-->

## Getting help

Feeling lost? Need mentoring? Do not hesitate to ask for help. There are several points of contact, all of which you should monitor if your goal is to be active on LXQt.

<!--
 * The [lxde-list mailing list](https://lists.sourceforge.net/lists/listinfo/lxde-list). All LXDE and LXQt matters are discussed there, it is both a user and developer list. 
-->
* [LXQt Discussions](https://github.com/lxqt/lxqt/discussions) is monitored by developers. Do not hesitate to ask for help, feedback and general input on issues or planned features there. It can also help a lot in keeping track of who is working on what.
* The [#lxqt channel on OFTC](https://webchat.oftc.net/?channels=#lxqt), [Telegram](https://t.me/lxqtofficial) or [Matrix](https://matrix.to/#/#lxqt:matrix.org) (which are all bridged among them) and the [#lxqt-dev channel on OFTC](https://webchat.oftc.net/?channels=#lxqt-dev). The former is intended to be a user channel and the latter is for developers.

Good luck!