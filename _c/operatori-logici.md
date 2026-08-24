---
title: 'C: gli operatori logici'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Combinare condizioni

Gli operatori logici combinano espressioni per formare condizioni più
complesse. In C i tre operatori logici sono `&&` (AND), `||` (OR) e `!`
(NOT). Non esistendo un vero tipo booleano, ogni espressione con valore
diverso da `0` è considerata vera.

---

## L'operatore && (AND)

Il risultato è vero (`1`) solo se **entrambe** le condizioni sono vere.

| A       | B       | A && B  |
|---------|---------|---------|
| vero    | vero    | vero    |
| vero    | falso   | falso   |
| falso   | vero    | falso   |
| falso   | falso   | falso   |

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int eta;
    printf("Inserisci la tua età: ");
    scanf("%d", &eta);

    if (eta >= 18 && eta <= 65) {
        printf("Sei in età lavorativa.\n");
    } else {
        printf("Non sei in età lavorativa.\n");
    }

    return 0;
}
{% endhighlight %}

---

## L'operatore || (OR)

Il risultato è vero se **almeno una** delle condizioni è vera.

| A       | B       | A \|\| B |
|---------|---------|----------|
| vero    | vero    | vero     |
| vero    | falso   | vero     |
| falso   | vero    | vero     |
| falso   | falso   | falso    |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int voto;
    printf("Inserisci il voto: ");
    scanf("%d", &voto);

    if (voto < 4 || voto > 10) {
        printf("Voto non valido.\n");
    } else {
        printf("Voto valido.\n");
    }

    return 0;
}
{% endhighlight %}

---

## L'operatore ! (NOT)

Inverte il valore logico: `!1` vale `0`, `!0` vale `1`.

| A       | !A      |
|---------|---------|
| vero    | falso   |
| falso   | vero    |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>
#include <stdbool.h>

int main() {
    bool autenticato = false;

    if (!autenticato) {
        printf("Accesso negato. Effettua il login.\n");
    } else {
        printf("Benvenuto!\n");
    }

    return 0;
}
{% endhighlight %}

---

## Valutazione cortocircuitata (short-circuit)

C valuta le condizioni da sinistra a destra e **si ferma appena il
risultato è determinato**:

- `A && B`: se `A` è falso, `B` non viene valutato (il risultato è già falso).
- `A || B`: se `A` è vero, `B` non viene valutato (il risultato è già vero).

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int n = 0;

    // la seconda condizione non viene valutata se n == 0
    if (n != 0 && 100 / n > 5) {
        printf("Condizione vera\n");
    } else {
        printf("Condizione falsa (divisione per zero evitata)\n");
    }

    return 0;
}
{% endhighlight %}

---

## && vs & e || vs |

In C esistono anche gli operatori bitwise `&` e `|`. Non sono operatori
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
Leggi tre numeri interi. Stampa `1` se almeno uno di essi è negativo.
Usa `||`.

#### Esercizio 8
Leggi un numero intero e stampa `1` se **non** è divisibile né per 2
né per 3. Usa `!` e `||`.

#### Esercizio 9
Scrivi un programma che legga due stringhe (username e password) con
`scanf("%s", ...)` e stampi "Accesso consentito" solo se username è
"admin" e password è "1234". Usa `&&` e `strcmp` per il confronto.
