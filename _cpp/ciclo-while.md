---
title: 'C++: il ciclo while'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Ripetere finché una condizione è vera

Il ciclo `while` esegue un blocco di codice ripetutamente finché la sua
condizione rimane vera. La condizione viene valutata **prima** di ogni
iterazione: se è già falsa alla prima valutazione, il blocco non viene
mai eseguito.

```
while (condizione) {
    // istruzioni
}
```

---

## Contatore

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int i = 1;
    while (i <= 5) {
        cout << i << endl;
        i++;
    }
    return 0;
}
{% endhighlight %}

**Traccia di esecuzione:**

| Iterazione | i (inizio) | condizione | output | i (fine) |
|:----------:|:----------:|:----------:|:------:|:--------:|
| 1          | 1          | 1 ≤ 5 ✓   | 1      | 2        |
| 2          | 2          | 2 ≤ 5 ✓   | 2      | 3        |
| 3          | 3          | 3 ≤ 5 ✓   | 3      | 4        |
| 4          | 4          | 4 ≤ 5 ✓   | 4      | 5        |
| 5          | 5          | 5 ≤ 5 ✓   | 5      | 6        |
| —          | 6          | 6 ≤ 5 ✗   | —      | —        |

---

## Accumulatore

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "Inserisci N: ";
    cin >> n;

    int somma = 0;
    int i = 1;
    while (i <= n) {
        somma += i;
        i++;
    }
    cout << "Somma da 1 a " << n << ": " << somma << endl;

    return 0;
}
{% endhighlight %}

---

## Input ripetuto fino a un valore sentinella

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.
Inserisci voti terminando con -1.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int somma = 0, contatore = 0;
    int voto;

    cout << "Inserisci i voti (-1 per terminare):" << endl;
    cin >> voto;

    while (voto != -1) {
        somma += voto;
        contatore++;
        cin >> voto;
    }

    if (contatore > 0) {
        double media = static_cast<double>(somma) / contatore;
        cout << "Media: " << media << endl;
    } else {
        cout << "Nessun voto inserito." << endl;
    }

    return 0;
}
{% endhighlight %}

---

## Il ciclo do-while

`do-while` esegue il blocco **almeno una volta** e poi controlla la
condizione. Utile per la validazione dell'input.

```
do {
    // istruzioni
} while (condizione);
```

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int numero;

    do {
        cout << "Inserisci un numero tra 1 e 10: ";
        cin >> numero;
    } while (numero < 1 || numero > 10);

    cout << "Hai inserito: " << numero << endl;

    return 0;
}
{% endhighlight %}

Con `while` puro sarebbe necessario leggere il numero prima del ciclo
e poi di nuovo dentro il ciclo. `do-while` evita questa duplicazione.

---

## Ciclo infinito con break

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int tentativo;
    const int SEGRETO = 42;

    while (true) {
        cout << "Indovina il numero (1-100): ";
        cin >> tentativo;

        if (tentativo == SEGRETO) {
            cout << "Corretto!" << endl;
            break;
        } else if (tentativo < SEGRETO) {
            cout << "Troppo basso." << endl;
        } else {
            cout << "Troppo alto." << endl;
        }
    }

    return 0;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Scrivi un programma che stampi la tavola pitagorica di un numero N
letto in input, usando un ciclo `while`.

#### Esercizio 7
Leggi numeri positivi finché l'utente inserisce 0. Stampa il massimo
tra i numeri letti.

#### Esercizio 8
Calcola il fattoriale di N usando un ciclo `while`.

#### Esercizio 9
Scrivi un programma che legga una password (stringa) con `do-while`
finché l'utente non inserisce quella corretta ("segreto").

#### Esercizio 10
Genera la successione di Fibonacci fino a un valore massimo M letto
in input: stampa tutti i termini ≤ M.
