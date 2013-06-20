Intervalli di confidenza (distribuzioni binomiali)
==================================================
Supponiamo di voler calcolare gli intervalli di confidenza di un esperimento modellato come di tipo bernoulliano e supponiamo i seguenti parametri:

<math display="block" xmlns="http://www.w3.org/1998/Math/MathML">
<mrow>
  <mi>n</mi>
  <mo>=</mo>
  <mn>1000</mn>
  <mspace width="1.5em" />
  <mtext>(numero di esperimenti)</mtext>
</mrow>
</math>
<math display="block" xmlns="http://www.w3.org/1998/Math/MathML">
<mrow>
  <mover>
          <mi>p</mi>
        <mo>&and;</mo>
  </mover>
  <mo>=</mo>
  <mn>0,576</mn>
  <mspace width="1.5em" />
  <mtext>(media campionaria di successi)</mtext>
</mrow>
</math>
Supponiamo _p_ la media reale della popolazione nel modello teorico.
Sappiamo che il valore atteso per <math display="inline"><mrow><mover><mi>p</mi><mo>&and;</mo></mover></mrow></math> è esattamente uguale a _p_ ovvero:
<math display="block">
<mrow>
  <mi>&Epsilon;</mi>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mover accent="true">
              <mi>p</mi>
            <mo>&Hat;</mo>
      </mover>
    </mrow>
  </mfenced>
  <mo>=</mo>
  <mi>p</mi>
</mrow>
</math>
E che la varianza campionaria è pari a:
<math display="block">
<mrow>
  <mi>Var</mi><mo>&ApplyFunction;</mo>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mover accent="true">
              <mi>p</mi>
            <mo>&Hat;</mo>
      </mover>
    </mrow>
  </mfenced>
  <mo>=</mo>
  <mfrac>
    <mrow>
      <mn>1</mn>
      <mo>-</mo>
      <mi>p</mi>
    </mrow>
    <mrow>
      <mi>n</mi>
    </mrow>
  </mfrac>
</mrow>
</math>
Se sussistono le condizioni affinché sia applicabile il teorema del limite centrale, possiamo affermare che <math display="inline"><mrow><mover><mi>p</mi><mo>&and;</mo></mover></mrow></math> segua una distribuzione normale con media il valore atteso per <math display="inline"><mrow><mover><mi>p</mi><mo>&and;</mo></mover></mrow></math> ovvero _p_ e come varianza la varianza campionaria:
<math display="block">
<mrow>
  <mover accent="true">
          <mi>p</mi>
        <mo>&Hat;</mo>
  </mover>
  <mo>&asymp;</mo>
  <mi>Norm</mi><mo>&ApplyFunction;</mo>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mi>p</mi>
      <mo>,</mo>
      <mfrac>
        <mrow>
          <mi>p</mi>
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
        </mrow>
      </mfrac>
    </mrow>
  </mfenced>
</mrow>
</math>
Con un po' di trasformazioni, sottraendo la media e dividendo per la deviazione standard:
<math display="block">
<mrow>
  <mfrac>
    <mrow>
      <mover accent="true">
              <mi>p</mi>
            <mo>&Hat;</mo>
      </mover>
      <mo>-</mo>
      <mi>p</mi>
    </mrow>
    <mrow>
      <msqrt>
        <mfrac>
          <mrow>
            <mi>p</mi>
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
          </mrow>
        </mfrac>
      </msqrt>
    </mrow>
  </mfrac>
  <mo>&asymp;</mo>
  <mi>Norm</mi><mo>&ApplyFunction;</mo>
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mn>0</mn>
      <mo>,</mo>
      <mn>1</mn>
    </mrow>
  </mfenced>
