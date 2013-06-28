Test A/B - Significatività statistica
======================================
Il calcolo della significatività statistica è fondamentale per misurare il successo di un test A/B. Le domande a cui dare risposte spaziano da:

* Le differenze di risultati che sono state rilevate hanno una significatività statistica o sono frutto della fluttuazione campionaria dei dati?
* Quali sono gli estremi dell'intervallo di confidenza delle misurazioni?
* Per quanto tempo devo mantenere online la campagna, affinché le misurazioni abbiano rilevanza statistica?

Per la quasi totalità dei test e delle misurazioni, si può fare uso di un package apposito di R per l'analisi delle distribuzioni binomiali:


```r
library("binom")
```

```
## Loading required package: lattice
```


Parametri simulazione
---------------------
Supponiamo di avere condotto una campagna di A/B che ha portato a proporre una versione alternativa di un contenuto rispetto alla versione non modificata. Questa esperienza è stata proposta ad un sottoinsieme del campione totale di visitatori del sito.
Con questi parametri, è possibile condurre una simulazione di come si può calcolare la significatività statistica in un test A/B.

Con tasso di conversione è inteso il numero di visite in cui l'utente compie l'azione che la campagna di A/B vuole facilitare, diviso il numero totale di visite che hanno subito la campagna.


```r
# intervallo di confidenza desiderato e massimo errore tipo-1 ammissibile
ci <- 0.95

# minimo miglioramento percentuale del tasso di conversione che implichi
# significatività determina la dimensione minima del campione, prima che
# la campagna possa essere 'spenta' con sicurezza
miglioramento <- 0.1

# power - è la probabilità minima attesa nell'individuare i veri positivi
# (un fenomeno quando questo è effettivamente presente) ovvero (1 - pow) è
# la massima probabilità di falsi negativi vista come accettabile
pow <- 0.8
```


Supponiamo che, dopo un adeguato tempo di campagna, sono stati raccolte le misurazioni sul campione che ha visualizzato l'esperienza (dataset esperienza 1) e sul campione di controllo che ha visualizzato la versione standard del contenuto (dataset di controllo):


```r
# Dati simulati

nc <- 100  # numero di esperimenti dataset di controllo
sc <- 10  # numero di conversioni dataset di controllo

n1 <- 87  # numero di esperimenti dataset esperienza 1
s1 <- 12  # numero di conversioni dataset esperienza 1

# Calcolo di alpha
alpha <- (1 - ci)
```

Intervalli di confidenza
-------------------------
Il calcolo degli intervalli di confidenza è stato ottenuto tramite l'approssimazione di Agresti-Coull per distribuzioni binomiali:


```r
confIntc <- binom.agresti.coull(sc, nc, conf.level = ci)
confIntc
```

```
##          method  x   n mean   lower  upper
## 1 agresti-coull 10 100  0.1 0.05348 0.1761
```

```r
confInt1 <- binom.agresti.coull(s1, n1, conf.level = ci)
confInt1
```

```
##          method  x  n   mean   lower  upper
## 1 agresti-coull 12 87 0.1379 0.07917 0.2273
```


