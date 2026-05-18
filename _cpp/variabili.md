---
title: 'C++: le variabili'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Un cassetto etichettato con nome e tipo

In C++ ogni variabile ha un **nome**, un **tipo** e un **valore**. Il tipo deve
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

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int eta = 25;
    double altezza = 1.75;
    bool attivo = true;

    cout << "Età: " << eta << endl;
    cout << "Altezza: " << altezza << endl;
    cout << "Attivo: " << attivo << endl;

    return 0;
}
{% endhighlight %}

I tipi fondamentali di C++ sono: `int` (intero), `double` (decimale),
`bool` (vero/falso), `char` (carattere), `string` (testo).

---

## Operatore di assegnazione e operatori composti

L'operatore `=` assegna un valore a una variabile. Gli operatori composti
`+=`, `-=`, `*=`, `/=`, `%=` modificano il valore corrente.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int punti = 10;
    cout << "Inizio: " << punti << endl;

    punti += 5;
    cout << "Dopo +=5: " << punti << endl;

    punti *= 2;
    cout << "Dopo *=2: " << punti << endl;

    punti -= 3;
    cout << "Dopo -=3: " << punti << endl;

    return 0;
}
{% endhighlight %}

---

## Incremento e decremento

Gli operatori `++` e `--` aumentano o diminuiscono di 1 il valore di una
variabile intera. Esistono in forma **prefissa** (`++i`) e **postfissa** (`i++`).

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int i = 5;

    cout << i++ << endl;  // stampa 5, poi i diventa 6
    cout << i   << endl;  // stampa 6
    cout << ++i << endl;  // i diventa 7, poi stampa 7

    return 0;
}
{% endhighlight %}

Con `i++` il valore viene usato *prima* di essere incrementato.
Con `++i` il valore viene incrementato *prima* di essere usato.

---

## Costanti

Una costante si dichiara con `const`. Il compilatore impedisce qualsiasi
modifica successiva.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    const double PI = 3.14159;
    double raggio = 5.0;
    double area = PI * raggio * raggio;

    cout << "Area del cerchio: " << area << endl;

    return 0;
}
{% endhighlight %}

---

## Traccia di esecuzione

Per il codice seguente, segui il valore di `x` passo per passo.

{% highlight cpp %}
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
