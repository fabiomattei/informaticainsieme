---
title: 'Leggere un pulsante'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Dopo aver imparato a [far lampeggiare un led]({{ site.baseurl }}{% link _arduino/blink.md %}.html), il passo naturale successivo è far reagire Arduino a qualcosa che accade nel mondo esterno: il progetto più semplice per farlo è leggere lo stato di un pulsante.

### Il collegamento

- Collegare una gamba del pulsante al pin **2** di Arduino.
- Collegare la stessa gamba, tramite una resistenza da 10 kΩ, a **GND**.
- Collegare l'altra gamba del pulsante a **5V**.

Questa configurazione si chiama **resistenza di pull-down**: quando il pulsante non è premuto, la resistenza "tira" il pin verso 0V (livello basso, `LOW`), garantendo una lettura stabile. Senza questa resistenza, il pin resterebbe **fluttuante** (*floating*): non collegato saldamente né a 0V né a 5V, e leggerebbe valori casuali per interferenza elettrica, rendendo il circuito inaffidabile.

### Il codice

{% highlight c %}
// Lettura dello stato di un pulsante

#define PULSANTE 2   // pulsante collegato al pin digitale 2
#define LED 13       // LED integrato su Arduino

void setup() {
  pinMode(PULSANTE, INPUT);   // imposta il pin come input
  pinMode(LED, OUTPUT);       // imposta il pin come output
  Serial.begin(9600);         // avvia la comunicazione seriale
}

void loop() {
  int stato = digitalRead(PULSANTE);  // legge lo stato del pulsante

  if (stato == HIGH) {
    digitalWrite(LED, HIGH);   // pulsante premuto: accende il LED
    Serial.println("Pulsante premuto");
  } else {
    digitalWrite(LED, LOW);    // pulsante rilasciato: spegne il LED
  }
}
{% endhighlight %}

La funzione `digitalRead()` restituisce `HIGH` o `LOW` a seconda della tensione presente sul pin in quel preciso istante. `Serial.begin()` e `Serial.println()` permettono di inviare messaggi dal Arduino al computer, visibili aprendo il **Monitor seriale** nell'IDE: uno strumento prezioso per capire cosa sta facendo la scheda senza doverla collegare a un led o a un display.

### Usare il resistore interno (alternativa senza resistenza esterna)

Arduino dispone di una resistenza di pull-up già integrata nel microcontrollore, attivabile via software, che evita di doverne collegare una esterna:

{% highlight c %}
pinMode(PULSANTE, INPUT_PULLUP);
{% endhighlight %}

Con `INPUT_PULLUP` la logica si inverte: il pin legge `HIGH` quando il pulsante **non** è premuto, e `LOW` quando è premuto, perché la resistenza interna tiene il pin normalmente alto, e il pulsante lo collega a GND quando viene premuto. È una soluzione comoda per risparmiare componenti, a patto di ricordarsi che il comportamento è invertito rispetto al pull-down esterno.

### Il problema del rimbalzo (debounce)

Un pulsante meccanico, premuto, non passa in modo perfettamente netto da 0V a 5V: il contatto "rimbalza" per pochi millisecondi, generando più transizioni rapidissime invece di una sola. Se il programma conta ogni pressione, rischia di contarne diverse per una singola pressione reale — un problema noto come **bouncing**. Le soluzioni più comuni sono aggiungere un piccolo ritardo (`delay()`) dopo aver rilevato una pressione, oppure usare una libreria dedicata al debounce, che filtra questi rimbalzi in modo più affidabile.
