---
title: 'Suoni con il buzzer: la funzione tone()'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Finora abbiamo controllato luci e letto sensori: proviamo ora a far produrre un suono ad Arduino, collegando un piccolo **buzzer** (cicalino) piezoelettrico. È un componente economico e diffusissimo per segnalazioni acustiche, allarmi e semplici melodie.

### Il collegamento

- Collegare il terminale positivo del buzzer al pin **8** di Arduino.
- Collegare il terminale negativo del buzzer a **GND**.

I buzzer piezoelettrici più comuni non richiedono resistenze aggiuntive, a differenza dei led.

### Il codice: un singolo suono

{% highlight c %}
// Un beep con il buzzer

#define BUZZER 8

void setup() {
  tone(BUZZER, 440);  // emette una nota a 440 Hz (il La centrale)
  delay(500);          // per mezzo secondo
  noTone(BUZZER);      // interrompe il suono
}

void loop() {
  // vuoto: il suono viene emesso una sola volta in setup()
}
{% endhighlight %}

La funzione `tone(pin, frequenza)` fa vibrare il buzzer alla frequenza indicata, espressa in **Hz** (hertz, oscillazioni al secondo): più alta la frequenza, più acuto il suono. `noTone()` interrompe l'emissione. È possibile anche indicare una durata direttamente in `tone()`, ad esempio `tone(BUZZER, 440, 500)`, senza dover gestire `delay()` e `noTone()` separatamente — utile per un singolo suono, meno per suonare più note in sequenza in modo controllato.

### Suonare una melodia

Combinando `tone()` con un array di frequenze e uno di durate, si può far suonare una breve sequenza di note:

{% highlight c %}
// Una breve sequenza di note

#define BUZZER 8

int note[] = {262, 294, 330, 349};       // Do, Re, Mi, Fa
int durate[] = {300, 300, 300, 600};     // durata di ciascuna nota in millisecondi

void setup() {
  for (int i = 0; i < 4; i++) {
    tone(BUZZER, note[i]);
    delay(durate[i]);
    noTone(BUZZER);
    delay(50);  // piccola pausa tra una nota e l'altra
  }
}

void loop() {
}
{% endhighlight %}

Il ciclo `for` scorre i due array in parallelo, usando lo stesso indice `i` per associare a ogni nota la propria durata: una struttura molto comune ogni volta che servono più informazioni "allineate" tra loro, come qui frequenza e durata.

### Un limite da conoscere

`tone()` usa internamente uno dei timer hardware di Arduino, lo stesso sfruttato dal [PWM su alcuni pin]({{ site.baseurl }}{% link _arduino/pwm-dimmer-led.md %}.html): mentre un suono è in corso, l'uscita PWM sui pin 3 e 11 (su Arduino Uno) può risultare temporaneamente disturbata. Per progetti semplici non è un problema, ma vale la pena saperlo se un led dimmerato con PWM inizia a comportarsi in modo strano non appena si aggiunge un buzzer al circuito.
