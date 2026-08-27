---
title: 'Blink senza delay(): il timing con millis()'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Nel primo progetto abbiamo fatto lampeggiare un [led con `delay()`]({{ site.baseurl }}{% link _arduino/blink.md %}.html). Funziona, ma ha un limite importante: mentre `delay()` aspetta, Arduino non fa altro. Se nello stesso programma volessimo anche [leggere un pulsante]({{ site.baseurl }}{% link _arduino/pulsante.md %}.html), il pulsante non verrebbe letto durante l'attesa, e una pressione rapida potrebbe passare inosservata. Il problema si chiama **codice bloccante**, e la soluzione si chiama **millis()**.

### Cosa restituisce millis()

La funzione `millis()` restituisce il numero di millisecondi trascorsi da quando Arduino si è acceso (o è stato resettato l'ultima volta), come un cronometro che parte da solo all'avvio. Non blocca mai l'esecuzione: si limita a restituire un numero, e sta al programma decidere cosa farne.

### Il blink senza delay()

L'idea è confrontare ogni giro di `loop()` con `millis()` e agire solo quando è passato abbastanza tempo dall'ultima azione, invece di fermare tutto con `delay()`:

{% highlight c %}
// Blink di un LED senza bloccare l'esecuzione

#define LED 13

unsigned long ultimoCambio = 0;   // memorizza quando il LED è cambiato l'ultima volta
const long intervallo = 1000;     // intervallo in millisecondi
bool statoLed = LOW;

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  unsigned long adesso = millis();

  if (adesso - ultimoCambio >= intervallo) {
    ultimoCambio = adesso;          // aggiorna il riferimento temporale
    statoLed = !statoLed;           // inverte lo stato del LED
    digitalWrite(LED, statoLed);
  }

  // qui si può aggiungere altro codice, ad esempio leggere un pulsante,
  // ed eseguirà comunque a ogni giro senza aspettare il LED
}
{% endhighlight %}

Il ciclo `loop()` gira continuamente, migliaia di volte al secondo, ma il led cambia stato solo quando è trascorso l'intervallo desiderato. Nel frattempo, qualsiasi altro codice inserito in `loop()` — leggere un sensore, controllare un pulsante — viene eseguito a ogni giro, senza mai fermarsi ad aspettare.

### Perché `unsigned long`

`millis()` restituisce un valore di tipo `unsigned long`, capace di contare fino a circa 4,3 miliardi: un numero enorme, ma non infinito. Dopo circa 50 giorni di funzionamento continuo, il contatore torna a zero (*overflow*). Il confronto `adesso - ultimoCambio >= intervallo` è scritto apposta per restare corretto anche in quel momento, grazie al modo in cui l'aritmetica dei numeri senza segno gestisce il "giro": usare invece `adesso >= ultimoCambio + intervallo` sembrerebbe equivalente, ma fallirebbe proprio nell'istante dell'overflow.

### Quando conviene usarlo

Non tutto il codice deve rinunciare a `delay()`: per un piccolo progetto isolato, come il primo blink, `delay()` resta più semplice da leggere e capire. Ma appena un programma deve fare più cose "contemporaneamente" — lampeggiare un led *e* leggere un sensore *e* controllare un pulsante — `millis()` diventa indispensabile, perché è la base con cui Arduino simula il multitasking pur eseguendo un solo programma alla volta.
