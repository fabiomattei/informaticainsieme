---
title: 'PWM: regolare la luminosità di un led'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Con [`digitalWrite()`]({{ site.baseurl }}{% link _arduino/blink.md %}.html) un pin può stare solo in due stati: acceso (5V) o spento (0V). Ma se volessimo un led non solo acceso o spento, ma via via più debole o più intenso, come una lampadina con il dimmer? La soluzione si chiama **PWM** (*Pulse Width Modulation*, modulazione a larghezza di impulso).

### Come funziona il PWM

Un pin PWM non genera davvero una tensione intermedia: accende e spegne il pin molto rapidamente (migliaia di volte al secondo), variando la percentuale di tempo in cui resta acceso rispetto al tempo totale — il cosiddetto **duty cycle**. Se il pin è acceso per metà del tempo, l'occhio umano (troppo lento per percepire le singole accensioni) percepisce un led alla metà della luminosità massima. Un duty cycle del 100% equivale a un led sempre acceso, uno dello 0% a un led sempre spento.

Su Arduino Uno, non tutti i pin digitali supportano il PWM: solo quelli contrassegnati con il simbolo **~** accanto al numero (tipicamente 3, 5, 6, 9, 10, 11).

### Il collegamento

Identico a quello del [primo progetto col led]({{ site.baseurl }}{% link _arduino/blink.md %}.html), con l'unica differenza che il led va collegato a un pin PWM, ad esempio il pin **9**, invece del pin 13.

### Il codice

{% highlight c %}
// Dimmerare un led con il PWM

#define LED 9   // LED collegato a un pin PWM (contrassegnato da ~)

void setup() {
  pinMode(LED, OUTPUT);
}

void loop() {
  // Aumenta gradualmente la luminosità da spento ad acceso
  for (int luminosita = 0; luminosita <= 255; luminosita++) {
    analogWrite(LED, luminosita);
    delay(10);
  }

  // Diminuisce gradualmente la luminosità da acceso a spento
  for (int luminosita = 255; luminosita >= 0; luminosita--) {
    analogWrite(LED, luminosita);
    delay(10);
  }
}
{% endhighlight %}

La funzione `analogWrite()` accetta un valore da **0** (sempre spento) a **255** (sempre acceso): è il duty cycle espresso su 8 bit, non un voltaggio reale. Il nome può trarre in inganno — non c'entra con la lettura di segnali analogici vista con `analogRead()` — ma è il modo in cui Arduino genera un'uscita "analogica simulata" a partire da pin digitali.

### Altri usi del PWM

La stessa tecnica non serve solo per i led: è alla base del controllo della velocità di un motore in corrente continua, della posizione di un servomotore (con una variante nei tempi degli impulsi) e della generazione di toni audio su un piccolo speaker. Aver capito il PWM sul caso più semplice — un led che si accende e spegne più o meno a lungo — è la chiave per capire come Arduino controlla in modo graduale qualsiasi componente elettronico.
