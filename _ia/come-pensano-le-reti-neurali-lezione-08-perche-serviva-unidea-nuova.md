---
title: 'Lezione 08 — Perché serviva un''idea completamente nuova'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 8.1 Due problemi, uno più profondo dell'altro

La Lezione 4 ha lasciato in sospeso due problemi distinti del "bigliettino che si aggiorna pagina per pagina", ed è arrivato il momento di tenerli ben separati, perché richiedono rimedi molto diversi. Il primo — il bigliettino che si sporca su sequenze lunghe — è in parte curato dal diario più furbo di LSTM e GRU (Sezione 4.5): un problema di *qualità* della memoria, alleviabile con un meccanismo migliore. Il secondo, più profondo, è che il tuo amico deve leggere una pagina alla volta, in ordine, senza scorciatoie: un vincolo che discende direttamente da *cosa significa* leggere in sequenza, non da un difetto correggibile con un diario più intelligente. Questa lezione si concentra sul secondo problema, perché è quello che ha davvero motivato l'abbandono di questo tipo di lettura pagina-per-pagina, non solo la sua correzione.

### 8.2 Un primo tentativo: tornare a guardare indietro

Un primo passo verso la soluzione, storicamente precedente al salto che racconteremo fra poco, ha lasciato intatta l'idea del bigliettino che avanza pagina per pagina, ma le ha affiancato una capacità nuova: invece di fidarsi solo del bigliettino compresso, il modello impara a "tornare a sfogliare" le pagine passate, decidendo quali contano di più per la decisione del momento, e leggendole di nuovo direttamente — invece di sperare che i dettagli importanti siano sopravvissuti nel bigliettino fino a lì. Questo risolve un pezzo del problema — ogni pagina resta consultabile, non solo il suo eco sbiadito nel bigliettino finale — ma in questa prima versione restava comunque incollata all'idea di leggere una pagina dopo l'altra: il vincolo di sequenzialità della Sezione 8.1 restava intatto, sotto la superficie.

### 8.3 Cosa servirebbe davvero: guardare tutto insieme, in un colpo solo

Poniamoci la domanda nel modo più diretto possibile: cosa servirebbe per eliminare del tutto l'obbligo di leggere in ordine, mantenendo comunque la capacità di collegare fra loro pagine qualsiasi, vicine o lontanissime? Servirebbe una capacità con due proprietà, entrambe assenti in un lettore che avanza pagina per pagina:

1. **Un collegamento diretto fra due punti qualunque, di lunghezza sempre uguale.** Nella lettura pagina per pagina, collegare la pagina 1 alla pagina 500 richiede di attraversare tutte le 499 pagine di mezzo. Servirebbe invece poter "guardare" direttamente una qualunque altra pagina in un solo passo, indipendentemente da quanto è lontana.
2. **Nessun obbligo di leggere in un ordine particolare.** Capire la pagina 200 non dovrebbe richiedere di aver già finito di elaborare la pagina 199: tutte le pagine dovrebbero poter essere lette contemporaneamente, sfruttando pienamente un esercito di lettori che lavorano in parallelo invece che uno solo in fila.

La capacità di "tornare a guardare indietro" della Sezione 8.2, presa da sola — senza il bigliettino sequenziale che l'accompagnava — soddisfa già la prima proprietà: il collegamento fra due pagine si calcola direttamente dal contenuto di entrambe, senza dover passare attraverso una lunga catena di bigliettini intermedi. Mancava solo il coraggio di chiedersi: e se eliminassimo *del tutto* la lettura pagina-per-pagina, lasciando che sia questa capacità di guardare-ovunque, da sola, a fare tutto il lavoro?

### 8.4 Il salto del 2017: se bastasse solo l'attenzione?

È esattamente questa la mossa compiuta nel 2017 da un gruppo di ricercatori, in un articolo dal titolo quasi provocatorio: *"L'attenzione è tutto ciò che serve"*. La loro proposta, il **Transformer**, buttava via del tutto l'idea del bigliettino che avanza pagina per pagina e costruiva un intero modello attorno alla sola capacità di guardare-ovunque — chiamata, in questo contesto più preciso, **attenzione**. Vale la pena essere chiari sul perché questo non sia un semplice miglioramento incrementale rispetto al "tornare a guardare indietro" della Sezione 8.2: non si tratta di rendere la lettura pagina-per-pagina più efficiente o più stabile, ma di rimuovere del tutto il vincolo che la rende, per quanto raffinata, incapace di leggere più di una pagina alla volta. È un cambiamento di categoria, non di grado — ed è la ragione per cui allenare modelli su quantità di testo enormi, oggi comuni, è diventato concretamente possibile.

