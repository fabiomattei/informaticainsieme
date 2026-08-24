---
title: 'C: le struct'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Raggruppare dati correlati

Una **struct** (struttura) raggruppa più variabili di tipi diversi sotto
un unico nome. È utile quando si vuole trattare un insieme di dati
correlati come una singola entità — ad esempio le coordinate di un
punto, i dati di uno studente, le proprietà di un prodotto.

A differenza del C++, in C una struct **non può contenere funzioni**:
solo dati.

---

## Definire e usare una struct

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

struct Punto {
    double x;
    double y;
};

int main() {
    struct Punto p1;
    p1.x = 3.0;
    p1.y = 4.0;

    struct Punto p2 = {1.0, 2.0};  // inizializzazione aggregata

    printf("p1: (%.1f, %.1f)\n", p1.x, p1.y);
    printf("p2: (%.1f, %.1f)\n", p2.x, p2.y);

    return 0;
}
{% endhighlight %}

I campi di una struct si accedono con il punto `.`. Per usare la
struct occorre scrivere `struct Punto`, non solo `Punto` — a meno di
usare `typedef` (vedi sotto).

---

## typedef: dare un nome più corto alla struct

`typedef` permette di usare il nome della struct senza ripetere la
parola chiave `struct` ogni volta.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

typedef struct {
    char nome[30];
    char cognome[30];
    int  eta;
} Persona;

int main() {
    Persona mario = {"Mario", "Rossi", 25};
    printf("%s %s, %d anni\n", mario.nome, mario.cognome, mario.eta);

    mario.eta = 26;
    printf("Nuova età: %d\n", mario.eta);

    return 0;
}
{% endhighlight %}

---

## Array di struct

Una delle applicazioni più comuni: un array di struct per rappresentare
una tabella di dati.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

typedef struct {
    char nome[20];
    int  voto;
} Studente;

int main() {
    Studente classe[4] = {
        {"Alice",  9},
        {"Bruno",  6},
        {"Carla",  8},
        {"Davide", 7}
    };

    for (int i = 0; i < 4; i++) {
        printf("%s: %d\n", classe[i].nome, classe[i].voto);
    }

    // trova il migliore
    int indiceMigliore = 0;
    for (int i = 0; i < 4; i++) {
        if (classe[i].voto > classe[indiceMigliore].voto) {
            indiceMigliore = i;
        }
    }
    printf("Migliore: %s (%d)\n", classe[indiceMigliore].nome, classe[indiceMigliore].voto);

    return 0;
}
{% endhighlight %}

---

## Passare una struct a una funzione

Passare una struct **per valore** copia tutti i suoi campi: se la
struct è grande, è più efficiente passare un **puntatore**.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

typedef struct {
    double base;
    double altezza;
} Rettangolo;

double area(Rettangolo r) {
    return r.base * r.altezza;
}

double perimetro(const Rettangolo *r) {
    return 2 * (r->base + r->altezza);
}

int main() {
    Rettangolo r = {5.0, 3.0};
    printf("Area: %.1f\n", area(r));
    printf("Perimetro: %.1f\n", perimetro(&r));

    return 0;
}
{% endhighlight %}

Con un puntatore a struct si usa l'operatore **freccia** `->` invece del
punto: `r->base` equivale a `(*r).base`. `const` impedisce alla
funzione di modificare la struct originale attraverso il puntatore.

---

## Modificare una struct tramite puntatore

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

typedef struct {
    double x;
    double y;
} Punto;

void trasla(Punto *p, double dx, double dy) {
    p->x += dx;
    p->y += dy;
}

int main() {
    Punto p = {1.0, 2.0};
    trasla(&p, 3.0, -1.0);
    printf("(%.1f, %.1f)\n", p.x, p.y);  // (4.0, 1.0)

    return 0;
}
{% endhighlight %}

Per modificare una struct dentro una funzione bisogna sempre passarle
un **puntatore**, esattamente come per le variabili semplici.

---

## Esercizi

#### Esercizio 6
Definisci (con `typedef`) una struct `Prodotto` con campi `nome`,
`prezzo` e `quantita`. Crea un array di 5 prodotti, calcola il valore
totale in magazzino (`prezzo * quantita`) e stampa il prodotto più
costoso.

#### Esercizio 7
Definisci una struct `Data` con campi `giorno`, `mese` e `anno`.
Scrivi una funzione `void stampaData(const Data *d)` che stampi nel
formato `GG/MM/AAAA`.

#### Esercizio 8
Definisci una struct `Vettore2D` con campi `x` e `y`. Scrivi le funzioni
`Vettore2D somma(Vettore2D a, Vettore2D b)` e `double modulo(Vettore2D v)`.
Testa con due vettori a scelta.
