---
title: 'Linguaggio c++'
date: '2021-10-10T22:33:00+02:00'
author: Fabio Mattei
layout: page
---

Il linguaggio C++ è stato inventato da **Bjarne Stroustrup** nel 1985 per estendere il linguaggio C con la sintassi della programmazione orientata agli oggetti.

![Nuova tabella in access](/images/cpp/BjarneStroustrup.jpg)

Il linguaggio C++ è estremamente efficiente. I campi di applicazione sono i più svariati: dal gaming alle applicazioni real-time, dai componenti per sistemi operativi ai software di grafica e musica, dalle app per cellulari ai sistemi per supercomputer. Praticamente, C++ è ovunque.

Per esercitarci con il C++ utilizzeremo un [compilatore on line reperibile qui](https://www.onlinegdb.com/online_c++_compiler). E’ preferibile però scaricare l'ide istallabile su computer [CodeBlock](https://www.codeblocks.org/) oppure l’ambiente della [bloodshed](http://bloodshed.net/).

Ecco un primo esempio di codice:

{% highlight cpp %}
#include <iostream>

using namespace std;

int main() {
    // definizione delle variabili
    int base, altezza, area;
    // inzializzazione delle variabili
    base = 10;
    altezza = 5;
    // esecuzione delle elaborazioni
    area = base * altezza;
    // comunicazione dei risultati
    cout << "Il rettangolo con: \n";
    cout << "base: " << base << "\n";
    cout << "altezza: " << altezza << "\n";
    cout << "ha come area: " << area << "\n";
}
{% endhighlight %}

Notiamo come la sintassi sia diversa dal Python ma non la logica.

Innanzi tutto, a differenza di Python, in linguaggio C++ le variabili vanno definite (o dichiarate) indicando il tipo a cui appartengono. In questo pezzetto di codice dichiariamo le variabili base, altezza e area di tipo intero.

L’assegnazione ha la stessa sintassi del Python così come gli operatori matematici sono analoghi.

Il comando per mandare le informazioni in output è: **cout**. cout va utilizzato con &lt;&lt; operatore di spostamento a sinistra. Questo indica che l’informazione contenta nella sezione seguente va spostata a sinistra all’interno dello stream buffer di output.

**Introduciamo l’input**

{% highlight cpp %}
#include <iostream>

using namespace std;

int main() {
    // definizione delle variabili
    int base, altezza, area;
    // input
    cin >> base;
    cin >> altezza;
    // esecuzione delle elaborazioni
    area = base * altezza;
    // comunicazione dei risultati
    cout << "Il rettangolo con: \n";
    cout << "base: " << base << "\n";
    cout << "altezza: " << altezza << "\n";
    cout << "ha come area: " << area << "\n";
}
{% endhighlight %}

Utilizziamo lo stream di input **cin** per chiedere informazioni all’utente. cin va seguito dai caratteri **&gt;&gt;** seguiti dalla **variabile** che andrà a contenere l’informazione.

**Condizione**

La sintassi della condizione in C++ non è molto diversa dal Python. La parola chiave da utilizzare è **if**. All’interno delle parentesi tonde si specifica l’espressione booleana da valutare. All’interno delle parentesi graffe si specifica il blocco di codice da eseguire.


{% highlight cpp %}
#include <iostream>

using namespace std;

int main() {
    int numero;
    cin >> numero;
    if (numero > 0) {
        cout << "numero positivo";
    } else {
        cout << "numero negativo";
    }
    return 0;
}
{% endhighlight %}

**L’iterazione**

L’iterazione in C++ può essere definita con la sintassi for. In questo caso inizializziamo un contatore **i** a zero. Al centro va definita la condizione che verrà valutata a ogni inizio iterazione; se l’esito della condizione è positivo vengono eseguite le istruzioni all’interno del blocco di codice definito dalle parentesi graffe.


{% highlight cpp %}
#include <iostream>

using namespace std;

int main() {
    
    cout << "inizio ciclo for \n";
    for (int i = 0; i < 10; i++) {
       cout << "i vale: " << i << "\n";
    }

    return 0;
}
{% endhighlight %}

**Il ciclo while**

L’iterazione in C++ può essere definita anche con la sintassi while. In questo caso inizializziamo un contatore **cont** a zero. Al centro va definita la condizione che verrà valutata a ogni inizio iterazione; se l’esito della condizione è positivo vengono eseguite le istruzioni all’interno del blocco di codice definito dalle parentesi graffe.


{% highlight cpp %}
#include <iostream>

using namespace std;

int main() {

    cout << "inizio ciclo while \n";
    int cont;
    cont = 0;
    while (cont < 10) {
        cout << "cont vale: " << cont << "\n";
        cont++;
    }

    return 0;
}
{% endhighlight %}


