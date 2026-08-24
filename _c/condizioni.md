---
title: 'C: le condizioni'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Eseguire codice solo in certi casi

Le istruzioni condizionali permettono di scegliere quale blocco di codice
eseguire in base al valore di un'espressione. In C le parole chiave sono
`if`, `else if`, `else` e `switch`.

Il blocco di codice da eseguire è racchiuso tra **parentesi graffe** `{ }`.

## if semplice

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int numero;
    printf("Inserisci un numero: ");
    scanf("%d", &numero);

    if (numero > 0) {
        printf("Il numero è positivo.\n");
    }

    return 0;
}
{% endhighlight %}

Il programma, mandato in esecuzione, si aspetta che l'utente inserisca un input.
L'input deve essere un numero, altrimenti il comportamento non è garantito.
Il valore inserito in input dall'utente viene assegnato alla variabile di tipo **int** chiamata **numero**.
L'espressione `numero > 0` ha come risultato il valore `1` se il numero inserito dall'utente è maggiore di zero, e `0` in caso il numero inserito ha valore uguale a zero o minore di zero.

Il blocco, cioè le istruzioni, inserite all'interno delle parentesi graffe `{ }` vengono eseguite soltanto se la condizione tra `( )` è diversa da zero.

## if / else

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int numero;
    printf("Inserisci un numero: ");
    scanf("%d", &numero);

    if (numero % 2 == 0) {
        printf("Il numero è pari.\n");
    } else {
        printf("Il numero è dispari.\n");
    }

    return 0;
}
{% endhighlight %}

## if / else if / else

Quando le alternative sono più di due si concatenano `else if`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int voto;
    printf("Inserisci il voto (0-10): ");
    scanf("%d", &voto);

    if (voto >= 9) {
        printf("Ottimo\n");
    } else if (voto >= 7) {
        printf("Buono\n");
    } else if (voto >= 6) {
        printf("Sufficiente\n");
    } else {
        printf("Insufficiente\n");
    }

    return 0;
}
{% endhighlight %}

Le condizioni vengono valutate dall'alto verso il basso: appena una è vera,
il relativo blocco viene eseguito e le successive vengono saltate.

## switch / case

`switch` è utile quando si deve confrontare un valore intero o `char`
con un insieme fisso di costanti. Ogni caso termina con `break` per
impedire la caduta al caso successivo (*fall-through*).

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int giorno;
    printf("Inserisci il numero del giorno (1-7): ");
    scanf("%d", &giorno);

    switch (giorno) {
        case 1: printf("Lunedì\n");    break;
        case 2: printf("Martedì\n");   break;
        case 3: printf("Mercoledì\n"); break;
        case 4: printf("Giovedì\n");   break;
        case 5: printf("Venerdì\n");   break;
        case 6: printf("Sabato\n");    break;
        case 7: printf("Domenica\n");  break;
        default: printf("Giorno non valido\n");
    }

    return 0;
}
{% endhighlight %}

`default` è il caso che scatta quando nessun `case` corrisponde.

## L'operatore ternario

Per assegnare un valore in base a una condizione si può usare l'operatore
ternario `condizione ? valore_se_vero : valore_se_falso`.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int a, b;
    printf("Inserisci due numeri: ");
    scanf("%d %d", &a, &b);

    int massimo = (a > b) ? a : b;
    printf("Il massimo è: %d\n", massimo);

    return 0;
}
{% endhighlight %}

## Esercizi

#### Esercizio 6
Leggi un numero intero e stampa "positivo", "negativo" o "zero" a seconda
del caso.

#### Esercizio 7
Leggi un anno e determina se è bisestile.
(Condizione: divisibile per 4, eccetto i centenari, che devono essere
divisibili per 400.)

#### Esercizio 8
Leggi tre interi e stampa il maggiore dei tre usando `if/else if/else`.

#### Esercizio 9
Scrivi un calcolatore semplice: leggi due numeri e un carattere operatore
(`+`, `-`, `*`, `/`). Usa `switch` per eseguire l'operazione scelta.
Gestisci la divisione per zero.

#### Esercizio 10
Leggi la temperatura (double) e classifica il tempo:
sotto 0 = "gelido", 0–10 = "freddo", 11–20 = "fresco", 21–30 = "caldo",
sopra 30 = "torrido".
