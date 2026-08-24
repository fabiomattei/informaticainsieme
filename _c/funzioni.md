---
title: 'C: le funzioni'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Dividere il programma in blocchi riutilizzabili

Una **funzione** è un blocco di codice con un nome che può essere
chiamato più volte. In C ogni funzione deve dichiarare il **tipo del
valore restituito** e il **tipo di ogni parametro**.

```
tipo_ritorno nome(tipo param1, tipo param2) {
    // corpo
    return valore;
}
```

Se la funzione non restituisce nulla si usa il tipo speciale `void`.

---

## Definire e chiamare una funzione

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int somma(int a, int b) {
    return a + b;
}

int main() {
    int risultato = somma(3, 7);
    printf("3 + 7 = %d\n", risultato);
    printf("10 + 5 = %d\n", somma(10, 5));
    return 0;
}
{% endhighlight %}

La funzione `somma` è definita **prima** di `main`. In alternativa si può
dichiarare il **prototipo** prima di `main` e definire la funzione dopo.

---

## Funzioni void (senza valore di ritorno)

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

void saluta(char *nome) {
    printf("Ciao, %s!\n", nome);
}

int main() {
    saluta("Alice");
    saluta("Bob");
    return 0;
}
{% endhighlight %}

---

## Passaggio per valore e per puntatore

In C i parametri sono passati **sempre per valore** (default): la
funzione riceve una copia e non può modificare la variabile originale.
Per modificarla occorre passare un **puntatore** (vedi la pagina
dedicata ai [puntatori]({{ site.baseurl }}{% link _c/puntatori.md %}.html)).

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

---

## Il prototipo di funzione

Per usare una funzione definita dopo `main` occorre dichiarare il
**prototipo** in anticipo: la firma della funzione seguita da `;`,
senza corpo.

{% highlight c %}
#include <stdio.h>

int quadrato(int n);  // prototipo

int main() {
    printf("%d\n", quadrato(5));
    return 0;
}

int quadrato(int n) {  // definizione
    return n * n;
}
{% endhighlight %}

A differenza del C++, il C **non supporta l'overloading**: non possono
esistere due funzioni con lo stesso nome, anche se con parametri
diversi.

---

## Funzioni ricorsive

Una funzione può chiamare **se stessa**: si parla di ricorsione. Serve
sempre un **caso base** che interrompe le chiamate.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int fattoriale(int n) {
    if (n <= 1) {       // caso base
        return 1;
    }
    return n * fattoriale(n - 1);  // chiamata ricorsiva
}

int main() {
    printf("%d\n", fattoriale(5));   // 120
    printf("%d\n", fattoriale(0));   // 1
    return 0;
}
{% endhighlight %}

---

## Restituire più valori tramite puntatori

Una funzione C può restituire un solo valore con `return`. Per
"restituire" più valori si usano parametri passati per puntatore.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

void minMax(int arr[], int dimensione, int *min, int *max) {
    *min = arr[0];
    *max = arr[0];
    for (int i = 1; i < dimensione; i++) {
        if (arr[i] < *min) *min = arr[i];
        if (arr[i] > *max) *max = arr[i];
    }
}

int main() {
    int v[] = {5, 2, 8, 1, 9, 3};
    int minimo, massimo;

    minMax(v, 6, &minimo, &massimo);
    printf("Min: %d, Max: %d\n", minimo, massimo);

    return 0;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Scrivi una funzione `int fattoriale(int n)` iterativa (con un ciclo, non
ricorsiva) che restituisca n!. Chiamala per n = 0, 5, 10.

#### Esercizio 7
Scrivi una funzione `int isPrimo(int n)` che restituisca `1` se n è
primo, `0` altrimenti. Stampa tutti i numeri primi fino a 50.

#### Esercizio 8
Scrivi una funzione `void scambia(int *a, int *b)` che scambi i valori
di due interi passati per puntatore.

#### Esercizio 9
Scrivi una funzione ricorsiva `int fibonacci(int n)` che calcoli
l'n-esimo termine della successione di Fibonacci.

#### Esercizio 10
Scrivi una funzione `void stampaRiga(int n, char c)` che stampi n volte
il carattere c.
