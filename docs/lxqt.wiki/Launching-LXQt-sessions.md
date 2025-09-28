LXQt X11 sessions are launched by a script `startlxqt` and handled by a binary ("session manager") `lxqt-session` which belongs to [lxqt-session](https://github.com/lxde/lxqt-session).  

LXQt Wayland sessions are launched by a script `startlxqtwayland` instead. 

## Launching from virtual terminals

In Linux, add the following line to the file `~/.xinitrc`.

```shell
exec startlxqt
```

In FreeBSD, add the following line to the file `~/.xinitrc`. 
```shell
exec dbus-launch --exit-with-x11 ck-launch-session startlxqt
``` 
The session can be launched by invoking `startx` shell command.

> [!WARNING]
> Ensure one of the following conditions is true regarding the `~/.xinitrc` file:
> * only one entry exists
> * multiple entries follow the rules described [here](https://wiki.archlinux.org/title/xinit).

The Wayland session can be started directly invoking `startlxqtwayland`.

## Launching by display managers

Display managers like [SDDM](https://github.com/sddm/sddm), the one recommended for LXQt, or LightDM are looking for available x11 sessions in files `$XDG_DATA_DIRS/xsessions/*.desktop` where `$XDG_DATA_DIRS` is set to `/usr/share/` most of the times,while for the Wayland sessions the directory is `/usr/share/wayland-sessions/`. 
LXQt is providing the files `lxqt.desktop` and `lxqt-wayland.desktop` (if `lxqt-wayland-session` is installed) so its sessions should normally be detected and made available by display managers automatically, restarting the latter may be needed upon installing LXQt.   

Note if LXQt was installed in `/usr/local` file `lxqt.desktop` is stored in `/usr/local/share/xsessions` which is *not* considered by most display managers. So you'll either have to copy the file to `/usr/share/xsessions/` or create a symbolic link.   



### Fixing crashes for apps using dbus-daemon (common when using lxqt-config-monitor)

Important: This applies only to systems running systemd which have this problem.

On some systems (like the default ArchLinux install), dbus-daemon is started as a systemd service before the xserver thus it has no $DISPLAY variable set in it's environment, leding to crashes. Systemd by default includes a script to fix this by fetching the variables in /etc/X11/xinit/xinitrc.d/50-systemd-user.sh. If you encounter this kind of crash add the following lines to your .xinitrc before `exec startlxqt`

```shell
if [ -d /etc/X11/xinit/xinitrc.d ] ; then
 for f in /etc/X11/xinit/xinitrc.d/?*.sh ; do
  [ -x "$f" ] && . "$f"
 done
 unset f
fi
```

