# Programmieren mit R: Indizierung
Stephan Haug  
(Programmierpraktikum MA8505)   



Zur Indizierung von Objekten stehen drei Operatoren zur Verfügung, `[, [[` und `$`. Alle besitzen unterschiedliche Eigenschaften (Einsatzmöglichkeiten) und können sich unterschiedlich verhalten je nach Datenstruktur.

Man unterscheidet sechs verschiedene Indizierungstypen. Indizierung über

 * positive Integervektoren
 * negative Integervektoren
 * logische Vektoren
 * character Vektoren
 * nichts
 * Null

## `[`: Vektoren

Beginnen wollen wir mit dem Operator `[`. Am einfachsten ist die Indizierung atomare Vektoren zu verstehen. Daher beginnen wir mit der Indizierung von Vektoren über positive Integers.


```r
> x <- c(1, 4, 3, 5)
> x[c(2, 4)]
```

```
## [1] 4 5
```

```r
> order(x) # gibt die Rangfolge an
```

```
## [1] 1 3 2 4
```

```r
> x[order(x)]
```

```
## [1] 1 3 4 5
```

## `[`: Vektoren

Im Gegensatz zur Indizierung über positive Integers, werden bei der Indizierung mit negativen Integers die ausgewählten Position entfernt.


```r
> x[-3]
```

```
## [1] 1 4 5
```

```r
> x[-(4:2)]
```

```
## [1] 1
```

## `[`: Vektoren

Bei der Indizierung über logische Vektoren, werden alle Elemente des Vektors ausgewählt deren zugehöriger logischer Wert gleich `TRUE` ist.


```r
> x[c(TRUE, FALSE, FALSE, TRUE)]
```

```
## [1] 1 5
```

```r
> x > 2
```

```
## [1] FALSE  TRUE  TRUE  TRUE
```

```r
> x[x > 2]
```

```
## [1] 4 3 5
```


## `[`: Vektoren

Man beachte

```r
> x[c(TRUE, FALSE)]
```

```
## [1] 1 3
```
Der logische Vektor wird zyklisch fortgesetzt bis er der Länge des zu indizierenden Vektors entspricht. Der obige Befehl entspricht also


```r
> x[c(TRUE, FALSE, TRUE, FALSE)]
```

```
## [1] 1 3
```


## `[`: Vektoren

Besitzt der zu indizierende Vektor das Attribute `names`, so kann auch über dieses auf die Elemente des Vektors zugegriffen werden.


```r
> y <- setNames(x, letters[1:4])
> y[c("d", "a", "a")]
```

```
## d a a 
## 5 1 1
```

```r
> y["e"]
```

```
## <NA> 
##   NA
```

## `[`: Vektoren

Gibt man nichts zur Indizierung ein, so erhält man wieder den kompletten Vektor. Im Gegensatz dazu liefert die Null auch einen Vektor der Länge Null.


```r
> x[]
```

```
## [1] 1 4 3 5
```

```r
> x[0]
```

```
## numeric(0)
```

## `[`: Listen

Für Listen arbeitet der Operator `[` ähnlich wie für Vektoren. Er liefert die einzelnen Elemente einer Liste, die jeweils wieder vom Typ List sind.


```r
> x <- list( a = c(1,3), b = c("eins", "drei"))
> x[1]
```

```
## $a
## [1] 1 3
```

```r
> str(x[1])
```

```
## List of 1
##  $ a: num [1:2] 1 3
```

## `[`: Matrizen und Arrays

Höherdimensionale Strukturen können über

* mehrere Vektoren (einer pro Dimension)
* einen Vektor
* eine Matrix

indiziert werden. Indiziert man über mehrere Vektoren, so kann einer (oder mehrere) gleich "nichts" gesetzt werden. Die entsprechende Komponente wird dann vollständig ausgewählt.


## `[`: Matrizen und Arrays



```r
> a <- matrix(1:9, nrow = 3)
> colnames(a) <- c("Sp1", "Sp2", "Sp3")
> a[1:2,]  # die beiden ersten Zeilen aller Spalten
```

