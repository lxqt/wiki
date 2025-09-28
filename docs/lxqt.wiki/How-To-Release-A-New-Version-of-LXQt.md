## Order

Preparing and releasing of LXQt components can be done in this order (only important components are listed and the order isn't strict near the end):
```
  lxqt-build-tools
  libqtxdg
  liblxqt
  libfm-qt
  pcmanfm-qt
  lxqt-qtplugin
  lximage-qt
  lxqt-archiver
  libsysstat
  lxqt-about
  lxqt-config
  lxqt-globalkeys
  lxqt-notificationd
  lxqt-policykit
  lxqt-powermanagement
  lxqt-session
  lxqt-sudo
  lxqt-panel
  lxqt-runner
  lxqt-themes
  qtermwidget
  qterminal
  lxqt-openssh-askpass
  lxqt-admin
  xdg-desktop-portal-lxqt 
  pavucontrol-qt
  screengrab
  qps
  …
```
You should have installed the latest git versions of all LXQt components and be satisfied with their performance before proceeding.

## Preparation

1. Prepare the components for their new releases by making PRs (with titles like "Pre-release changes") for:
    * Incrementing versions;
    * Incrementing ABI versions if relevant;
    * Incrementing versions of dependencies;
    * Incrementing the minimum Qt version only if needed; and
    * Updating CHANGELOGs.
2. Merge the above-mentioned PRs when ready and prepare release notes for release pages. Release notes can be extracted from change logs — from the most to the least exciting changes — so that the average user could understand them easily.

## Releasing

Release the components in the above-mentioned order. To do so, for each component:
  1. Make a signed tag (the version), verify it and push it.
  2. Create `PACKAGE-TAG.tar.xz` from the latest git source and also `PACKAGE-TAG.tar.xz.asc` by using `gpg`.
  3. Go to GitHub release page, edit the latest release (which has already been created because of 2), add its release note (which you've already made) and upload `PACKAGE-TAG.tar.xz` and `PACKAGE-TAG.tar.xz.asc`.
  4. Release it!

## Announcement

Announce the new version of LXQt with some highlights as the release note of `lxqt-about`.

Tell distro maintainers that they can start their jobs. If someone finds an issue in your work, fix it — of course, reluctantly because you're tired now.