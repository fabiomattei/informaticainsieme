---
title: 'Blink di un led'
date: '2020-02-07T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

oggi inizieremo ad usare la scheda Arduino: imparerai a far lampeggiare un led in modo pulsato e regolare.

Questo è solitamente il primo progetto che si realizza con Arduino uno, ed è la base per chi inizia a programmarlo.

Possiamo utilizzare anche il led integrato sulla scheda indicato con la lettera L senza necessariamente collegare il led come da schema. Il led L ha già una resistenza smd integrata sulla scheda, questo ci consente di inserire il led direttamente nei pin GND e 13.

- Collegare il pin 13 di Arduino alla resistenza.
- Collegare alla resistenza la gamba del led più lunga.
- Collegare la gamba più corta del led al pin GND (ground) di Arduino.

Inserire il codice su Arduino tramite il software IDE

![Modifica PATH](/images/arduino/blinkproject.png){:class="aside-image"}


{% highlight c %}
// Arduino blink facciamo lampeggiare un led  
  
#define LED 13            // LED collegato al pin digitale 13  
  
void setup() {  
  pinMode(LED, OUTPUT);     // imposta il pin digitale come output  
}  
  
void loop() {  
  digitalWrite(LED, HIGH);  // accende il LED  
  delay(1000);              // aspetta un secondo  
  digitalWrite(LED, LOW);   // spegne il LED  
  delay(1000);              // aspetta un secondo  
}  

{% endhighlight %}