```
##      Sp1 Sp2 Sp3
## [1,]   1   4   7
## [2,]   2   5   8
```

```r
> a[c(TRUE, FALSE), c("Sp2", "Sp3")]
```

```
##      Sp2 Sp3
## [1,]   4   7
## [2,]   6   9
```

```r
> a[3, -2]
```

```
## Sp1 Sp3 
##   3   9
```

## `[`: Matrizen und Arrays

Matrizen und Arrays sind als Vektoren implementiert. Daher kann auch nur über einen Vektor auf die entsprechenden Elemente zugegriffen werden.

Die Objekte sind dabei spaltenweise "abgelegt". Um also auf die letzte Zeile der $3\times 3$ Matrix `a` zuzugreifen, gehen wir wie folgt vor: 


```r
> a[seq(from = 3, to = 9, length = 3)]
```

```
## [1] 3 6 9
```


## `[`: Matrizen und Arrays

Der Zugriff kann auch über eine integer oder character Matrix erfolgen. Jede Zeile bestimmt dabei eine Position, d.h. jeder Spalteneintrag definiert eine Dimension.


```r
> (b <- outer(1:5, 1:5, FUN = "paste", sep = ",")) 
```

```
##      [,1]  [,2]  [,3]  [,4]  [,5] 
## [1,] "1,1" "1,2" "1,3" "1,4" "1,5"
## [2,] "2,1" "2,2" "2,3" "2,4" "2,5"
## [3,] "3,1" "3,2" "3,3" "3,4" "3,5"
## [4,] "4,1" "4,2" "4,3" "4,4" "4,5"
## [5,] "5,1" "5,2" "5,3" "5,4" "5,5"
```

```r
> # äußeres Produkt der Funktion paste
> b[ matrix( ncol = 2, byrow = TRUE, c(1, 1, 3, 1, 2, 4))]
```

```
## [1] "1,1" "3,1" "2,4"
```

## `[`: Data Frames

Data Frames zeigen ein unterschiedliches Verhalten, je nachdem ob man mit einem oder mehreren Vektoren die Indizierung durchführt.


```r
> d <- data.frame( x = 1:3, y = 3:1, z = letters[1:3])
> d[2,]
```

```
##   x y z
## 2 2 2 b
```

```r
> d[,c(1, 3)]
```

```
##   x z
## 1 1 a
## 2 2 b
## 3 3 c
```


## `[`: Data Frames


```r
> d[c("x", "y")]  # wird eine Liste
```

```
##   x y
## 1 1 3
## 2 2 2
## 3 3 1
```

```r
> d[, c("x", "y")] # wird eine Matrix
```

```
##   x y
## 1 1 3
## 2 2 2
## 3 3 1
```

## `[`: Data Frames


Greift man nur auf eine Spalte/Zeile zu, so wird daraus ein Data Frame bzw. ein Vektor.


```r
> str(d["x"])
```

```
## 'data.frame':	3 obs. of  1 variable:
##  $ x: int  1 2 3
```

```r
> str(d[, "x"])
```

```
##  int [1:3] 1 2 3
```


## Weitere Operatoren

Zur Indizierung existieren noch die Operatoren `[[` und `$`. `[[` liefert im Unterschied zu `[` immer nur eine Komponente des zu indizierenden Objekts. Daher wird `[[` auch nur mit einem positiven Integer oder einem character Ausdruck verwendet. 


```r
> a <- list( eins = 1, zwei = c(2, 3))
> a[[1]]
```

```
## [1] 1
```

```r
> a[["zwei"]]
```

```
## [1] 2 3
```

## Weitere Operatoren

Data Frames sind Listen von Spaltenvektoren. Daher kann man mit `[[` auf die Spalten eines Data Frames zugreifen indem man den Index der jeweiligen Spalte angibt.


```r
> d
```

```
##   x y z
## 1 1 3 a
## 2 2 2 b
## 3 3 1 c
```

```r
> d[[2]]
```

```
## [1] 3 2 1
```

## Weitere Operatoren

