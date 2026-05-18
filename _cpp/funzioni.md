---
title: 'C++: le funzioni'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Dividere il programma in blocchi riutilizzabili

Una **funzione** è un blocco di codice con un nome che può essere chiamato
più volte. In C++ ogni funzione deve dichiarare il **tipo del valore
restituito** e il **tipo di ogni parametro**.

```cpp
tipo_ritorno nome(tipo param1, tipo param2) {
    // corpo
    return valore;
}
```

Se la funzione non restituisce nulla si usa il tipo speciale `void`.

---

## Definire e chiamare una funzione

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int somma(int a, int b) {
    return a + b;
}

int main() {
    int risultato = somma(3, 7);
    cout << "3 + 7 = " << risultato << endl;
    cout << "10 + 5 = " << somma(10, 5) << endl;
    return 0;
}
{% endhighlight %}

La funzione `somma` è definita **prima** di `main`. In alternativa si può
dichiarare il **prototipo** prima di `main` e definire la funzione dopo.

---

## Funzioni void (senza valore di ritorno)

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

void saluta(string nome) {
    cout << "Ciao, " << nome << "!" << endl;
}

int main() {
    saluta("Alice");
    saluta("Bob");
    return 0;
}
{% endhighlight %}

---

## Parametri con valore di default

Un parametro può avere un valore di default usato quando il chiamante
non lo fornisce.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

double potenza(double base, int esp = 2) {
    double risultato = 1.0;
    for (int i = 0; i < esp; i++) {
        risultato *= base;
    }
    return risultato;
}

int main() {
    cout << potenza(3)    << endl;  // 9  (esp = 2 di default)
    cout << potenza(3, 3) << endl;  // 27
    cout << potenza(2, 8) << endl;  // 256
    return 0;
}
{% endhighlight %}

I parametri con default devono essere **in fondo** alla lista dei parametri.

---

## Passaggio per valore e per riferimento

In C++ i parametri sono passati **per valore** (default): la funzione
riceve una copia e non può modificare la variabile originale.
Con `&` si passa **per riferimento**: la funzione lavora direttamente
sulla variabile originale.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

void raddoppiaValore(int x) {
    x *= 2;  // modifica solo la copia locale
}

void raddoppiaRif(int& x) {
    x *= 2;  // modifica la variabile originale
}

int main() {
    int a = 5;
    raddoppiaValore(a);
    cout << a << endl;  // 5 (invariato)

    raddoppiaRif(a);
    cout << a << endl;  // 10 (modificato)

    return 0;
}
{% endhighlight %}

---

## Overloading: stessa funzione, parametri diversi

In C++ è possibile definire più funzioni con lo stesso nome se hanno
firme diverse (numero o tipo di parametri diversi).

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int massimo(int a, int b) {
    return a > b ? a : b;
}

double massimo(double a, double b) {
    return a > b ? a : b;
}

int massimo(int a, int b, int c) {
    return massimo(massimo(a, b), c);
}

int main() {
    cout << massimo(3, 7)       << endl;  // 7
    cout << massimo(3.5, 2.1)   << endl;  // 3.5
    cout << massimo(1, 9, 4)    << endl;  // 9
    return 0;
}
{% endhighlight %}

---

## Prototipo di funzione

Per usare una funzione definita dopo `main` occorre dichiarare il
**prototipo** in anticipo.

{% highlight cpp %}
#include <iostream>
using namespace std;

int quadrato(int n);  // prototipo

int main() {
    cout << quadrato(5) << endl;
    return 0;
}

int quadrato(int n) {  // definizione
    return n * n;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Scrivi una funzione `fattoriale(int n)` che restituisca n!.
Chiamala per n = 0, 5, 10.

#### Esercizio 7
Scrivi una funzione `isPrimo(int n)` che restituisca `true` se n è primo.
Stampare tutti i numeri primi fino a 50.

#### Esercizio 8
Scrivi una funzione `scambia(int& a, int& b)` che scambi i valori di due
interi passati per riferimento.

#### Esercizio 9
Scrivi una funzione `media(double a, double b, double c = 0)` con terzo
parametro opzionale. Se il terzo parametro vale 0, calcola la media di
due valori; altrimenti la media di tre.

#### Esercizio 10
Scrivi due versioni di `stampaRiga(int n)` in overloading:
la prima stampa n asterischi, la seconda stampa n volte un carattere
passato come secondo parametro.
