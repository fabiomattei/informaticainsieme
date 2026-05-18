---
title: 'C++: input e output'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## cout e cin

In C++ l'output avviene tramite lo **stream** `cout` (character output) e
l'input tramite `cin` (character input). Entrambi fanno parte della libreria
standard ed è necessario includere `<iostream>`.

L'operatore `<<` invia dati verso `cout`; l'operatore `>>` estrae dati da `cin`.

---

## Output con cout

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    cout << "Ciao, mondo!" << endl;
    cout << "La risposta è: " << 42 << endl;

    int eta = 25;
    cout << "Hai " << eta << " anni." << endl;

    return 0;
}
{% endhighlight %}

`endl` manda a capo e svuota il buffer di output. In alternativa si usa
il carattere di escape `\n` dentro una stringa: è più rapido ma non svuota
il buffer.

{% highlight cpp %}
cout << "Prima riga\n";
cout << "Seconda riga\n";
{% endhighlight %}

---

## Input con cin

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int eta;
    cout << "Inserisci la tua età: ";
    cin >> eta;
    cout << "Hai " << eta << " anni." << endl;

    return 0;
}
{% endhighlight %}

`cin >> eta` legge un valore dallo stream di input e lo memorizza nella
variabile `eta`. La variabile deve essere già dichiarata con il tipo corretto.

---

## Leggere più valori

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int base, altezza;
    cout << "Inserisci base e altezza: ";
    cin >> base >> altezza;

    int area = base * altezza;
    cout << "Area del rettangolo: " << area << endl;

    return 0;
}
{% endhighlight %}

Si possono concatenare più estrazioni: `cin >> a >> b` legge due valori
separati da spazio o da invio.

---

## Leggere una riga intera con getline

`cin >>` si ferma al primo spazio. Per leggere una riga intera (con spazi)
si usa `getline(cin, variabile)`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    string nome;
    cout << "Inserisci il tuo nome completo: ";
    getline(cin, nome);
    cout << "Benvenuto, " << nome << "!" << endl;

    return 0;
}
{% endhighlight %}

---

## Riepilogo: differenze tra cin >> e getline

| Caratteristica            | `cin >>`              | `getline`             |
|---------------------------|-----------------------|-----------------------|
| Si ferma a               | spazio, tab, invio    | solo invio            |
| Legge più parole          | no                    | sì                    |
| Tipo della variabile      | qualsiasi             | solo `string`         |

---

## Esercizi

#### Esercizio 5
Scrivi un programma che legga nome e cognome separatamente (due `cin >>`)
e li stampi nella forma "Cognome, Nome".

#### Esercizio 6
Leggi due numeri decimali (`double`) e stampa la loro somma, differenza e
prodotto.

#### Esercizio 7
Leggi il raggio di un cerchio e stampa area e circonferenza.
Usa `const double PI = 3.14159`.

#### Esercizio 8
Leggi una frase intera con `getline` e stampala al contrario carattere per
carattere (suggerimento: usa un ciclo con indice decrescente e `frase[i]`).

#### Esercizio 9
Scrivi un programma che legga tre interi, calcoli la loro media come
`double` e la stampi con due cifre decimali
(suggerimento: `cout << fixed << setprecision(2)` con `#include <iomanip>`).