Der `$` Operator erlaubt folgende verkürzte Schreibweise `x$y`,
die äquivalent ist zum Befehl `x[["y", exact = FALSE]]`


```r
> l <- list( eins = 1, zwei = c(1, 2))
> l[["z"]]
```

```
## NULL
```

```r
> l$z
```

```
## [1] 1 2
```


## Vereinfachendes und erhaltendes Indizieren

Vereinfachendes Indizieren wählt die einfachste Datenstruktur, die das Ergebnis noch darstellen kann.

Beim erhaltenden Indizieren haben Ein- und Ausgabe die gleiche Struktur.

Die Befehle zum vereinfachenden und erhaltenden Indizieren variieren ja nach Datenstruktur.

## Vereinfachendes und erhaltendes Indizieren

Struktur | vereinfachend | erhaltend
---------|---------------|----------
Vektor | `x[[1]]` | `x[1]`
Liste | `x[[1]]` | `x[1]`
Faktor | `x[1:4, drop = TRUE]` | `x[1:4]`
Array | `x[1,]`  | `x[1, , drop = FALSE]`
Array | `x[,1]` | `x[,1, drop = FALSE]`
Data Frame | `x[,1]` | `x[, 1, drop = FALSE]`  
Data Frame | `x[[1]]` | `x[1]`

## Vereinfachendes und erhaltendes Indizieren

Das vereinfachende Indizieren führt (je nach Struktur der Eingabe) folgende Vereinfachungen durch


```r
> x <- c(a = 1, b = 2)
> x[[1]] # entfernt den Namen
```

```
## [1] 1
```

```r
> l <- list(a = 1, b = 2)
> l[[1]] # liefert den Inhalt des ersten Listenelements
```

```
## [1] 1
```

```r
> str(l[[1]])
```

```
##  num 1
```

## Vereinfachendes und erhaltendes Indizieren


```r
> f <- factor( c("gruppe 1", "gruppe 2"))
> f[1, drop = TRUE] # löscht nicht verwendete levels
```

```
## [1] gruppe 1
## Levels: gruppe 1
```

```r
> m <- matrix(1:4, nrow = 2)
> m[1, ] # löscht die Dimension eindim. Komponenten
```

```
## [1] 1 3
```

```r
> # zum Vergleich
> m[1, , drop = FALSE]
```

```
##      [,1] [,2]
## [1,]    1    3
```

## Vereinfachendes und erhaltendes Indizieren



```r
> d <- data.frame( a = 1:2, b = 1:2)
> str(d[[1]]) # eine Spalte wird zu einem Vektor
```

```
##  int [1:2] 1 2
```

```r
> # zum Vergleich
> str(d[, "a", drop = FALSE])
```

```
## 'data.frame':	2 obs. of  1 variable:
##  $ a: int  1 2
```


## Indizieren und Zuweisen

Mit jeder Indizierung können auch neue Werte den entsprechenden Objekten zugewiesen werden. 


```r
> x <- 1:5
> x[1:2] <- c(2, 7) 
> x
```

```
## [1] 2 7 3 4 5
```

```r
> x[-1] <- 4:1 # Länge RS = Vielfaches Länge LS 
> x[-1] <- 4 # funktioniert ohne Warnung
```


```r
> x[-1] <- 3:1
```

```
## Warning in x[-1] <- 3:1: Anzahl der zu ersetzenden Elemente ist kein
## Vielfaches der Ersetzungslänge
```


## Indizieren und Zuweisen

Komponenten einer Liste können durch eine Zuweisung auch entfernt werden. Hierzu wird mit dem Operator `[[` dem entsprechenden Listenelement NULL zugewiesen. Verwendet man `[` so ist der Inhalt tatsächlich NULL.


```r
> x <- list(a = 1, b = 2)
> x[["b"]] <- NULL
> str(x)
```

```
## List of 1
##  $ a: num 1
```

```r
> y <- list(a = 1)
> y["b"] <- list(NULL)
> str(y)
```

```
## List of 2
##  $ a: num 1
##  $ b: NULL
```


## (Sinnvolle) Anwendungen

