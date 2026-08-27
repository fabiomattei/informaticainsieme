---
title: 'La breadboard: come funziona e come si usa'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Prima di collegare il primo [led]({{ site.baseurl }}{% link _arduino/blink.md %}.html) ad Arduino, vale la pena capire bene lo strumento che rende possibile costruire un circuito senza saldare nulla: la **breadboard** (in italiano, un po' impropriamente, "basetta millefori" o "breadboard" tout court). Capire come sono collegati i suoi fori internamente evita la maggior parte degli errori di collegamento dei primi progetti.

### Cos'è e da dove viene il nome

La breadboard è una tavoletta di plastica forata, piena di piccoli connettori metallici nascosti sotto la superficie, che permette di collegare tra loro componenti elettronici (resistenze, led, sensori) e fili semplicemente inserendoli nei fori, senza saldature. Il nome, curiosamente, viene da un'abitudine reale: negli anni '20 e '30, prima che esistessero le breadboard moderne, alcuni sperimentatori inchiodavano davvero i componenti su un tagliere da pane (*bread board*) per costruire i primi circuiti radio.

### Come sono collegati i fori

La parte più importante da capire — e la fonte più comune di errori per chi inizia — è che i fori **non sono tutti indipendenti**: sono collegati elettricamente in gruppi nascosti sotto la plastica.

* **Le righe di alimentazione** (i due binari ai lati, contrassegnati di solito da una linea rossa e una blu o nera): tutti i fori di una stessa riga sono collegati tra loro in orizzontale, per tutta la lunghezza della breadboard. Sono pensati per distribuire, rispettivamente, l'alimentazione positiva (rosso) e il GND (blu/nero) a tutto il circuito.
* **Le colonne centrali**: nell'area centrale, i fori sono collegati in piccoli gruppi verticali di 5, tipicamente organizzati in due blocchi separati da un canale centrale (dove si inseriscono i componenti come i circuiti integrati). Un gruppo di 5 fori in colonna è quindi elettricamente equivalente a un unico punto: inserire due fili nello stesso gruppo li collega automaticamente tra loro.

Il canale centrale che divide la breadboard a metà **non** è collegato: le due metà sono elettricamente indipendenti, a meno di collegarle esplicitamente con un filo.

### Come collegare un componente

Per accendere un led con Arduino, ad esempio, lo schema tipico è:

1. Un filo dal pin digitale di Arduino a un foro di una colonna della breadboard.
2. La gamba lunga del led (anodo) inserita in un foro **della stessa colonna** del filo — così risultano collegati elettricamente, pur non toccandosi fisicamente.
3. Una resistenza che collega la gamba corta del led (catodo) a un'altra colonna.
4. Un filo da quella colonna alla riga GND della breadboard.
5. Un filo dalla riga GND della breadboard al pin **GND** di Arduino, per chiudere il circuito.

Il punto chiave è che due componenti si considerano "collegati" non perché si toccano fisicamente, ma perché condividono la stessa colonna (o la stessa riga di alimentazione) sotto la breadboard.

### Errori comuni da evitare

* **Corto circuito**: collegare direttamente la riga positiva e quella negativa senza nessun componente in mezzo (ad esempio un filo mal posizionato) crea un cortocircuito, che può danneggiare Arduino o il componente alimentato.
* **Led al contrario**: un led si accende solo se collegato con la polarità corretta (anodo verso l'alimentazione, catodo verso GND). Al contrario, semplicemente non si accende: non è un errore che danneggia il circuito, ma è la causa più frequente di "il mio led non funziona" nei primi progetti.
* **Dimenticare la resistenza**: un led collegato direttamente, senza una resistenza che limiti la corrente, rischia di bruciarsi quasi subito.
* **Confondere righe e colonne**: inserire due fili nella stessa riga di alimentazione pensando che siano indipendenti (lo sono su tutta la riga, quindi ogni filo in quella riga è collegato a tutti gli altri).

Con questi pochi principi chiari, la breadboard smette di essere un mistero e diventa lo strumento più veloce per passare da un'idea a un circuito funzionante — pronto per iniziare con il primo progetto vero e proprio: [far lampeggiare un led]({{ site.baseurl }}{% link _arduino/blink.md %}.html).
