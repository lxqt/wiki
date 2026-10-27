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
    * lxqt-build-tools: Update also README.md.
    * qtermwidget: The version in `pyqt/pyproject.toml` and `lib/qtermwidget.json` should be kept in sync with the version in `CMakeLists.txt`.
2. Prepare release notes for release pages. Release notes can be extracted from change logs — from the most to the least exciting changes — so that the average user could understand them easily.

## Releasing

Release the components in the above-mentioned order. To do so, for each component:

1. Check and merge prerelease-changes PR (Browser/Web) and pull master
2. Make a signed tag (the version), verify it and push it, e.g:
```
git tag -s 2.X.0 -m "Release 2.X.0"
git tag -v 2.X.0
git push origin 2.X.0
```
**Note**: never use the web interface to create a tag.

3. Create `PACKAGE-TAG.tar.xz` from the latest git source and also `PACKAGE-TAG.tar.xz.asc` by using `gpg`.
Make sure to exclude th `.git` folder. An example script `sign-release`:
```bash
#!/bin/bash
# use inside cloned repository
REPO=${PWD##*/}

cd ~/git/lxqt/releases #Edit to your needs
git clone https://github.com/lxqt/$REPO
cd $REPO

TAG=$(git describe --tags --abbrev=0)
echo "Tag: $TAG"
#TAG=2.4.0 #fallback
read -p "Continue?"

rm -rf .git
cd ..
mv $REPO $REPO-$TAG

tar -cJf $REPO-$TAG.tar.xz $REPO-$TAG
gpg --armor --detach-sign $REPO-$TAG.tar.xz

firefox https:github.com/lxqt/$REPO/releases/new
```
4. Go to the GitHub repository's release page and draft a new release from the tag created, title=tag, add its release notes (which you've already made - if not paste the latest CHANGELOG) and upload `PACKAGE-TAG.tar.xz` and `PACKAGE-TAG.tar.xz.asc` as binaries.
5. Publish the release.

## Announcement

Announce the new version of LXQt with some highlights on https://github.com/lxqt/lxqt/releases with a new tag.

Tell distro maintainers that they can start their jobs. If someone finds an issue in your work, fix it — of course, reluctantly because you're tired now.