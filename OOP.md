# Objektorientiertes Programmieren



Objektorientiertes Programmieren (OOP) basiert auf den beiden Konzepten "Klasse" und "Methode". Die Klasse eines Objekts definiert das Verhalten von Objekten. Bei der Auswahl einer Methode wird ebenfalls die Klasse berücksichtigt. Methoden können sich unterschiedlich verhalten je nach Klasse.

In R existieren mehrere Systeme zum OOP. Am bekanntesten sind die **S3** und **S4** Systeme. Wir werden uns auf das (einfachere) **S3** System beschränken.


## S3

**S3** ist informell und erlaubt somit das schnelle Erzeugen von **S3** Objekten. Die meisten Objekte sind **S3** Objekte. In der Basisversion gibt es aber keine "einfache" Möglichkeit zur Überprüfung ob es sich um ein **S3** Objekt oder z.B. ein Basisobjekt handelt. Hier bietet sich `pryr::otype()` an



```r
> library(pryr)
> df <- data.frame(x = 1:5, y = letters[1:5])
> otype(df)
```

```
## [1] "S3"
```

```r
> otype(df$x)
```

```
## [1] "base"
```

```r
> is.object(df) & !isS4(df)
```

```
## [1] TRUE
```

## Methoden

In **S3** werden Methoden den jeweiligen Funktionen zugeordnet. Diese werden dann generische Funktionen genannt. Eine generische Funktion erkennt man an `UseMethod()` im Source Code oder mit `pryr::ftype()`.


```r
> mean
```

```
## function (x, ...) 
## UseMethod("mean")
## <bytecode: 0x000000000692ba20>
## <environment: namespace:base>
```

```r
> ftype(mean)
```

```
## [1] "s3"      "generic"
```

Für eine gegebene Klasse bestimmt die generische Funktion die passende **S3** Methode. Deren Name ist von der Form `generic.class()`, wie z.B. `print.factor()`. 

> *Bemerkung:* Daher sollte man `.` nicht im Namen eigener Funktionen verwenden. Dies kann leicht zu Verwirrungen führen. 

`pryr::ftype()` hilft aber auch in solchen Fällen weiter. 


```r
> ftype(t.data.frame)
```

```
## [1] "s3"     "method"
```

```r
> ftype(t.test)
```

```
## [1] "s3"      "generic"
```

Obwohl beide Funktionen aussehen wie eine Methode, ist `t.test()` keine Methode der Funktion `t()` für die (nicht vorhandene) `test` Klasse. `t.data.frame()` ist hingegen die Transponierungs-Methode für Data Frames. 


Alle Methoden einer generischen Funktion erhält man mit `methods()`.


```r
> methods(mean)
```

```
## [1] mean.Date     mean.default  mean.difftime mean.POSIXct  mean.POSIXlt 
## see '?methods' for accessing help and source code
```

```r
> methods(head)
```

```
## [1] head.data.frame* head.default*    head.ftable*     head.function*  
## [5] head.matrix      head.table*     
## see '?methods' for accessing help and source code
```
Die Methoden von `mean()` sind alle sichtbar. `head()` hat hingegen verborgene Methoden. Diese sind mit einem \* gekennzeichnet. Den Source Code einer "verborgenen" Funktion erhält man mit `getS3method()`. 


```r
> head.default
```

```
## Error in eval(expr, envir, enclos): Objekt 'head.default' nicht gefunden
```

```r
> getS3method("head", "default")
```

```
## function (x, n = 6L, ...) 
## {
##     stopifnot(length(n) == 1L)
##     n <- if (n < 0L) 
##         max(length(x) + n, 0L)
##     else min(n, length(x))
##     x[seq_len(n)]
## }
## <bytecode: 0x0000000007a8a618>
## <environment: namespace:utils>
```


Alle generischen Funktionen zu einer speziellen Klasse erhält man ebenfalls mit `methods()`.


```r
> methods(class = "factor")
```

```
##  [1] [             [[            [[<-          [<-           all.equal    
##  [6] as.character  as.data.frame as.Date       as.list       as.logical   
## [11] as.POSIXlt    as.vector     coerce        droplevels    format       
## [16] initialize    is.na<-       length<-      levels<-      Math         
## [21] Ops           plot          print         relevel       relist       
## [26] rep           show          slotsFromS3   summary       Summary      
## [31] xtfrm        
## see '?methods' for accessing help and source code
```


## Klasse definieren

Mit `class()` kann die Klasse eines Objekts abgefragt und auch gesetzt werden. `structure()` erlaubt die Definition eines Objekts mit Klassenangabe in einem Befehl.


```r
> m_obj <- structure(list(x = 1:4, y = c(3,7)), class = "m_klasse")
> m_obj <- list(x = 1:4, y = c(3,7))
> class(m_obj) <- "m_klasse"
> 
> class(m_obj)
```

