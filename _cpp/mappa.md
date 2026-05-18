---
title: 'C++: std::map'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Associare chiavi a valori

`std::map` è un contenitore che associa ogni **chiave** a un **valore**.
È l'equivalente C++ dell'hash di Ruby o del dizionario di Python. Le chiavi
sono **uniche** e vengono mantenute in ordine crescente.

Per usarlo è necessario `#include <map>`.

---

## Creare e popolare una mappa

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> voti;

    voti["fisica"]      = 8;
    voti["matematica"]  = 6;
    voti["storia"]      = 7;
    voti["inglese"]     = 9;

    cout << "Fisica: "     << voti["fisica"]     << endl;
    cout << "Matematica: " << voti["matematica"] << endl;
    cout << "Dimensione: " << voti.size()        << endl;

    return 0;
}
{% endhighlight %}

`map<chiave, valore>` richiede che entrambi i tipi siano specificati.
L'accesso con `[]` crea la chiave con valore di default se non esiste!

---

## Inizializzazione con lista

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, string> capitali = {
        {"italia",   "Roma"},
        {"francia",  "Parigi"},
        {"germania", "Berlino"},
        {"spagna",   "Madrid"}
    };

    cout << "Capitale d'Italia: " << capitali["italia"] << endl;

    return 0;
}
{% endhighlight %}

---

## Verificare se una chiave esiste

Accedere con `[]` a una chiave inesistente la **crea** con valore di
default. Per controllare senza creare si usa `.count()` o `.find()`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> m;
    m["a"] = 1;
    m["b"] = 2;

    if (m.count("a") > 0) {
        cout << "Chiave 'a' presente: " << m["a"] << endl;
    }

    if (m.count("z") == 0) {
        cout << "Chiave 'z' non presente" << endl;
    }

    auto it = m.find("b");
    if (it != m.end()) {
        cout << "Trovato: " << it->first << " = " << it->second << endl;
    }

    return 0;
}
{% endhighlight %}

`.count(k)` restituisce 1 se la chiave esiste, 0 altrimenti (una mappa
non ammette duplicati). `.find(k)` restituisce un iteratore.

---

## Visitare una mappa

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> voti = {
        {"fisica", 8}, {"matematica", 6}, {"storia", 7}, {"inglese", 9}
    };

    for (const auto& coppia : voti) {
        cout << coppia.first << ": " << coppia.second << endl;
    }

    return 0;
}
{% endhighlight %}

Ogni elemento di una `map` è una `pair<chiave, valore>`. `.first` è la
chiave, `.second` è il valore. Il range-based for visita le coppie in
ordine alfabetico per chiave.

---

## Eliminare e modificare

{% highlight cpp %}
m.erase("fisica");   // rimuove la chiave "fisica"
m["storia"] = 8;     // modifica il valore
m.clear();           // svuota la mappa
{% endhighlight %}

---

## Esercizi

#### Esercizio 5
Crea una mappa `string → int` che conti le occorrenze di parole in un
testo. Leggi parole in un ciclo finché l'utente inserisce "fine" e poi
stampa ogni parola con il suo conteggio.

#### Esercizio 6
Crea una rubrica: mappa `string → string` (nome → numero di telefono).
Leggi N contatti, poi chiedi un nome e stampa il numero oppure
"Non trovato".

#### Esercizio 7
Dati i voti di una classe in una `map<string, int>` (nome → voto),
stampa solo gli studenti con voto ≥ 7.

#### Esercizio 8
Scrivi un programma che conti la frequenza di ogni carattere in una
stringa letta in input usando una `map<char, int>`.
