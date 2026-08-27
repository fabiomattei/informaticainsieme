---
title: 'Misurare una distanza con il sensore a ultrasuoni'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Dopo aver letto un valore continuo con il [potenziometro]({{ site.baseurl }}{% link _arduino/sensore-analogico.md %}.html), vediamo un sensore più interessante: l'**HC-SR04**, un modulo a ultrasuoni capace di misurare una distanza senza toccare l'oggetto, esattamente come fa un pipistrello per orientarsi al buio.

### Come funziona

Il modulo ha quattro pin: alimentazione, GND, **Trig** e **Echo**. Il funzionamento si basa su un principio semplice: si invia un breve impulso ultrasonico dal pin Trig, l'onda sonora viaggia nell'aria, colpisce un ostacolo e torna indietro; il pin Echo resta alto per tutto il tempo trascorso tra l'invio e il ritorno dell'eco. Conoscendo la velocità del suono nell'aria, da quel tempo si ricava la distanza.

### Il collegamento

- **VCC** del sensore a **5V** di Arduino.
- **GND** del sensore a **GND** di Arduino.
- **Trig** al pin digitale **9**.
- **Echo** al pin digitale **10**.

### Il codice

{% highlight c %}
// Misura della distanza con HC-SR04

#define TRIG 9
#define ECHO 10

void setup() {
  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);
  Serial.begin(9600);
}

void loop() {
  // Invia un impulso ultrasonico di 10 microsecondi
  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  // Misura per quanto tempo il pin ECHO resta alto
  long durata = pulseIn(ECHO, HIGH);

  // Converte il tempo (microsecondi) in distanza (centimetri)
  float distanza = durata * 0.0343 / 2;

  Serial.print("Distanza: ");
  Serial.print(distanza);
  Serial.println(" cm");

  delay(200);
}
{% endhighlight %}

### Il calcolo della distanza

`pulseIn(ECHO, HIGH)` restituisce, in microsecondi, quanto tempo il pin Echo resta a livello alto: cioè il tempo totale del viaggio di andata e ritorno dell'onda sonora. La velocità del suono nell'aria è di circa 343 metri al secondo, pari a **0,0343 cm per microsecondo**. Moltiplicando la durata per questo valore si ottiene la distanza percorsa dal suono nel viaggio completo (andata **e** ritorno): dividendo per 2 si ottiene la sola distanza tra sensore e ostacolo.

### Un progetto naturale: allarme di prossimità

Combinando questo sensore con quanto visto finora, è facile costruire un piccolo allarme di parcheggio: quando la distanza scende sotto una soglia, si accende un [led]({{ site.baseurl }}{% link _arduino/blink.md %}.html) o si emette un suono con il [buzzer]({{ site.baseurl }}{% link _arduino/buzzer-tone.md %}.html), magari facendo diventare i beep sempre più frequenti man mano che l'ostacolo si avvicina — un buon esercizio per mettere insieme sensori e attuatori in un unico programma.
