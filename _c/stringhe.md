---
title: 'C: le stringhe di testo'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Array di char terminati da \0

In C non esiste un vero e proprio tipo stringa. Una stringa è un **array
di `char`** che come ultimo carattere ha il **terminatore**: `'\0'`
(carattere nullo). La gestione delle stringhe è affidata a una serie di
funzioni contenute in `<string.h>` e `<stdlib.h>`.

---

## Dichiarare una stringa

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    char nome1[] = "Mario";       // dimensione dedotta automaticamente
    char nome2[20] = "Luigi";     // dimensione fissata a 20 caratteri
    char iniziale = 'M';

    printf("%s\n", nome1);
    printf("%s\n", nome2);
    printf("%c\n", iniziale);

    return 0;
}
{% endhighlight %}

`"Mario"` occupa 6 byte in memoria: 5 caratteri più il terminatore `\0`,
aggiunto automaticamente dal compilatore.

---

## Output e input di stringhe

Una stringa si stampa con `printf` usando `%s`. Per leggerla si può
usare `scanf("%s", ...)` (si ferma al primo spazio) oppure `fgets`
(legge l'intera riga).

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    char nome[50];
    printf("Scrivi il tuo nome: ");
    scanf("%s", nome);  // NOTA: nessun & prima di nome
    printf("Il tuo nome: %s\n", nome);

    return 0;
}
{% endhighlight %}

A differenza di `scanf("%d", &numero)`, con le stringhe **non si usa**
l'operatore `&`: il nome di un array è già l'indirizzo del suo primo
elemento.

`scanf("%s", ...)` si ferma al primo spazio: per leggere "Mario Rossi"
serve `fgets`.

{% highlight c %}
#include <stdio.h>

int main() {
    char nome[50];
    printf("Scrivi il tuo nome completo: ");
    fgets(nome, sizeof(nome), stdin);
    printf("Il tuo nome: %s", nome);

    return 0;
}
{% endhighlight %}

---

## Lunghezza di una stringa

La funzione `strlen` (da `<string.h>`) restituisce il numero di
caratteri di una stringa, **senza contare** il terminatore `\0`.

{% highlight c %}
#include <stdio.h>
#include <string.h>

int main() {
    char saluto[] = "ciao";
    int lunghezza = strlen(saluto);  // 4

    printf("%d\n", lunghezza);

    return 0;
}
{% endhighlight %}

---

## Copia e concatenazione

Le stringhe **non si copiano né si concatenano con `=` o `+`**: bisogna
usare le funzioni `strcpy` e `strcat`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>
#include <string.h>

int main() {
    char saluto[20] = "Ciao";
    char nome[]      = "Alice";
    char frase[50];

    strcpy(frase, saluto);   // frase = "Ciao"
    strcat(frase, ", ");     // frase = "Ciao, "
    strcat(frase, nome);     // frase = "Ciao, Alice"
    strcat(frase, "!");      // frase = "Ciao, Alice!"

    printf("%s\n", frase);
    printf("Lunghezza: %d\n", (int)strlen(frase));

    return 0;
}
{% endhighlight %}

L'array di destinazione (`frase`) deve essere **abbastanza grande** da
contenere il risultato: `strcpy` e `strcat` non controllano i limiti.

---

## Confronto di stringhe

Le stringhe **non si confrontano con `==`** (confronterebbe indirizzi,
non contenuti): si usa `strcmp`, che restituisce `0` se le stringhe
sono uguali.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>
#include <string.h>

int main() {
    char a[] = "abc";
    char b[] = "abc";
    char c[] = "abd";

    printf("%d\n", strcmp(a, b));  // 0 (uguali)
    printf("%d\n", strcmp(a, c));  // valore negativo (a < c)

    if (strcmp(a, b) == 0) {
        printf("Le stringhe sono uguali\n");
    }

    return 0;
}
{% endhighlight %}

---

## Accedere ai singoli caratteri

Una stringa è un array: si accede ai suoi caratteri con l'operatore `[]`.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>
#include <string.h>

int main() {
    char parola[] = "liceo";

    printf("%c\n", parola[0]);              // l
    printf("%c\n", parola[strlen(parola) - 1]); // o

    // visita carattere per carattere
    for (int i = 0; parola[i] != '\0'; i++) {
        printf("%c ", parola[i]);
    }
    printf("\n");

    return 0;
}
{% endhighlight %}

---

## Funzioni utili della libreria string.h

| Funzione              | Significato                                    |
|------------------------|-------------------------------------------------|
| `strlen(s)`            | lunghezza della stringa (senza `\0`)            |
| `strcpy(dest, src)`    | copia `src` in `dest`                           |
| `strcat(dest, src)`    | concatena `src` in fondo a `dest`               |
| `strcmp(s1, s2)`       | `0` se uguali, negativo/positivo se diverse     |
| `strchr(s, c)`         | puntatore alla prima occorrenza del carattere `c`|
| `strstr(s, sub)`       | puntatore alla prima occorrenza della sottostringa `sub` |

---

## Esercizi

#### Esercizio 6
Leggi una stringa con `fgets` e stampa quante volte appare la lettera
'a' (maiuscola o minuscola) nella stringa.

#### Esercizio 7
Leggi una parola e stampa se è un palindromo (uguale letta al contrario).

#### Esercizio 8
Leggi una frase e stampa quante parole contiene. Suggerimento: conta gli
spazi con un ciclo su `frase[i]`.

#### Esercizio 9
Leggi due stringhe e stampa la loro concatenazione usando `strcat`,
assicurandoti che l'array di destinazione sia abbastanza grande.
