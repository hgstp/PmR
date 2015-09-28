# Einführung


##  Was ist R?


-  R ist eine statistische Programmierumgebung für numerische und graphische
Datenanalyse
-  R basiert auf der Programmiersprache **S** und wurde anfänglich entwickelt von Ross Ithaka und Robert Gentleman
-  R ist frei erhältlich  unter der GNU GPL Lizenz
-  Wird aktiv weiterentwickelt (2 Releases pro Jahr)
-  Offizielle R Homepage http://www.R-project.org




## Woher bekomme ich R?
R besteht aus der Basisdistribution (`base`) und einer Vielzahl (Stand 23.09.14: 5871) von Zusatzpaketen (`contrib`).

Im Kurs arbeiten wir mit der Basisdistribution und ein paar weiteren Zusatzpakten, auf die wir später genauer eingehen.

Alles ist erhältlich über das Comprehensive R Archive Network
http://CRAN.R-project.org

Dort gibt es R Distributionen für 

 -  Windows: http://cran.r-project.org/bin/windows/base/
-  MacOS X: http://cran.r-project.org/bin/macosx/
-  Linux: http://cran.r-project.org/bin/linux/


## Wer benutzt R?


* Universität: Lehrveranstaltungen, Abschlussarbeiten,  Forschung (führende Software im Bereich Statistik)
* Wirtschaft: AXA, Munich Re, Credit Suisse, Bank of America, Swiss Re, Google, Facebook, Pfizer, Merck, The New York Times, ....

siehe z.B. http://www.revolutionanalytics.com/companies-using-r

Zur Popularität von R siehe z.B. auch
http://blog.revolutionanalytics.com/r-is-hot/


## Die ersten Schritte mit R?

R besitzt keine eigene komfortable Benutzeroberfläche. Kommandos werden am Prompt eingegeben:



```r
>  2 + 2
```

```
## [1] 4
```

```r
> x <- 3.14
> sin( x )
```

```
## [1] 0.001592653
```

```r
> ( z <- 3:6 )
```

```
## [1] 3 4 5 6
```



## RStudio

RStudio ist ein integrierte Entwicklungsumgebung für R. Sie ist ebenfalls frei erhältlich, http://www.rstudio.com

Im Kurs werden wir mit RStudio arbeiten.

Es existieren weitere grafische Benutzeroberflächen wie  RCommander, Tinn-R, Emacs Speaks Statistics oder Verknüpfungen zu beispielsweise Excel (RExcel). Auf diese wird im Kurs aber nicht weiter eingegangen.

## Literatur

Der Kurs basiert auf den beiden Quellen

- Ligges, U. (2008). Programmieren mit R, Springer. 
- Wickham, H. (2014). Advanced R. http://adv-r.had.co.nz/


Zu R gibt es natürlich eine Vielzahl weiterer Literaturquellen, siehe z.B. http://www.r-project.org/doc/bib/R-books.html

