# Einführung


##  Was ist R?


-  R ist eine statistische Programmierumgebung für numerische und graphische
Datenanalyse
-  R basiert auf der Programmiersprache **S** und wurde anfänglich entwickelt von Ross Ithaka und Robert Gentleman
-  R ist frei erhältlich  unter der GNU GPL Lizenz
-  Wird aktiv weiterentwickelt (2 Releases pro Jahr)
-  Offizielle R Homepage http://www.R-project.org




## Woher bekomme ich R?
R besteht aus der Basisdistribution (`base`) und einer Vielzahl (Stand 04.10.2015: 7264) von Zusatzpaketen (`contrib`). Im Kurs arbeiten wir mit der Basisdistribution und ein paar weiteren Zusatzpakten, auf die wir später genauer eingehen.

Alles ist erhältlich über das Comprehensive R Archive Network
http://CRAN.R-project.org

Dort gibt es R Distributionen für 

 -  Windows: http://cran.r-project.org/bin/windows/base/
 -  MacOS X: http://cran.r-project.org/bin/macosx/
 -  Linux: http://cran.r-project.org/bin/linux/


## Wer benutzt R?

R kann eine stetig wachsende "Fangemeinde" vorweisen. Im aktuellen [IEEE spectrum ranking](http://spectrum.ieee.org/computing/software/the-2015-top-ten-programming-languages) hat R den größten Sprung in den Top 10 gemacht. Eine Aufstellung von Firmen, die R nutzen findet man auf der [Webseite](http://www.revolutionanalytics.com/companies-using-r) von [Revolution Analytics](http://www.revolutionanalytics.com/).


Interessante Beiträge von R Nutzern findet man z.B. auf den Seiten

- http://blog.revolutionanalytics.com/
- http://www.r-bloggers.com/

Die Konferenz [EARL 2015](http://www.earl-conference.com/london/index.html) hatte verschiedene Anwendungsmöglichkeiten von R zum Thema.


## Die ersten Schritte mit R?

R besitzt keine eigene komfortable Benutzeroberfläche. Kommandos werden am Prompt eingegeben:



```r
> 2 + 2
```

```
## [1] 4
```

```r
> x <- 3.14
> sin(x)
```

```
## [1] 0.001592653
```

```r
> (z <- 3 : 6)
```

```
## [1] 3 4 5 6
```



## RStudio

RStudio ist ein integrierte Entwicklungsumgebung für R. Sie ist ebenfalls frei erhältlich über die Webseite http://www.rstudio.com

Im Kurs werden wir intensiv mit RStudio arbeiten. Speziell werden wir das Paket [`rmarkdown`](http://rmarkdown.rstudio.com/) nutzen, welches stark in RStudio integriert ist. `rmarkdown` kombiniert [Markdown](https://de.wikipedia.org/wiki/Markdown) Syntax mit R Befehlen (und deren Output). Dies ermöglicht eine einfache Erstellung dynamischer (reproduzierbarer) Dokumente.

Es existieren weitere grafische Benutzeroberflächen wie z.B. der RCommander, Tinn-R, Emacs Speaks Statistics oder Verknüpfungen zu beispielsweise Excel (RExcel). Auf diese wird im Kurs aber nicht weiter eingegangen.

## Literatur

Der Kurs sowie diese Unterlagen basieren auf den beiden Quellen

- Ligges, U. (2008). [Programmieren mit R](http://link.springer.com/book/10.1007%2F978-3-540-79998-6), Springer. 
- Wickham, H. (2014). [Advanced R](https://www.crcpress.com/Advanced-R/Wickham/9781466586963), Chapman and Hall/CRC.

Das Buch *Programmieren mit R* ist als eBook erhältlich. Es ist Teil des [Springer Informatik Pakets](http://rzblx10.uni-regensburg.de.eaccess.ub.tum.de/dbinfo/dbliste.php?bib_id=tum&colors=63&ocolors=40&lett=f&gebiete=30) der Bibliothek. Zum Buch Advanced R, gibt es auch eine eigene Webseite: http://adv-r.had.co.nz/


Natürlich gibt es zu R eine Vielzahl weiterer Literaturquellen, siehe z.B. http://www.r-project.org/doc/bib/R-books.html. Für den Kurs sind die angegebenen Quellen aber mehr als ausreichend.

