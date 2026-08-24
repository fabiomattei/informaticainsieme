---
title: 'C: input e output'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## printf e scanf

In C l'output avviene tramite la funzione `printf` e l'input tramite
`scanf`. Entrambe fanno parte della libreria standard ed è necessario
includere `<stdio.h>`.

Entrambe usano una **stringa di formato** con dei segnaposto (`%d`, `%f`,
`%c`, `%s`, ...) che indicano il tipo del dato da stampare o leggere.

---

## Output con printf

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    printf("Ciao, mondo!\n");
    printf("La risposta è: %d\n", 42);

    int eta = 25;
    printf("Hai %d anni.\n", eta);

    return 0;
}
{% endhighlight %}

| Segnaposto | Tipo               |
|:----------:|---------------------|
| `%d`       | `int`               |
| `%ld`      | `long`              |
| `%f`       | `float` / `double`  |
| `%c`       | `char`              |
| `%s`       | stringa (`char[]`)  |

`\n` manda a capo. A differenza di alcuni linguaggi, `printf` non svuota
automaticamente il buffer: si può forzarlo con `fflush(stdout)`.

---

## Input con scanf

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int eta;
    printf("Inserisci la tua età: ");
    scanf("%d", &eta);
    printf("Hai %d anni.\n", eta);

    return 0;
}
{% endhighlight %}

`scanf("%d", &eta)` legge un valore dallo standard input e lo memorizza
nella variabile `eta`. L'operatore `&` è **obbligatorio**: `scanf` ha
bisogno dell'**indirizzo** della variabile per poterci scrivere dentro.

---

## Leggere più valori

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int base, altezza;
    printf("Inserisci base e altezza: ");
    scanf("%d %d", &base, &altezza);

    int area = base * altezza;
    printf("Area del rettangolo: %d\n", area);

    return 0;
}
{% endhighlight %}

Si possono leggere più valori in una sola chiamata: `scanf("%d %d", &a, &b)`
legge due interi separati da spazio o da invio.

---

## Leggere una riga intera con fgets

`scanf("%s", ...)` si ferma al primo spazio. Per leggere una riga intera
(con spazi) si usa `fgets`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    char nome[100];
    printf("Inserisci il tuo nome completo: ");
    fgets(nome, sizeof(nome), stdin);
    printf("Benvenuto, %s!\n", nome);

    return 0;
}
{% endhighlight %}

`fgets` legge fino a `\n` incluso o fino a riempire il buffer. Il
carattere di invio resta nella stringa: spesso va rimosso manualmente.

---

## Riepilogo: differenze tra scanf("%s", ...) e fgets

| Caratteristica            | `scanf("%s", ...)`    | `fgets`                |
|---------------------------|------------------------|-------------------------|
| Si ferma a                | spazio, tab, invio     | solo invio o fine buffer|
| Legge più parole           | no                     | sì                      |
| Rischio di overflow        | sì (nessun limite)     | no (limite esplicito)   |

---

## Esercizi

#### Esercizio 5
Scrivi un programma che legga nome e cognome separatamente (due `scanf`
con `%s`) e li stampi nella forma "Cognome, Nome".

#### Esercizio 6
Leggi due numeri decimali (`double`, formato `%lf`) e stampa la loro
somma, differenza e prodotto.

#### Esercizio 7
Leggi il raggio di un cerchio e stampa area e circonferenza.
Usa `const double PI = 3.14159;`.

#### Esercizio 8
Leggi una frase intera con `fgets` e stampala al contrario carattere per
carattere (suggerimento: usa un ciclo con indice decrescente e `frase[i]`).

#### Esercizio 9
Scrivi un programma che legga tre interi, calcoli la loro media come
`double` e la stampi con due cifre decimali
(suggerimento: `printf("%.2f\n", media)`).
