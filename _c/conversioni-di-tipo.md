---
title: 'C: conversioni di tipo'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Conversioni implicite ed esplicite

In C è possibile passare da un tipo a un altro in due modi:

- **Implicita** — il compilatore la esegue automaticamente quando non c'è
  perdita di informazione (es. da `int` a `double`).
- **Esplicita** — il programmatore la richiede con il **cast** quando
  potrebbe esserci perdita di informazione (es. da `double` a `int`).

---

## Conversione implicita (widening)

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int    interi = 7;
    double decimale = interi;  // conversione implicita int -> double

    printf("%.1f\n", decimale);  // 7.0

    double pi = 3.14;
    int    tronco = pi;          // conversione implicita double -> int
    printf("%d\n", tronco);      // 3  (la parte decimale viene scartata)

    return 0;
}
{% endhighlight %}

La conversione da `double` a `int` tronca la parte decimale **senza
arrotondare**. Il compilatore può mostrare un avviso (*warning*).

---

## Il cast esplicito

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int a = 7, b = 2;

    // divisione intera: il risultato è 3, non 3.5
    printf("%d\n", a / b);

    // per ottenere 3.5 si converte uno dei due operandi
    double risultato = (double)a / b;
    printf("%.1f\n", risultato);  // 3.5

    return 0;
}
{% endhighlight %}

`(tipo)valore` è la sintassi del cast in C: si scrive il tipo di
destinazione tra parentesi tonde subito prima del valore da convertire.

---

## Da stringa a numero: atoi, atof, strtol

Per convertire una stringa che rappresenta un numero si usano funzioni
della libreria `<stdlib.h>`.

| Funzione       | Converte in | Esempio                        |
|----------------|-------------|---------------------------------|
| `atoi(s)`      | `int`       | `atoi("42")` → `42`            |
| `atof(s)`      | `double`    | `atof("3.14")` → `3.14`        |
| `strtol(s,...)`| `long`      | conversione con controllo errori|

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>
#include <stdlib.h>

int main() {
    char s1[] = "42";
    char s2[] = "3.14";

    int    n = atoi(s1);
    double d = atof(s2);

    printf("%d\n", n + 1);  // 43
    printf("%.2f\n", d * 2); // 6.28

    return 0;
}
{% endhighlight %}

A differenza di `strtol`, `atoi` e `atof` non segnalano errori se la
stringa non è un numero valido: restituiscono semplicemente `0`.

---

## Da numero a stringa: sprintf

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int    punteggio = 100;
    double media     = 7.5;
    char   messaggio[50];

    sprintf(messaggio, "Punteggio: %d", punteggio);
    printf("%s\n", messaggio);

    sprintf(messaggio, "Media: %.1f", media);
    printf("%s\n", messaggio);

    return 0;
}
{% endhighlight %}

`sprintf` funziona come `printf` ma scrive il risultato in un array di
`char` invece che sullo schermo.

---

## Convertire char ↔ int

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    char c = 'A';
    int  codice = (int)c;
    printf("%d\n", codice);   // 65

    int  num = 97;
    char lettera = (char)num;
    printf("%c\n", lettera);  // a

    return 0;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Leggi due interi con `scanf` e stampa la loro divisione come numero
decimale (non intera). Usa un cast per forzare la divisione reale.

#### Esercizio 7
Leggi una stringa dall'utente che rappresenta un prezzo (es. "12.50"),
convertila in `double` con `atof` e stampa il prezzo con IVA al 22%.

#### Esercizio 8
Leggi un numero intero con `scanf`. Convertilo in stringa con `sprintf`
e stampa il risultato con `printf("%s\n", ...)`.

#### Esercizio 9
Scrivi un programma che converta gradi Celsius (interi, letti con
`scanf`) in Fahrenheit (double). Formula: `F = C * 9.0 / 5 + 32`.
Spiega perché è necessario scrivere `9.0` e non `9`.
