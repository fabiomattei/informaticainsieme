---
title: 'Lezione 02, Il vicino più simile'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un punto nuovo circondato dai suoi vicini più simili, usati per decidere il suo colore](/images/ia/come-pensano-le-macchine-che-imparano-lezione-02-il-vicino-piu-simile/come-pensano-le-macchine-che-imparano-lezione-02-il-vicino-piu-simile.svg){:class="aside-image"}

### 2.1 Chi ti somiglia di più?

Immagina di arrivare in una città sconosciuta e di dover indovinare il mestiere di uno sconosciuto seduto al bar, guardando solo come è vestito e cosa ha appoggiato sul tavolo. Non conosci nessuna regola generale scritta da qualcuno, ma conosci già, per esperienza, tante persone di mestieri diversi. La strategia più naturale è: penso a chi, fra le persone che già conosco, gli somiglia di più nell'abbigliamento e negli oggetti sul tavolo, e scommetto che faccia lo stesso mestiere di quelle.

Questa idea, applicata a un mucchio di esempi già etichettati, è uno degli algoritmi di machine learning più semplici che esistano: si chiama **k-NN**, dall'inglese *k-Nearest Neighbors*, cioè "k vicini più prossimi". Non costruisce nessuna regola generale, non riassume mai i dati in una formula compatta: si limita a tenere in memoria tutti gli esempi visti, e ogni volta che arriva un caso nuovo, guarda quali esempi già noti gli somigliano di più.

### 2.2 Trasformare gli esempi in punti su una mappa

Per far funzionare questa idea serve un modo preciso di dire "quanto si somigliano" due esempi, non basta un'impressione, serve un numero. L'idea alla base è trasformare ogni esempio in un punto su una mappa, usando le sue caratteristiche come coordinate.

Riprendiamo le angurie della Lezione 1, ma questa volta con caratteristiche numeriche invece che a categorie: al posto di "cupo o acuto" usiamo un punteggio di gravità del suono da 0 a 10, e al posto di "intenso o pallido" un punteggio di intensità del giallo da 0 a 10. Ogni anguria diventa così una coppia di numeri, per esempio (7, 8) per un suono piuttosto cupo e un giallo piuttosto intenso, che puoi disegnare come un punto su un piano, esattamente come le coordinate di un punto su una cartina. Questo piano immaginario, dove ogni caratteristica è un asse e ogni esempio è un punto, si chiama **spazio delle caratteristiche**.

Una volta che gli esempi sono punti su questa mappa, "quanto si somigliano due angurie" diventa qualcosa di molto concreto: quanto sono vicini i loro due punti sulla mappa. Due angurie con punteggi simili di suono e di giallo finiscono vicine; due angurie molto diverse finiscono lontane. La **distanza** fra due punti si misura esattamente come la distanza fra due località su una cartina stradale, più le coordinate differiscono, più i punti sono lontani.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="spazio-title spazio-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="spazio-title">Lo spazio delle caratteristiche</title>
  <desc id="spazio-desc">Un piano con due assi, gravità del suono e intensità del giallo. Alcuni punti pieni (angurie mature) e alcuni vuoti (non mature) sono sparsi sul piano, più un punto nuovo segnato con un punto interrogativo.</desc>

  <line x1="60" y1="270" x2="440" y2="270" stroke="#828282" stroke-width="1.5" />
  <line x1="60" y1="270" x2="60" y2="30" stroke="#828282" stroke-width="1.5" />
  <text x="250" y="300" fill="#828282" font-size="12" text-anchor="middle">gravità del suono →</text>
  <text x="30" y="150" fill="#828282" font-size="12" text-anchor="middle" transform="rotate(-90 30 150)">intensità del giallo →</text>

  <g fill="#c85506">
    <circle cx="300" cy="90" r="7" /><circle cx="340" cy="120" r="7" /><circle cx="270" cy="70" r="7" />
  </g>
  <g fill="#fdfdfd" stroke="#2a7ae2" stroke-width="2">
    <circle cx="120" cy="230" r="7" /><circle cx="160" cy="200" r="7" /><circle cx="100" cy="180" r="7" />
  </g>

  <circle cx="290" cy="130" r="9" fill="none" stroke="#111" stroke-width="2.5" />
  <text x="290" y="118" fill="#111" font-size="11" text-anchor="middle" font-weight="bold">?</text>
  <line x1="290" y1="130" x2="300" y2="90" stroke="#828282" stroke-width="1" stroke-dasharray="3 2" />
  <line x1="290" y1="130" x2="340" y2="120" stroke="#828282" stroke-width="1" stroke-dasharray="3 2" />
  <line x1="290" y1="130" x2="270" y2="70" stroke="#828282" stroke-width="1" stroke-dasharray="3 2" />

  <text x="250" y="310" fill="#828282" font-size="0"></text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Il punto "?" è più vicino ai tre punti pieni (mature): probabilmente è matura anche lei.</figcaption>
</figure>

### 2.3 L'algoritmo: guarda i k più vicini, vota a maggioranza

