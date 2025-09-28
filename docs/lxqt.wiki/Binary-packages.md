Pre-compiled binary packages are provided by all major Linux and BSD distributions.   

Basic knowledge of the respective package management tools is required and not within the scope of this wiki. Please refer to the documentation of the distribution you're using.   

For a list of LXQt versions on various distributions, please check https://repology.org/project/lxqt-session/versions.

!!! note
     If your distribution has an older package version, please do not report bugs on Github, but rather on their respective bug trackers. Consult "Building from Source" if you want the latest LXQt on such a distribution. 

## Arch Linux

The stable release is available in community repo, packages' names are the same as upstream. A group `lxqt` exists and pulls all relevant ones.

Latest VCS code is provided in the AUR, packages' names are the same as upstream suffixed by `-git`.

## Manjaro, EndeavourOs, CachyOs

Nearly identical as Arch Linux.

## Debian, Derivatives

### Debian

LXQt is available in Debian's official repositories (LXQt 1.2.0 in Debian 12 "bookworm" and LXQt 2.1.x in Debian 13 "Trixie"). There are metapackages `lxqt-core` providing core features and `lxqt` providing LXQt as a whole plus some additional applications. See https://github.com/lxqt/lxqt/discussions/2712 for LXQt 2.2 in Debian Trixie. 

### Ubuntu, Derivatives

#### Official repositories

LXQt is available in the official repositories as of Ubuntu 18.04 "Bionic Beaver" and later.
The metapackages are the same as in Debian.   

#### PPAs

A ppa with LXQt 1.4.x packages for Ubuntu "Jammy Jellyfish" 22.04 LTS and 24.04 "Noble Numbat" is [available](https://launchpad.net/~lubuntu-dev/+archive/ubuntu/backports/+packages),

## Fedora

LXQt is included in the official repositories. A corresponding group "lxqt" is available as well as a spin: https://fedoraproject.org/spins/lxqt .

## FreeBSD

LXQt is available in the official repositories. You can find information about installing [here](https://wiki.freebsd.org/LXQt).

## Gentoo

LXQt stable releases are made available via the official Gentoo repository.  You can find information about installing [here](https://wiki.gentoo.org/wiki/LXQt).

Additionally, "live ebuilds" using git commits can be found from the Qt overlay.


## Mageia

A meta-package to install LXQt on Mageia's latest release can be found [here](http://mageia.madb.org/package/show/application/0/name/task-lxqt).    

## openSUSE

Stable versions are maintained in `X11:LXQt` and included in openSUSE Leap and Tumbleweed. LXQt can be installed via the LXQt pattern: `zypper in -t pattern lxqt`, or via clicking on the pattern in YaST Software or by installing every component individually. More information can be found in the [openSUSE Wiki](https://en.opensuse.org/LXQT).   
Git snapshots are available in `X11:LXQt:git`.   

