---
date: 2010-07-24T15:07:54+02:00
layout: post
slug: mysql-datenbanken-einzeln-sichern
status: publish
comments: true
title: MySQL | Datenbanken einzeln sichern
alias: /archives/1156
categories:
- Coding
- Debian
- Linux
- PlanetenBlogger
- SQL
tags:
- backup
- mysql
- single file
- sql
---

Bis vor kurzem reichte mir ein volles MySQL-Backup der alle DB's komplett in ein File gesichert hat. Eine Zeile CronJob.
```
mysqldump -u root --password=x --all-databases > /pfad/$(date +%Y-%m-%d).sql
```


Das funktioniert so lange, bis einmal recovered werden muss. Alle DB's neu einspielen ist dann doch irgendwie kein Spass. Weder von der Dauer noch vom Datenverlust. 25 MB sind jetzt zwar nicht die Welt, aber trotzdem doof wenn etwas verloren geht. Deshalb kombinierte ich mein Voll-Backup mit einem File pro DB. Folgendes Script dient dazu:



    #!/bin/bash
    pass=
    backuppath=/var/cache/mysqlbackups

    mysqldump --password=$pass --all-databases > ${backuppath}/$(date +%Y-%m-%d).sql
    return1=$?

    for x in $(mysql --password=$pass -Bse 'show databases'); do
    mysqldump --password=$pass $x > ${backuppath}/$(date +%Y-%m-%d)-${x}.sql
    done
    return2=$?

    if [ $return1 -eq 0 ] && [ $return2 -eq 0 ]; then
    logger -p local0.info -t MYSQLBACKUP MySQL Backup successful
    else
    logger -p local0.err -t MYSQLBACKUP MySQL Backup failed
    fi




Ausschlaggebender Teil ist die for-Schleife. Für jede Zeile Output von "show databases" wird ein seperates .sql File erstellt. Versehen mit Datum und DB-Name. Nebenbei wird auch noch via logger in /var/log/syslog geloggt.