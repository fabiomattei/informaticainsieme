---
title: 'C++: transform, copy_if e accumulate'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Trasformare, filtrare, ridurre

Le tre operazioni fondamentali sulle collezioni sono:

- **trasformare** ogni elemento in qualcos'altro (`std::transform`)
- **filtrare** tenendo solo gli elementi che soddisfano una condizione (`std::copy_if`)
- **ridurre** tutti gli elementi a un unico valore (`std::accumulate`)

In C++ queste funzioni sono nella libreria `<algorithm>` (per `transform`
e `copy_if`) e in `<numeric>` (per `accumulate`).

---

## Lambda in C++

Prima di vedere le funzioni, è utile capire le **lambda**: funzioni anonime
che si scrivono direttamente dove servono.

```cpp
[](tipo param) { return espressione; }
```

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    auto doppio = [](int x) { return x * 2; };
    auto quadr  = [](int x) { return x * x; };

    cout << doppio(5) << endl;  // 10
    cout << quadr(4)  << endl;  // 16

    return 0;
}
{% endhighlight %}

`auto` deduce il tipo della lambda. Tra le `[]` si possono catturare
variabili esterne (es. `[&n]` cattura `n` per riferimento).

---

## std::transform — trasformare ogni elemento

`std::transform` applica una funzione a ogni elemento di un range e
scrive il risultato in un altro range.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> numeri = {1, 2, 3, 4, 5};
    vector<int> doppi(numeri.size());

    transform(numeri.begin(), numeri.end(), doppi.begin(),
              [](int x) { return x * 2; });

    for (int n : doppi) cout << n << " ";  // 2 4 6 8 10
    cout << endl;

    // trasformazione in-place (sovrascrive il vettore originale)
    transform(numeri.begin(), numeri.end(), numeri.begin(),
              [](int x) { return x * x; });

    for (int n : numeri) cout << n << " ";  // 1 4 9 16 25
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## std::copy_if — filtrare elementi

`std::copy_if` copia in un nuovo range solo gli elementi per cui il
predicato restituisce `true`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> numeri = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    vector<int> pari;

    copy_if(numeri.begin(), numeri.end(), back_inserter(pari),
            [](int x) { return x % 2 == 0; });

    cout << "Pari: ";
    for (int n : pari) cout << n << " ";  // 2 4 6 8 10
    cout << endl;

    return 0;
}
{% endhighlight %}

`back_inserter(pari)` è un inseritore che aggiunge elementi in fondo al
vettore `pari` automaticamente.

---

## std::accumulate — ridurre a un valore

`std::accumulate` riduce un range a un singolo valore applicando
un'operazione cumulativa.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <numeric>
using namespace std;

int main() {
    vector<int> numeri = {1, 2, 3, 4, 5};

    int somma = accumulate(numeri.begin(), numeri.end(), 0);
    cout << "Somma: " << somma << endl;   // 15

    int prodotto = accumulate(numeri.begin(), numeri.end(), 1,
                              [](int acc, int x) { return acc * x; });
    cout << "Prodotto: " << prodotto << endl;  // 120

    return 0;
}
{% endhighlight %}

Il terzo parametro di `accumulate` è il **valore iniziale**. Il quarto
(opzionale) è la funzione binaria da applicare; di default è l'addizione.

---

## Concatenare le tre operazioni

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;

int main() {
    vector<int> numeri = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    // 1. filtra i pari
    vector<int> pari;
    copy_if(numeri.begin(), numeri.end(), back_inserter(pari),
            [](int x) { return x % 2 == 0; });

    // 2. eleva al quadrato
    vector<int> quadrati(pari.size());
    transform(pari.begin(), pari.end(), quadrati.begin(),
              [](int x) { return x * x; });

    // 3. somma
    int tot = accumulate(quadrati.begin(), quadrati.end(), 0);
    cout << "Somma quadrati dei pari: " << tot << endl;
    // 4 + 16 + 36 + 64 + 100 = 220

    return 0;
}
{% endhighlight %}

---

## Riepilogo

| Funzione           | Header       | Scopo                                  |
|--------------------|--------------|----------------------------------------|
| `std::transform`   | `<algorithm>`| trasforma ogni elemento (map)          |
| `std::copy_if`     | `<algorithm>`| copia solo gli elementi filtrati        |
| `std::accumulate`  | `<numeric>`  | riduce a un valore (reduce)            |
| `std::count_if`    | `<algorithm>`| conta gli elementi che soddisfano      |
| `std::find_if`     | `<algorithm>`| trova il primo elemento che soddisfa   |
| `std::for_each`    | `<algorithm>`| applica una funzione a ogni elemento   |

---

## Esercizi

#### Esercizio 6
Usa `std::transform` per convertire un `vector<string>` in un
`vector<int>` di lunghezze delle stringhe.

#### Esercizio 7
Usa `std::copy_if` per estrarre da un vettore di interi solo i numeri
maggiori della media del vettore.

#### Esercizio 8
Usa `std::accumulate` con una lambda per trovare il massimo di un
vettore (senza usare `std::max_element`).

#### Esercizio 9
Concatena le tre operazioni: dato un vettore di interi, calcola la
somma dei cubi dei dispari.

#### Esercizio 10
Usa `std::for_each` per stampare ogni elemento di un `vector<string>`
preceduto dal suo indice (usa una variabile catturata nella lambda).
