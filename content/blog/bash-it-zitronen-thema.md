---
aliases:
- /archives/1342
- /blog/2010/11/01/bash-it-zitronen-thema
date: '2010-11-01T13:00:47'
tags:
- development
- bash-theme
- style
- git
- github
- terminal
- theme
- shell
- xubuntu
- linux
- zitron
- theming
- bash
title: bash-it | Zitronen-Thema
---

Benutze nun schon seit längerem ein Community Framework für Bash, welches
sich [bash-it](http://github.com/revans/bash-it) nennt. Darin lassen sich
allerhand tolle Sachen einheitlich deklarieren und auslagern. Da meine
.bashrc jetzt auch schon an die 300 Zeilen schwer ist, dachte ich es wäre
eine gute Idee meine Konfigurationen dort hin auszulagern. Da wären dann
auch gleich meine Completion und Theming Files untergebracht.

Mir gefiel zwar nicht wirklich auf Anhieb ein ausgeliefertes Theme, aber es
war relativ einfach ein Neues zu erstellen. Es war dann gelb, weil sich das
schön vom Rest meines Terminals abhob. In einem Anflug wahnsinniger
Einfallslosigkeit, nannte ich es dann einfach zitron. Zitron sieht
folgendermaßen aus:

{{< figure src="/uploads/2010/11/zitron.png" >}}

Das Theme reagiert (als Teil von bash-it) auf git Repos. Wenn man sich in
einem versionierten Verzeichnis befindet, wird im Prompt automatisch der
Name des Branch (master) eingefügt und zusätzlich dazu überprüft, ob das
Working-Directory "dirty" ist (also uncommittete Änderungen). Falls ja wird
ein gelbes Asterisk eingefügt (*).

Mein Fork auf github von bash-it:
[http://github.com/noqqe/bash-it](http://github.com/noqqe/bash-it) Das
Theme als Quelltext:
[http://github.com/noqqe/bash-it/blob/master/themes/zitron/zitron.theme.bash](http://github.com/noqqe/bash-it/blob/master/themes/zitron/zitron.theme.bash)
