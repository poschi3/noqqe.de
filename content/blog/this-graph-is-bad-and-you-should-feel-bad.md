---
comments:
- author: noqqe
  content: <p>Huch, da hab ich eine Null vergessen bei den Einwohner zahlen der USA
    :)</p>
  date: '2013-01-26T11:22:41'
date: '2013-01-25T20:02:00'
tags:
- web
- shell
- stats
- usa
- twitter
- mordrate
- google
- internet explorer
- ie
- google charts
- bash
title: This graph is bad and you should feel bad!
---

Vor kurzer Zeit ging ja dieser
[Tweet](http://twitter.com/altonncf/status/293392615225823232) im Netz
herum.  Es ist nicht so das ich mit der Message des Graphs nicht
einverstanden wäre :P oder den Spass nicht verstanden hätte, aber da ich
mir zur Zeit ein paar Bücher über Statistik reinziehe bin ich was Graphen
angeht etwas aufmerksam.

{{< figure src="/uploads/2013/01/original.jpg" >}}

Warum also die Zoidberg'sche Headline für diesen Post?

## Der Nachbau und die Lesbarkeit

Zuerstmal hab ich den Graphen mit Google Charts einfach nachgebaut um zu
sehen, ob ich die Werte abbilden kann. Scheint so. Aber beim Herauslesen
der Zahlen fängts schon an. Durch die krummen Gridlines (800 Morde/15%
Marktanteil) der Achsen sind die Werte relativ bescheiden herauszulesen.
Trotzdem:

## Dramatische Darstellung mit Hilfe der Achsen

An den minimalen und maximalen Werten der beiden Achsen kann man schnell
erkennen was das Ziel war. Größtmögliche Übereinstimmung zwischen den
Variablen schaffen.

Eigentlich ist alles ganz einfach und üblich. Der durschnittliche
Vertriebler möchte seine Umsätze natürlich nur realtiv zueinander
darstellen, da Unterschiede so wesentlich größer aussehen (dramatisiert
sind). A la "Umsatzsteigerung over 9000, bin ich geil oder was?" Gleiches
ist mit den Morden hier passiert.

Zum MarketShare des IE brauch ich wohl nicht viel sagen.  Welcher normale
Mensch stellt prozentuale Values von 10%-90% dar?

Wenn ich beide Achsen von 0 bis $max gehen lasse, sieht die Geschichte
gleich ganz anders aus.

## Äpfel und Birnen vergleichen

Prozentuale Werte gegen absolute Zahlen vergleichen. Unschön. Wenn ich
schon zwei Ereignisse korreliert haben möchte, dann wenigstens in der
gleichen Maßeinheit.  Da ich jetzt nicht sagen kann wie viele Browser 46%
entsprechen würde ich wohl eher die Anzahl der Morde wählen.

Bei Morden ist es auch nochmal schwierig die richtige Population zu wählen.
Also die Grundgesamtheit wie das in deutsch so schön heisst. Am nähesten
dran käme die Anzahl der Einwohner in den USA. Wie viele US-Amerikaner
wurden 2010 eigentlich nicht ermordet? Seltsame Null-Hypothese.

``` bash
for x in 17500 17250 16900 16400 15550 15250 ; do echo "$x/30000000" | bc -l ; done
.00058333333333333333
.00057500000000000000
.00056333333333333333
.00054666666666666666
.00051833333333333333
.00050833333333333333
```

Es macht auch nicht so umbedingt Sinn hier jetzt Promille Werte zu vercharten.
Gute Graphen machen bleibt eben schwierig.

Abgesehen davon weichen die Werte von [denen hier](http://projects.wsj.com/murderdata/#view=all)
sowieso total ab. Was solls. Für Graphen bin ich immer zu haben. Gelacht hab ich
jedenfalls :)
