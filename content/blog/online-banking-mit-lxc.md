---
title: "Online Banking mit LXC"
date: 2013-11-08T18:00:00+02:00
comments: true
categories:
- osbn
- ubuntuusers
- Debian
- crypto
tags:
- lxc
- chromium
- onlinebanking
- bank
- ssl
- trust
- certificates
---

Das Thema Online Banking ist eines bei dem ich regelmäßig mit
Bekannten/Verwandten aneinander geraten könnte. Bzw. es mittlerweile
einfach sein lasse mich zu Diskussionen hinreissen zu lassen. Um halbwegs
sicher zu sein, muss man dabei garnicht so viel beachten.

{{< figure src="/uploads/2013/11/birdienumnum.gif" >}}

Online-Banking möchte man wohl möglichst nicht auf einer Maschine betreiben,
die auch zum Surfen/Arbeiten/Sonstigem benutzt wird, da die Gefahr hoch ist
sich auf normalen Seiten etwas einzufangen. Viele greifen deshalb zu einer
VM in Virtualbox oder dergleichen.

### Setup

Ich mag aber kein Virtualbox, alles fühlt sich so doof an. Vagrant? Wäre
gegangen, aber tatsächlich habe ich bereits [LXC](http://lxc.sourceforge.com).
Entscheidung fiel also auf einen Banking-LinuxContainer.

Ein kleines Debian stable installiert und mit den Paketen bestückt ist die zum Browsen
gebraucht werden.

```
$ mlxc clone vm13-stable vm24-bankingtemplate
$ ssh vm24-bankingtemplate -l root "aptitude install chromium xauth"
$ mlxc clone vm24-bankingtemplate vm99-tempbanking
```

Noch alle Zertifikate entfernt und fertig wars.
Bis auf alle 3 (oder 5) Jahre das Zertifikat der Bank updaten und Sicherheitsupdates im
OS muss ich an das Template erstmal nicht mehr hinfassen. Das Skript
[mlxc](https://gist.github.com/noqqe/2693967) ist ehrlich gesagt nur ein billiger
Wrapper um `lxc`, den ich damals wegen der _intuitiven_ Bedienung von `lxc`
geschrieben habe. Heutzutage würde man für sowas eher
[docker](http://docker.io) benutzen.

### Ablauf

Bei jedem Start des Browsers wird die temporäre VM (vm99) mit dem
Stand nach der Installation (vm24) überbügelt. Bewerkstelligen tut das
biser noch `rsync`. Es gäbe coolere Lösungen a la Snapshot in LVM
oder sonst was, bisher funktioniert es aber mit `rsync` ganz gut
für mich.

```
$ rsync -av --delete /home/lxc/vm24-bankingtemplate/rootfs/ /home/lxc/vm99-tempbanking/rootfs/
$ mlxc start vm99-tempbanking
$ ssh -X -l noqqe vm99-tempbanking "chromium"
$ mlxc stop vm99-tempbanking
```

Im Endeffekt sehr simpel gehalten, nur umständlich zu bedienen. Irgendwie wollte
ich noch einen Tick mehr Usability.


### Integration

Ich hatte erstmal nur ein Skript, ähnlich der Abfolge weiter oben. Da
das alles aber schon ein kleinwenig dauert möchte wenn möglich schon
etwas Progress sehen. [Zenity](https://help.gnome.org/users/zenity/stable/)
baut sehr leicht parameterisierbare GTK Oberflächen.

{{< figure src="/uploads/2013/11/update.png" >}}
{{< figure src="/uploads/2013/11/start.png" >}}

Gerade für den Traditionellen Ladebalken gibt es ein sehr schönes
[Beispiel](https://help.gnome.org/users/zenity/stable/progress.html.en)

Das sehr businessmäßig aussehende Logo oben in der Ecke wurde freundlicherweise
von [GNU Cash](http://gnucash.org) zur Verfügung gestellt, war aber eher ein
Zufallsfund auf meinem System, da ich mir GNUCash zuletzt mal angesehen hatte.

Und weil ich jetzt auch noch hart multimedial werde: Vimeo.

<iframe src="//player.vimeo.com/video/78758620" width="600" height="376"
frameborder="0" webkitallowfullscreen mozallowfullscreen
allowfullscreen></iframe>