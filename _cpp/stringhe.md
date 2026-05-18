---
title: 'C++: le stringhe di testo'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## std::string

In C++ le stringhe di testo si rappresentano con il tipo `string` della
libreria standard. Richiede `#include <string>`. A differenza degli array
di caratteri del C, `string` gestisce automaticamente la memoria ed espone
metodi comodi per le operazioni più comuni.

---

## Creare e concatenare stringhe

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    string saluto = "Ciao";
    string nome   = "Alice";
    string frase  = saluto + ", " + nome + "!";

    cout << frase << endl;
    cout << "Lunghezza: " << frase.length() << endl;

    return 0;
}
{% endhighlight %}

L'operatore `+` concatena stringhe. `+=` aggiunge in fondo.

---

## Accedere ai singoli caratteri

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    string parola = "liceo";

    cout << parola[0]       << endl;  // l
    cout << parola.at(4)    << endl;  // o
    cout << parola.front()  << endl;  // l
    cout << parola.back()   << endl;  // o

    // visita carattere per carattere
    for (char c : parola) {
        cout << c << " ";
    }
    cout << endl;

    return 0;
}
{% endhighlight %}

---

## Metodi di ricerca e sostituzione

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s = "il gatto sul tetto";

    // ricerca
    size_t pos = s.find("gatto");
    if (pos != string::npos) {
        cout << "Trovato a posizione: " << pos << endl;  // 3
    }

    // sottostringa
    string sub = s.substr(3, 5);  // 5 caratteri da posizione 3
    cout << sub << endl;  // gatto

    // sostituzione
    s.replace(3, 5, "cane");
    cout << s << endl;  // il cane sul tetto

    return 0;
}
{% endhighlight %}

`string::npos` è un valore speciale che indica "non trovato".

---

## Maiuscolo, minuscolo e confronto

`string` non ha metodi `toupper`/`tolower` diretti, ma si può usare
la funzione `toupper`/`tolower` da `<cctype>` su ogni carattere.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
#include <cctype>
using namespace std;

int main() {
    string s = "Ciao Mondo";

    string maiusc = s;
    for (char& c : maiusc) c = toupper(c);
    cout << maiusc << endl;  // CIAO MONDO

    string minusc = s;
    for (char& c : minusc) c = tolower(c);
    cout << minusc << endl;  // ciao mondo

    // confronto lessicografico
    cout << ("abc" < "abd") << endl;  // 1 (true)
    cout << ("abc" == "abc") << endl; // 1 (true)

    return 0;
}
{% endhighlight %}

---

## Metodi utili

| Metodo                  | Significato                                         |
|-------------------------|-----------------------------------------------------|
| `.length()` / `.size()` | numero di caratteri                                 |
| `.empty()`              | `true` se la stringa è vuota                       |
| `[i]` / `.at(i)`        | accesso al carattere i-esimo                        |
| `.front()` / `.back()`  | primo / ultimo carattere                            |
| `.find(s)`              | posizione della prima occorrenza, `npos` se assente |
| `.substr(pos, n)`       | sottostringa di `n` caratteri da `pos`              |
| `.replace(pos, n, s)`   | sostituisce `n` caratteri da `pos` con `s`          |
| `.insert(pos, s)`       | inserisce `s` alla posizione `pos`                  |
| `.erase(pos, n)`        | cancella `n` caratteri da `pos`                     |
| `+` / `+=`              | concatenazione                                      |

---

## Esercizi

#### Esercizio 5
Leggi una stringa con `getline` e stampa quante volte appare la lettera
'a' (maiuscola o minuscola) nella stringa.

#### Esercizio 6
Leggi una parola e stampa se è un palindromo (uguale letta al contrario).
Suggerimento: confronta `s` con `string(s.rbegin(), s.rend())`.

#### Esercizio 7
Leggi una frase e stampa quante parole contiene (separatamente dal numero
di caratteri). Suggerimento: conta gli spazi.

#### Esercizio 8
Leggi una stringa e sostituisci tutte le occorrenze di una sottostringa
con un'altra. (Usa `find` e `replace` in un ciclo.)

#### Esercizio 9
Leggi una stringa e stampa separatamente le consonanti e le vocali che
contiene.
