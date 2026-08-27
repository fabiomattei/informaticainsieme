---
title: 'La struttura di uno sketch: setup() e loop()'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Ogni programma scritto per Arduino, chiamato in gergo **sketch**, ha un'unica struttura obbligatoria: due funzioni, `setup()` e `loop()`, sempre presenti, sempre con questo nome esatto. Prima ancora di collegare un [led su una breadboard]({{ site.baseurl }}{% link _arduino/breadboard.md %}.html), vale la pena capire bene cosa fanno e perché il linguaggio di Arduino è organizzato proprio così.

### Lo scheletro minimo

{% highlight c %}
void setup() {
  // codice eseguito una sola volta, all'avvio
}

void loop() {
  // codice eseguito ripetutamente, all'infinito
}
{% endhighlight %}

Anche uno sketch che non fa nulla deve contenere entrambe le funzioni: è la prima cosa che l'IDE di Arduino controlla prima di compilare, ed è il motivo per cui ogni progetto — dal più semplice al più complesso — parte sempre da questo scheletro.

### setup(): cosa succede all'accensione

`setup()` viene eseguita **una sola volta**, nel momento in cui Arduino si accende o viene resettato. È il posto giusto per tutto ciò che serve preparare prima che il programma vero e proprio inizi a girare:

* Dichiarare la modalità dei pin con `pinMode()` (input o output).
* Avviare la comunicazione seriale con `Serial.begin()`.
* Inizializzare librerie o sensori che richiedono una configurazione iniziale.
* Impostare eventuali variabili al loro valore di partenza.

Una volta terminata l'esecuzione di `setup()`, Arduino passa a `loop()` e non torna più indietro, a meno di un reset fisico o di un nuovo caricamento del programma.

### loop(): il cuore del programma

`loop()`, come dice il nome, viene eseguita **ripetutamente, all'infinito**, decine o centinaia di volte al secondo a seconda di quanto è "pesante" il codice al suo interno. Appena Arduino arriva alla fine della funzione, ricomincia immediatamente dall'inizio. È qui che vive la logica vera del programma: leggere sensori, controllare attuatori, prendere decisioni con `if`.

Il [primo progetto col led lampeggiante]({{ site.baseurl }}{% link _arduino/blink.md %}.html) è l'esempio più semplice di questo comportamento: `loop()` accende il led, aspetta, lo spegne, aspetta, e ricomincia — il "lampeggio" non è altro che questo ciclo che si ripete all'infinito.

### Un esempio commentato

{% highlight c %}
#define LED 13

void setup() {
  pinMode(LED, OUTPUT);   // eseguito una volta sola, prepara il pin
  Serial.begin(9600);     // eseguito una volta sola, avvia la seriale
}

void loop() {
  digitalWrite(LED, HIGH);       // eseguito a ogni ciclo
  Serial.println("LED acceso");  // eseguito a ogni ciclo
  delay(1000);
  digitalWrite(LED, LOW);
  Serial.println("LED spento");
  delay(1000);
}
{% endhighlight %}

Se `pinMode(LED, OUTPUT)` venisse messo per errore dentro `loop()` invece che dentro `setup()`, il programma funzionerebbe comunque: verrebbe semplicemente ripetuto inutilmente migliaia di volte, senza alcun beneficio, dato che configurare un pin è un'operazione che ha senso fare una sola volta.

### Perché questa struttura e non un "main" come in altri linguaggi

Chi ha già programmato in C o C++ sa che normalmente un programma parte da una funzione `main()`. Su Arduino `main()` esiste comunque, ma è nascosto dentro le librerie di sistema: chiama automaticamente `setup()` una volta e poi `loop()` all'infinito, dietro le quinte. Questa scelta semplifica molto la scrittura di programmi per microcontrollori, dove quasi ogni progetto ha davvero bisogno solo di "una fase di preparazione" seguita da "un ciclo che si ripete per sempre" — ed è anche il motivo per cui non serve mai scrivere esplicitamente un ciclo `while(true)` per far girare il programma: `loop()` lo fa già, implicitamente.

### Uscire (concettualmente) dal ciclo

`loop()` non è pensata per terminare: se contiene un `return`, semplicemente Arduino richiama `loop()` da capo subito dopo. Per far sì che parti del codice vengano eseguite solo in certe condizioni — non a ogni giro — si usano invece variabili di stato e confronti temporali, come visto nella pagina su [millis() e il timing non bloccante]({{ site.baseurl }}{% link _arduino/millis-non-blocking.md %}.html): la struttura di `loop()` resta identica, cambia solo cosa succede al suo interno.
