---
title: 'C++: le condizioni'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Eseguire codice solo in certi casi

Le istruzioni condizionali permettono di scegliere quale blocco di codice
eseguire in base al valore di un'espressione booleana. In C++ le parole
chiave sono `if`, `else if`, `else` e `switch`.

Il blocco di codice da eseguire è racchiuso tra **parentesi graffe** `{ }`.

## if semplice

![Flowchart di un if senza else: se falso il blocco viene saltato](/images/cpp/condizioni/if-semplice.svg){:class="aside-image"}

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int numero;
    cout << "Inserisci un numero: ";
    cin >> numero;

    if (numero > 0) {
        cout << "Il numero è positivo." << endl;
    }

    return 0;
}
{% endhighlight %}

Il programma, mandato in esecuzione, si aspetta che l'utente inserisca un input. 
L'input deve essere un numero, altrimenti il programma restituisce un errore e si blocca. 
Il valore inserito in input dall'utente viene assegnato alla variabile di tipo **int** chiamata **numero**.
L'espressione booleana numero > 0 ha come risultato il valore True se il numero inserito dall'utente è maggiore di zero, e False in caso il numero inserito ha valore uguale a zero o minore di zero.

Il blocco, cioè le istruzioni, inserite all'interno delle parentesi graffe `{ }` vengono eseguite soltanto se la condizione tra `( )` è vera.

## if / else

![Flowchart di if / else: uno dei due blocchi viene sempre eseguito](/images/cpp/condizioni/if-else.svg){:class="aside-image"}

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int numero;
    cout << "Inserisci un numero: ";
    cin >> numero;

    if (numero % 2 == 0) {
        cout << "Il numero è pari." << endl;
    } else {
        cout << "Il numero è dispari." << endl;
    }

    return 0;
}
{% endhighlight %}

## if / else if / else

![Flowchart a rombo con i rami if / else if / else](/images/cpp/condizioni/condizioni.svg){:class="aside-image"}

Quando le alternative sono più di due si concatenano `else if`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int voto;
    cout << "Inserisci il voto (0-10): ";
    cin >> voto;

    if (voto >= 9) {
        cout << "Ottimo" << endl;
    } else if (voto >= 7) {
        cout << "Buono" << endl;
    } else if (voto >= 6) {
        cout << "Sufficiente" << endl;
    } else {
        cout << "Insufficiente" << endl;
    }

    return 0;
}
{% endhighlight %}

Le condizioni vengono valutate dall'alto verso il basso: appena una è vera,
il relativo blocco viene eseguito e le successive vengono saltate.

## switch / case

`switch` è utile quando si devono confrontare un valore intero o `char`
con un insieme fisso di costanti. Ogni caso termina con `break` per
impedire la caduta al caso successivo (*fall-through*).

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int giorno;
    cout << "Inserisci il numero del giorno (1-7): ";
    cin >> giorno;

    switch (giorno) {
        case 1: cout << "Lunedì" << endl;    break;
        case 2: cout << "Martedì" << endl;   break;
        case 3: cout << "Mercoledì" << endl; break;
        case 4: cout << "Giovedì" << endl;   break;
        case 5: cout << "Venerdì" << endl;   break;
        case 6: cout << "Sabato" << endl;    break;
        case 7: cout << "Domenica" << endl;  break;
        default: cout << "Giorno non valido" << endl;
    }

    return 0;
}
{% endhighlight %}

`default` è il caso che scatta quando nessun `case` corrisponde.

## L'operatore ternario

Per assegnare un valore in base a una condizione si può usare l'operatore
ternario `condizione ? valore_se_vero : valore_se_falso`.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int a, b;
    cout << "Inserisci due numeri: ";
    cin >> a >> b;

    int massimo = (a > b) ? a : b;
    cout << "Il massimo è: " << massimo << endl;

    return 0;
}
{% endhighlight %}

## Esercizi

#### Esercizio 6
Leggi un numero intero e stampa "positivo", "negativo" o "zero" a seconda
del caso.

#### Esercizio 7
Leggi un anno e determina se è bisestile.
(Condizione: divisibile per 4, eccetto i centenari, che devono essere
divisibili per 400.)

#### Esercizio 8
Leggi tre interi e stampa il maggiore dei tre usando `if/else if/else`.

#### Esercizio 9
Scrivi un calcolatore semplice: leggi due numeri e un carattere operatore
(`+`, `-`, `*`, `/`). Usa `switch` per eseguire l'operazione scelta.
Gestisci la divisione per zero.

#### Esercizio 10
Leggi la temperatura (double) e classifica il tempo:
sotto 0 = "gelido", 0–10 = "freddo", 11–20 = "fresco", 21–30 = "caldo",
sopra 30 = "torrido".
