---
title: 'C++: visibilità delle variabili'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Dove "vive" una variabile

La **visibilità** (o *scope*) di una variabile è la porzione di codice in
cui quella variabile esiste ed è accessibile. In C++ lo scope è definito
dalle **parentesi graffe** `{ }`: una variabile dichiarata all'interno di
un blocco è visibile solo in quel blocco e cessa di esistere quando il
blocco termina.

---

## Variabili locali

Una variabile dichiarata dentro una funzione (o un blocco) è **locale**:
non esiste fuori da quel contesto.

#### Esercizio 1
Copia il seguente codice nell'editor e osserva cosa succede.

{% highlight cpp %}
#include <iostream>
using namespace std;

void funzione() {
    int x = 10;  // x esiste solo dentro funzione()
    cout << x << endl;
}

int main() {
    funzione();
    // cout << x << endl;  // ERRORE: x non esiste qui
    return 0;
}
{% endhighlight %}

---

## Scope di blocco

Le parentesi graffe di `if`, `for`, `while` creano uno scope. Una
variabile dichiarata dentro non è visibile fuori.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int x = 5;
    cout << "fuori: " << x << endl;   // 5

    {
        int x = 100;  // shadowing: nuova x locale al blocco
        cout << "dentro: " << x << endl;   // 100
    }

    cout << "fuori: " << x << endl;   // 5 (la x esterna è invariata)

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

{% highlight cpp %}
#include <iostream>
using namespace std;

int contatore = 0;  // variabile globale

void incrementa() {
    contatore++;
}

int main() {
    incrementa();
    incrementa();
    incrementa();
    cout << contatore << endl;  // 3
    return 0;
}
{% endhighlight %}

---

## Variabili della stessa funzione vs blocchi interni

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    for (int i = 0; i < 3; i++) {
        int quadrato = i * i;  // dichiarata a ogni iterazione
        cout << i << "^2 = " << quadrato << endl;
    }
    // i e quadrato non esistono qui

    int somma = 0;
    for (int i = 1; i <= 5; i++) {
        somma += i;  // somma è dichiarata fuori: rimane
    }
    cout << "Somma: " << somma << endl;  // 15

    return 0;
}
{% endhighlight %}

`i` del ciclo `for` esiste solo dentro il ciclo. `somma` è dichiarata
fuori e persiste dopo il ciclo.

---

## Esercizi

#### Esercizio 5
Scrivi un programma con una funzione `conta()` che incrementa e stampa
una variabile locale ogni volta che viene chiamata. Poi confronta con
una versione che usa una variabile globale. Qual è la differenza?

#### Esercizio 6
Scrivi un programma con tre blocchi annidati `{ }`, ognuno con una
variabile `n` con valore diverso. Stampa `n` in ciascun blocco e spiega
quale `n` viene usata.

#### Esercizio 7
Scrivi una funzione `massimoDiArray(int arr[], int size)` che trovi il
massimo. La variabile `massimo` deve essere locale alla funzione.
Chiama la funzione da `main` su tre array diversi.
