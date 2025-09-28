People often confuse LXQt as a *rewrite* of [LXDE](https://www.lxde.org) in Qt. However, this is not true.

## LXDE-Qt
[PCMan](https://github.com/PCMan), the original author of LXDE, started [experimenting with Qt](https://web.archive.org/web/20221128180217/https://blog.lxde.org/2013/02/19/pcmanfm-file-manager-is-ported-to-qt/) by implementing a GUI for [PCManFM](https://en.wikipedia.org/wiki/PCMan_File_Manager) in Qt. While doing so he wrote [a guide for migrating from GTK+ to Qt](https://web.archive.org/web/20231113055042/https://blog.lxde.org/2013/04/25/a-guide-for-porting-gtk-applications-to-qt/), where he sums up his experiences.

When he released a [preview](https://web.archive.org/web/20250316084622/https://blog.lxde.org/2013/07/03/lxde-qt-preview/), some people feared that a Qt-based desktop environment would be resource-heavy and bloated. So, he posted about [LXDE-Qt not being bloated](https://web.archive.org/web/20240929094811/https://blog.lxde.org/2013/07/05/no-lxde-qt-is-not-bloated/). LXDE was written in GTK+2, but times changed, and GTK+3 arrived. GTK+3 changed people's opinions so much so that an LXDE port to Qt was considered superior to a port to GTK+3.

!!! note
     For those interested in numbers, read [PCMan's post about resource usage and performance](https://web.archive.org/web/20240428171503/https://blog.lxde.org/2014/05/09/some-numbers-about-lxqt-for-those-who-are-curious/).

## Razor-qt
While PCMan was experimenting with Qt, another group of people was working on a Qt-based desktop environment called Razor-qt. Eventually the people behind the LXDE-Qt and Razor-qt projects [decided to work together](https://web.archive.org/web/20241201170854/https://blog.lxde.org/2013/07/22/the-future-of-razor-and-lxde-qt/). Thus was born *LXQt*. So, instead of being a rewrite of LXDE in Qt, LXQt is rather a *lightweight* [desktop environment heavily based on the Razor-qt code base](https://web.archive.org/web/20230528045511/https://blog.lxde.org/2014/11/21/in-memory-of-razor-qt/), and developed by a team consisting of LXDE and ex-Razor-qt developers.

## LXQt
[The announcement of the first alpha release of LXQt](https://web.archive.org/web/20250320123209/https://blog.lxde.org/2014/04/27/the-first-alpha-release-of-lxde-qt-or-lxqt-is-in-preparation/) used the phrase "LXDE-Qt or LXQt". Some distributions still had it categorized under the name LXDE-Qt, for historical reasons that should be clear by now. However, the debate is settled, and the official name of the project is LXQt.

Initially LXQt used Qt4. In June 2014, LXQt got full Qt5 support. Since version 0.9.0, LXQt only supports Qt5, since version 2.0.0 only Qt6.

It had been switching from using many individually developed libraries to instead use the KFrameworks5 library. Again, some people where afraid of introducing *bloat* because they thought it would mean including all KDE libraries and dependencies, but once again this has not been the case. KFrameworks5 was actually introduced in an attempt to split the basic functionality from KDE specific libraries and thus make it usable in other projects than just KDE. Currently KF6 and Qt6 are used.
