---
date: 2011-10-02T23:58:50+02:00
layout: post
slug: statistiken-einfache-graphen-mit-r-und-mysql-anbindung
status: publish
comments: true
title: Statistiken | Einfache Graphen mit R und MySQL Anbindung
alias: /archives/1780
categories:
- Bash
- Coding
- Debian
- General
- git
- Linux
- SQL
- Ubuntu
- ubuntuusers
- Web
- R
tags:
- analyse
- anbindung
- einfach
- graphen
- mysql
- R
- sql
- statistic
- Statistik
- stats
- zombies
- zre
- zufall
---

[Immer mal wieder](/archives/1458) reizt mich die Programmiersprache für
Statistiken [R](http://www.r-project.org/). Um diesen Reiz dann auszuleben hab
ich vor ein paar Monaten angefangen kleine Graphen für den zufallsbasierten
Simulator [ZRE](http://zombies.n0q.org) zu bauen. Das Spiel "läuft" einfach 24/7
und schreibt für jedes geschehene Event Einträge in die Datenbank. Diese
Einträge werte ich dann mit Hilfe von R aus.

Dazu gibt es ein Skript. Nämlich [zre.R](https://gist.github.com/1031260) (Ob
das die Konvention bei R-Skriptnamen ist, kann ich nicht sagen ;) )

``` r
#!/usr/bin/env Rscript

# MySQL
library(RMySQL)
con <- dbConnect(MySQL(), user="", password="", host="", client.flag=CLIENT_MULTI_RESULTS)
# Style
zre_colors <- colors()[grep("green",colors())]
zre_mint <- colors()[c(48,86,50)]
```


Im Klartext wird aus dem [CRAN Library Verzeichnis](http://cran.r-project.org)
die Library RMySQL includiert und die Verbindung in der Variable **con**
abgelegt. Ähnlich wie bei PHP. Für alle Debian / Ubuntu Benutzer empfiehlt sich
aber, die Library einfach über das Paketsystem nachzuinstallieren.

``` bash
$ aptitude install r-cran-rmysql
```

Standardmäßig sehen Graphen die mit R erstellt werden ziemlich mau aus. Die
weiteren Variablen unter Style habe ich gewählt um mir die Colorierung der
Graphen etwas zu erleichtern. Diese werden später einfach als Attribute in den
Plots/Barplots gesetzt und ausgewertet. Ich fange einfach mal der Reihe nach an:

Die Abfolge ist immer ziemlich ähnlich. Zu aller Erst wird der Query für die
Datenbank an die Variable **sql** übergeben. Diese Variable wiederrum wird
zusammen mit der Connection an die Funktion **dbGetQuery** übergeben und das
Ergebnis dessen schliesslich in **zre_wins** gespeichert. Anschliessend ein paar
kleine Informationen an das Dateiformat übergeben und den Graphen bauen.

Die Funktion par lässt sich erstmal als eine Art Environment Funktion für
Graphen verstehen. Hier werden Eigenschaften wie Schriftfarbe, Hintergrund,
Axenfarbe, und Liniendicke definiert. Danach kommt (wie ich finde) der
schwierigste Teil. Bauen des Graphen. Je nach Art des Graphen (Balken, Linien,
Torte u.ä.) werden logischerweise verschiedene Parameter erwartet. Die Daten
werden hierbei jetzt als Matrix an ein Balkendiagram übergeben. Weitere
Informationen wie die Überschrift (main) und die Beschreibung der Balken
(names.arg) werden einfach angefügt.



``` r
sql <- paste("SELECT COUNT(id) AS sum, side FROM zombies.zre_wins GROUP BY side;")
zre_wins <- dbGetQuery(con, sql)
png(file="wins.png", width=400, height=400)
par(col="white", bg="transparent", col.axis="white", col.lab="white", col.main="white", lwd=2)
barplot(as.matrix(zre_wins$sum), main="Game Summary", names.arg=c(zre_wins$side), beside=TRUE, col=zre_mint)
```

{{< figure src="/uploads/2011/10/gamesummary.png" >}}

Selbes Spiel wieder, nur mit mehr Balken und anderem Use-Case. Diesmal werden
die 25 Konflikte mit den meisten Opfern visualisiert.


``` r
sql <- paste("SELECT id, kills FROM zombies.zre_kills ORDER BY kills DESC LIMIT 25;")
zre_matches_highest <- dbGetQuery(con,sql)
png(file="highestkills.png", width = 400, height = 400, bg="transparent")
par(col="white", bg="transparent", col.axis="white", col.lab="white", col.main="white", lwd=4)
barplot(zre_matches_highest$kills, zre_matches_highest$id, main="25 Highest Kills", beside = TRUE, ylab="Kills", col=zre_colors)
```

{{< figure src="/uploads/2011/10/50highest.png" >}}

Aber da Balkendiagramme auch irgendwann Langweilig werden geht das natürlich auch anders. Die 25 letzten Konflikte werden im "Opferverlauf" wie folgt dargestellt:


``` r
sql <- paste("SELECT kills FROM zombies.zre_kills ORDER BY id DESC limit 25;")
zre_kills <- dbGetQuery(con,sql)
yrange <- range(zre_kills$kills)
xrange <- length(zre_kills$kills)
png(file="kills.png", width=400, height=400, bg="transparent")
par(col="white", bg="transparent", col.axis="white", col.lab="white", col.main="white", lwd=4)
plot(zre_kills$kills, xlab="Games", type="b", ylab="Kills", main="Kills from last 25 Attacks", col=zre_mint)
```


{{< figure src="/uploads/2011/10/25kills.png" >}}

Damit es nicht immer nur um Tote geht, auch mal was erfreuliches. Die Geburtenrate in ZRE steigt! :)


``` r
sql <- paste("SELECT Month(date) AS month, count(id) AS born FROM (SELECT *, Month(date) AS M FROM zombies.zre_born) t Group by M; ")
zre_birthrate <- dbGetQuery(con,sql)
png(file="birthrate.png", width=400, height=400)
par(col="white", bg="transparent", col.axis="white", col.lab="white", col.main="white", lwd=3)
barplot(zre_birthrate$born, xlab="Month", ylab="Born Humans/Zombies", names.arg=c(zre_birthrate$month),main="BirthRate per Month", col=zre_colors)
```


{{< figure src="/uploads/2011/10/birthrate.png" >}}

Und auch das Wetter soll bei der ganzen Sache nicht zu kurz kommen. Hierbei
bitte besonderes Augenmerk auf die Legende rechts oben. Eine direkte Zuordnung
der Werte und Farben ist nicht nötig, da die Farben in der selben Reihenfolge
von zre_colors befüllt werden wie die Balken. Die erschreckend hohe Zahl an
Naturkatastrophen erklärt das aber trotzdem nicht :)


``` r
sql <- paste("SELECT COUNT(id) AS count, weather FROM zombies.zre_weather GROUP BY weather ORDER BY count DESC;")
zre_weather <- dbGetQuery(con,sql)
png(file="weather.png", width=400, height=400)
par(col="white", bg="transparent", col.axis="white", col.lab="white", col.main="white", lwd=2)
barplot(zre_weather[,1], main="Weather in ZRE", beside = TRUE, col=zre_colors)
legend( 5, 40000, zre_weather$weather, cex=0.9, fill=zre_colors, col="white")
```


{{< figure src="/uploads/2011/10/weather.png" >}}

Das volle zre.R Skript befindet sich wie das meiste auf Github: [https://gist.github.com/1031260](https://gist.github.com/noqqe/1031260)
