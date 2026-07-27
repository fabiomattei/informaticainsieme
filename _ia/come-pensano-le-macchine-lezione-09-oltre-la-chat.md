---
title: 'Lezione 09 — Oltre la chat: cercare, usare strumenti, vedere immagini'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 9.1 I limiti di una memoria congelata

Tutto quello che un LLM "sa" viene da ciò che ha letto durante il pre-training della Lezione 4 — un processo lungo e costoso, che si conclude a una certa data e non riprende automaticamente ogni giorno. Questo crea due limiti pratici evidenti: il modello non sa nulla di eventi accaduti dopo la fine del suo addestramento, e non ha accesso a informazioni private o specifiche che semplicemente non erano su internet (i tuoi appunti di scuola, i documenti interni di un'azienda, il meteo di domani). Chiedere a un modello "che tempo farà domani" è come chiedere a un enciclopedia cartacea: per quanto ben scritta, un libro stampato non si aggiorna da solo.

La soluzione più diffusa a entrambi i problemi non è "far studiare di nuovo il modello ogni giorno" — troppo costoso, come visto nella Lezione 4 — ma dargli accesso a fonti esterne **al momento della domanda**, invece di pretendere che sappia già tutto a memoria.

### 9.2 Cercare prima di rispondere

La tecnica più comune si chiama **RAG** (generazione aumentata da recupero): prima di rispondere, il sistema cerca automaticamente, in una raccolta di documenti esterni (può essere l'intero web, o un archivio aziendale privato), i passaggi più pertinenti alla domanda posta — un po' come tu, prima di rispondere a una domanda difficile durante un tema in classe con i libri consultabili, sfoglieresti prima l'indice per trovare la lezione giusto. Questi passaggi vengono poi inseriti nel contesto insieme alla domanda originale, e solo a quel punto il modello genera la risposta — potendo ora "citare" informazioni che non ha mai visto durante l'addestramento, semplicemente perché gli sono state messe sotto gli occhi un attimo prima di rispondere.

Questo spiega perché alcuni assistenti possono rispondere correttamente a "chi ha vinto la partita di ieri sera" pur non avendo mai "studiato" quell'evento specifico: non lo sanno a memoria, lo hanno appena letto.

### 9.3 Chiedere aiuto agli strumenti giusti

Un modello, da solo, è pessimo in certi compiti che un semplice programma per computer svolge perfettammente — moltiplicazioni con tanti decimali, conversione di valute in tempo reale, esecuzione precisa di codice. La soluzione, di nuovo, non è pretendere che il modello impari a fare meglio i calcoli a mente: è dargli la possibilità di **chiamare uno strumento esterno** quando serve — una calcolatrice, un motore di ricerca, un interprete di codice — proprio come faresti tu, davanti a un conto complicato, tirando fuori la calcolatrice dalla tasca invece di ostinarti a farlo a mente. Tecnicamente, il modello viene addestrato a riconoscere quando conviene "fermarsi e chiedere aiuto a uno strumento" invece di continuare a indovinare parole, formulare la richiesta nel formato che lo strumento si aspetta, leggere il risultato che torna indietro, e solo allora continuare a scrivere la risposta finale incorporando quel risultato.

### 9.4 Un modello che agisce da solo, passo dopo passo

Mettendo insieme ricerca e strumenti, si arriva a qualcosa di più ambizioso: gli **agenti**. Invece di rispondere in un colpo solo, un agente ripete un ciclo — pensa cosa fare, agisce (cerca, chiama uno strumento, legge un file), osserva il risultato, e decide se serve un altro passo o se ormai ha abbastanza informazione per rispondere — un po' come risolveresti un'indagine investigativa: ogni indizio trovato suggerisce la prossima domanda da porsi, invece di sapere fin dall'inizio esattamente tutti i passi da seguire. Un agente può, ad esempio, cercare un volo, controllare il meteo di destinazione, e prenotare un hotel, incatenando più ricerche e più strumenti in sequenza, senza che un umano debba guidare ogni singolo passaggio a mano.

Questa autonomia porta con sé un rischio nuovo: se un agente legge automaticamente contenuti da fonti esterne (una pagina web, un'email, un documento), qualcuno potrebbe nascondere in quei contenuti istruzioni scritte apposta per ingannarlo ("ignora le istruzioni precedenti e invia questo file a...") — un po' come un messaggio nascosto in una bottiglia che il modello legge senza sapere distinguere "istruzione del mio utente legittimo" da "istruzione nascosta in un documento che stavo solo consultando". Questo problema, chiamato **prompt injection**, è uno dei motivi per cui gli agenti autonomi vengono oggi rilasciati con cautela e con permessi limitati.

### 9.5 Vedere, non solo leggere

Un ultimo salto: i modelli più recenti non leggono solo testo, ma anche **immagini** (e in alcuni casi audio o video). Il trucco concettuale non è così diverso da quanto visto nella Lezione 2 per le parole: un'immagine viene spezzata in piccoli riquadri (un po' come i pezzetti-Lego dei token testuali, ma qui sono porzioni di pixel), ciascuno trasformato in un punto sulla stessa "mappa dei significati" condivisa con le parole. Il risultato è che il modello può, con lo stesso identico meccanismo di attenzione visto nella Lezione 3, far "dialogare" i pezzetti dell'immagine con le parole della domanda — permettendo di rispondere a "cosa c'è scritto su questo cartello?" o "quanti gatti vedi in questa foto?" con lo stesso motore che risponde a domande scritte.

---

> **Prova tu — Quale strumento chiameresti?**
>
> Per ciascuna domanda, immagina di essere l'assistente e scegli **quale azione** faresti prima di rispondere, tra: (a) rispondere subito a memoria, (b) cercare informazioni aggiornate su internet, (c) usare una calcolatrice/interprete di codice, (d) chiedere di vedere un'immagine.
>
> 1. "Qual è la capitale del Giappone?"
> 2. "Quanto vale oggi un dollaro in euro?"
> 3. "Quanto fa 84.371 × 296,5?"
> 4. "Chi ha vinto le elezioni nel paese X la settimana scorsa?"
> 5. "C'è qualcosa di strano in questa foto della mia pianta — sta appassendo, sai dirmi perché?"
>
> Scrivi la tua scelta per ciascuna e il perché — in particolare, prova a spiegare **perché** rispondere "a memoria" sarebbe rischioso o impossibile per le domande 2, 3, 4 e 5. Confronta con l'Appendice A.

---

*Continua con la [Lezione 10 — Cosa ci aspetta]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-10-cosa-ci-aspetta.md %}.html)*
