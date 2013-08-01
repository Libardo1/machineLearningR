Dice Rolling e distribuzioni statistiche
=========================================
Qui di seguito sono modellate le distribuzioni statistiche dei principali tipi di tiro di dado utilizzati nel gioco di ruolo.

Pool di dadi
------------
Nel pool di dadi, viene decreato il successo se è conseguito un numero minimo di successi al tiro di un insieme di dadi, dove con _successo_ si intende che il risultato del singolo dado sia minore o uguale ad una soglia (roll-under), esattamente uguale, maggiore o uguale ad una soglia (roll-upper).

Essendo una distribuzione di una variabile discreta, l'esperimento è modellabile come una distribuzione binomiale.

### Teoria sulle distribuzioni binomiali
In teoria della probabilità la distribuzione binomiale è una distribuzione di probabilità discreta che descrive il numero di successi in un processo di Bernoulli. In pratica, la variabile aleatoria
<math display="block" xmlns="http://www.w3.org/1998/Math/MathML">
<mrow>
  <msub>
          <mi>S</mi>
        <mi>n</mi>
  </msub>
  <mo>=</mo>
  <msub>
          <mi>X</mi>
        <mn>1</mn>
  </msub>
  <mo>+</mo>
  <msub>
          <mi>X</mi>
        <mn>2</mn>
  </msub>
  <mo>+</mo>
  <mo>&hellip;</mo>
  <mo>+</mo>
  <msub>
          <mi>X</mi>
        <mi>n</mi>
  </msub>
</mrow>
</math>
somma _n_ variabili aleatorie indipendenti di uguale distribuzione di Bernoulli **B(p)**. Tale distribuzione può presentare solo due valori d'uscita: **successo** (con probabilità _p_) o **insuccesso** (con probabilità _1 - p_).

La distribuzione binomiale _B(n, p)_ è, perciò, descritta dai due parametri:

* Probabilità _p_ di successo
* Numero _n_ di esperimenti

e presenta la distribuzione di probabilità (la probabilità di avere esattamente _k_ successi su _n_ tentativi):
<math display="block">
<mrow>
  <mi>P</mi><mo>&ApplyFunction;</mo>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mi>k</mi>
    </mrow>
  </mfenced>
  <mo>=</mo>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mfrac linethickness="0">
        <mrow>
          <mi>n</mi>
        </mrow>
        <mrow>
          <mi>k</mi>
        </mrow>
      </mfrac>
    </mrow>
  </mfenced>
  <msup>
          <mi>p</mi>
        <mi>k</mi>
  </msup>
  <msup>
        <mrow>
          <mfenced open="(" close=")" separators=",">
            <mrow>
              <mn>1</mn>
              <mo>-</mo>
              <mi>p</mi>
            </mrow>
          </mfenced>
        </mrow>
      <mrow>
        <mi>n</mi>
        <mo>-</mo>
        <mi>k</mi>
      </mrow>
  </msup>
</mrow>
</math>
che esprime la probabilità di ottenere _k_ successi e _n - k_ fallimenti in una successione di _n_ esperimenti. Il coefficiente binomiale
<math display="block">
<mrow>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mfrac linethickness="0">
        <mrow>
          <mi>n</mi>
        </mrow>
        <mrow>
          <mi>k</mi>
        </mrow>
      </mfrac>
    </mrow>
  </mfenced>
  <mo>=</mo>
  <mfrac>
    <mrow>
      <mi>n</mi>
      <mo>!</mo>
    </mrow>
    <mrow>
      <mi>k</mi>
      <mo>!</mo>
      <mfenced open="(" close=")" separators=",">
        <mrow>
          <mi>n</mi>
          <mo>-</mo>
          <mi>k</mi>
        </mrow>
      </mfenced>
      <mo>!</mo>
    </mrow>
  </mfrac>
</mrow>
</math>
esprime il numero di tali successioni, ovvero le combinazioni in cui possono essere disposti i _k_ successi negli _n_ tentativi.

### Esempi con R
Il successo sul pool di dati può essere modellato come una distribuzione binomiale in quanto gli _n_ dadi tirati rappresentano altrettanti esperimenti indipendenti ed ognuno dei quali con la stessa probabilità _p_ di successo.
Con il software statistico R è possibile calcolare le probabilità di successo per le distribuzioni binomiali e simulare meccaniche e situazioni di gioco in cui il successo di un'azione è determinato da un lancio.

