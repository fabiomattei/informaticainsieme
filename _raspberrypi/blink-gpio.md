---
title: 'Blink di un led con i GPIO'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Come per Arduino, anche con il Raspberry Pi il primo progetto tipico è far lampeggiare un led. Non serve altro che un led, una resistenza e due fili: è l'occasione per imparare a collegare un componente ai pin **GPIO** e a controllarlo con qualche riga di Python. Se non l'hai ancora fatto, vale la pena leggere prima [cos'è il Raspberry Pi]({{ site.baseurl }}{% link _raspberrypi/introduzione.md %}.html) per capire cosa sono i GPIO e perché lavorano a 3.3V.

### Il collegamento

* Collegare il pin **GPIO17** a una resistenza da 220-330 Ω.
* Collegare la resistenza alla gamba più lunga del led (anodo).
* Collegare la gamba più corta del led (catodo) a un pin **GND** (ground) del Raspberry Pi.

A differenza di Arduino, sulla scheda del Raspberry Pi non c'è un led integrato pilotabile via GPIO: il collegamento esterno è sempre necessario.

### Il codice con gpiozero

La libreria **gpiozero**, inclusa di default in Raspberry Pi OS, permette di controllare i componenti collegati ai GPIO con pochissime righe, senza dover gestire manualmente lo stato basso/alto del pin:

{% highlight python %}
from gpiozero import LED
from time import sleep

led = LED(17)  # LED collegato al pin GPIO17

while True:
    led.on()     # accende il LED
    sleep(1)     # aspetta un secondo
    led.off()    # spegne il LED
    sleep(1)     # aspetta un secondo
{% endhighlight %}

Si esegue con `python3 nome_file.py` dal terminale del Raspberry Pi (o via SSH da un altro computer).

### Il codice con RPi.GPIO

Un'alternativa più "a basso livello", che espone in modo più esplicito la logica di lettura e scrittura dei pin, è la libreria **RPi.GPIO**:

{% highlight python %}
import RPi.GPIO as GPIO
from time import sleep

LED = 17
GPIO.setmode(GPIO.BCM)     # usa la numerazione BCM dei pin
GPIO.setup(LED, GPIO.OUT)  # imposta il pin come output

try:
    while True:
        GPIO.output(LED, GPIO.HIGH)  # accende il LED
        sleep(1)
        GPIO.output(LED, GPIO.LOW)   # spegne il LED
        sleep(1)
finally:
    GPIO.cleanup()  # rilascia i pin alla fine del programma
{% endhighlight %}

Da notare `GPIO.setmode(GPIO.BCM)`: i pin GPIO possono essere indicati con due numerazioni diverse, quella fisica (la posizione sul connettore) e quella **BCM** (il numero logico assegnato dal processore Broadcom). Nel codice si usa quasi sempre la numerazione BCM, mentre è bene tenere sotto mano uno schema del connettore per non confondersi durante i collegamenti.

### Perché due librerie

**gpiozero** è pensata per essere semplice e leggibile, ideale per iniziare e per progetti didattici. **RPi.GPIO** offre un controllo più diretto e "grezzo" del pin, utile quando serve capire davvero cosa succede a basso livello o quando un progetto richiede funzionalità non coperte da gpiozero. Non è necessario sceglierne una definitivamente: è comune usare l'una o l'altra a seconda del progetto.
