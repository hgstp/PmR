# Rechnen mit Vektoren und Matrizen




## Rechnen mit Vektoren
  

Viele mathematische Operatoren sind komponentenweise definiert 


```r
> x <- 1:6 
> x + 2 
```

```
## [1] 3 4 5 6 7 8
```

```r
> (y <- rep(10, 6))
```

```
## [1] 10 10 10 10 10 10
```

```r
> x * y
```

```
## [1] 10 20 30 40 50 60
```

```r
> y / c(5,10) # Vektoren haben unterschiedliche Länge
```

```
## [1] 2 1 2 1 2 1
```




## Rechnen mit Vektoren
	

Zahlreiche Funktionen ebenfalls 


```r
> c(exp(1:3), sin(1:3))
```

```
## [1]  2.7182818  7.3890561 20.0855369  0.8414710  0.9092974  0.1411200
```
Andere Funktionen berechnen schnell skalare Zusammenfassungen von Vektoren, z.B.

```r
> max(x)
```

```
## [1] 6
```

```r
> sum(x)
```

```
## [1] 21
```

```r
> mean(x)
```

```
## [1] 3.5
```



## Rechnen mit Vektoren
	
Funktion | Beschreibung
---------|--------------
`sum()`| Summe der Elemente 
`prod()` | Produkt der Elemente 
`max(), min()` | Maximum, Minimum 
`union()` | Vereinigung der Elemente zweier Vektoren 
`intersect()` | Schnitt der Elemente zweier Vektoren 
`rev()` | Reihenfolge invertieren 
`sort()` | Sortieren 
`order()` | Reihenfolge im unsortierten Vektor
... | ...


## Rechnen mit Matrizen

`%*%` bezeichnet die Matrixmultiplikation. Die Inverse einer Matrix berechne man mit `solve()`.


```r
> A <- c(1, 2, 2, 1)
> dim(A) <- c(2,2)
> (A.inv <- solve(A))
```

```
##            [,1]       [,2]
## [1,] -0.3333333  0.6666667
## [2,]  0.6666667 -0.3333333
```

```r
> A.inv %*% A
```

```
##      [,1] [,2]
## [1,]    1    0
## [2,]    0    1
```



## Rechnen mit Matrizen


```r
> t(2:4)
```

```
##      [,1] [,2] [,3]
## [1,]    2    3    4
```

```r
> t(2:4) %*% 1:3
```

```
##      [,1]
## [1,]   20
```

```r
> crossprod(2:4, 1:3)
```

```
##      [,1]
## [1,]   20
```

```r
> 2:4 %*% 1:3
```

```
##      [,1]
## [1,]   20
```



## Rechnen mit Matrizen


```r
> 2:4 %*% t(1:3)
```

```
##      [,1] [,2] [,3]
## [1,]    2    4    6
## [2,]    3    6    9
## [3,]    4    8   12
```

```r
> tcrossprod(2:4, 1:3)
```

```
##      [,1] [,2] [,3]
## [1,]    2    4    6
## [2,]    3    6    9
## [3,]    4    8   12
```


## Rechnen mit Matrizen

$X^2$

Funktion | Beschreibung
----------|--------------
`t()` | Transponierte Matrix  
`det()` | Determinante  
`solve()` | Matrixinverse  
`solve(A,b)` | Löst das Gleichungssystem $Ax=b$
`crossprod(X,Y)` | $X^\top Y$ 
`tcrossprod(X,Y)` | $X Y^\top$ 
`qr()` | QR-Zerlegung 
`eigen()` | Eigenwerte und -vektoren
`%*%` | Matrixmultiplikation
`nrow(), ncol()` | Zeilen-, Spaltenanzahl
