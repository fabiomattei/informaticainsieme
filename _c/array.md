---
title: 'C: gli array'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Collezioni di elementi dello stesso tipo

Un **array** in C è una sequenza di elementi dello stesso tipo, allocati
in posizioni di memoria contigue. A differenza di `std::vector` in C++,
un array in C ha **dimensione fissa**, decisa al momento della
dichiarazione, e non può crescere o ridursi.

---

## Dichiarare e usare un array

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int numeri[5] = {10, 20, 30, 40, 50};

    printf("%d\n", numeri[0]);  // 10
    printf("%d\n", numeri[4]);  // 50

    numeri[2] = 99;
    printf("%d\n", numeri[2]);  // 99

    // visita con for
    for (int i = 0; i < 5; i++) {
        printf("%d ", numeri[i]);
    }
    printf("\n");

    return 0;
}
{% endhighlight %}

Gli indici partono da 0. Accedere a un indice fuori dai limiti causa
**comportamento indefinito**: il compilatore non lo impedisce, il
programma può leggere memoria che non gli appartiene.

---

## sizeof su un array

A differenza di un puntatore, un array dichiarato localmente conosce la
propria dimensione: `sizeof(array) / sizeof(array[0])` restituisce il
numero di elementi.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int v[] = {3, 1, 4, 1, 5, 9, 2};
    int n = sizeof(v) / sizeof(v[0]);

    printf("Numero di elementi: %d\n", n);

    for (int i = 0; i < n; i++) {
        printf("%d ", v[i]);
    }
    printf("\n");

    return 0;
}
{% endhighlight %}

---

## Passare un array a una funzione

Un array passato a una funzione **decade** in un puntatore al primo
elemento: la funzione non conosce la dimensione originale, va sempre
passata a parte.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int somma(int arr[], int dimensione) {
    int totale = 0;
    for (int i = 0; i < dimensione; i++) {
        totale += arr[i];
    }
    return totale;
}

int main() {
    int numeri[5] = {1, 2, 3, 4, 5};
    printf("Somma: %d\n", somma(numeri, 5));

    return 0;
}
{% endhighlight %}

---

## Ordinamento con la bubble sort

Il C standard non offre una funzione di ordinamento pronta per array
semplici come lo `sort` del C++ (esiste `qsort`, ma richiede una
funzione di confronto). Un ordinamento semplice da implementare è la
**bubble sort**.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int v[] = {5, 2, 8, 1, 9, 3};
    int n = sizeof(v) / sizeof(v[0]);

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (v[j] > v[j + 1]) {
                int temp = v[j];
                v[j] = v[j + 1];
                v[j + 1] = temp;
            }
        }
    }

    for (int i = 0; i < n; i++) printf("%d ", v[i]);
    printf("\n");

    return 0;
}
{% endhighlight %}

---

## Leggere un array da input

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int n;
    printf("Quanti numeri? ");
    scanf("%d", &n);

    int v[100];
    for (int i = 0; i < n; i++) {
        scanf("%d", &v[i]);
    }

    int massimo = v[0];
    for (int i = 0; i < n; i++) {
        if (v[i] > massimo) massimo = v[i];
    }
    printf("Massimo: %d\n", massimo);

    return 0;
}
{% endhighlight %}

Con array a dimensione fissa dichiarati come `v[100]` bisogna sempre
assicurarsi che `n` non superi la capacità dell'array.

---

## Esercizi

#### Esercizio 6
Leggi N numeri in un array di `double` e calcola la media aritmetica.

#### Esercizio 7
Leggi N interi in un array e stampa separatamente i pari e i dispari.

#### Esercizio 8
Leggi N stringhe (parole singole) in un array di array di `char`
(`char parole[50][20]`), ordinale con bubble sort usando `strcmp` e
stampale in ordine alfabetico.

#### Esercizio 9
Implementa la ricerca lineare: leggi N interi, poi un valore da cercare;
stampa l'indice della prima occorrenza o -1 se non trovato.

#### Esercizio 10
Scrivi una funzione `void inverti(int arr[], int dimensione)` che inverte
l'ordine degli elementi in-place.
