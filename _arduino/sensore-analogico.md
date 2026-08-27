---
title: 'Leggere un sensore analogico: il potenziometro'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Finora abbiamo visto segnali digitali: un [pulsante]({{ site.baseurl }}{% link _arduino/pulsante.md %}.html) premuto o rilasciato, un [led]({{ site.baseurl }}{% link _arduino/blink.md %}.html) acceso o spento. Molti sensori del mondo reale, però, non restituiscono solo due stati, ma un valore che varia con continuità: la luce ambientale, la temperatura, la posizione di una manopola. Per leggerli servono i pin **analogici** di Arduino.

### Il potenziometro

Il componente più semplice per esercitarsi è il **potenziometro**: una resistenza variabile a tre terminali, tipicamente comandata da una manopola o da un cursore. Ruotandola, cambia la tensione che si preleva dal terminale centrale, tra un minimo (0V) e un massimo (5V) a seconda della posizione.

### Il collegamento

- Collegare i due terminali esterni del potenziometro rispettivamente a **5V** e **GND**.
- Collegare il terminale centrale (il cursore) al pin **A0** di Arduino, uno dei pin analogici.

### Il codice

{% highlight c %}
// Lettura di un potenziometro

#define POTENZIOMETRO A0   // potenziometro collegato al pin analogico A0

void setup() {
  Serial.begin(9600);
}

void loop() {
  int valore = analogRead(POTENZIOMETRO);  // legge il pin analogico
  Serial.println(valore);
  delay(100);
}
{% endhighlight %}

A differenza di `digitalRead()`, che restituisce solo `HIGH` o `LOW`, `analogRead()` restituisce un numero tra **0** e **1023**: Arduino converte la tensione letta sul pin (tra 0V e 5V) in 1024 livelli discreti, grazie a un componente interno chiamato **ADC** (*Analog-to-Digital Converter*, convertitore analogico-digitale). Questa risoluzione a 10 bit (2^10 = 1024) è il motivo per cui, ruotando lentamente il potenziometro, si vedono nel Monitor seriale valori che cambiano con continuità invece di saltare bruscamente tra pochi stati.

### Ricalibrare il valore letto

Il valore grezzo (0-1023) raramente è comodo da usare direttamente: spesso serve convertirlo in un intervallo più significativo, ad esempio per pilotare la luminosità di un led con il [PWM]({{ site.baseurl }}{% link _arduino/pwm-dimmer-led.md %}.html), che accetta valori da 0 a 255:

{% highlight c %}
int valore = analogRead(POTENZIOMETRO);       // 0-1023
int luminosita = map(valore, 0, 1023, 0, 255); // riporta il valore in scala 0-255
analogWrite(LED, luminosita);
{% endhighlight %}

La funzione `map()` è pensata esattamente per questo: riscalare un valore da un intervallo di partenza a uno di arrivo, senza doverlo calcolare manualmente con una proporzione.

### Altri sensori analogici

Lo stesso principio del potenziometro si applica a moltissimi sensori: una **fotoresistenza (LDR)** che varia resistenza in base alla luce, un **termistore** che varia resistenza in base alla temperatura, un sensore di umidità del suolo. Cambia il componente, ma il codice per leggerlo resta lo stesso: `analogRead()` su un pin analogico, restituendo sempre un valore tra 0 e 1023 da interpretare in base alla scheda tecnica del sensore usato.
