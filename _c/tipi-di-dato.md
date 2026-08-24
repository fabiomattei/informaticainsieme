---
title: 'C: i tipi di dato'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Tipizzazione statica

In C ogni variabile ha un tipo **fisso** stabilito al momento della
dichiarazione. Il compilatore usa il tipo per allocare la giusta quantità
di memoria e per controllare che le operazioni eseguite siano compatibili.

---

## Tipi interi

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int       a = 42;
    long      b = 1000000L;
    long long c = 9000000000LL;

    printf("int:       %d\n", a);
    printf("long:      %ld\n", b);
    printf("long long: %lld\n", c);

    return 0;
}
{% endhighlight %}

| Tipo        | Dimensione tipica | Intervallo approssimativo       |
|-------------|:-----------------:|---------------------------------|
| `int`       | 4 byte            | −2 miliardi … +2 miliardi       |
| `long`      | 4–8 byte          | dipende dalla piattaforma       |
| `long long` | 8 byte            | ±9,2 × 10¹⁸                    |

---

## Tipi a virgola mobile

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    float  f = 3.14f;
    double d = 3.14159265358979;

    printf("float:  %f\n", f);
    printf("double: %lf\n", d);

    return 0;
}
{% endhighlight %}

`double` ha più cifre significative di `float` ed è preferito per la maggior
parte dei calcoli. La costante `3.14f` è di tipo `float`; senza il suffisso
`f` sarebbe di tipo `double`.

---

## Il tipo char

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    char lettera = 'A';
    printf("%c\n", lettera);         // A
    printf("%d\n", lettera);         // 65  (codice ASCII)

    char prossima = lettera + 1;
    printf("%c\n", prossima);        // B

    return 0;
}
{% endhighlight %}

Un `char` memorizza un singolo carattere tra apici singoli. Internamente è
un numero intero a 1 byte (codice ASCII): `'A'` = 65, `'a'` = 97, `'0'` = 48.

---

## Il tipo bool (stdbool.h)

Il C standard non ha un tipo booleano nativo: `0` è falso, qualsiasi valore
diverso da `0` è vero. Lo header `<stdbool.h>` introduce `bool`, `true`
e `false` come alias comodi.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>
#include <stdbool.h>

int main() {
    bool vero  = true;
    bool falso = false;

    printf("%d\n", vero);   // stampa 1
    printf("%d\n", falso);  // stampa 0

    return 0;
}
{% endhighlight %}

---

## Nessun tipo stringa nativo

A differenza di Python o Ruby, in C non esiste un tipo `string`. Il testo si
rappresenta come un **array di `char`** terminato dal carattere speciale
`'\0'`. L'argomento è approfondito nella pagina dedicata alle
[stringhe]({{ site.baseurl }}{% link _c/stringhe.md %}.html).

---

## sizeof

`sizeof(tipo)` restituisce la dimensione in byte di un tipo o di una variabile.

{% highlight c %}
printf("%zu\n", sizeof(int));    // tipicamente 4
printf("%zu\n", sizeof(double)); // tipicamente 8
printf("%zu\n", sizeof(char));   // sempre 1
{% endhighlight %}

Il formato `%zu` va usato per stampare valori di tipo `size_t`, restituito
da `sizeof`.

---

## Esercizi

#### Esercizio 5
Dichiara una variabile di ciascun tipo fondamentale (`int`, `double`,
`char`), assegna valori a scelta e stampali tutti con `printf`.

#### Esercizio 6
Dichiara due variabili `int` e una `double`. Esegui una divisione tra i
due interi assegnando il risultato al `double`. Cosa noti se non usi il
cast? (suggerimento: prova `5 / 2` vs `5.0 / 2`)

#### Esercizio 7
Scrivi un programma che stampi i codici ASCII delle lettere `'A'` fino a
`'Z'` usando un ciclo `for` che incrementa un `char`.

#### Esercizio 8
Dichiara un `long long` con il valore 10.000.000.000 (dieci miliardi) e
stampalo. Prova lo stesso con un `int` e osserva cosa succede (overflow).