L'algoritmo k-NN, a questo punto, è quasi imbarazzante nella sua semplicità. Per prevedere l'etichetta di un esempio nuovo:

1. Calcola la distanza fra il punto nuovo e *ogni* punto già noto nell'insieme di addestramento.
2. Prendi i **k** punti più vicini (k è un numero che scegli tu in anticipo: per esempio 3, o 5).
3. Guarda le etichette di questi k vicini, e assegna al punto nuovo l'etichetta più frequente fra loro, una specie di votazione a maggioranza fra i vicini.

Non c'è nessuna fase separata di "addestramento" in cui il modello elabora i dati e ne ricava una regola compatta, come invece succede negli algoritmi delle prossime lezioni: k-NN si limita a *ricordare* tutti gli esempi, e fa tutto il lavoro nel momento in cui deve fare una previsione. Per questo si dice a volte che k-NN è un algoritmo "pigro" (*lazy learning*): rimanda ogni sforzo di calcolo al momento in cui serve davvero una risposta.

### 2.4 Quanto conta la scelta di k

La lettera k non è un dettaglio tecnico trascurabile: cambia in modo sostanziale il comportamento del modello, ed è utile capire perché con un caso limite.

Se scegli **k = 1**, il punto nuovo prende semplicemente l'etichetta del suo unico vicino più prossimo. Questo rende il modello estremamente sensibile a ogni singolo esempio, compresi quelli anomali: un'unica anguria matura finita per sbaglio in mezzo a un gruppo di non mature, magari perché qualcuno l'ha etichettata male, può "contagiare" tutti i punti nuovi che le capitano vicino, anche se nella zona circostante la stragrande maggioranza dice il contrario.

Se scegli un **k molto grande**, diciamo, pari a tutto l'insieme di addestramento, succede l'opposto: ogni previsione diventa semplicemente "l'etichetta più comune in assoluto", sempre la stessa per qualunque punto nuovo, ignorando completamente dove si trova sulla mappa. Un k enorme annega il segnale locale (chi sono davvero i vicini di questo punto specifico) dentro la media generale di tutto il dataset.

La scelta pratica sta nel mezzo: un k abbastanza piccolo da restare sensibile alla zona specifica della mappa dove cade il punto nuovo, ma abbastanza grande da non farsi ingannare da un singolo esempio anomalo. Non esiste un valore di k giusto in assoluto, dipende da quanto sono "rumorosi" i dati e da quanti esempi hai, ed è un tema a cui torneremo con più precisione nella Lezione 5, quando parleremo di come misurare se un modello sta generalizzando bene oppure no.

### 2.5 Il prezzo della semplicità

k-NN ha un pregio enorme, è facile da capire e da spiegare, non nasconde nulla dentro una scatola nera, ma anche due limiti pratici che vale la pena conoscere fin da subito, perché motiveranno gli algoritmi delle prossime lezioni.

Il primo è il costo: per ogni singola previsione, k-NN deve calcolare la distanza dal punto nuovo a *tutti* gli esempi noti. Con dieci angurie non è un problema; con dieci milioni di transazioni bancarie, ricalcolare tutte quelle distanze ogni volta diventa lento.

Il secondo è più sottile e riguarda proprio la nozione di "vicinanza": funziona bene quando le caratteristiche numeriche sono comparabili e ben scelte, ma se una caratteristica è misurata su una scala molto più ampia delle altre (il peso in grammi, da 2000 a 9000, contro un punteggio di giallo da 0 a 10), finisce per dominare da sola il calcolo della distanza, schiacciando il contributo di tutte le altre. È un problema pratico, risolvibile riportando tutte le caratteristiche sulla stessa scala prima di calcolare le distanze, un dettaglio tecnico che qui basta conoscere di nome, senza approfondirlo.

---

> **Prova tu, Trova i vicini più vicini**
>
> Ecco cinque angurie già note, con due caratteristiche numeriche (gravità del suono da 0 a 10, intensità del giallo da 0 a 10) e la loro etichetta:
>
> | Anguria | Suono | Giallo | Matura? |
> |---|---|---|---|
> | A | 8 | 7 | sì |
> | B | 7 | 9 | sì |
> | C | 2 | 3 | no |
> | D | 3 | 1 | no |
> | E | 9 | 8 | sì |
>
> Arriva un'anguria nuova, **N**, con suono = 8 e giallo = 6. Per misurare la vicinanza, usa questa regola semplificata di distanza: somma il valore assoluto della differenza su ciascuna caratteristica (per esempio, la distanza fra N e A è |8−8| + |7−6| = 0 + 1 = 1).
>
> 1. Calcola la distanza fra N e ciascuna delle cinque angurie note.
> 2. Con **k = 3**, quali sono le tre angurie più vicine a N? Che etichetta prevede il voto a maggioranza fra loro?
> 3. Con **k = 5** (cioè guardando tutte e cinque le angurie), la previsione cambia? Perché, secondo te, k = 5 in questo caso particolare produce lo stesso risultato, o un risultato diverso, rispetto a k = 3?

---

*Continua con la [Lezione 03, Un albero di domande]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-lezione-03-un-albero-di-domande.md %}.html)*
