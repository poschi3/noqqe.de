---
categories:
- administration
- bsd
- hardware
- network
- osbn
- ubuntuusers
date: 2016-06-26T12:44:36+02:00
tags:
- Hue
- Phillips
- IPv6
- OpenBSD
title: IPv6 für Phillips Hue Bridge
---

In der K4CG haben wir seit Längerem ein paar Phillips Hue Lampen die wir
auch über ein [CLI](https://github.com/k4cg/k4cglicht) steuern können.
Wir wollen das auch übers Internet steuerbar haben um die Glühbirnen letztendlich mit
dem Bot [Rezeptionistin](https://github.com/k4cg/rezeptionistin) zu
bedienen.

Das Problem dabei: Wir haben nur eine permanente IP die v6 ist und die Hue
Bridge kann nur IPv4. Deshalb musste irgendwie eine Weiterleitung von einer v6
Adresse zu einer internen v6 Adresse her. Am besten nur Portforwarding.

Am Anfang hab ich's mit `pf` versucht, aber das endet mit dem Error `AF
Mismatch`. Danach wurde es eine Lösung mit einem Netzwerktool, das wohl so
ziemlich alles kann: `socat`.

```
/usr/local/bin/socat TCP6-LISTEN:1337,fork TCP4:10.0.0.17:80
```

Das alles über `supervisord` gestartet, mit `pf` abgesichert und fertig ist die permanente
Portweiterleitung von v4 auf v6.