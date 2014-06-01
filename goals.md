# Fortuna Torverteilung

Tore geparst von http://www.fussballdaten.de/vereine/fortunaduesseldorf/2014/

So grob mit
```javascript
$('table.Spiele i').map(function(){
  return {
    minute: parseInt($(this).text().replace(/[^\d]/g,'')),
    player: $(this).prev().text(),
    heimgast: $(this).closest('td').attr('class'),
    team: $(this).closest('tr').prev().find(
            '.'+$(this).closest('td').attr('class')
          ).text()
  }
}).toArray();
```

Und dann http://jsfiddle.net/sturtevant/vUnF9/ nach CSV konvertiert.


```r
goals = read.table('goals.csv', header=TRUE, sep=',')
```

## Gegentore

Davon interessieren uns erst mal nur die Gegentore:


```r
(against = subset(goals, team != 'Fortuna Düsseldorf'))
```

```
##    minute      player heimgast                 team
## 2      67        Ujah     Heim           1. FC Köln
## 5       9       Lauth     Gast     TSV 1860 München
## 6      78     Tomasov     Gast     TSV 1860 München
## 7      43       Nemec     Heim   1. FC Union Berlin
## 8      56       Nemec     Heim   1. FC Union Berlin
## 11     21        Rahn     Heim    Arminia Bielefeld
## 12     29     Hübener     Heim    Arminia Bielefeld
## 13     47        Klos     Heim    Arminia Bielefeld
## 14     86       Hille     Heim    Arminia Bielefeld
## 15     90       Hille     Heim    Arminia Bielefeld
## 18     82      Müller     Gast       Dynamo Dresden
## 19     82      Kringe     Heim         FC St. Pauli
## 23     15      Trinks     Gast SpVgg Greuther Fürth
## 24     90       Matip     Heim     FC Ingolstadt 04
## 29     28      Saglik     Gast      SC Paderborn 07
## 30     45      Saglik     Gast      SC Paderborn 07
## 31     63      Saglik     Gast      SC Paderborn 07
## 32     70      Saglik     Gast      SC Paderborn 07
## 33     76    Kachunga     Gast      SC Paderborn 07
## 34     23  Lechleiter     Heim            VfR Aalen
## 36     18        Fink     Heim    FC Erzgebirge Aue
## 37     37      Janjic     Heim    FC Erzgebirge Aue
## 38     52       Kocer     Heim    FC Erzgebirge Aue
## 39     28      Alibaz     Gast        Karlsruher SC
## 40     71       Peitz     Gast        Karlsruher SC
## 42      7      Affane     Heim   FC Energie Cottbus
## 48     29        Ujah     Gast           1. FC Köln
## 49     38      Helmes     Gast           1. FC Köln
## 50     75        Ujah     Gast           1. FC Köln
## 51     63       Osako     Heim     TSV 1860 München
## 54     58      Brandy     Gast   1. FC Union Berlin
## 57     55       Ouali     Heim       Dynamo Dresden
## 59     21       Maier     Gast         FC St. Pauli
## 60     90         Thy     Gast         FC St. Pauli
## 61     39       Azemi     Heim SpVgg Greuther Fürth
## 62     55       Azemi     Heim SpVgg Greuther Fürth
## 63     65   Brosinski     Heim SpVgg Greuther Fürth
## 64     78     Stieber     Heim SpVgg Greuther Fürth
## 66     53        Meha     Heim      SC Paderborn 07
## 72     83     Junglas     Gast            VfR Aalen
## 80     22    Hennings     Heim        Karlsruher SC
## 81     68    Hennings     Heim        Karlsruher SC
## 88     33     Matmour     Gast 1. FC Kaiserslautern
## 89     40    Idrissou     Gast 1. FC Kaiserslautern
## 90     90 Studtrucker     Heim       SC Wiedenbrück
```

## Analyse der Gegentore

Das durchschnittliche Gegentor fällt in der 52.6222-ten Minute. Der Boxplot sieht folgendermaßen aus:


```r
boxplot(against$minute)
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

Vielmehr sieht es sogar nach einer Gleichverteilung der Tore auf die Minuten aus. Sortiert man die Tore und plottet sie ihren Index kommt folgender Graph heraus:


```r
plot(sort(against$minute), 1:length(against$minute))
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

Das lässt sich auch statistisch überprüfen mit dem Chi-Quadrat-Anpassungstest:


```r
test = chisq.test(table(cut(against$minute, 0:90)))
```

```
## Warning: Chi-squared approximation may be incorrect
```

Der p-Wert ist mit 0.2636 weit über jedem Schwellwert, so dass an der Gleichverteilungshypothese festgehalten werden kann.

# Fazit:

Die Fortuna erhält nicht, wie intuitiv angenommen, übermäßig viele Gegentore gegen Ende des Spiels.
