---
title: 'C: il ciclo while'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Ripetere finché una condizione è vera

Il ciclo `while` esegue un blocco di codice ripetutamente finché la sua
condizione rimane diversa da zero (vera). La condizione viene valutata
**prima** di ogni iterazione: se è già falsa alla prima valutazione, il
blocco non viene mai eseguito.

```
while (condizione) {
    // istruzioni
}
```

---

## Contatore

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int i = 1;
    while (i <= 5) {
        printf("%d\n", i);
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

{% highlight c %}
#include <stdio.h>

int main() {
    int n;
    printf("Inserisci N: ");
    scanf("%d", &n);

    int somma = 0;
    int i = 1;
    while (i <= n) {
        somma += i;
        i++;
    }
    printf("Somma da 1 a %d: %d\n", n, somma);

    return 0;
}
{% endhighlight %}

---

## Input ripetuto fino a un valore sentinella

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.
Inserisci voti terminando con -1.

{% highlight c %}
#include <stdio.h>

int main() {
    int somma = 0, contatore = 0;
    int voto;

    printf("Inserisci i voti (-1 per terminare):\n");
    scanf("%d", &voto);

    while (voto != -1) {
        somma += voto;
        contatore++;
        scanf("%d", &voto);
    }

    if (contatore > 0) {
        double media = (double)somma / contatore;
        printf("Media: %.2f\n", media);
    } else {
        printf("Nessun voto inserito.\n");
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

{% highlight c %}
#include <stdio.h>

int main() {
    int numero;

    do {
        printf("Inserisci un numero tra 1 e 10: ");
        scanf("%d", &numero);
    } while (numero < 1 || numero > 10);

    printf("Hai inserito: %d\n", numero);

    return 0;
}
{% endhighlight %}

Con `while` puro sarebbe necessario leggere il numero prima del ciclo
e poi di nuovo dentro il ciclo. `do-while` evita questa duplicazione.

---

## Ciclo infinito con break

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int tentativo;
    const int SEGRETO = 42;

    while (1) {
        printf("Indovina il numero (1-100): ");
        scanf("%d", &tentativo);

        if (tentativo == SEGRETO) {
            printf("Corretto!\n");
            break;
        } else if (tentativo < SEGRETO) {
            printf("Troppo basso.\n");
        } else {
            printf("Troppo alto.\n");
        }
    }

    return 0;
}
{% endhighlight %}

`while (1)` è il modo classico in C di scrivere un ciclo infinito:
`1` è sempre vero.

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
finché l'utente non inserisce quella corretta ("segreto"). Usa `strcmp`.

#### Esercizio 10
Genera la successione di Fibonacci fino a un valore massimo M letto
in input: stampa tutti i termini ≤ M.
