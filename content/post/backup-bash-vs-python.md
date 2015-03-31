---
date: 2009-03-18T15:10:49+02:00
layout: post
slug: backup-bash-vs-python
status: publish
comments: true
title: Backup | Bash vs. Python
alias: /archives/568
categories:
- Coding
- Linux
tags:
- backup
- bash
- python
- shell
- skript
---

Mein erstes BackupScript war in Python geschrieben. Das sah dann wie folgt aus:

```
#!/usr/bin/python
import os
import time
quellen = ['/home/ /etc/ /var/www/ /root/ ']
ziel_verzeichnis = ('/media/backup/backup/')
ziel = ziel_verzeichnis + time.strftime('%Y-%m-%d') + '.tar.gz'
zip_befehl = 'tar -czvf %s %s' % (ziel, ' '.join(quellen))
loggingok = 'echo " " >> /var/log/backup.log; echo `date +%d-%m-%Y-%H:%M:%S` --- NEW Action ---  >> /var/log/backup.log; echo `date +%d-%m-%Y-%H:%M:%S` Erfolgreich gebackupped!  >> /var/log/backup.log; echo `date +%d-%m-%Y-%H:%M:%S` --- END Action ---  >> /var/log/backup.log '
loggingno = 'echo " " >> /var/log/backup.log; echo `date +%d-%m-%Y-%H:%M:%S` --- NEW Action ---  >> /var/log/backup.log; echo `date +%d-%m-%Y-%H:%M:%S` Backup fehlgeschlagen! >> /var/log/backup.log; echo `date +%d-%m-%Y-%H:%M:%S` --- END Action ---  >> /var/log/backup.log '
if os.system(zip_befehl) == 0:
print 'Erfolgreiche Sicherung nach', ziel
os.system(loggingok)
else:
print 'Sicherung fehlgeschlagen!'
os.system(loggingno)
```


Aber irgendwie... wurde mir das bissl zu blöd. Alle meine Skripte laufen auf Bash. Warum beim Backuppen aus der Reihe tanzen?
Eigentlich wäre das ganze schon nach _einer_ Zeile gelaufen. Wenn ich nicht noch die LoggingFunktion hätte.

```
#!/bin/bash
tar -czvf /media/backup/backup/`date +%Y-%m-%d-%H-%M`.tar.gz /home /etc /var/www /root
return=`echo $?

if [ $return -eq 0 ]; then
echo " " >> /var/log/backup.log
echo `date +%d-%m-%Y-%H:%M:%S` --- NEW Action ---  >> /var/log/backup.log
echo `date +%d-%m-%Y-%H:%M:%S` Files Erfolgreich gebackupped!  >> /var/log/backup.log
echo `date +%d-%m-%Y-%H:%M:%S` --- END Action ---  >> /var/log/backup.log
else
echo " " >> /var/log/backup.log
echo `date +%d-%m-%Y-%H:%M:%S` --- NEW Action ---  >> /var/log/backup.log
echo `date +%d-%m-%Y-%H:%M:%S` File Backup fehlgeschlagen! >> /var/log/backup.log
echo `date +%d-%m-%Y-%H:%M:%S` --- END Action ---  >> /var/log/backup.log
mail -s "Backup fehlgeschlagen!" root@zwetschge.org < /var/log/backup.log
fi
```


Ich weiss nicht. Mir gefällts besser.