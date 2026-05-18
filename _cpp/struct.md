---
title: 'C++: le struct'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Raggruppare dati correlati

Una **struct** (struttura) raggruppa più variabili di tipi diversi sotto
un unico nome. È utile quando si vuole trattare un insieme di dati correlati
come una singola entità — ad esempio le coordinate di un punto, i dati di
uno studente, le proprietà di un prodotto.

A differenza di una `class`, i membri di una `struct` sono **pubblici** per
default.

---

## Definire e usare una struct

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

struct Punto {
    double x;
    double y;
};

int main() {
    Punto p1;
    p1.x = 3.0;
    p1.y = 4.0;

    Punto p2 = {1.0, 2.0};  // inizializzazione aggregata

    cout << "p1: (" << p1.x << ", " << p1.y << ")" << endl;
    cout << "p2: (" << p2.x << ", " << p2.y << ")" << endl;

    return 0;
}
{% endhighlight %}

I campi di una struct si accedono con il punto `.`.

---

## Struct con più campi

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

struct Persona {
    string nome;
    string cognome;
    int    eta;
};

int main() {
    Persona mario = {"Mario", "Rossi", 25};
    cout << mario.nome << " " << mario.cognome
         << ", " << mario.eta << " anni" << endl;

    mario.eta = 26;
    cout << "Nuovo età: " << mario.eta << endl;

    return 0;
}
{% endhighlight %}

---

## Aggiungere metodi a una struct

In C++ una `struct` può contenere anche funzioni (metodi), esattamente
come una `class`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <cmath>
using namespace std;

struct Punto {
    double x;
    double y;

    double distanzaDallOrigine() {
        return sqrt(x * x + y * y);
    }

    void stampa() {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Punto p = {3.0, 4.0};
    p.stampa();
    cout << "Distanza: " << p.distanzaDallOrigine() << endl;  // 5.0

    return 0;
}
{% endhighlight %}

---

## Array di struct

Una delle applicazioni più comuni: un array (o vettore) di struct per
rappresentare una tabella di dati.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Studente {
    string nome;
    int    voto;
};

int main() {
    vector<Studente> classe = {
        {"Alice",  9},
        {"Bruno",  6},
        {"Carla",  8},
        {"Davide", 7}
    };

    for (const Studente& s : classe) {
        cout << s.nome << ": " << s.voto << endl;
    }

    // trova il migliore
    Studente migliore = classe[0];
    for (const Studente& s : classe) {
        if (s.voto > migliore.voto) migliore = s;
    }
    cout << "Migliore: " << migliore.nome << " (" << migliore.voto << ")" << endl;

    return 0;
}
{% endhighlight %}

---

## Passare una struct a una funzione

Le struct si passano per valore (copia) o per riferimento (`&`).

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

struct Rettangolo {
    double base;
    double altezza;
};

double area(const Rettangolo& r) {
    return r.base * r.altezza;
}

double perimetro(const Rettangolo& r) {
    return 2 * (r.base + r.altezza);
}

int main() {
    Rettangolo r = {5.0, 3.0};
    cout << "Area: "      << area(r)      << endl;  // 15
    cout << "Perimetro: " << perimetro(r) << endl;  // 16

    return 0;
}
{% endhighlight %}

`const Rettangolo&` passa per riferimento senza permettere modifiche:
evita la copia (efficiente) e garantisce che la funzione non modifichi
la struct originale.

---

## Esercizi

#### Esercizio 6
Definisci una struct `Prodotto` con campi `nome`, `prezzo` e `quantita`.
Crea un vettore di 5 prodotti, calcola il valore totale in magazzino
(`prezzo * quantita`) e stampa il prodotto più costoso.

#### Esercizio 7
Definisci una struct `Data` con campi `giorno`, `mese` e `anno`.
Scrivi una funzione `stampaData(const Data& d)` che stampi nel formato
`GG/MM/AAAA`.

#### Esercizio 8
Definisci una struct `Vettore2D` con campi `x` e `y`. Aggiungi metodi
`somma(Vettore2D altro)` e `modulo()`. Testa con due vettori a scelta.
