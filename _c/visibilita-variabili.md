---
title: 'C: visibilità delle variabili'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Dove "vive" una variabile

La **visibilità** (o *scope*) di una variabile è la porzione di codice in
cui quella variabile esiste ed è accessibile. In C lo scope è definito
dalle **parentesi graffe** `{ }`: una variabile dichiarata all'interno di
un blocco è visibile solo in quel blocco e cessa di esistere quando il
blocco termina.

---

## Variabili locali

Una variabile dichiarata dentro una funzione (o un blocco) è **locale**:
non esiste fuori da quel contesto.

#### Esercizio 1
Copia il seguente codice nell'editor e osserva cosa succede.

{% highlight c %}
#include <stdio.h>

void funzione() {
    int x = 10;  // x esiste solo dentro funzione()
    printf("%d\n", x);
}

int main() {
    funzione();
    // printf("%d\n", x);  // ERRORE: x non esiste qui
    return 0;
}
{% endhighlight %}

---

## Scope di blocco

Le parentesi graffe di `if`, `for`, `while` creano uno scope. Una
variabile dichiarata dentro non è visibile fuori.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int x = 5;
    printf("fuori: %d\n", x);   // 5

    {
        int x = 100;  // shadowing: nuova x locale al blocco
        printf("dentro: %d\n", x);   // 100
    }

    printf("fuori: %d\n", x);   // 5 (la x esterna è invariata)

    return 0;
}
{% endhighlight %}

Dichiarare una variabile con lo stesso nome di una variabile esterna si
chiama **shadowing** e può generare confusione. È meglio evitarlo.

---

## Variabili globali

Una variabile dichiarata fuori da qualsiasi funzione è **globale**: è
visibile in tutte le funzioni del file. Il loro uso è generalmente
sconsigliato perché rendono il codice difficile da tracciare.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int contatore = 0;  // variabile globale

void incrementa() {
    contatore++;
}

int main() {
    incrementa();
    incrementa();
    incrementa();
    printf("%d\n", contatore);  // 3
    return 0;
}
{% endhighlight %}

---

## static: una variabile locale che ricorda il suo valore

Una variabile locale dichiarata `static` mantiene il proprio valore tra
una chiamata e l'altra della funzione, invece di essere ricreata ogni
volta.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

void conta() {
    static int chiamate = 0;  // inizializzata una sola volta
    chiamate++;
    printf("Chiamata numero: %d\n", chiamate);
}

int main() {
    conta();  // Chiamata numero: 1
    conta();  // Chiamata numero: 2
    conta();  // Chiamata numero: 3
    return 0;
}
{% endhighlight %}

Senza `static`, `chiamate` verrebbe azzerata a ogni chiamata e il
programma stamperebbe sempre "Chiamata numero: 1".

---

## Variabili della stessa funzione vs blocchi interni

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    for (int i = 0; i < 3; i++) {
        int quadrato = i * i;  // dichiarata a ogni iterazione
        printf("%d^2 = %d\n", i, quadrato);
    }
    // i e quadrato non esistono qui

    int somma = 0;
    for (int i = 1; i <= 5; i++) {
        somma += i;  // somma è dichiarata fuori: rimane
    }
    printf("Somma: %d\n", somma);  // 15

    return 0;
}
{% endhighlight %}

`i` del ciclo `for` esiste solo dentro il ciclo. `somma` è dichiarata
fuori e persiste dopo il ciclo.

---

## Esercizi

#### Esercizio 6
Scrivi un programma con una funzione `conta()` che usa una variabile
`static` per contare quante volte è stata chiamata. Poi confronta con
una versione che usa una variabile globale. Qual è la differenza?

#### Esercizio 7
Scrivi un programma con tre blocchi annidati `{ }`, ognuno con una
variabile `n` con valore diverso. Stampa `n` in ciascun blocco e spiega
quale `n` viene usata.

#### Esercizio 8
Scrivi una funzione `int massimoDiArray(int arr[], int dimensione)` che
trovi il massimo. La variabile `massimo` deve essere locale alla
funzione. Chiama la funzione da `main` su tre array diversi.
