---
title: 'C++: gli operatori logici'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Combinare condizioni

Gli operatori logici combinano espressioni booleane per formare condizioni
più complesse. In C++ i tre operatori logici sono `&&` (AND), `||` (OR)
e `!` (NOT).

---

## L'operatore && (AND)

Il risultato è `true` solo se **entrambe** le condizioni sono vere.

| A       | B       | A && B  |
|---------|---------|---------|
| `true`  | `true`  | `true`  |
| `true`  | `false` | `false` |
| `false` | `true`  | `false` |
| `false` | `false` | `false` |

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int eta;
    cout << "Inserisci la tua età: ";
    cin >> eta;

    if (eta >= 18 && eta <= 65) {
        cout << "Sei in età lavorativa." << endl;
    } else {
        cout << "Non sei in età lavorativa." << endl;
    }

    return 0;
}
{% endhighlight %}

---

## L'operatore || (OR)

Il risultato è `true` se **almeno una** delle condizioni è vera.

| A       | B       | A \|\| B |
|---------|---------|----------|
| `true`  | `true`  | `true`   |
| `true`  | `false` | `true`   |
| `false` | `true`  | `true`   |
| `false` | `false` | `false`  |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int voto;
    cout << "Inserisci il voto: ";
    cin >> voto;

    if (voto < 4 || voto > 10) {
        cout << "Voto non valido." << endl;
    } else {
        cout << "Voto valido." << endl;
    }

    return 0;
}
{% endhighlight %}

---

## L'operatore ! (NOT)

Inverte il valore booleano: `!true` = `false`, `!false` = `true`.

| A       | !A      |
|---------|---------|
| `true`  | `false` |
| `false` | `true`  |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    bool autenticato = false;

    if (!autenticato) {
        cout << "Accesso negato. Effettua il login." << endl;
    } else {
        cout << "Benvenuto!" << endl;
    }

    return 0;
}
{% endhighlight %}

---

## Valutazione cortocircuitata (short-circuit)

C++ valuta le condizioni da sinistra a destra e **si ferma appena
il risultato è determinato**:

- `A && B`: se `A` è `false`, `B` non viene valutato (il risultato è già `false`).
- `A || B`: se `A` è `true`, `B` non viene valutato (il risultato è già `true`).

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int n = 0;

    // la seconda condizione non viene valutata se n == 0
    if (n != 0 && 100 / n > 5) {
        cout << "Condizione vera" << endl;
    } else {
        cout << "Condizione falsa (divisione per zero evitata)" << endl;
    }

    return 0;
}
{% endhighlight %}

---

## && vs & e || vs |

In C++ esistono anche gli operatori bitwise `&` e `|`. Non sono operatori
logici: operano bit a bit sui valori interi e **non applicano il
cortocircuito**. Usa sempre `&&` e `||` per le condizioni logiche.

---

## Esercizi

#### Esercizio 5
Leggi un anno e scrivi se è bisestile. Un anno è bisestile se è divisibile
per 4, tranne i centenari, che devono essere divisibili per 400.
Condizione: `(anno % 4 == 0 && anno % 100 != 0) || anno % 400 == 0`.

#### Esercizio 6
Leggi un carattere e scrivi se è una lettera minuscola (tra `'a'` e `'z'`).
Usa `&&` per combinare i due confronti.

#### Esercizio 7
Leggi tre numeri interi. Stampa `true` se almeno uno di essi è negativo.
Usa `||`.

#### Esercizio 8
Leggi un numero intero e stampa `true` se **non** è divisibile né per 2
né per 3. Usa `!` e `||`.

#### Esercizio 9
Scrivi un programma che legga username e password (stringhe) e stampi
"Accesso consentito" solo se username è "admin" e password è "1234".
Usa `&&` e l'operatore `==` su `string`.