</mrow>
</math>
che è una normale standard di cui conosciamo le aree (probabilità) sottese. Ad es. che l'area sottesa tra -1.96 e 1.96 è il 0.95 (95%) e che quindi solo il 5% delle volte si è al di fuori di tali confini. In termini probabilistici:
<math display="block">
<mrow>
  <mi>&popf;</mi>
  <mfenced open="[" close="]" separators=",">
    <mrow>
      
        <mrow>
          <mo>|</mo>
          <mfrac>
            <mrow>
              <mover accent="true">
                      <mi>p</mi>
                    <mo>&Hat;</mo>
              </mover>
              <mo>-</mo>
              <mi>p</mi>
            </mrow>
            <mrow>
              <msqrt>
                <mfrac>
                  <mrow>
                    <mi>p</mi>
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
                  </mrow>
                </mfrac>
              </msqrt>
            </mrow>
          </mfrac>
          <mo>|</mo>
        </mrow>
      
      <mo>&geq;</mo>
      <mn>1,96</mn>
    </mrow>
  </mfenced>
  <mo>&asymp;</mo>
  <mn>0,05</mn>
  <mspace width="0.5em" />
  <mfenced open="(" close=")" separators=",">
    <mrow>
      <mtext>5%</mtext>
    </mrow>
  </mfenced>
</mrow>
</math>
Ovvero:
<math display="block">
<mrow>
  <mi>&popf;</mi>
  <mfenced open="[" close="]" separators=",">
    <mrow>
      <mo>|</mo>
      
        <mrow>
          <mfrac>
            <mrow>
              <mover accent="true">
                      <mi>p</mi>
                    <mo>&Hat;</mo>
              </mover>
              <mo>-</mo>
              <mi>p</mi>
            </mrow>
            <mrow>
              <msqrt>
                <mfrac>
                  <mrow>
                    <mi>p</mi>
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
                  </mrow>
                </mfrac>
              </msqrt>
            </mrow>
          </mfrac>
          <mo>|</mo>
        </mrow>
      
      <mo>&le;</mo>
      <mn>1,96</mn>
    </mrow>
  </mfenced>
  <mo>&asymp;</mo>
  <mn>0,95</mn>
  <mspace width="0.5em" />
  <mtext>(95%)</mtext>
</mrow>
</math>
Ovvero:
<math display="block">
<mrow>
  <mi>&popf;</mi>
  <mfenced open="[" close="]" separators=",">
    <mrow>
      <mo>|</mo>
      
        <mrow>
          <mfrac>
            <mrow>
              <mover accent="true">
                      <mi>p</mi>
                    <mo>&Hat;</mo>
              </mover>
              <mo>-</mo>
              <mi>p</mi>
            </mrow>
            <mrow>
              <msqrt>
                <mfrac>
                  <mrow>
                    <mi>p</mi>
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
                  </mrow>
                </mfrac>
              </msqrt>
            </mrow>
          </mfrac>
          <mo>|</mo>
        </mrow>
      
      <mo>&le;</mo>
      <mn>1,96</mn>
    </mrow>
  </mfenced>
  <mo>&asymp;</mo>
  <mn>0,95</mn>
  <mspace width="0.5em" />
  <mtext>(95%)</mtext>
  <mo>&rArr;</mo>
  <mi>&popf;</mi>
  
    <mrow>
      <mfenced open="[" close="]" separators=",">
        <mrow>
          <mi>-1,96</mi>
          <mo>&le;</mo>
          <mfrac>
            <mrow>
              <mover accent="true">
                      <mi>p</mi>
                    <mo>&Hat;</mo>
              </mover>
              <mo>-</mo>
              <mi>p</mi>
            </mrow>
            <mrow>
              <msqrt>
                <mfrac>
                  <mrow>
                    <mi>p</mi>
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
                  </mrow>
                </mfrac>
              </msqrt>
            </mrow>
          </mfrac>
          <mo>&le;</mo>
          <mn>1,96</mn>
        </mrow>
      </mfenced>
    </mrow>
  
  <mo>&asymp;</mo>
  <mn>0,95</mn>
  <mspace width="0.5em" />
  <mtext>(95%)</mtext>
