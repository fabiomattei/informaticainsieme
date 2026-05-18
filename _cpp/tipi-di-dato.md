---
title: 'C++: i tipi di dato'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Tipizzazione statica

In C++ ogni variabile ha un tipo **fisso** stabilito al momento della
dichiarazione. Il compilatore usa il tipo per allocare la giusta quantità
di memoria e per controllare che le operazioni eseguite siano compatibili.

---

## Tipi interi

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int         a = 42;
    long        b = 1000000L;
    long long   c = 9000000000LL;

    cout << "int:       " << a << endl;
    cout << "long:      " << b << endl;
    cout << "long long: " << c << endl;

    return 0;
}
{% endhighlight %}

| Tipo        | Dimensione tipica | Intervallo approssimativo       |
|-------------|:-----------------:|---------------------------------|
| `int`       | 4 byte            | −2 miliardi … +2 miliardi       |
| `long`      | 4–8 byte          | dipende dalla piattaforma       |
| `long long` | 8 byte            | ±9,2 × 10¹⁸                    |

---

## Tipi a virgola mobile

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    float  f = 3.14f;
    double d = 3.14159265358979;

    cout << "float:  " << f << endl;
    cout << "double: " << d << endl;

    return 0;
}
{% endhighlight %}

`double` ha più cifre significative di `float` ed è preferito per la maggior
parte dei calcoli. La costante `3.14f` è di tipo `float`; senza il suffisso
`f` sarebbe di tipo `double`.

---

## Il tipo bool

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    bool vero  = true;
    bool falso = false;

    cout << vero  << endl;   // stampa 1
    cout << falso << endl;   // stampa 0

    cout << boolalpha;       // attiva la stampa come true/false
    cout << vero  << endl;   // stampa true
    cout << falso << endl;   // stampa false

    return 0;
}
{% endhighlight %}

In C++ `true` vale `1` e `false` vale `0` quando usati in contesti numerici.

---

## Il tipo char

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    char lettera = 'A';
    cout << lettera << endl;          // A
    cout << (int)lettera << endl;     // 65  (codice ASCII)

    char prossima = lettera + 1;
    cout << prossima << endl;         // B

    return 0;
}
{% endhighlight %}

Un `char` memorizza un singolo carattere tra apici singoli. Internamente è
un numero intero (codice ASCII): `'A'` = 65, `'a'` = 97, `'0'` = 48.

---

## Il tipo string

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    string nome = "Mario";
    string cognome = "Rossi";
    string completo = nome + " " + cognome;

    cout << completo << endl;
    cout << "Lunghezza: " << completo.length() << endl;

    return 0;
}
{% endhighlight %}

`string` non è un tipo primitivo ma una classe della libreria standard.
Richiede `#include <string>`.

---

## Il tipo dedotto: auto

Con `auto` il compilatore deduce il tipo dall'espressione di inizializzazione.

{% highlight cpp %}
auto x = 42;        // int
auto y = 3.14;      // double
auto s = "ciao"s;   // string
{% endhighlight %}

È utile quando il tipo è lungo da scrivere, ma per i tipi semplici è meglio
essere espliciti per maggiore chiarezza.

---

## sizeof

`sizeof(tipo)` restituisce la dimensione in byte di un tipo o di una variabile.

{% highlight cpp %}
cout << sizeof(int)    << endl;  // tipicamente 4
cout << sizeof(double) << endl;  // tipicamente 8
cout << sizeof(char)   << endl;  // sempre 1
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Dichiara una variabile di ciascun tipo fondamentale (`int`, `double`,
`bool`, `char`, `string`), assegna valori a scelta e stampali tutti.

#### Esercizio 7
Dichiara due variabili `int` e una `double`. Esegui una divisione tra i
due interi assegnando il risultato al `double`. Cosa noti se non usi il
cast? (suggerimento: prova `5 / 2` vs `5.0 / 2`)

#### Esercizio 8
Scrivi un programma che stampi i codici ASCII delle lettere `'A'` fino a
`'Z'` usando un ciclo `for` che incrementa un `char`.

#### Esercizio 9
Dichiara un `long long` con il valore 10.000.000.000 (dieci miliardi) e
stampalo. Prova lo stesso con un `int` e osserva cosa succede (overflow).
