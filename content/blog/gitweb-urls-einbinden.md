---
date: 2010-04-21T10:35:43+02:00
comments: true
title: Gitweb | URLs einbinden
aliases:
- /blog/2010/04/21/gitweb-urls-einbinden
- /archives/987
categories:
- Development
- Linux
tags:
- clone
- cloneurl
- einbinden
- einfügen
- git
- gitweb
- insert
- url
---

Ich habe heute längerfristig das Web durchforsten müssen, wie ich eine URL
für den

```
git clone git://zwetschge.org/roborobo.git
```

Befehl in gitweb hinterlege. Das fand ich sehr hilfreich, denn seit ich
meinen git-daemon wieder installiert habe, wäre es theoretisch möglich die
PublicProjects darüber auszuchecken. Damit gitweb nun diese URL kennt, ist
es nötig in dem Remote-Verzeichnis die Datei cloneurl anlegen. Kurz und
prägnant damit ich es nicht wieder vergesse:

```
echo "git://zwetschge.org/roborobo.git" > /home/git/public/roborobo.git/.git/cloneurl
```

Dieses File liesst gitweb dann aus und schreibt die URL (in meinem Falle
für roborobo) in die Summary-Übersicht mit rein.

Beispiel: [http://git.zwetschge.org/?p=roborobo.git;a=summary](http://git.zwetschge.org/?p=roborobo.git;a=summary)