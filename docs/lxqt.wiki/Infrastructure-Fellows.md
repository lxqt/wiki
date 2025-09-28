To guarantee sustainability, all services in the LXQt should be accessible by at least two currently active LXQt maintainers. Here is a list of each service.

| Service                            | Developer/maintainer with access                  | Related threads/issues                         |
| -----------------------------------| ------------------------------------------------- | ---------------------------------------------- |
| SSH for Weblate                    | stefonarch, yan12125,                             | https://github.com/lxqt/lxqt/issues/1495       |
| Weblate Admin                      | luis-pereira, stefonarch, yan12125                | https://github.com/lxqt/lxqt/issues/1495       |
| Weblate VPS project on Hetzner     | stefonarch, yan12125                              | https://github.com/lxqt/lxqt/issues/1495       |
| Domain Client Account on Hetzner   | stefonarch, file in /root                         | https://github.com/lxqt/lxqt/issues/1829       |
| DNS Console Hetzner                | stefonarch, file in /root                         | https://github.com/lxqt/lxqt/issues/1829       |
| Opencollective.com/lxqt            | yan12125,telmo1241                                | https://github.com/lxqt/lxqt/issues/1846       |
| IRC                                | yan12125 (MASTER), PhaseReverse (boost on IRC, CHANOP) |                                           |
| LXQtBot GitHub account             | yan12125, stefonarch, file in /root               | https://github.com/lxqt/lxqt/issues/1495       |
| Matrix                             | paulolieuthier, stefonarch                         | https://github.com/lxqt/lxqt/discussions/1923  |
| Telegram                           | stefonarch (owner),paololieuthier               | https://github.com/lxqt/lxqt/discussions/1949  |
| Twitter @lxqt_project              | stefonarch,                     |https://github.com/lxqt/lxqt/issues/2070#issuecomment-925563975 |
| Mastodon @LXQt@mastodon.social     | stefonarch,                     |https://github.com/lxqt/lxqt/issues/2070#issuecomment-1126789491 |
| VK                                 | shlyakpavel                      | https://github.com/lxqt/lxqt/issues/2070#issuecomment-922707099 |
| Email *@lxqt-project.org           | stefonarch, file in /root                         |                                                |

### Handy IRC commands

* To see current IRC nicknames with `CHANOP` or `MASTER` role, run `/msg chanserv access #lxqt list` and `/msg chanserv access #lxqt-dev list` after connecting to OFTC. The table above only lists active ones.
* Make/cancel myself as OP: `/msg ChanServ op #lxqt yan12125`, `/msg ChanServ deop #lxqt yan12125`
* Ask ChanServ to change the mode lock (MLOCK) for a channel: `/msg ChanServ set #lxqt-dev MLOCK -ncR+ts`. [Here](https://oftc.net/ChannelModes/) is the list of supported channel modes on OFTC. Note that `/mode` does not work here as channel modes will be reverted immediately by ChanServ once changed.