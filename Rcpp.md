# Rcpp



## Einbinden von "externem Code"

In R ist es möglich beispielsweise C, C++, Fortran oder Matlab Code einzubinden. Dazu gibt es verschiedene Möglichkeiten. Wir werden uns das  Paket `Rcpp` etwas genauer anschauen. Dieses ermöglicht das Einbinden von C++ Code auf relative einfache Weise. Daher installieren wir das Paket

```r
> install.packages("Rcpp")
```

Zusätzlich brauchen wir natürlich einen C++ Compiler. Falls Sie noch keinen C++ Compiler installiert haben, dann

- Windows: installieren Sie [Rtools](http://cran.r-project.org/bin/windows/Rtools/)
- Mac: installieren Sie die App [Xcode](https://developer.apple.com/xcode/)
- Linux: führen Sie z.B. den Befehl `sudo apt-get install r-base-dev` aus


Der Compiler sollte dann auch in der PATH Variable (siehe z.B. [Windows Toolset](http://cran.r-project.org/doc/manuals/R-admin.html#The-Windows-toolset)) eingetragen werden. Bei der Installation von Rtools kann der passende Eintrag in der PATH Variable automatisch gesetzt werden. Eine etwas genauere Anleitung findet man auf der [RStudio Support Seite](https://support.rstudio.com/hc/en-us/articles/200486498).


## Erste Schritte

Die Funktion `Rcpp::cppFunction()` erlaubt die Definition einer R Funktion über C++ Code.


```r
> library(Rcpp)
```

```
## Warning: package 'Rcpp' was built under R version 3.2.3
```

```r
> cppFunction('int add(int x, int y, int z) {
+   int sum = x + y + z;
+   return sum;
+ }')
```
Die Funktion `add()` können wir nun direkt in R aufrufen.


```r
> add
```

```
## function (x, y, z) 
## .Primitive(".Call")(<pointer: 0x0000000005cc1770>, x, y, z)
```
Wir sehen, dass `add()` eine "Primitive" Funktion ist, die über `.Call()` (siehe `help(.Call)`) kompilierten (in diesem Fall C++) Code aufruft.


```r
> add(1, 2, 3)
```

```
## [1] 6
```


## C++

Wir wollen und können an dieser Stelle keine eingehende Einführung in C++ geben. Aber anhand einiger Beispiele sollen ein paar Grundlagen vorgestellt und Unterschiede zu R aufgezeigt werden. Am Beispiel der Funktion `add()` erkennt man bereits folgende Unterschiede:

- Die Definition einer Funktion in C++ sieht aus wie der Aufruf einer Funktion in R. Man benötigt keine Zuweisung um eine Funktion zu definieren.
- Dafür muss die Struktur der Ausgabe festgelegt sein, wie z.B. `NumericVector`, `IntegerVector`, `CharacterVector` und `LogicalVector`.
- Skalare sind keine Vektoren der Länge 1. Die verschiedenen Klassen sind: `double, int, String` und `bool`.
- Die Ausgabe muss mit `return` festgelegt werden.
- Jeder Ausdruck endet mit `;`.


## Beispiele: R vs. C++

Wir betrachten nun eine Reihe von Beispielen und geben neben der C++ Implementation oftmals auch eine reine R Implementation an. Wir starten mit einer Funktion, die das Vorzeichen des Eintrags eines Vektors der Länge 1 ausgibt. 


```r
> sign_R <- function(x) {
+   if (x > 0) {
+     1
+   } else if (x == 0) {
+     0
+   } else {
+     -1
+   }
+ }
```

In der C++ Variante ist der Input eine Integer Variable.

```r
> cppFunction('int sign_C(int x) {
+   if (x > 0) {
+     return 1;
+   } else if (x == 0) {
+     return 0;
+   } else {
+     return -1;
+   }
+ }')
> sign_C(-4)
```

```
## [1] -1
```

Die Ausgabe von `sign_C()` ist ebenfalls eine Integer Variable. Am Beispiel erkennen wir auch, dass die `if` Schleife analog zur R Version funktioniert. Dies gilt ebenso für eine `while` Schleife.


Im nächsten Beispiel betrachten wir die Verwendung einer `for` Schleife. Eigentlich würden wir nie eine `for` Schleife verwenden um eine Alternative für `sum()` zu implementieren. Zu Vergleichszwecken machen wir es hier trotzdem.


```r
> sum_R <- function(x){
+   summe <- 0
+   for(i in seq_along(x)){
+     summe <- summe + x[i]
+   }
+   summe
+ }
```

In C++ sind Schleifen viel weniger problematisch und können daher gut verwendet werden.


```r
> cppFunction('double sum_C(NumericVector x) {
+   int n = x.size();
+   double summe = 0;
+   for(int i = 0; i < n; ++i) {
+     summe += x[i];
+   }
+   return summe;
+ }')
```

Der Code sieht auf den ersten Blick ähnlich aus. Es gibt aber ein paar Unterschiede:

- Die Länge von `x` wird über die Methode `.size()` abgefragt.
- Die `for` Schleife hat die Struktur: `for(anfang; kontrolle; zuwachs)`.
- Zuweisungen sind nur mithilfe von `=` möglich.
- Die Operatoren `+=, -=, *=` und `\=` arbeiten auf der linken Seite und weißen dieser anschließend das Ergebnis zu.

Das Beispiel `sum_R()` würden wir nicht ernsthaft verwenden. Trotzdem eignet es sich dazu die Effizienzvorteile von C++ gegenüber R zu zeigen. Um die Performance zu vergleichen, verwenden wir die Funktion `microbenchmark()` aus dem gleichnamigen Paket. Diese ist eine (genauere) Alternative zur Funktion `system.time()`.


```r
> library(microbenchmark)
> x <- rnorm(1e3)
> (m <- microbenchmark(sum(x), sum_R(x), sum_C(x)))
```

```
## Unit: microseconds
##      expr    min        lq       mean    median        uq      max neval
##    sum(x)   1.14    1.5210    4.32612    3.4215    5.3220   36.113   100
##  sum_R(x) 610.12 1029.7900 1131.00507 1110.9490 1208.8340 2697.069   100
##  sum_C(x)   2.66    3.6115    9.75823    9.5035   12.7345   56.260   100
##  cld
##   a 
##    b
##   a
```
Wir erkennen eine deutlich längere Laufzeit von `sum_R()` im Vergleich zu `sum_C()`. Über 100 Auswertungen ist die Laufzeit von `sum_R()` im Mittel um den Faktor 116.898932 länger als die von `sum_C()`. Wir erkennen aber auch, dass die Standardfunktion `sum()` nochmal um ca. die Hälfte schneller ist.


Analog zu den verschiedenen Vektortypen existieren die Typen: `NumericMatrix`, `IntegerMatrix`, `CharacterMatrix` und `LogicalMatrix`. Für eine `NumericMatrix` wollen wir nun deren Zeilensumme berechnen, also eine Alternative zu `rowSums()` schreiben. 


```r
> cppFunction('NumericVector rowSums_C(NumericMatrix x) {
+   int nrow = x.nrow(), ncol = x.ncol();
+   NumericVector out(nrow);
+ 
+   for (int i = 0; i < nrow; i++) {
+     double summe = 0;
+     for (int j = 0; j < ncol; j++) {
+       summe += x(i, j);
+     }
+     out[i] = summe;
+   }
+   return out;
+ }')
```




```r
> set.seed(11214)
> x <- matrix(sample(100), 10)
> rowSums(x)
```

```
##  [1] 549 586 641 384 398 540 459 474 501 518
```

```r
> rowSums_C(x)
```

```
##  [1] 549 586 641 384 398 540 459 474 501 518
```

Wir sehen, dass `rowSums_C()` die gleiche Ausgabe wie `rowSums()` liefert. Wenn wir uns den Code von `rowSums_C()` nochmal ansehen, fällt folgendes auf.

- `NumericVector out(n)` erzeugt einen neuen Vektor der Länge `n`.
- Die Indizierung von Matrizen erfolgt über `()` und nicht `[]`.
- Die Methoden `.nrow()` und `.ncol()` liefern die Dimensionen einer Matrix.


## `sourceCpp()`

Bisher wurde der C++ Code über `cppFunction()` verarbeitet. Umfangreicheren Code will man aber eher in eigene `.cpp` Dateien schreiben. Diese können dann mit `sourceCpp()` eingelesen werden. Der Kopf dieser Dateien sollte folgendermaßen aussehen

```
#include <Rcpp.h>
using namespace Rcpp;
```

Wird in der Datei eine Funktion definiert, die nach R exportiert werden soll, so muss

```
// [[Rcpp::export]]
```
hinzugefügt werden. Als Beispiel schreiben wir nun eine Funktion `mean_C()`. Der nachfolgende Code

```{ engine='Rcpp'}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double mean_C(NumericVector x) {
  int n = x.size();
  double summe = 0;

  for(int i = 0; i < n; ++i) {
    summe += x[i];
  }
  return summe / n;
}

```

sei in der Datei `mean.cpp` abgespeichert. Diesen Inhalt lesen wir dann mithilfe des Befehls


```r
> sourceCpp("mean.cpp")
```
ein. Vergleichen wir nun wieder mit `microbenchmark()` die Performance der beiden Funktionen `mean()` und `mean_C()`


```r
> x <- runif(1e5)
> microbenchmark(mean(x), mean_C(x))
```

```
## Unit: microseconds
##       expr     min       lq     mean  median       uq     max neval cld
##    mean(x) 218.579 244.6175 246.3779 245.188 246.3285 337.941   100   b
##  mean_C(x) 105.298 120.8830 120.8265 121.264 122.0240 184.367   100  a
```

so erkennen wir deutliche Laufzeitvorteile von `mean_C()`. Diese werden aber durch numerische Ungenauigkeiten im Vergleich zu `mean()` erkauft.


## Attribute und Klassen

Für die Ausgabe einer Funktion in C++ können R Attribute gesetzt werden. Dazu verwendet man `.attr()` (für das Attribut `name` kann auch `.name()` verwendet werden). 


```r
> cppFunction('NumericVector attribs() {
+   NumericVector out = NumericVector::create(1, 2, 3);
+ 
+   out.names() = CharacterVector::create("a", "b", "c");
+   out.attr("mein_attr") = "mein_wert";
+   out.attr("class") = "meine_klasse";
+ 
+   return out;
+ }')
```

Die Methode `::create()` erzeugt die entsprechenden Vektoren.


```r
> x <- attribs()
> attributes(x)
```

```
## $names
## [1] "a" "b" "c"
## 
## $mein_attr
## [1] "mein_wert"
## 
## $class
## [1] "meine_klasse"
```

Rcpp kennt auch die Klassen `List` und `DataFrame`. Speziell `List` eignet sich als Output-Format einer Funktion. Als Beispiel wollen wir eine C++ Version von `lapply()` schreiben. Diese könnte so aussehen:


```r
> cppFunction('List lapply_C(List input, Function f) {
+    int n = input.size();
+    List out(n);
+  
+    for(int i = 0; i < n; i++) {
+      out[i] = f(input[i]);
+    }
+  
+    return out;
+  }')
```

Input wie Output dieser Funktion ist also eine Liste. Wir erkennen, dass man über `[` auf die einzelnen Listenelemente zugreifen kann.


```r
> x <- list(a = 1:5, b = 2:9)
> (y <- lapply_C(x, mean))
```

```
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 5.5
```

```r
> class(y)
```

```
## [1] "list"
```

In diesem Beispiel haben wir zudem gesehen, dass auch Objekte der Klasse `function` mit `Function` übergeben (definiert) werden können. 

## Fehlende Werte in C++

Im Gegensatz zu R enthält in C++ die Definition fehlender Werte den Typ des Objekts. Man unterscheidet `NA_INTEGER`, `NA_STRING`, `NA_LOGICAL` und `NA_REAL`. 


Die Abfrage nach fehlenden Werten kann mit `::is_na()` realisiert werden.


```r
> cppFunction('LogicalVector is_na_C(NumericVector x) {
+   int n = x.size();
+   LogicalVector out(n);
+ 
+   for (int i = 0; i < n; ++i) {
+     out[i] = NumericVector::is_na(x[i]);
+   }
+   return out;
+ }')
> 
> is_na_C(c(1, NA, 3, 4, NA))
```

```
## [1] FALSE  TRUE FALSE FALSE  TRUE
```

Dies sieht aus, als ob (bis auf die unterschiedlichen Typen) als relativ analog zu R funktioniert. Dies ist aber nicht der Fall. Es gibt doch ein paar feine Unterschiede. Auf diese wollen wir hier aber nicht näher eingehen und verweisen deshalb auf [Advanced R - Rcpp: Missing Values](http://adv-r.had.co.nz/Rcpp.html#rcpp-na).


## Sugar

Rcpp sugar hat das Ziel C++ Funktionen zu definieren, die sich möglichst ähnlich (im Aufruf) zu entsprechenden R Funktion verhalten. Ein Vorteil von R ist ja die
Übersichtlichkeit des Codes (vor allem durch die Vektorisierung). Diese Eigenschaft soll übertragen werden ohne dabei viel an Effizienz zu verlieren. So sind z.B. die Operatoren `+`, `*`, `-`, `/`, `pow` (Potenz), `<`, `<=`, `>`, `>=`, `==`, `!=`, `!` alle vektorisiert.  
In R lässt sich beispielsweise der euklidische Abstand der Einträge eines Vektors zu einem Punkt `x` folgendermaßen berechnen


```r
> abstand_punkt_R <- function(x, vektor){
+   sqrt((x - vektor) ^ 2)
+ }
```

Rcpp sugar erlaubt nun eine ähnlich komprimierte Schreibweise


```r
> cppFunction('NumericVector abstand_punkt_C(double x, NumericVector vektor) {
+   return sqrt(pow((x - vektor), 2));
+ }')
```



```r
> abstand_punkt_R(3, 1:5)
```

```
## [1] 2 1 0 1 2
```

```r
> abstand_punkt_C(3, 1:5)
```

```
## [1] 2 1 0 1 2
```

Ebenso existieren die sugar Funktionen `any()` und `all()`. In Kombination mit `.is_true(), .is_false()` oder `.is_na()` können die Abfragen dann in den entsprechenden logischen Wert konvertiert werden. Eine C++ Alternative zu


```r
> any_na_R <- function(x) any(is.na(x))
```
lautet

```r
> cppFunction('bool any_na_C(NumericVector x) {
+   return is_true(any(is_na(x)));
+ }')
```
Der Vorteil der C++ Variante ist, dass `any()` (wie auch `all()`) lazy agieren, also in diesem Fall nicht der komplette Vektor nach fehlenden Werten abgesucht wird.



```r
> x0 <- runif(1e5)
> x1 <- c(NA, x0)
> microbenchmark(any_na_R(x0), any_na_C(x0), any_na_R(x1), any_na_C(x1))
```

```
## Unit: nanoseconds
##          expr    min       lq      mean   median       uq     max neval
##  any_na_R(x0) 379757 418150.5 637842.63 428604.5 438487.0 4623600   100
##  any_na_C(x0) 616201 677593.0 698002.92 684816.0 691468.5 1194769   100
##  any_na_R(x1) 276740 301639.0 411288.85 309051.0 316464.0 2333658   100
##  any_na_C(x1)    760   1901.0   3729.23   2851.0   4372.0   17106   100
##  cld
##    c
##    c
##   b 
##  a
```

Auf `x0` arbeiten beide Implementierungen ungefähr gleich lange. Die R Version ist sogar etwas schneller. Für `x1` hingegen ist `any_na_c()` deutlich schneller, da die Auswertung bereits nach dem ersten Eintrag abgebrochen wird. Danach steht ja auch schon fest, dass der Vektor fehlende Werte enthält.

Weitere sugar Funktionen sind z.B.

- `head(), tail(), rep_each(), rep_len(), rev(), seq_along(), seq_len()`
- `abs(), acos(), asin(), atan(), beta(), ceil(), ceiling(), choose(), cos(), cosh(), digamma(), exp(), expm1(), factorial(), floor(), gamma(), lbeta(), lchoose(), lfactorial(), lgamma(), log()`
- `mean(), min(), max(), sum(), sd(), var()`
- `cumsum(), diff(), pmin(), pmax()`
- `match(), self_match(), which_max(), which_min()`
- `duplicated(), unique()`

## Beispiel: Fibonacci Zahlen

Zum Abschluss noch ein Standardbeispiel. Eine einfache R Implementierung zur Berechnung der Fibonacci Zahlen, 
  $$
  F_n = F_{n-1} + F_{n-2}, \qquad n\geq 2
  $$
mit $$F_0=0$$ und $$F_1=1$$, beinhaltet einen rekursiven Funktionsaufruf


```r
> fibonacci_R <- function(n){
+   if(n < 0) stop("n darf nicht negativ sein")
+   if(n < 2) return(n)
+   fibonacci_R(n - 1) + fibonacci_R(n - 2)
+ }
```

Analog kann eine C++ Version definiert werden


```r
> cppFunction('int fibonacci_C(int n) {
+     if(n < 0) stop("n darf nicht negativ sein");
+     if (n < 2)
+     return n;
+     else
+     return fibonacci_C(n - 1) + fibonacci_C(n - 2);
+ }')
```

Im Gegensatz zur R Implementierung ist der rekursive Funktionsaufruf hier weniger problematisch, wie man im folgenden Vergleich sieht.


```r
> (m <- microbenchmark(fibonacci_R(20), fibonacci_C(20)))
```

```
## Unit: microseconds
##             expr       min         lq       mean    median         uq
##  fibonacci_R(20) 40994.300 42752.0510 46383.9327 44638.669 48250.3465
##  fibonacci_C(20)   535.612   580.6585   633.3379   601.566   615.4415
##        max neval cld
##  93347.459   100   b
##   1033.591   100  a
```

Die R Implementierung ist um den Faktor 74.2041089 langsamer. Der Wechsel zu C++ hat also einen deutlichen Laufzeitvorteil gebracht. Allerdings sollte man bei Algorithmen, die exponentiell wachsen, lieber auch nach alternativen Implementierungen suchen, wie z.B. 


```r
> fibonacci_R_iter <- function(n){
+   eins <- 0
+   zwei <- 1
+   drei <- 0
+   for (i in seq_len(n)){
+     drei <- eins + zwei
+     eins <- zwei
+     zwei <- drei
+   }
+   eins
+ }
```



```r
> fibonacci_C(20); fibonacci_R_iter(20)
```

```
## [1] 6765
```

```
## [1] 6765
```

```r
> (m <- microbenchmark(fibonacci_R_iter(20), fibonacci_C(20)))
```

```
## Unit: microseconds
##                  expr     min       lq      mean  median      uq     max
##  fibonacci_R_iter(20)  10.644  11.7850  20.01811  18.247  19.387  81.349
##       fibonacci_C(20) 534.852 589.0215 592.95607 590.352 596.435 796.006
##  neval cld
##    100  a 
##    100   b
```

Die Laufzeit der alternativen R Implementierung beträgt im Mittel nur das  
0.0309087-fache der Laufzeit von `fibonacci_C()`. Dies soll verdeutlichen, dass nicht immer nur eine C++ Implementierung zur besten Lösung führt.



## Literatur zu Rcpp

Diese Folien können nur einen kleinen Einblick in das Paket `Rcpp` geben. Eine ausführliche Dokumentation findet man im Buch 

![Seamless R and C++ Integration with Rcpp](https://images.springer.com/sgw/books/medium/9781461468677.jpg)

Eddelbuettel, D. (2013). [Seamless R and C++ Integration with Rcpp](http://www.springer.com/statistics/computational+statistics/book/978-1-4614-6867-7), Springer.

Dieses ist als [eBook](https://www.ub.tum.de/ebooks) in der Bibliothek der TUM vorhanden. Das Buch enthält auch eine kurze Einführung in C++ für R Nutzer und weitere Literaturvorschläge zu C++.
