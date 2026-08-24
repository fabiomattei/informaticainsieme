---
title: 'C: i puntatori'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Una variabile che contiene un indirizzo

Un **puntatore** è una variabile il cui valore è l'**indirizzo di
memoria** di un'altra variabile. I puntatori sono il concetto più
caratteristico del C: permettono di accedere e modificare direttamente
la memoria, passare dati a funzioni senza copiarli, e costruire strutture
dati dinamiche.

```
tipo *nome;      // dichiara un puntatore a "tipo"
nome = &variabile;  // & = operatore "indirizzo di"
```

---

## Dichiarare un puntatore e ottenere un indirizzo

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int eta = 25;
    int *p = &eta;   // p contiene l'indirizzo di eta

    printf("Valore di eta: %d\n", eta);
    printf("Indirizzo di eta: %p\n", (void *)&eta);
    printf("Valore di p: %p\n", (void *)p);

    return 0;
}
{% endhighlight %}

`&eta` restituisce l'indirizzo di `eta`. `p` è dichiarato come `int *`:
un puntatore a `int`. Il valore stampato con `%p` è un indirizzo
esadecimale, diverso a ogni esecuzione.

---

## L'operatore di dereferenziazione *

L'asterisco `*`, usato **davanti a un puntatore già dichiarato**, accede
al valore contenuto all'indirizzo puntato (*dereferenziazione*).

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int eta = 25;
    int *p = &eta;

    printf("%d\n", *p);  // 25 (il valore puntato da p)

    *p = 30;             // modifica eta attraverso il puntatore
    printf("%d\n", eta); // 30

    return 0;
}
{% endhighlight %}

`*p = 30` scrive `30` all'indirizzo contenuto in `p`, cioè modifica
direttamente `eta`. Nota che `*` ha due significati diversi: nella
dichiarazione (`int *p`) indica il **tipo** puntatore; nell'uso (`*p`)
indica **dereferenziazione**.

---

## Puntatori e funzioni: passaggio per riferimento

In C i parametri sono passati **sempre per valore**: la funzione riceve
una copia. Per permettere a una funzione di modificare una variabile del
chiamante, occorre passarle un **puntatore**.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

void raddoppiaValore(int x) {
    x *= 2;  // modifica solo la copia locale
}

void raddoppiaPuntatore(int *x) {
    *x *= 2;  // modifica la variabile originale
}

int main() {
    int a = 5;
    raddoppiaValore(a);
    printf("%d\n", a);  // 5 (invariato)

    raddoppiaPuntatore(&a);
    printf("%d\n", a);  // 10 (modificato)

    return 0;
}
{% endhighlight %}

Questo è anche il motivo per cui `scanf("%d", &eta)` richiede `&`:
`scanf` deve poter **scrivere** dentro `eta`, quindi ha bisogno del suo
indirizzo.

---

## Puntatori e array

Il nome di un array, usato in un'espressione, **decade** in un puntatore
al suo primo elemento. Per questo un array può essere scandito anche con
l'aritmetica dei puntatori.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int numeri[5] = {10, 20, 30, 40, 50};
    int *p = numeri;   // equivalente a p = &numeri[0]

    printf("%d\n", *p);        // 10
    printf("%d\n", *(p + 1));  // 20 (aritmetica dei puntatori)
    printf("%d\n", p[2]);      // 30 (notazione ad array su un puntatore)

    for (int i = 0; i < 5; i++) {
        printf("%d ", *(numeri + i));
    }
    printf("\n");

    return 0;
}
{% endhighlight %}

`p + 1` non aggiunge 1 byte, ma **la dimensione di un elemento** del
tipo puntato (qui 4 byte, la dimensione di `int`).

---

## Il puntatore NULL

Un puntatore che non punta a nulla dovrebbe essere inizializzato a
`NULL`, per poterlo controllare prima di usarlo.

{% highlight c %}
#include <stdio.h>
#include <stddef.h>

int main() {
    int *p = NULL;

    if (p == NULL) {
        printf("Il puntatore non punta a nulla.\n");
    }

    return 0;
}
{% endhighlight %}

Dereferenziare un puntatore `NULL` (`*p`) causa un crash del programma:
va sempre controllato prima dell'uso.

---

## Esercizi

#### Esercizio 5
Dichiara una variabile intera e un puntatore ad essa. Stampa il valore
della variabile sia direttamente sia tramite dereferenziazione del
puntatore.

#### Esercizio 6
Scrivi una funzione `void scambia(int *a, int *b)` che scambi i valori
di due interi passati per puntatore. Chiamala da `main` su due variabili.

#### Esercizio 7
Scrivi una funzione `int sommaArray(int *arr, int dimensione)` che
sommi gli elementi di un array usando l'aritmetica dei puntatori
(`*(arr + i)`) invece della notazione `arr[i]`.

#### Esercizio 8
Scrivi una funzione `int *trovaMassimo(int *arr, int dimensione)` che
restituisca un puntatore all'elemento massimo dell'array (non il suo
valore).