</mrow>
</math>
da cui deriviamo la definizione di intervallo di confidenza, generalizzato 1.96 con z, il valore che assume una normale a un dato alpha/2:
<math display="block" xmlns="http://www.w3.org/1998/Math/MathML">
<mrow>
  <mi>confInt</mi>
  <mo>=</mo>
  <mfenced open="[" close="]" separators=",">
    <mrow>
      
        <mrow>
          <mover accent="true">
                  <mi>p</mi>
                <mo>&Hat;</mo>
          </mover>
          <mo>-</mo>
          <msub>
                  <mi>z</mi>
                <mfrac>
                  <mrow>
                    <mi>&alpha;</mi>
                  </mrow>
                  <mrow>
                    <mn>2</mn>
                  </mrow>
                </mfrac>
          </msub>
          <msqrt>
            <mfrac>
              <mrow>
                
                    <mi>p</mi>
                
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
              </mrow>
            </mfrac>
            <mtext>,</mtext>
            <mspace width="0.5em" />
          </msqrt>
        </mrow>
      
      <mover accent="true">
              <mi>p</mi>
            <mo>&Hat;</mo>
      </mover>
      <mo>+</mo>
      <msub>
              <mi>z</mi>
            <mfrac>
              <mrow>
                <mi>&alpha;</mi>
              </mrow>
              <mrow>
                <mn>2</mn>
              </mrow>
            </mfrac>
      </msub>
      <msqrt>
        <mfrac>
          <mrow>
            <mi>p</mi>
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
          </mrow>
        </mfrac>
      </msqrt>
    </mrow>
  </mfenced>
</mrow>
</math>
Soluzione 1
------------
Supponendo di voler calcolare l'intervallo di confidenza al 95% poniamo alfa = 0.05 e utilizziamo la funzione qnorm per valutare il valore z che corrisponda al valore che delimita un area pari ad alfa/2, in quanto essendo la distribuzione simmetrica, il 5% di area residua è distribuito in egual misura sulla cosa destra e sulla coda sinistra.

Usiamo il valore osservato di successi <math display="inline"><mrow><mover><mi>p</mi><mo>&and;</mo></mover></mrow></math> per il valore non noto _p_. L'area colorata in azzurro è alpha/2=.025
![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1.png) 


Possiamo a questo punto calcolare il Margine d'errore (ME) e quindi l'intervallo di confidenza, specificando lower.tail=FALSE in quanto siamo interessati solo al valore positivo di z (in questo modo z sarà uno scalare e non un vettore a due componenti).


```r
a <- 0.05
n <- 1000
psegnato <- 0.576
z <- qnorm(a/2, mean = 0, sd = 1, lower.tail = FALSE)
# z è uno scalare positivo, calcoliamo il margine d'errore
ME <- z * sqrt(psegnato * (1 - psegnato)/n)
psegnato + c(-ME, +ME)
```

```
## [1] 0.5454 0.6066
```

Soluzione 2
-----------
Essendo una distribuzione binomiale, utilizziamo il package binom che contiene alcune funzioni per rendere più rapida l'esecuzione, come la binom.confint.


```r
library("binom")
```

```
## Loading required package: lattice
```

```r
a <- 0.05
n <- 1000
psegnato <- 0.576
binom.confint(psegnato * 1000, n, conf.level = 1 - a, methods = "asymptotic")
```

```
##       method   x    n  mean  lower  upper
## 1 asymptotic 576 1000 0.576 0.5454 0.6066
```

Possiamo essere più conservativi in termini di margine d'errore, considerando p=1/2 (nel caso precedente abbiamo utilizzato la meno conservativa assunzione che la media reale fosse approssimabile alla media campionaria ovvero p=psegnato), dobbiamo ricalcolare il margine d'errore in base alla nuova stima per p.


```r
a <- 0.05
p <- 1/2
psegnato <- 0.576
z <- qnorm(a/2, mean = 0, sd = 1, lower.tail = FALSE)
ME <- z * sqrt(p * (1 - p)/n)
psegnato + c(-ME, +ME)
```

```
## [1] 0.545 0.607
```

