# Funktionen



Wie in jeder anderen Programmierumgebung, besteht auch in R die entscheidende Fähigkeit darin eigene Funktionen definieren zu können. Eine der wichtigsten Erkenntnisse dabei ist die Tatsache, dass auch Funktionen Objekte sind. Generell ist in R alles ein Objekt. Neue Funktionen werden nun mit der Funktion `function()` erzeugt. 



## Bestandteile von Funktionen

Alle R Funktionen bestehen aus den drei Teilen:

* `formals()`, Argumente, die den Funktionsaufruf "regeln"
* `body()`, der eigentliche Code
* `environment()`, Verweis auf die Funktionseingabe


```r
> f <- function(x) x + sin(x)
> formals(f)
```

```
## $x
```

```r
> body(f)
```

```
## x + sin(x)
```

```r
> environment(f)
```

```
## <environment: R_GlobalEnv>
```

## `body()`

Wie alle Objekte besitzen Funktionen Attribute (standardmäßig: `srcref` - source code).


```r
> attributes(f)
```

```
## $srcref
## function(x) x + sin(x)
```

Den Unterschied zu `body()` sehen wir im folgenden Beispiel.


```r
> f <- function(x){
+   # Summe von x und sin(x)
+   x + sin(x)
+ }
> body(f)
```

```
## {
##     x + sin(x)
## }
```

```r
> attr(f, "srcref")
```

```
## function(x){
##   # Summe von x und sin(x)
##   x + sin(x)
## }
```

## "Einfache" Funktionen

Es gibt eine Ausnahme von der obigen Regel, dass Funktionen aus drei Teilen bestehen. Die primitive functions wie `sum(), sin()` oder `exp()` rufen direkt über die Funktion `.Primitive()` C Code auf. Sie enthalten somit keinen R Code und daher sind alle drei Teile gleich `NULL`.


```r
> formals(sin)
```

```
## NULL
```

```r
> body(sin)
```

```
## NULL
```

```r
> environment(sin)
```

```
## NULL
```


## Jede Operation ist ein Funktionsaufruf

>    “To understand computations in R, two slogans are helpful:

>       Everything that exists is an object.
>       Everything that happens is a function call."

>    — John Chambers


Alle Operationen bestehen aus Funktionsaufrufen. Also auch `+, -, /,:, for, if, while` oder `[]`und `$`. Dies sieht man an diesem Beispiel


```r
> x <- 10; y <- 5
> x + y
```

```
## [1] 15
```

```r
> `+`(x, y)
```

```
## [1] 15
```

und auch diesen Beispielen


```r
> for (i in 1:2) print(i)
```

```
## [1] 1
## [1] 2
```

```r
> `for`(i, 1:2, print(i))
```

```
## [1] 1
## [1] 2
```

```r
> x[3]
```

```
## [1] NA
```