```
## [1] "m_klasse"
```

`m_obj` "erbt" nun Eigenschaften der Klasse `m_klasse`

```r
> inherits(m_obj, "m_klasse")
```

```
## [1] TRUE
```

Für viele **S3** Klassen existiert eine Funktion zur Konstruktion von Objekten dieser Klasse, wie z.B. `factor(), data.frame(), ...` Daher wollen wir nun auch eine solche Funktion für die Klasse `m_klasse` schreiben. Objekte aus dieser Klasse sollen, wie zuvor gesehen, sind Listen mit numerischen Einträgen. Demzufolge könnte eine solche Funktion die nachfolgende Form haben.


```r
> m_klasse <- function(x, y){
+   if(!is.numeric(x) | !is.numeric(y)) 
+     stop("x und y muessen numerisch sein")
+   structure(list(x, y), class = "m_klasse")
+ }
```




```r
> m_obj <- m_klasse(1:3,"a")
```

```
## Error in m_klasse(1:3, "a"): x und y muessen numerisch sein
```

```r
> m_obj <- m_klasse(x = 1:3, y = c(4,5))
> class(m_obj)
```

```
## [1] "m_klasse"
```

## Neue Methoden und generische Funktionen

Zur Definition einer neuen generischen Funktion benutzt man `UseMethod()`.


```r
> m_generic <- function(x) UseMethod("m_generic")
```

Eine generische Funktion ist nutzlos ohne (mindestens) eine Methode. Daher definieren wir nun für die Klasse `m_klasse` eine Methode zur generischen Funktion `m_generice()`. 


```r
> m_generic.m_klasse <- function(x) x[1]
> ftype(m_generic.m_klasse)
```

```
## [1] "s3"     "method"
```

Die Methode `m_generic.m_klasse()` gibt also für ein Objekt der Klasse `m_klasse` das erste Listenelement zurück.


```r
> m_generic(m_obj)
```

```
## [[1]]
## [1] 1 2 3
```

Ebenso kann man eine neue Methode für eine bereits existierende generische Funktion definieren.


```r
> mean.m_klasse <- function(x) mean(x[[1]])
> mean(m_obj)
```

```
## [1] 2
```


## Methodenauswahl

`UseMethod()` erzeugt einen Vektor von Funktionsnamen vom Typ

```r
> paste0("generic",".",c("klasse","default"))
```

```
## [1] "generic.klasse"  "generic.default"
```
und sucht nach den entsprechenden Funktionen. Für unsere generische Funktion `m_generic()` liegen bisher ja folgende Methoden vor.


```r
> methods(m_generic)
```

```
## [1] m_generic.m_klasse
## see '?methods' for accessing help and source code
```

Es ist also nur eine Methode vorgesehen für die Klasse `m_klasse`. Daher ist klar, dass der Befehl


```r
> m_generic(1:5)
```

```
## Error in UseMethod("m_generic"): nicht anwendbare Methode für 'm_generic' auf Objekt der Klasse "c('integer', 'numeric')" angewendet
```

zu einem Fehler führt. Es sollte daher immer eine Default Methode angegeben werden. 


```r
> m_generic.default <- function(x) "m_generic nicht definiert fuer diese Klasse"
> m_generic(structure(list(1, 2), class = c("d_klasse", "m_klasse")))
```

```
## [[1]]
## [1] 1
```

```r
> m_generic(structure(list(), class = c("e_klasse")))
```

```
## [1] "m_generic nicht definiert fuer diese Klasse"
```


Die nachfolgende Tabelle enthät eine Übersicht wichtiger Funktionen in Bezug auf **S3** Objekte.

Funktion | Beschreibung
---------|-------------
`attribute()`| Erfragen und Setzen aller Attribute eines Objekts
`attr()` | Erfragen und Setzen konkreter Attribute
`class()` | Erfragen und Setzen des Klassen-Attributs
`inherits()` |  Von einer anderen Klasse Merkmale erben
`methods()` |  Alle zu einer generischen Funktion gehörenden Methoden
`NextMethod()` | Verweis auf die nächste in Frage kommende Methode
`UseMethod()` |  Verweis von der generischen Funktion an die Methode

## S4

Das **S4** System arbeitet ähnlich wie **S3**. Es ist aber deutlich formeller gestaltet. Hauptunterschiede:

- eine formellere Klassen-Definition wird verwendet (Slots/Felder der Objekte müssen beschrieben werden)
- die Methodenauswahl kann sich auf mehr als ein Argument beziehen
- der Operator `@` erlaubt den Zugriff auf Slots/Felder eines **S4** Objekts

Weitere Informationen zu **S4** findet man in [*Advanced R: S4*](http://adv-r.had.co.nz/OO-essentials.html#s4) und den dort angegebenen Referenzen.



