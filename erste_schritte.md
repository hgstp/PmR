# Erste Schritte in R

## Kommandozeile

Die Kommunikation mit R erfolgt typischerweise über die Kommandozeile.

-  Befehle werden am ''Prompt'' `>` eingegeben
-  Fortsetzungszeilen werden mit einem `+` gekennzeichnet
-  ENTER sendet den Befehl an den Interpreter
-  Zuweisungen erfolgen über `<-` oder `=`


```r
x <- 2 + 2
x
```

```
## [1] 4
```


## Workspace - der Arbeitsplatz

Alle Objekte (in R ist alles ein Objekt) die während eine R Sitzung erzeugt werden landen im Workspace.


-  `ls()` zeigt Objekte im aktuellen Workspace
-  `rm(Objektname)` löscht `Objektname` aus dem aktuellen Workspace
-  Das aktuelle Arbeitsverzeichnis erhält man durch `getwd()` und ändert man durch `setwd()`



```r
getwd()
```

```
## [1] "C:/Users/haug/Documents/GitHub/pmr"
```



```r
 setwd("..")
 getwd()
```

```
## [1] "C:/Users/haug/Documents/GitHub"
```


## Workspace - der Arbeitsplatz


Beenden von R erfolgt durch `q()`. Dabei kann man
den Workspace samt Befehlen speichern. Die eingegebenen Befehle werden in einer Datei `.Rhistory` und der Workspace in `.Rdata` gespeichert. 

`.Rdata` Dateien können mit `load()` in der nächsten Sitzung neu geladen werden. Auf die History kann mit `loadhistory()` wieder zugegriffen werden.


## R als Taschenrechner
Die Notation für die vier Grundrechenarten lautet in R:
`+, -, *` und `/`. Potenzen berechnet man mit `^`.



```r
2 + 2
```

```
## [1] 4
```

```r
3^2
```

```
## [1] 9
```

```r
(1 - 2) * 4
```

```
## [1] -4
```

```r
1 - 2 * 4
```

```
## [1] -7
```

## R als Taschenrechner
Es existieren eine Vielzahl von mathematischen und statistischen Funktionen in R. Funktionen werden aufgerufen durch ihren 
Namen gefolgt von runden Klammern. Funktionsargumente können
innerhalb der Klammern übergeben werden.



```r
sqrt(2)
```

```
## [1] 1.414214
```

```r
cos(pi)
```

```
## [1] -1
```

```r
sin(pi)
```

```
## [1] 1.224606e-16
```

## R als Taschenrechner


```r
exp(1)
```

```
## [1] 2.718282
```

```r
log(10)
```

```
## [1] 2.302585
```


## R als Taschenrechner
Vielen Funktionen in R kann man mehrere Argumente übergeben,
z.B.: 


```r
log(10, 10)
```

```
## [1] 1
```

```r
log(10, base = 10)
```

```
## [1] 1
```

```r
log
```

```
## function (x, base = exp(1))  .Primitive("log")
```



```r
log(10, base = exp(1))
```

```
## [1] 2.302585
```

## R als Taschenrechner
Verwendet man Funktionen nicht korrekt, so erhält Fehlermeldungen bzw. Warnungen, wie z.B.: 


```r
squareroot( 2 )
```

```
## Error in eval(expr, envir, enclos): konnte Funktion "squareroot" nicht finden
```

```r
sqrt( -2 )
```

```
## Warning in sqrt(-2): NaNs wurden erzeugt
```

```
## [1] NaN
```

## Hilfe

Informationen über die korrekte Verwendung von Funktionen findet man in der Hilfeseite zur jeweiligen Funktion. Diese wird aufgerufen mit dem Befehl `help(Funktionsname)` oder `?Funktionsname`, also z.B.


```r
?sqrt
help(log)
```

Sollte die vorhandene Dokumentation (die Beispiele sind meistens sehr hilfreich) nicht ausreichend sein, so

* schlägt man in einem evtl. vorhanden Buch nach
* sucht man im Netz (Google oder auf Seiten wie http://stackoverflow.com/tags/r/info). Aufgrund der großen Verbreitung von R, wird man (fast) immer fündig.


## Pakete

R ist durch eine Vielzahl von Paketen (5881 auf CRAN: Stand 25.09.2014) "erweiterbar". Bei einem Paket handelt es sich um

> Eine strukturierte, standardisierte Einheit. Diese besteht aus R
Code, Dokumentation, Daten, externem Quellcode.

Pakete werden mit 

```r
library("Paketname")
```
geladen und mit `detach()` entladen. Hilfe zu Paketen gibt es mit
`library(help = "Paketname")`. Man findet (fast) alle statistischen Methoden implementiert in einem R Paket.

Standardmäßig werden die Pakete `base, datasets, graphics, grDevices, methods, stats` und `utils` beim Starten geladen. Alle weiteren Pakete müssen per Befehl geladen werden.

## Pakete

Neben `base` enthält die R Basisdistribution noch die Pakete

`compiler, datasets, graphics, grDevices, grid, methods, parallel, splines, stats, stats4, tcltk, tools, util` (Standard-Pakete),

`boot, cluster, codetools, foreign, KernSmooth, lattice, Matrix, mgcv, nlme, rpart, survival, class, MASS, nnet` und `spatial` (empfohlene Pakete).

Alle weiteren Pakete müssen mit dem Befehl

```r
install.packages("Paketname")
```
installiert werden (automatisches Herunterladen der aktuellsten Version von CRAN und anschließendes Installieren).

## Pakete

Installierte Pakete liegen in einer Bibliothek. Man kann mit mehreren Bibliotheken arbeiten. In der Regel verwendet man zwei. Der Befehl


```r
.libPaths()
```

```
## [1] "C:/Users/haug/Documents/R/win-library/3.2"
## [2] "C:/Program Files/R/R-3.2.2/library"
```
zeigt die vorhandenen Bibliotheken. Standardmäßig werden neue Pakete in die erste Bibliothek von `.libPaths()` installiert. Will man in eine andere Bibliothek installieren, so verwendet man

```r
install.packages("Paketname", 
                 lib = "Pfad zur Bibliothek")
```

Mit `update.packages()` werden neue Versionen installiert.


## Hausaufgaben

Jede Woche ist ein Übungsblatt zu bearbeiten. Zum Bestehen des Moduls ist eine *sinnvolle Bearbeitung* (ausgedrückt durch einen variierenden Anteil der pro Aufgabe zu erreichenden Punkte) von

> 75% der Aufgaben

notwendig.

Die Abgabe der Aufgaben erfolgt einzeln im Moodle-Kurs. Wir wollen Sie aber dazu auffordern die Aufgaben in der Gruppe zu besprechen/bearbeiten. Wir erwarten aber auch einen **eigen- ständigen** Beitrag.

## Abgabe: Hausaufgaben

Die Abgabe **muss** im pdf Dateiformat erfolgen. Dieses pdf erstellen Sie indem Sie in RStudio 

- eine R Markdown Datei erstellen
- als Default Ausgabe der R Markdown Datei das pdf Format wählen

Dazu benötigen Sie das Paket `knitr` (siehe auch erste Folie) und eine [TeX Installation](http://www.ma.tum.de/Studienbeitraege/LaTeX) (MiKTeX, MacTeX, TeX Live).

**Hinweis:** Ihre Lösung muss *immer* auch den Output Ihrer Befehle enthalten. Die reine Angabe von Befehlen ist *nicht ausreichend*.