### 8.5 Cinquecento pagine, un solo sguardo

Prova a immaginare un libro di 500 pagine, e la necessità di collegare un indizio nascosto a pagina 10 con una rivelazione a pagina 490 — 480 pagine di distanza. Con un lettore pagina-per-pagina, quell'indizio deve attraversare tutte e 480 le pagine di mezzo, indebolendosi un po' a ogni passaggio (il bigliettino che si sporca, Sezione 4.4) — più le due pagine sono lontane, più il collegamento fatica a sopravvivere. Con la capacità di guardare-ovunque del Transformer, lo stesso collegamento si calcola con un confronto diretto fra le due pagine — un percorso di lunghezza sempre uguale, che la distanza sia di 480 pagine o di 2.

E sul fronte del tempo necessario a leggere: un lettore pagina-per-pagina impiega necessariamente 500 passaggi in sequenza per l'intero libro, uno alla volta, nessuno saltabile. Un modello basato sulla sola attenzione, senza alcun obbligo di ordine fra le pagine, può elaborare tutte e 500 le pagine **contemporaneamente**, in un solo passaggio — non perché faccia meno calcoli in totale, ma perché nessuno di quei calcoli deve aspettare che un altro sia finito prima di cominciare. Ed è proprio questo, più di ogni altro fattore, a determinare quanto tempo reale serve per allenare un modello su quantità di testo enormi.

### 8.6 Dove andare da qui

Se sei arrivato fin qui, hai visto — con numeri piccoli, tracciabili a mano — ogni pezzo che serve per capire come funziona una rete neurale che impara: un interruttore che si corregge, più interruttori impilati per superare i propri limiti, uno stencil che riconosce motivi ovunque appaiano, un bigliettino che tiene traccia del tempo, un modo per misurare quanto si sta sbagliando, un modo per far risalire la colpa fino al primo peso della rete, gli accorgimenti che rendono l'allenamento efficace e non solo una memorizzazione, e infine il motivo preciso per cui leggere una pagina alla volta, a un certo punto, ha smesso di bastare.

Se vuoi vedere questi stessi meccanismi raccontati con le formule vere, i pesi calcolati fino in fondo, una lezione per ciascuna delle idee di questo libro, *Come Imparano le Reti Neurali* ti aspetta. Se invece l'interesse è specificamente per i chatbot che usi ogni giorno — cosa fanno esattamente con l'attenzione appena incontrata, come imparano a essere utili e non solo a indovinare la parola successiva, perché a volte inventano cose false — *Come Pensano le Macchine che Parlano* riprende esattamente da qui, con lo stesso spirito di questo libro: nessuna formula, solo analogie scelte per restare vere nella sostanza.

---

> **Prova tu — Distanza e parallelismo**
>
> Un romanzo giallo di 300 pagine nasconde, a pagina 15, un dettaglio apparentemente insignificante (un personaggio secondario che porta un ombrello nonostante il sole) che si rivela cruciale solo a pagina 290, quando quello stesso ombrello viene usato come arma del delitto.
>
> 1. Un lettore che procede pagina per pagina, tenendo un bigliettino mentale aggiornato (Lezione 4), deve "portarsi dietro" il dettaglio dell'ombrello per quante pagine, prima che diventi rilevante?
> 2. Con LSTM/GRU (Sezione 4.5, il diario più furbo), quel dettaglio ha più chance di sopravvivere fino a pagina 290 rispetto a un bigliettino semplice — ma il lettore deve comunque leggere le pagine in un ordine particolare, una alla volta? Sì o no, e perché?
> 3. Un lettore basato sulla sola attenzione (Sezione 8.4), messo di fronte alla pagina 290, può "guardare" direttamente la pagina 15 in un solo passo, oppure deve comunque ripassare mentalmente le 274 pagine di mezzo? Cosa cambia, in termini di quanto lontano si trova l'indizio (pagina 15 o pagina 289), per la difficoltà di questo collegamento diretto?
> 4. Immagina ora 10 lettori diversi che devono leggere insieme, in parallelo, le 300 pagine del libro (uno per ogni blocco di 30 pagine). Con un lettore pagina-per-pagina, questo tipo di squadra aiuterebbe a leggere il libro più in fretta? Con un lettore basato sulla sola attenzione, cambierebbe qualcosa?

---

*Continua con l'[Appendice A — Soluzioni ai giochi]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-appendice-a-soluzioni.md %}.html)*