In questo modo, è possibile ottenere, in questo caso al 95% di confidenza, gli intervalli di confidenza assoluti delle due distribuzioni campionarie. Probabilmente, però, è più utile calcolare lo scostamento percentuale tra p1 (il tasso di conversione per l'esperienza 1) con pc (il corrispettivo per il campione di controllo).

Poiché (t.l.c.) i due valori attesi sc e s1 sono variabili aleatorie che seguono una distribuzione normale, anche la differenza attesa segue una distribuzione normale con media la differenza tra le medie e la varianza come la radice quadrata della somma delle deviazioni standard.

Dividendo per il valore medio del dataset di controllo, si otterrà lo scostamento percentuale.

E' chiaramente possibile trovare gli intervalli di confidenza calcolando il valore z per il dato alpha/2 e da esso derivare il margine d'erroe, ma la procedura è più laboriosa. In questo modo, però, calcoliamo direttamente l'intervallo di confidenza relativo al campione di controllo


```r
pc <- sc/nc
p1 <- s1/n1
z <- qnorm(alpha/2, mean = 0, sd = 1, lower.tail = FALSE)
ME1 <- z * sqrt(p1 * (1 - p1)/n1 + pc * (1 - pc)/nc)
((confInt1$mean - confIntc$mean) + c(-ME1, +ME1))/confIntc$mean
```

```
## [1] -0.5538  1.3125
```


Calcolo delle dimensioni del campione
-------------------------------------
Il calcolo delle dimensioni minime del campione è influenzato dal massimo errore di tipo-1 o dal massimo errore di tipo-2 tollerabile. Nel caso in esame, si è deciso di controllare la massima possibilità di incorrere in un massimo negativo (20%).
Altre possibili scelte sono, però, possibili: http://37signals.com/svn/posts/3004-ab-testing-tech-note-determining-sample-size


```r
needed_sample <- ceiling(power.prop.test(p1 = confIntc$mean, p2 = (confIntc$mean + 
    miglioramento * confIntc$mean), power = pow, alternative = "two.sided", 
    sig.level = 1 - ci)$n)

# Il sample è di dimensioni adeguate?
needed_sample <= n1 + nc
```

```
## [1] FALSE
```


Test per significatività statistica
-----------------------------------
Per valutare la significatività statistica del fenomeno, saranno condotti due test:
* C'è una _differenza_ statisticamente significativa nel comportamento tra chi ha subito l'esperienza e chi ha visualizzato il sito standard?
* C'è stato un _miglioramento_ nel tasso di conversione?

### Test 1:
Verifichiamo se il trattamento (esperienza) ha portato ad una modifica nel tasso di conversione, definendo le seguenti ipotesi nulla ed alternativa:
* H0: p1 = pc
* Ha: p1 != pc
Per risolverlo in modo esteso applichiamo un test per ipotesi utilizzando la statistica di test p1 - pc e calcoliamo il p-value o, alternativamente, il valore di z-critico, cos' da valutare se è possibile rigettare l'ipotesi.

Nel caso specifico è possibile approssimare la deviazione standard dei due campioni alla forma #numero di successi totali/#numero totale di esperimenti (pooled variance) in quanto i due campioni (esperienza e controllo) hanno dimensioni e deviazioni standard comparabili.



```r
z <- qnorm(alpha/2, mean = 0, sd = 1, lower.tail = FALSE)  # two-sided
p_pooled <- (s1 + sc)/(n1 + nc)  # stimatore, pooled variance è utilizzabile
z_pooled <- (confInt1$mean - confIntc$mean)/sqrt(p_pooled * (1 - p_pooled) * 
    (1/n1 + 1/nc))  # Valore critico per la statistica di test p1 - pc
p_value_pooled <- 2 * pnorm(-abs(z_pooled))

p_value_pooled
```

```
## [1] 0.422
```

```r
# è possibile rigettare H0 ?
z_pooled > z
```

```
## [1] FALSE
```


Il tutto è ottenibile più rapidamente con l'apposita funzione del package binom per i test sulle proporzioni:


```r
prop.test(c(s1, sc), c(n1, nc), conf.level = ci, correct = FALSE, alternative = "two.sided")
```

```
## 
## 	2-sample test for equality of proportions without continuity
## 	correction
## 
## data:  c(s1, sc) out of c(n1, nc)
## X-squared = 0.6448, df = 1, p-value = 0.422
## alternative hypothesis: two.sided
## 95 percent confidence interval:
##  -0.05538  0.13125
## sample estimates:
## prop 1 prop 2 
## 0.1379 0.1000
```


che ci consente di valutare anche gli intervalli di confidenza relativi tra p1 e pc (calcolati in modo esteso pocanzi)

### Test 2
Verifichiamo se il trattamento, oltre ad essere apprezzabilmente diverso rispetto al campione di controllo, ha portato ad un miglioramento nel tasso di conversione rispetto a questo. In pratica:
* H0: p1 - pc <= 0
* Ha: p1 - pc > 0

Questo test è one-sided, ne si tiene conto nella determinazione dello z-score critico:


```r
z <- qnorm(alpha, mean = 0, sd = 1, lower.tail = FALSE)  # one-sided
z_unpooled <- (p1 - pc)/sqrt(p1 * (1 - p1)/n1 + pc * (1 - pc)/nc)
p_value_unpooled <- pnorm(-abs(z_unpooled))
p_value_unpooled
```

```
## [1] 0.2128
```

```r
# Possibile rigettare H0?
z_unpooled > z
```

```
## [1] FALSE
```


In modo più compatto usando l'apposita funzione del package binom:


```r
prop.test(c(s1, sc), c(n1, nc), conf.level = ci, correct = FALSE, alternative = "greater")
```

```
## 
## 	2-sample test for equality of proportions without continuity
## 	correction
## 
## data:  c(s1, sc) out of c(n1, nc)
## X-squared = 0.6448, df = 1, p-value = 0.211
## alternative hypothesis: greater
## 95 percent confidence interval:
##  -0.04038  1.00000
## sample estimates:
## prop 1 prop 2 
## 0.1379 0.1000
```

In questo caso non è stata usata la pooled variance, solo per motivi di completezza didattica. Qui riassunte le condizioni che ne consentono l'uso in contesti reali:

* Solo se le dimensioni e le sd dei campioni sono simili
* Se i sample size sono molto differenti, ma le sd sono simili e la sd piu' ampia e' associata al sample size piu' ampio
