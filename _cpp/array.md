---
title: 'C++: array e vector'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Collezioni di elementi dello stesso tipo

Un **array** in C++ è una sequenza di elementi dello stesso tipo, allocati
in posizioni di memoria contigue. Esistono due forme principali:

- **Array in stile C** — dimensione fissa, dichiarata al momento della
  definizione.
- **`std::vector`** — dimensione variabile, dalla libreria standard;
  la scelta preferita per il codice moderno.

---

## Array in stile C

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int numeri[5] = {10, 20, 30, 40, 50};

    cout << numeri[0] << endl;  // 10
    cout << numeri[4] << endl;  // 50

    numeri[2] = 99;
    cout << numeri[2] << endl;  // 99

    // visita con for classico
    for (int i = 0; i < 5; i++) {
        cout << numeri[i] << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}

Gli indici partono da 0. Accedere a un indice fuori dai limiti causa
comportamento indefinito (nessun errore garantito!).

---

## std::vector — array dinamico

`vector<tipo>` si ridimensiona automaticamente. Richiede `#include <vector>`.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};

    v.push_back(4);   // aggiunge in fondo
    v.push_back(5);

    cout << "Dimensione: " << v.size() << endl;  // 5
    cout << "Primo: "      << v.front() << endl; // 1
    cout << "Ultimo: "     << v.back()  << endl; // 5

    v.pop_back();   // rimuove l'ultimo
    cout << "Dopo pop_back: " << v.size() << endl;  // 4

    return 0;
}
{% endhighlight %}

---

## Accesso sicuro con at()

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<string> frutti = {"mela", "pera", "banana"};

    cout << frutti.at(0) << endl;  // mela
    cout << frutti[1]    << endl;  // pera  (senza controllo limiti)

    // range-based for
    for (const string& f : frutti) {
        cout << f << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}

`at(i)` lancia un'eccezione se `i` è fuori dai limiti; `[i]` non fa
controlli (più veloce ma meno sicuro).

---

## Metodi utili di vector

| Metodo             | Significato                               |
|--------------------|-------------------------------------------|
| `.push_back(x)`    | aggiunge `x` in fondo                     |
| `.pop_back()`      | rimuove l'ultimo elemento                 |
| `.size()`          | numero di elementi                        |
| `.empty()`         | `true` se il vettore è vuoto             |
| `.clear()`         | svuota il vettore                         |
| `.front()`         | primo elemento                            |
| `.back()`          | ultimo elemento                           |
| `.at(i)`           | accesso con controllo dei limiti          |

---

## Ordinamento con sort

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> v = {5, 2, 8, 1, 9, 3};

    sort(v.begin(), v.end());   // ordine crescente
    for (int n : v) cout << n << " ";
    cout << endl;

    sort(v.begin(), v.end(), greater<int>());  // ordine decrescente
    for (int n : v) cout << n << " ";
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## Leggere un vettore da input

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cout << "Quanti numeri? ";
    cin >> n;

    vector<int> v;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        v.push_back(x);
    }

    int massimo = v[0];
    for (int x : v) {
        if (x > massimo) massimo = x;
    }
    cout << "Massimo: " << massimo << endl;

    return 0;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Leggi N numeri in un `vector<double>` e calcola la media aritmetica.

#### Esercizio 7
Leggi N interi in un vettore e stampa separatamente i pari e i dispari.

#### Esercizio 8
Leggi N stringhe in un `vector<string>`, ordinale con `sort` e stampale
in ordine alfabetico.

#### Esercizio 9
Implementa la ricerca lineare: leggi N interi, poi un valore da cercare;
stampa l'indice della prima occorrenza o -1 se non trovato.

#### Esercizio 10
Scrivi una funzione `inverti(vector<int>& v)` che inverte l'ordine degli
elementi in-place (senza usare `reverse`).
