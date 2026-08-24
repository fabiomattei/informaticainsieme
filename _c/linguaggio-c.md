---
title: 'Linguaggio C'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Il linguaggio **C** è stato sviluppato da **Dennis Ritchie** nei Bell Labs tra il 1969 e il 1973, insieme allo sviluppo del sistema operativo Unix. È uno dei linguaggi più influenti nella storia dell'informatica: praticamente tutti i sistemi operativi moderni, i compilatori e molti linguaggi successivi (C++, Java, Python, ecc.) derivano concetti o sintassi dal C.

Il C è un linguaggio **compilato**, a **tipizzazione statica**, che dà accesso diretto alla memoria tramite i **puntatori**. Per questo motivo è estremamente efficiente ed è ancora oggi usato per sistemi operativi, driver, firmware, sistemi embedded e software che richiedono il massimo controllo sulle risorse della macchina.

Per esercitarci con il C useremo un [compilatore on line reperibile qui](https://www.onlinegdb.com/online_c_compiler). È preferibile però scaricare l'IDE installabile su computer [CodeBlock](https://www.codeblocks.org/) oppure l'ambiente [bloodshed](http://bloodshed.net/).

Ecco un primo esempio di codice:

{% highlight c %}
#include <stdio.h>

int main() {
    // definizione delle variabili
    int base, altezza, area;
    // inizializzazione delle variabili
    base = 10;
    altezza = 5;
    // esecuzione delle elaborazioni
    area = base * altezza;
    // comunicazione dei risultati
    printf("Il rettangolo con: \n");
    printf("base: %d\n", base);
    printf("altezza: %d\n", altezza);
    printf("ha come area: %d\n", area);
    return 0;
}
{% endhighlight %}

A differenza di Python, in linguaggio C le variabili vanno dichiarate indicando esplicitamente il tipo a cui appartengono. In questo pezzetto di codice dichiariamo le variabili base, altezza e area di tipo intero.

L'assegnazione ha la stessa sintassi del Python così come gli operatori matematici sono analoghi.

Il comando per mandare le informazioni in output è **printf**, che usa una stringa di formato con dei segnaposto (`%d` per interi, `%f` per decimali, ecc.) seguita dai valori da stampare.

**Introduciamo l'input**

{% highlight c %}
#include <stdio.h>

int main() {
    // definizione delle variabili
    int base, altezza, area;
    // input
    scanf("%d", &base);
    scanf("%d", &altezza);
    // esecuzione delle elaborazioni
    area = base * altezza;
    // comunicazione dei risultati
    printf("Il rettangolo con: \n");
    printf("base: %d\n", base);
    printf("altezza: %d\n", altezza);
    printf("ha come area: %d\n", area);
    return 0;
}
{% endhighlight %}

Utilizziamo la funzione **scanf** per chiedere informazioni all'utente. scanf va seguito dalla stringa di formato e dall'**indirizzo** della variabile (operatore `&`) che andrà a contenere l'informazione.

**Condizione**

La sintassi della condizione in C non è molto diversa dal Python. La parola chiave da utilizzare è **if**. All'interno delle parentesi tonde si specifica l'espressione da valutare. All'interno delle parentesi graffe si specifica il blocco di codice da eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int numero;
    scanf("%d", &numero);
    if (numero > 0) {
        printf("numero positivo\n");
    } else {
        printf("numero negativo\n");
    }
    return 0;
}
{% endhighlight %}

**L'iterazione**

L'iterazione in C può essere definita con la sintassi for. In questo caso inizializziamo un contatore **i** a zero. Al centro va definita la condizione che verrà valutata a ogni inizio iterazione; se l'esito della condizione è positivo vengono eseguite le istruzioni all'interno del blocco di codice definito dalle parentesi graffe.

{% highlight c %}
#include <stdio.h>

int main() {

    printf("inizio ciclo for \n");
    for (int i = 0; i < 10; i++) {
        printf("i vale: %d\n", i);
    }

    return 0;
}
{% endhighlight %}

**Il ciclo while**

L'iterazione in C può essere definita anche con la sintassi while. In questo caso inizializziamo un contatore **cont** a zero. Al centro va definita la condizione che verrà valutata a ogni inizio iterazione; se l'esito della condizione è positivo vengono eseguite le istruzioni all'interno del blocco di codice definito dalle parentesi graffe.

{% highlight c %}
#include <stdio.h>

int main() {

    printf("inizio ciclo while \n");
    int cont;
    cont = 0;
    while (cont < 10) {
        printf("cont vale: %d\n", cont);
        cont++;
    }

    return 0;
}
{% endhighlight %}
