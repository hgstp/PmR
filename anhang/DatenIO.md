# Daten IO



Da es sich bei R um eine statistische Programmierumgebung handelt, ist eine grundlegende Eigenschaft Dateien zu importieren und exportieren. Wir betrachten zuerst die Fälle eines Datenimports aus 


* einer Textdatei
* einer Excel-Datei
* einem anderen Statistikprogramm


## Dateninput aus einer Textdatei


Die Daten in der Textdatei `datei` seien tabellenförmig angelegt. Dabei werden verschiedene Möglichkeiten zur Trennung der einzelnen Elemente unterschieden. Es existieren eine Reihe von Funktionen für die verschiedenen Kombinationen aus Trennelement und Dezimaltrennzeichen.


```r
> read.delim(datei,)  # Tabulator getrennt, dec="."
> read.delim(datei,sep="|")  # | getrennt
> read.delim2(datei,)  # Tabulator getrennt, dec=","
> read.csv(datei)  # Komma getrennt, dec="."
> read.csv2(datei) # Strichpunkt getrennt, dec=","
```
Diese Funktionen sind Spezialfälle der Funktion `read.table()`. Jede dieser Funktionen erzeugt ein Objekt der Klasse `data.frame`.


## Excel Dateien

Die einfachste Möglichkeit eine Excel Datei einzulesen besteht darin diese im csv Format zu speichern und dann mit der Funktion `read.csv2()` zu arbeiten, z.B.


```r
> read.csv2(datei,sep=";",dec=".",...)
```
falls man `;` als Trennelement und `.`  als Dezimaltrennzeichen beim der Abspeichern csv Datei gewählt hat. Excel Dateien können aber natürlich auch direkt eingelesen werden, z.B. mit der Funktion `read.xlsx()` aus dem Paket `xlsx`



```r
> library(xlsx)
> xls_file <- read.xlsx("test.xlsx", sheetIndex = 1)
> class(xls_file)
```

```
## [1] "data.frame"
```


## Dateien aus anderen Statistikprogrammen

Das Paket `foreign` enthält eine Reihe von Funktionen zum Einlesen von Output-Dateien anderen Statistik-Programme



```r
> foreign::read.dta   # liest Stata Dateien
> foreign::read.spss  # liest SPSS Dateien
> foreign::read.xport # liest SAS Dateien
```

## Daten speichern


Innerhalb von R werden Daten typischerweise in einem Data Frame abgespeichert. Um nun den Inhalt eines Data Frames in eine Datei zu speichern, verwendet man die Funktion `write.table()` (und ihre Varianten, siehe `?write.table`).


```r
> df <- data.frame(eins = 1:2, zwei = letters[1:2])
> write.csv(df, file = "bsp_daten_speichern.csv", row.names = FALSE)
> read.csv(file = "bsp_daten_speichern.csv")
```

```
##   eins zwei
## 1    1    a
## 2    2    b
```


## R Objekte

Wir wissen bereits, dass wir mit `save()` eine Reihe von R Objekten und mit `save.image()` den kompletten Workspace abspeichern können. Mit `load()` kann dann der Inhalt einer `.Rdata` Datei (Speicherformat von `save()`) neu geladen werden. R Objekte, die mit `save()` abgespeichert werden, behalten beim erneuten Laden ihren ursprünglichen Namen. Mit `saveRDS()` kann zwar nur ein Objekt abgespeichert werden, diesem kann man aber beim erneuten Einlesen mit `readRDS()` einen neuen Namen geben.


```r
> save(df, file = "bsp_save.Rdata")
> rm(df)
> load("bsp_save.Rdata")
> saveRDS(df, file = "bsp_saveRDS.rds")
> df_neu <- readRDS("bsp_saveRDS.rds")
> identical(df, df_neu)
```

```
## [1] TRUE
```

## Weitere Möglichkeiten

Eine Übersicht weiterer Import-/Exportmöglichkeiten findet man z.B. im
[R programming Wikibook](http://en.wikibooks.org/wiki/R_Programming/) unter dem Link ["Importing and Exporting Data"](http://en.wikibooks.org/wiki/R_Programming/Importing_and_exporting_data).

