---
title: 'C: le variabili'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Un cassetto etichettato con nome e tipo

In C ogni variabile ha un **nome**, un **tipo** e un **valore**. Il tipo deve
essere dichiarato esplicitamente: il compilatore usa questa informazione per
riservare la giusta quantità di memoria e rilevare errori prima dell'esecuzione.

```
tipo nome = valore;
int eta = 25;
```

A differenza di Ruby e Python, non è possibile usare una variabile non dichiarata:
il compilatore segnala un errore.

---

## Dichiarare e inizializzare una variabile

#### Esercizio 1
Copia il seguente codice nell'editor e fallo compilare ed eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int eta = 25;
    double altezza = 1.75;
    char iniziale = 'M';

    printf("Età: %d\n", eta);
    printf("Altezza: %.2f\n", altezza);
    printf("Iniziale: %c\n", iniziale);

    return 0;
}
{% endhighlight %}

I tipi fondamentali di C sono: `int` (intero), `double` (decimale),
`char` (carattere). Non esiste un tipo `bool` nativo: si usa `int`
(0 = falso, diverso da 0 = vero) oppure `#include <stdbool.h>`.

---

## Operatore di assegnazione e operatori composti

L'operatore `=` assegna un valore a una variabile. Gli operatori composti
`+=`, `-=`, `*=`, `/=`, `%=` modificano il valore corrente.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int punti = 10;
    printf("Inizio: %d\n", punti);

    punti += 5;
    printf("Dopo +=5: %d\n", punti);

    punti *= 2;
    printf("Dopo *=2: %d\n", punti);

    punti -= 3;
    printf("Dopo -=3: %d\n", punti);

    return 0;
}
{% endhighlight %}

---

## Incremento e decremento

Gli operatori `++` e `--` aumentano o diminuiscono di 1 il valore di una
variabile intera. Esistono in forma **prefissa** (`++i`) e **postfissa** (`i++`).

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int i = 5;

    printf("%d\n", i++);  // stampa 5, poi i diventa 6
    printf("%d\n", i);    // stampa 6
    printf("%d\n", ++i);  // i diventa 7, poi stampa 7

    return 0;
}
{% endhighlight %}

Con `i++` il valore viene usato *prima* di essere incrementato.
Con `++i` il valore viene incrementato *prima* di essere usato.

---

## Costanti

Una costante si dichiara con `const`, oppure con la direttiva del
preprocessore `#define`. Il compilatore impedisce qualsiasi modifica
successiva a una variabile `const`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

#define IVA 0.22

int main() {
    const double PI = 3.14159;
    double raggio = 5.0;
    double area = PI * raggio * raggio;

    printf("Area del cerchio: %.2f\n", area);
    printf("IVA: %.2f\n", IVA);

    return 0;
}
{% endhighlight %}

`#define` sostituisce il nome con il valore **prima** della compilazione
(preprocessore); `const` crea invece una vera variabile a sola lettura.

---

## Traccia di esecuzione

Per il codice seguente, segui il valore di `x` passo per passo.

{% highlight c %}
int x = 4;
x += 3;
x *= 2;
x -= 5;
{% endhighlight %}

| Istruzione | x    |
|------------|-----:|
| `int x = 4` | 4   |
| `x += 3`    | 7   |
| `x *= 2`    | 14  |
| `x -= 5`    | 9   |

---

## Esercizi

#### Esercizio 5
Dichiara una variabile intera `base` e una `altezza`, inizializzale con
valori a scelta, calcola e stampa `area = base * altezza`.

#### Esercizio 6
Dichiara una variabile `contatore = 0`. Incrementala di 1 per cinque volte
usando `++` e stampa il valore finale.

#### Esercizio 7
Dichiara due variabili intere e scambia i loro valori usando una terza
variabile temporanea. Stampa i valori prima e dopo lo scambio.

#### Esercizio 8
Dichiara una costante `IVA = 0.22`. Leggi un prezzo netto e stampa il
prezzo lordo (prezzo * (1 + IVA)).

#### Esercizio 9
Dichiara le variabili `a = 10`, `b = 3`. Stampa il risultato di `a / b`
(divisione intera) e di `a % b` (resto). Spiega il risultato.