**Esempio: risultato esatto**
* Probabilità di conseguire esattamente 3 successi su un pool di 5 dadi a 10 facce in cui il successo è rappresentato da un risultato sul dado di 6 o più (_p = 5/10_).
In questo caso va usata la funzione di densità di probabilità *dbinom* il cui k=3 (numero di successi), p=1/2 (probabilità che il risultato sul dado sia 6 o più sul singolo tiro) e n=7 (numero di tentativi). 

```r
n <- 7  # Numero di tentativi (dimensione del pool di dadi)
p <- 1/2  # Probabilità di successo sul singolo tentativo
k <- 3  # Numero esatto di successi
dbinom(k, n, p)
```

```
## [1] 0.2734
```

**Esempio: numero minimo di successi**
* A differenza del caso precedente, vuole essere calcolata la probabilità di avere _almeno_ k successi e non _esattamente_ k successi. Per linearità, basta sommare le probabilità P(k=3) + P(k=4) + P(k=5) + P(k=6) + P(k=7).

```r
sum(dbinom(k:n, n, p))
```

```
## [1] 0.7734
```

In alternativa, si può usare la funzione _pnorm_ per il calcolo della probabilità cumulativa, ovvero la probabilità che P(k<3).

```r
1 - pbinom(k - 1, n, p)  #   Poiché P(k>=3) = 1 - P(k<=2)
```

```
## [1] 0.7734
```

### Tabelle
Le tabelle sono calcolate con questa funzione, in modo che sia possibile crearne di personalizzate per le proprie esigenze.

```r
poolDistribution <- function(n, sides = 10, digits = 2, roll.Under = FALSE) {
    m <- 1:sides
    names(m) <- paste(m, ifelse(roll.Under, "-", "+"), sep = "")
    s <- 1:n
    names(s) <- paste(s, n, sep = "/")
    sapply(m, function(m.value) round((if (roll.Under) (1 - pbinom(s - 1, n, 
        (m.value)/sides)) * 100 else (1 - pbinom(s - 1, n, (sides - m.value + 
        1)/sides)) * 100), digits = digits))
    
}
```

#### Tabella 1: probabilità di successo roll-upper
* Questa tabella, data la dimensione _n_ del pool di dadi ed il loro numero di facce, restituisce la probabilità che il tiro abbia un numero di successi almeno pari a quelli riportati sulle righe. Sulle colonne è riportata la soglia minima sul singolo tiro di dado per decretare un successo.


```r
library(hwriter)
tab1 <- poolDistribution(n, roll.Under = FALSE)
cat(hwrite(tab1, table.border = "1", table.class = "tab1", width = "100%"))
```

<table border="1" class="tab1" width="100%">
<tr>
<td></td><td>1+</td><td>2+</td><td>3+</td><td>4+</td><td>5+</td><td>6+</td><td>7+</td><td>8+</td><td>9+</td><td>10+</td></tr>
<tr>
<td>1/7</td><td>100</td><td>100</td><td>100</td><td>99.98</td><td>99.84</td><td>99.22</td><td>97.2</td><td>91.76</td><td>79.03</td><td>52.17</td></tr>
<tr>
<td>2/7</td><td>100</td><td>100</td><td>99.96</td><td>99.62</td><td>98.12</td><td>93.75</td><td>84.14</td><td>67.06</td><td>42.33</td><td>14.97</td></tr>
<tr>
<td>3/7</td><td>100</td><td>99.98</td><td>99.53</td><td>97.12</td><td>90.37</td><td>77.34</td><td>58.01</td><td>35.29</td><td>14.8</td><td>2.57</td></tr>
<tr>
<td>4/7</td><td>100</td><td>99.73</td><td>96.67</td><td>87.4</td><td>71.02</td><td>50</td><td>28.98</td><td>12.6</td><td>3.33</td><td>0.27</td></tr>
<tr>
<td>5/7</td><td>100</td><td>97.43</td><td>85.2</td><td>64.71</td><td>41.99</td><td>22.66</td><td>9.63</td><td>2.88</td><td>0.47</td><td>0.02</td></tr>
<tr>
<td>6/7</td><td>100</td><td>85.03</td><td>57.67</td><td>32.94</td><td>15.86</td><td>6.25</td><td>1.88</td><td>0.38</td><td>0.04</td><td>0</td></tr>
<tr>
<td>7/7</td><td>100</td><td>47.83</td><td>20.97</td><td>8.24</td><td>2.8</td><td>0.78</td><td>0.16</td><td>0.02</td><td>0</td><td>0</td></tr>
</table>

