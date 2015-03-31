---
date: 2010-01-26T18:28:38+02:00
layout: post
slug: web-paste-service-via-commandline-sprunge-us
status: publish
comments: true
title: Web | Paste-Service via CommandLine (Sprunge.us)
alias: /archives/854
categories:
- Coding
- Linux
- PHP
- ubuntuusers
tags:
- alias
- bash
- debian
- easy_install
- py
- python
- python-setuptools
- script
- shell
- skript
- sprang
- sprang.py
- sprunge
- sprunge.us
- ubuntu
---

[Sprunge.us](http://sprunge.us) ist ein Paste-Service den ich heute von [Chris](http://cryzed.de) gezeigt bekommen habe. Sprunge ist aber außerdem noch _awesome_, weil er ohne Registrierung oder Umstände alles annimmt was man ihm via `curl -F `übergibt. Von den Entwicklern ist das wie folgt vorgesehen:

```
<command> | curl -F 'sprunge=<-' http://sprunge.us
INFO: Code: gJIJ
INFO: URL: http://sprunge.us/gJIJ

```

Und man kann unter der ausgespuckten URL den SourceCode begutachten. Den curl-Aufruf finde ich persöhnlich ziemlich lang und nicht wirklich eingängig. Das fanden anscheinend auch die Entwickler von "[sprang](http://github.com/jingleman/sprang)". Usage ungefähr so:

```
cat /usr/local/scripts/script.sh | sprang
INFO: Code: gJIJ
INFO: URL: http://sprunge.us/gJIJ
```


[sprang](http://github.com/jingleman/sprang) ist ein Python-Script das mit dem sprunge.us Pastebin-Dienst interagieren kann. Man kann ihm zum Bleistift auch mit sprang -f ein Fileübergeben, mit -L Logfiles definieren oder ähnliches bewerkstelligen (genaueres mit sprang --help). Durch die Installation des python-setuptools bzw dem Kommando

```
aptitude install python-setuptools; easy_install sprang
```


wird der Helfer für den Dienst nutzbar. Ich muss ehrlich gestehen ich bin kein Fan von Fremdpaketsystemen. Aber diesbezüglich muss es eben sein. Alternative ist natürlich ein Bash-alias

```
alias sprang="curl -F 'sprunge=<-' http://sprunge.us"
```


Wobei somit die Restfunktionalität des sprang-scripts verloren geht. Besonders schön ist auch das Syntax Highlightning. Je nach Eingespeisten Source kann man der URL beispielsweise ein ?bash oder ?py mitgeben

```
http://sprunge.us/gJIJ?bash
http://sprunge.us/gJIJ?py
```


und erhält schön bunt und leserlich ge-Highlightete Versionen des gesendeten.