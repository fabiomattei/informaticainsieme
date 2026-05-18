---
title: 'C++: std::set'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Un insieme senza duplicati

`std::set` è un contenitore che memorizza elementi **unici** in ordine
crescente. Inserire un elemento già presente non produce errori ma viene
ignorato silenziosamente. È l'equivalente C++ del Set di Ruby.

Per usarlo è necessario `#include <set>`.

---

## Creare un set

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> numeri = {5, 3, 8, 1, 3, 5, 9};
    // i duplicati vengono eliminati automaticamente

    cout << "Elementi: ";
    for (int n : numeri) {
        cout << n << " ";  // stampa: 1 3 5 8 9 (ordinati, senza dup.)
    }
    cout << endl;
    cout << "Dimensione: " << numeri.size() << endl;  // 5

    return 0;
}
{% endhighlight %}

---

## Aggiungere, rimuovere e cercare

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <set>
#include <string>
using namespace std;

int main() {
    set<string> frutti = {"mela", "pera", "banana"};

    frutti.insert("kiwi");
    frutti.insert("mela");  // ignorato: già presente

    cout << "Dimensione: " << frutti.size() << endl;  // 4

    if (frutti.count("pera") > 0) {
        cout << "pera è presente" << endl;
    }

    frutti.erase("pera");
    cout << "Dopo erase: " << frutti.size() << endl;  // 3

    return 0;
}
{% endhighlight %}

`.count(x)` restituisce 1 se `x` è presente, 0 altrimenti. Per i set
è equivalente a un controllo di appartenenza.

---

## Operazioni su insiemi

C++ non ha operatori integrati per unione e intersezione, ma la libreria
`<algorithm>` fornisce `set_union` e `set_intersection`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <set>
#include <algorithm>
#include <iterator>
using namespace std;

int main() {
    set<int> A = {1, 2, 3, 4, 5};
    set<int> B = {3, 4, 5, 6, 7};

    // Intersezione
    set<int> inter;
    set_intersection(A.begin(), A.end(), B.begin(), B.end(),
                     inserter(inter, inter.begin()));
    cout << "Intersezione: ";
    for (int n : inter) cout << n << " ";  // 3 4 5
    cout << endl;

    // Unione
    set<int> uni;
    set_union(A.begin(), A.end(), B.begin(), B.end(),
              inserter(uni, uni.begin()));
    cout << "Unione: ";
    for (int n : uni) cout << n << " ";  // 1 2 3 4 5 6 7
    cout << endl;

    // Differenza A - B
    set<int> diff;
    set_difference(A.begin(), A.end(), B.begin(), B.end(),
                   inserter(diff, diff.begin()));
    cout << "Differenza A-B: ";
    for (int n : diff) cout << n << " ";  // 1 2
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## Eliminare duplicati da un vettore

Un uso pratico del `set` è rimuovere duplicati da un `vector`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <set>
using namespace std;

int main() {
    vector<int> v = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};

    set<int> unici(v.begin(), v.end());

    cout << "Originale: ";
    for (int n : v)      cout << n << " ";
    cout << endl;

    cout << "Unici:     ";
    for (int n : unici)  cout << n << " ";
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## Metodi utili

| Metodo          | Significato                                      |
|-----------------|--------------------------------------------------|
| `.insert(x)`    | inserisce `x` (ignorato se già presente)         |
| `.erase(x)`     | rimuove `x`                                      |
| `.count(x)`     | 1 se `x` è presente, 0 altrimenti               |
| `.size()`       | numero di elementi                               |
| `.empty()`      | `true` se il set è vuoto                        |
| `.clear()`      | svuota il set                                    |
| `.begin()`      | iteratore al primo elemento                      |
| `.end()`        | iteratore "oltre" l'ultimo elemento              |

---

## Esercizi

#### Esercizio 5
Leggi N parole e stampa quelle distinte (senza ripetizioni) in ordine
alfabetico usando un `set<string>`.

#### Esercizio 6
Leggi i nomi degli studenti di due classi (separatamente) e usa
`set_intersection` per trovare chi è in entrambe.

#### Esercizio 7
Verifica se due set di interi sono disgiunti (non hanno nessun elemento
in comune) usando `set_intersection`.

#### Esercizio 8
Dato il testo di una frase letto con `getline`, usa un `set<char>` per
trovare le lettere distinte usate (esclusi gli spazi), stampale in ordine.
