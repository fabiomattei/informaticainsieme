---
title: 'C++: il ciclo for'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Iterare un numero noto di volte

Il ciclo `for` è la scelta naturale quando si conosce in anticipo quante
volte iterare. Raccoglie in una sola riga inizializzazione, condizione e
aggiornamento del contatore.

```
for (inizializzazione; condizione; aggiornamento) {
    // istruzioni
}
```

---

## for classico

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        cout << i << endl;
    }
    return 0;
}
{% endhighlight %}

**Traccia di esecuzione:**

| i (inizio) | condizione | output | aggiornamento |
|:----------:|:----------:|:------:|:-------------:|
| 1          | 1 ≤ 5 ✓   | 1      | i = 2         |
| 2          | 2 ≤ 5 ✓   | 2      | i = 3         |
| 3          | 3 ≤ 5 ✓   | 3      | i = 4         |
| 4          | 4 ≤ 5 ✓   | 4      | i = 5         |
| 5          | 5 ≤ 5 ✓   | 5      | i = 6         |
| 6          | 6 ≤ 5 ✗   | —      | —             |

---

## Contare all'indietro e usare step diversi

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    // conto alla rovescia
    for (int i = 5; i >= 1; i--) {
        cout << i << " ";
    }
    cout << endl;

    // solo numeri pari da 2 a 20
    for (int i = 2; i <= 20; i += 2) {
        cout << i << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## Accumulatore con for

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "Inserisci N: ";
    cin >> n;

    int somma = 0;
    for (int i = 1; i <= n; i++) {
        somma += i;
    }
    cout << "Somma da 1 a " << n << ": " << somma << endl;

    return 0;
}
{% endhighlight %}

---

## Range-based for (C++11)

Il ciclo `for` con intervallo visita ogni elemento di un contenitore
(array, vector, string) senza gestire manualmente l'indice.

```
for (tipo elemento : contenitore) { ... }
```

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numeri = {10, 20, 30, 40, 50};

    for (int n : numeri) {
        cout << n << " ";
    }
    cout << endl;

    // iterare su una stringa carattere per carattere
    string parola = "ciao";
    for (char c : parola) {
        cout << c << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## break e continue

`break` interrompe il ciclo immediatamente; `continue` salta alla
prossima iterazione.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    // break: si ferma al primo multiplo di 7
    for (int i = 1; i <= 100; i++) {
        if (i % 7 == 0) {
            cout << "Primo multiplo di 7: " << i << endl;
            break;
        }
    }

    // continue: stampa solo i dispari
    for (int i = 1; i <= 10; i++) {
        if (i % 2 == 0) continue;
        cout << i << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## Cicli annidati

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    // tavola pitagorica 5x5
    for (int i = 1; i <= 5; i++) {
        for (int j = 1; j <= 5; j++) {
            cout << i * j << "\t";
        }
        cout << endl;
    }
    return 0;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 7
Scrivi un programma che legga N e stampi i primi N numeri della
successione di Fibonacci usando un ciclo `for`.

#### Esercizio 8
Leggi 10 numeri interi con un ciclo `for` e stampa il massimo
e il minimo.

#### Esercizio 9
Stampa un triangolo rettangolo di asterischi di altezza N letta in
input: la prima riga ha 1 asterisco, la seconda 2, ecc.

#### Esercizio 10
Scrivi un programma che calcoli N! (fattoriale) con un ciclo `for`.

#### Esercizio 11
Usando cicli annidati, stampa le tabelline dalla 1 alla 10 nel formato
`1 x 1 = 1`, `1 x 2 = 2`, …