```r
> `[`(x, 3)
```

```
## [1] NA
```
Die Verwendung von `' `  erlaubt hierbei den Zugriff auf reservierte Ausdrücke.

Eine sinnvolle Anwendung dieser Zugriffsmethode liegt in der Kombination mit den apply Funktionen (`apply(), lapply(), sapply, mapply(), tapply()`, siehe auch [Kapitel 5](Konstrukte_VP.html)). Diese wenden eine zu übergebende Funktion auf weitere Inputgrößen wie Listen oder Data Frames an.


```r
> x <- list(1:3, 4:9, 10:12)
> sapply(x, `[`, 2)
```

```
## [1]  2  5 11
```

```r
> sapply(x, function(x) x[2])
```

```
## [1]  2  5 11
```


## lapply, sapply

`sapply()` ist eine Version von `lapply()`, die - wenn möglich - eine "vereinfachte" Ausgabe liefert.


```r
> x <- list(1:3, 4:6, 7:9)
> lapply(x, `+`, 3)
```

```
## [[1]]
## [1] 4 5 6
## 
## [[2]]
## [1] 7 8 9
## 
## [[3]]
## [1] 10 11 12
```

```r
> is.matrix(sapply(x, `+`, 3))
```

```
## [1] TRUE
```


## Funktionsargumente

Funktionen besitzen formelle Argumente, denen beim Funktions- aufruf (oder per Default) Werte zugewiesen werden. Beim Aufruf können die Argumente durch ihre Position, den vollen oder teilweisen Namen ausgewählt werden.


```r
> f <- function(abc, bcd, bdc) c(abc, bcd, bdc)
> f(3, 2, 1)
```

```
## [1] 3 2 1
```

```r
> f(2, 1, abc=3)
```

```
## [1] 3 2 1
```

```r
> f(2, 1, a=3)
```

```
## [1] 3 2 1
```


## Funktionsargumente

Der folgende Aufruf kann aber nicht funktionieren.


```r
> f(3, b=2, 1)
```

```
## Error in f(3, b = 2, 1): Argument 2 passt auf mehrere formale Argumente
```

Generell sollte man die zwei, drei wichtigsten Inputgrößen an den Beginn stellen. Alle anderen sollten eher nicht durch die Position ausgewählt werden. Dabei ist der vollständige Namen sicherlich zu bevorzugen.


## Funktionsargumente

Funktionsargumenten können Default-Werte zugeordnet werden. Dabei können Default-Werte auch in Abhängigkeit anderer Argumente definiert werden.


```r
> f <- function(a = 1, b = 2) {
+   c(a, b)
+ }
> f()
```

```
## [1] 1 2
```

```r
> g <- function(a = 1, b = a * 2) {
+   c(a, b)
+ }
> g(a=10)
```

```
## [1] 10 20
```

## Funktionsargumente: Lazy Evaluation

R wertet Funktionsargumente nur aus, wenn diese tatsächlich benützt werden. Dies entspricht der Idee der *Lazy Evaluation*.


```r
> f <- function(x) {
+   10
+ }
> f(stop("x ist nicht 10"))
```

```
## [1] 10
```
Mit `force()` kann man die Auswertung eines Arguments *erzwingen*.

```r
> f <- function(x) {
+   force(x)
+   10
+ }
> f(stop("x ist nicht 10"))
```

```
## Error in force(x): x ist nicht 10
```


## Funktionsargumente: Lazy Evaluation

Benutzt man eine verschachtelte Funktion in Kombination mit `lapply()` oder einer Schleife, so kann die Lazy Evaluation entscheidend sein.


```r
> addiere_x <- function(x){
+   function(y) x+y
+ }
> addiere_zu_1bis10 <- lapply(1:10, addiere_x)
> addiere_zu_1bis10[[1]](10)
```

```
## [1] 11
```

```r
> addiere_zu_1bis10[[5]](10)
```

```
## [1] 15
```
`x` wird beim ersten Aufruf von `addiere_zu_1bis10()` ausgewertet. Bis dahin ist `x` aber bereits von 1 bis 10 "gelaufen" und hat damit den Wert 10. Somit wird stets 10 zum Input addiert.

## Funktionsargumente: Lazy Evaluation


```r
> addiere_x <- function(x){
+   force(x)
+   function(y) x+y
+ }
> addiere_zu_1bis10 <- lapply(1:10, addiere_x)
> addiere_zu_1bis10[[1]](10)
```

```
## [1] 11
```

```r
> addiere_zu_1bis10[[5]](10)
```

```
## [1] 15
```

Erzwingt man die Auswertung, so erhält man das gewünschte Ergebnis.

## Funktionsargumente: Lazy Evaluation

Bisher schien die Lazy Evaluation eher nachteilig. Sie kann aber auch Vorteile haben. Die Befehle


```r
> x <- NULL
> if (!is.null(x) && x > 0) {
+ 
+ }
```

erzeugen keinen Fehler, obwohl `NULL>0` keinen zulässigen Input für `if` darstellt. Da `!is.null(x)` bereits `FALSE` liefert, wird die zweite Abfrage nicht mehr ausgewertet.

> Hauptvorteil ist aber die Effizienz. Nur notwendige Ausdrücke werden ausgewertet.

## Funktionsargumente: `...`

Für eine Funktion kann das formale `...` Drei-Punkte Argument verwendet werden. Alle nicht über Position und/oder Name zugeordneten Argumente werden durch `...` aufgenommen. In der Regel werden über `...` Argumente übergeben, die innerhalb der Funktion an andere Funktionen weitergegeben werden.


```r
> summe_plus2 <- function(x, ...){
+   x <- x+2
+   sum(x, ...)
+ }
> summe_plus2(c(1, 3, 5))
```

```
## [1] 15
```

```r
> summe_plus2(c(1, 3, NA), na.rm = TRUE)
```

```
## [1] 8
```


## Funktionsargumente: `...`

`...` erhöht also offensichtlich die Flexibilität. Ein Nachteil ist, dass falsch geschriebene Argumente keinen Fehler erzeugen und alle Argumente nach `...` voll ausgeschrieben werden müssen.


```r
> sum(1, 2, NA, rm.na = TRUE)
```

```
## [1] NA
```

```r
> sum(1, 2, NA, na.rm = TRUE)
```

```
## [1] 3
```


## Funktionsrückgabe

Wir haben in den Beispielen bereits gesehen wie die Rückgabe funktioniert. Der letzte Ausdruck, der ausgewertet wird, bildet die Rückgabe. 


```r
> f <- function(x) {
+   if (x < 10) {
+     0
+   } else {
+     10
+   }
+ }
> f(5)
```

```
## [1] 0
```

```r
> f(15)
```

```
## [1] 10
```

## Funktionsrückgabe

Falls bereits zu einem früheren Zeitpunkte eine Rückgabe erfolgen soll (z.B. STOP Kriterium erfüllt) und diese speziell gekennzeichnet werden soll, kann die Funktion `return()` verwendet werden.


Funktionen können nur ein Objekt zurückgeben. Soll die Ausgabe einer Funktion aus mehreren Objekten bestehen, so schreibt man diese einfach in eine Liste und gibt die Liste zurück.

## Fragen

Fragen zum Inhalt dieser Folien oder zum aktuellen Übungsblatt können im [Diskussionsforum](https://www.moodle.tum.de/mod/forum/view.php?id=238250) gestellt werden.