#### Tabella 2: probabilità di successo roll-under
* Questa tabella è l'antisimmetrica della precedente, in cui però la soglia a decretare il successo sul dado è che il risultato sia al massimo quello riportato in colonna.


```r
library(hwriter)
tab2 <- poolDistribution(n, roll.Under = TRUE)
cat(hwrite(tab2, table.border = "1", table.class = "tab1", width = "100%"))
```

<table border="1" class="tab1" width="100%">
<tr>
<td></td><td>1-</td><td>2-</td><td>3-</td><td>4-</td><td>5-</td><td>6-</td><td>7-</td><td>8-</td><td>9-</td><td>10-</td></tr>
<tr>
<td>1/7</td><td>52.17</td><td>79.03</td><td>91.76</td><td>97.2</td><td>99.22</td><td>99.84</td><td>99.98</td><td>100</td><td>100</td><td>100</td></tr>
<tr>
<td>2/7</td><td>14.97</td><td>42.33</td><td>67.06</td><td>84.14</td><td>93.75</td><td>98.12</td><td>99.62</td><td>99.96</td><td>100</td><td>100</td></tr>
<tr>
<td>3/7</td><td>2.57</td><td>14.8</td><td>35.29</td><td>58.01</td><td>77.34</td><td>90.37</td><td>97.12</td><td>99.53</td><td>99.98</td><td>100</td></tr>
<tr>
<td>4/7</td><td>0.27</td><td>3.33</td><td>12.6</td><td>28.98</td><td>50</td><td>71.02</td><td>87.4</td><td>96.67</td><td>99.73</td><td>100</td></tr>
<tr>
<td>5/7</td><td>0.02</td><td>0.47</td><td>2.88</td><td>9.63</td><td>22.66</td><td>41.99</td><td>64.71</td><td>85.2</td><td>97.43</td><td>100</td></tr>
<tr>
<td>6/7</td><td>0</td><td>0.04</td><td>0.38</td><td>1.88</td><td>6.25</td><td>15.86</td><td>32.94</td><td>57.67</td><td>85.03</td><td>100</td></tr>
<tr>
<td>7/7</td><td>0</td><td>0</td><td>0.02</td><td>0.16</td><td>0.78</td><td>2.8</td><td>8.24</td><td>20.97</td><td>47.83</td><td>100</td></tr>
</table>

#### Tabella 3: distribuzione dei successi sui tiri roll-under
* Questa tabella evidenzia le probabilità di avere almeno il dato numero di successi in base al numero di tiri, posta una soglia di successo sul dado di 7+.

```r
library(hwriter)
tiri <- 1:n
names(tiri) <- paste(tiri, "tiri")
succ <- 0:n
names(succ) <- paste(succ, "+", " ", "successi", sep = "")
tab3 <- sapply(succ, function(m.value) round(100 * (1 - pbinom(m.value - 1, 
    tiri, 4/10)), digits = 2))
cat(hwrite(tab3, table.border = "1", table.class = "tab1", width = "100%"))
```

<table border="1" class="tab1" width="100%">
<tr>
<td></td><td>0+ successi</td><td>1+ successi</td><td>2+ successi</td><td>3+ successi</td><td>4+ successi</td><td>5+ successi</td><td>6+ successi</td><td>7+ successi</td></tr>
<tr>
<td>1 tiri</td><td>100</td><td>40</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr>
<td>2 tiri</td><td>100</td><td>64</td><td>16</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr>
<td>3 tiri</td><td>100</td><td>78.4</td><td>35.2</td><td>6.4</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
<tr>
<td>4 tiri</td><td>100</td><td>87.04</td><td>52.48</td><td>17.92</td><td>2.56</td><td>0</td><td>0</td><td>0</td></tr>
<tr>
<td>5 tiri</td><td>100</td><td>92.22</td><td>66.3</td><td>31.74</td><td>8.7</td><td>1.02</td><td>0</td><td>0</td></tr>
<tr>
<td>6 tiri</td><td>100</td><td>95.33</td><td>76.67</td><td>45.57</td><td>17.92</td><td>4.1</td><td>0.41</td><td>0</td></tr>
<tr>
<td>7 tiri</td><td>100</td><td>97.2</td><td>84.14</td><td>58.01</td><td>28.98</td><td>9.63</td><td>1.88</td><td>0.16</td></tr>
</table>