Im Folgenden wollen wir ein paar Beispiele zeigen, in denen durch Indizieren (und Zuweisen) speziellere Aufgaben gelöst werden. Einige dieser Aufgaben können auch durch spezialisiertere Funktionen wie `subset(), merge()` oder 
`plyr::arrange()` gelöst werden.


## (Sinnvolle) Anwendungen

*Beispiel 1:*

```r
> noten <- sample( 1:5, 4, replace = TRUE)
> info.noten <- data.frame(
+   note = 1:5, beschr. = c("sehr gut", "gut", "befr.", 
+                           "ausreich.","mangelh."),
+   bestanden = c(rep(TRUE, 4), FALSE))
> rownames(info.noten) <- info.noten$note
> info.noten[ noten, ]
```

```
##     note   beschr. bestanden
## 5      5  mangelh.     FALSE
## 3      3     befr.      TRUE
## 4      4 ausreich.      TRUE
## 5.1    5  mangelh.     FALSE
```

## (Sinnvolle) Anwendungen

*Beispiel 2:* `order()` gibt die Indizes zurück die den Input in die gewünschte (hier gibt es mehrere Möglichkeiten) Ordnung bringen.


```r
> x <- c("b", "c", "a")
> order(x)
```

```
## [1] 3 1 2
```

```r
> x[order(x)]
```

```
## [1] "a" "b" "c"
```

## (Sinnvolle) Anwendungen

Damit können nun leicht die Zeilen oder Spalten höherdimensionaler Objekte sortiert werden.


```r
> df <- data.frame(x = sample(1:6), y = 6:1, 
+                  z = letters[1:6])
> df[order(df$x), ] # sortieren der Zeilen nach Spalte x
```

```
##   x y z
## 6 1 1 f
## 2 2 5 b
## 1 3 6 a
## 5 4 2 e
## 4 5 3 d
## 3 6 4 c
```



## (Sinnvolle) Anwendungen

*Beispiel 3:* Zwei Möglichkeiten Spalten eines Data Frames zu löschen.

```r
> df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
> df$z <- NULL # schon bei Listen (Data Frame ist eine 
> # Liste) gesehen
> df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
> df[c("x", "y")] # Auswahl gewünschter Spalten
```

```
##   x y
## 1 1 3
## 2 2 2
## 3 3 1
```


## (Sinnvolle) Anwendungen

*Beispiel 4:* Indizieren über logische Abfragen.


```r
> mtcars[mtcars$gear == 5, c("mpg","cyl","gear")]
```

```
##                 mpg cyl gear
## Porsche 914-2  26.0   4    5
## Lotus Europa   30.4   4    5
## Ford Pantera L 15.8   8    5
## Ferrari Dino   19.7   6    5
## Maserati Bora  15.0   8    5
```

```r
> mtcars[mtcars$gear == 5 & mtcars$cyl == 4, c("cyl","hp")]
```

```
##               cyl  hp
## Porsche 914-2   4  91
## Lotus Europa    4 113
```

Dies ist die häufigste Methode zur Auswahl spezieller Teile eines Datensatzes.


## (Sinnvolle) Anwendungen

Die vorherige Aufgabe kann auch mit der Funktion `subset()` bearbeitet werden.


```r
> subset(mtcars, gear == 5, 
+        select = c("mpg", "cyl", "gear"))
```

```
##                 mpg cyl gear
## Porsche 914-2  26.0   4    5
## Lotus Europa   30.4   4    5
## Ford Pantera L 15.8   8    5
## Ferrari Dino   19.7   6    5
## Maserati Bora  15.0   8    5
```

```r
> subset(mtcars, gear == 5 & mtcars$cyl == 4, 
+        select = c("cyl","hp"))
```

```
##               cyl  hp
## Porsche 914-2   4  91
## Lotus Europa    4 113
```

## Fragen

Fragen zum Inhalt dieser Folien oder zum aktuellen Übungsblatt können im [Diskussionsforum](https://www.moodle.tum.de/mod/forum/view.php?id=238250) gestellt werden.
