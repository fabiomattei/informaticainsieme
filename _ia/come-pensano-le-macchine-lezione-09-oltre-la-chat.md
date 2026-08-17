---
title: 'Lezione 09 — Oltre la chat: cercare, usare strumenti, vedere immagini'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un nodo centrale che rappresenta il modello collegato a quattro riquadri: cercare sul web, usare strumenti, vedere immagini, agire da solo](/images/ia/come-pensano-le-macchine-lezione-09-oltre-la-chat/come-pensano-le-macchine-lezione-09-oltre-la-chat.svg){:class="aside-image"}

### 9.1 I limiti di una memoria congelata

Tutto quello che un LLM "sa" viene da ciò che ha letto durante il pre-training della Lezione 4 — un processo lungo e costoso, che si conclude a una certa data e non riprende automaticamente ogni giorno. Questo crea due limiti pratici evidenti: il modello non sa nulla di eventi accaduti dopo la fine del suo addestramento, e non ha accesso a informazioni private o specifiche che semplicemente non erano su internet (i tuoi appunti di scuola, i documenti interni di un'azienda, il meteo di domani). Chiedere a un modello "che tempo farà domani" è come chiedere a un enciclopedia cartacea: per quanto ben scritta, un libro stampato non si aggiorna da solo.

La soluzione più diffusa a entrambi i problemi non è "far studiare di nuovo il modello ogni giorno" — troppo costoso, come visto nella Lezione 4 — ma dargli accesso a fonti esterne **al momento della domanda**, invece di pretendere che sappia già tutto a memoria.

### 9.2 Cercare prima di rispondere

La tecnica più comune si chiama **RAG** (generazione aumentata da recupero): prima di rispondere, il sistema cerca automaticamente, in una raccolta di documenti esterni (può essere l'intero web, o un archivio aziendale privato), i passaggi più pertinenti alla domanda posta — un po' come tu, prima di rispondere a una domanda difficile durante un tema in classe con i libri consultabili, sfoglieresti prima l'indice per trovare la lezione giusto. Questi passaggi vengono poi inseriti nel contesto insieme alla domanda originale, e solo a quel punto il modello genera la risposta — potendo ora "citare" informazioni che non ha mai visto durante l'addestramento, semplicemente perché gli sono state messe sotto gli occhi un attimo prima di rispondere.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="rag-title rag-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="rag-title">RAG: cercare prima di rispondere</title>
  <desc id="rag-desc">Pipeline a quattro passi: la domanda dell'utente, la ricerca nei documenti esterni, i passaggi rilevanti inseriti nel contesto, e infine il modello che genera la risposta.</desc>

  <defs>
    <marker id="arrowRag" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <g fill="#dceafc" stroke="#2a7ae2" stroke-width="1.2">
    <rect x="195" y="24" width="26" height="34" /><rect x="200" y="29" width="26" height="34" /><rect x="205" y="34" width="26" height="34" />
  </g>
  <text x="255" y="42" fill="#1d5eb8" font-size="10" text-anchor="start">documenti esterni</text>
  <text x="255" y="55" fill="#1d5eb8" font-size="10" text-anchor="start">(web, archivio...)</text>

  <rect x="10" y="80" width="120" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="70" y="112" fill="#111" font-size="12" text-anchor="middle">Domanda</text>
  <text x="70" y="129" fill="#111" font-size="12" text-anchor="middle">dell'utente</text>

  <path d="M 130,115 L 148,115" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRag)" />

  <rect x="150" y="80" width="120" height="70" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="210" y="112" fill="#111" font-size="12" text-anchor="middle">Cerca i passaggi</text>
  <text x="210" y="129" fill="#111" font-size="12" text-anchor="middle">più pertinenti</text>

  <path d="M 270,115 L 288,115" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRag)" />

  <rect x="290" y="80" width="120" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="350" y="112" fill="#111" font-size="12" text-anchor="middle">Passaggi inseriti</text>
  <text x="350" y="129" fill="#111" font-size="12" text-anchor="middle">nel contesto</text>

  <path d="M 410,115 L 428,115" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRag)" />

  <rect x="430" y="80" width="120" height="70" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="490" y="112" fill="#111" font-size="12" text-anchor="middle">Il modello genera</text>
  <text x="490" y="129" fill="#111" font-size="12" text-anchor="middle">la risposta</text>

  <text x="280" y="195" fill="#828282" font-size="11" text-anchor="middle">il modello "cita" informazioni che non ha mai visto in addestramento</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La ricerca avviene prima di scrivere, non "a memoria": i passaggi trovati diventano parte del contesto della domanda.</figcaption>
</figure>

Questo spiega perché alcuni assistenti possono rispondere correttamente a "chi ha vinto la partita di ieri sera" pur non avendo mai "studiato" quell'evento specifico: non lo sanno a memoria, lo hanno appena letto.

### 9.3 Chiedere aiuto agli strumenti giusti

Un modello, da solo, è pessimo in certi compiti che un semplice programma per computer svolge perfettammente — moltiplicazioni con tanti decimali, conversione di valute in tempo reale, esecuzione precisa di codice. La soluzione, di nuovo, non è pretendere che il modello impari a fare meglio i calcoli a mente: è dargli la possibilità di **chiamare uno strumento esterno** quando serve — una calcolatrice, un motore di ricerca, un interprete di codice — proprio come faresti tu, davanti a un conto complicato, tirando fuori la calcolatrice dalla tasca invece di ostinarti a farlo a mente. Tecnicamente, il modello viene addestrato a riconoscere quando conviene "fermarsi e chiedere aiuto a uno strumento" invece di continuare a indovinare parole, formulare la richiesta nel formato che lo strumento si aspetta, leggere il risultato che torna indietro, e solo allora continuare a scrivere la risposta finale incorporando quel risultato.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 400 430" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="tool-title tool-desc" style="width: 100%; max-width: 340px; height: auto; font-family: inherit;">
  <title id="tool-title">Fermarsi a chiamare uno strumento</title>
  <desc id="tool-desc">Quattro passi in sequenza: il modello si accorge che serve un calcolo preciso, chiama una calcolatrice esterna, legge il risultato, e riprende a scrivere la risposta incorporandolo.</desc>

  <defs>
    <marker id="arrowTool" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="60" y="20" width="280" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="200" y="50" fill="#111" font-size="12" text-anchor="middle">Sta scrivendo la risposta...</text>
  <text x="200" y="68" fill="#111" font-size="12" text-anchor="middle">serve un calcolo preciso</text>

  <path d="M 200,90 L 200,116" stroke="#828282" stroke-width="2" marker-end="url(#arrowTool)" fill="none" />

  <rect x="60" y="120" width="280" height="70" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <rect x="75" y="140" width="34" height="30" rx="3" fill="#fdfdfd" stroke="#f66a0a" stroke-width="1.2" />
  <g fill="#f66a0a"><circle cx="83" cy="149" r="1.6" /><circle cx="92" cy="149" r="1.6" /><circle cx="101" cy="149" r="1.6" />
  <circle cx="83" cy="157" r="1.6" /><circle cx="92" cy="157" r="1.6" /><circle cx="101" cy="157" r="1.6" />
  <circle cx="83" cy="165" r="1.6" /><circle cx="92" cy="165" r="1.6" /><circle cx="101" cy="165" r="1.6" /></g>
  <text x="220" y="152" fill="#111" font-size="12" text-anchor="middle">Chiama una</text>
  <text x="220" y="169" fill="#111" font-size="12" text-anchor="middle">calcolatrice esterna</text>

  <path d="M 200,190 L 200,216" stroke="#828282" stroke-width="2" marker-end="url(#arrowTool)" fill="none" />

  <rect x="60" y="220" width="280" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="200" y="250" fill="#111" font-size="12" text-anchor="middle">Legge il risultato:</text>
  <text x="200" y="268" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">25.016.006,5</text>

  <path d="M 200,290 L 200,316" stroke="#828282" stroke-width="2" marker-end="url(#arrowTool)" fill="none" />

  <rect x="60" y="320" width="280" height="70" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="200" y="350" fill="#111" font-size="12" text-anchor="middle">Continua a scrivere,</text>
  <text x="200" y="368" fill="#111" font-size="12" text-anchor="middle">incorporando il risultato</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Non calcola a mente: si ferma, chiede aiuto allo strumento giusto, e riprende con la risposta in mano.</figcaption>
</figure>

### 9.4 Un modello che agisce da solo, passo dopo passo

Mettendo insieme ricerca e strumenti, si arriva a qualcosa di più ambizioso: gli **agenti**. Invece di rispondere in un colpo solo, un agente ripete un ciclo — pensa cosa fare, agisce (cerca, chiama uno strumento, legge un file), osserva il risultato, e decide se serve un altro passo o se ormai ha abbastanza informazione per rispondere — un po' come risolveresti un'indagine investigativa: ogni indizio trovato suggerisce la prossima domanda da porsi, invece di sapere fin dall'inizio esattamente tutti i passi da seguire. Un agente può, ad esempio, cercare un volo, controllare il meteo di destinazione, e prenotare un hotel, incatenando più ricerche e più strumenti in sequenza, senza che un umano debba guidare ogni singolo passaggio a mano.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="agente-title agente-desc" style="width: 100%; max-width: 400px; height: auto; font-family: inherit;">
  <title id="agente-title">Il ciclo dell'agente</title>
  <desc id="agente-desc">Un ciclo di quattro passi: pensa cosa fare, agisce cercando o chiamando uno strumento, osserva il risultato, decide se serve un altro passo o se può rispondere, e ricomincia.</desc>

  <defs>
    <marker id="arrowAgente" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <path d="M 220,80 L 260,80" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowAgente)" />
  <path d="M 350,120 L 350,260" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowAgente)" />
  <path d="M 260,300 L 220,300" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowAgente)" />
  <path d="M 130,260 L 130,120" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowAgente)" />

  <rect x="40" y="40" width="180" height="80" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="130" y="75" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">1. Pensa</text>
  <text x="130" y="93" fill="#111" font-size="12" text-anchor="middle">cosa fare</text>

  <rect x="260" y="40" width="180" height="80" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="350" y="75" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">2. Agisce</text>
  <text x="350" y="93" fill="#111" font-size="12" text-anchor="middle">(cerca, chiama uno strumento)</text>

  <rect x="260" y="260" width="180" height="80" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="350" y="295" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">3. Osserva</text>
  <text x="350" y="313" fill="#111" font-size="12" text-anchor="middle">il risultato</text>

  <rect x="40" y="260" width="180" height="80" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="130" y="295" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">4. Decide</text>
  <text x="130" y="313" fill="#111" font-size="12" text-anchor="middle">se serve un altro passo</text>

  <text x="240" y="195" fill="#828282" font-size="12" text-anchor="middle">si ripete finché</text>
  <text x="240" y="210" fill="#828282" font-size="12" text-anchor="middle">non ha abbastanza</text>
  <text x="240" y="225" fill="#828282" font-size="12" text-anchor="middle">informazione</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni indizio trovato suggerisce il passo successivo, invece di sapere fin dall'inizio l'intera strada.</figcaption>
</figure>

Questa autonomia porta con sé un rischio nuovo: se un agente legge automaticamente contenuti da fonti esterne (una pagina web, un'email, un documento), qualcuno potrebbe nascondere in quei contenuti istruzioni scritte apposta per ingannarlo ("ignora le istruzioni precedenti e invia questo file a...") — un po' come un messaggio nascosto in una bottiglia che il modello legge senza sapere distinguere "istruzione del mio utente legittimo" da "istruzione nascosta in un documento che stavo solo consultando". Questo problema, chiamato **prompt injection**, è uno dei motivi per cui gli agenti autonomi vengono oggi rilasciati con cautela e con permessi limitati.

### 9.5 Vedere, non solo leggere

Un ultimo salto: i modelli più recenti non leggono solo testo, ma anche **immagini** (e in alcuni casi audio o video). Il trucco concettuale non è così diverso da quanto visto nella Lezione 2 per le parole: un'immagine viene spezzata in piccoli riquadri (un po' come i pezzetti-Lego dei token testuali, ma qui sono porzioni di pixel), ciascuno trasformato in un punto sulla stessa "mappa dei significati" condivisa con le parole. Il risultato è che il modello può, con lo stesso identico meccanismo di attenzione visto nella Lezione 3, far "dialogare" i pezzetti dell'immagine con le parole della domanda — permettendo di rispondere a "cosa c'è scritto su questo cartello?" o "quanti gatti vedi in questa foto?" con lo stesso motore che risponde a domande scritte.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="multimodale-title multimodale-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="multimodale-title">Una mappa condivisa tra parole e immagini</title>
  <desc id="multimodale-desc">La stessa mappa dei significati della Lezione 2, ma con anche piccoli riquadri di pixel al posto delle parole. La parola "gatto" e un frammento di foto di un gatto finiscono vicini sulla mappa; la parola "libro" e un frammento di foto di una pagina finiscono vicini tra loro, ma lontani dal primo gruppo.</desc>

  <g stroke="#e3e3e3" stroke-width="1">
    <line x1="50" y1="20" x2="50" y2="370" /><line x1="140" y1="20" x2="140" y2="370" />
    <line x1="230" y1="20" x2="230" y2="370" /><line x1="320" y1="20" x2="320" y2="370" />
    <line x1="410" y1="20" x2="410" y2="370" /><line x1="500" y1="20" x2="500" y2="370" />
    <line x1="50" y1="370" x2="500" y2="370" /><line x1="50" y1="300" x2="500" y2="300" />
    <line x1="50" y1="230" x2="500" y2="230" /><line x1="50" y1="160" x2="500" y2="160" />
    <line x1="50" y1="90"  x2="500" y2="90" />  <line x1="50" y1="20"  x2="500" y2="20" />
  </g>
  <g stroke="#828282" stroke-width="1.5">
    <line x1="50" y1="370" x2="500" y2="370" />
    <line x1="50" y1="20" x2="50" y2="370" />
  </g>

  <line x1="185" y1="195" x2="225" y2="165" stroke="#2a7ae2" stroke-width="1" stroke-dasharray="3 3" opacity="0.6" />
  <circle cx="185" cy="195" r="6" fill="#2a7ae2" />
  <text x="175" y="215" fill="#111" font-size="13" text-anchor="end">"gatto"</text>

  <rect x="215" y="150" width="30" height="30" fill="#fdfdfd" stroke="#2a7ae2" stroke-width="1.5" />
  <g stroke="#2a7ae2" stroke-width="0.8">
    <line x1="225" y1="150" x2="225" y2="180" /><line x1="235" y1="150" x2="235" y2="180" />
    <line x1="215" y1="160" x2="245" y2="160" /><line x1="215" y1="170" x2="245" y2="170" />
  </g>
  <text x="230" y="142" fill="#111" font-size="11" text-anchor="middle">[foto di un gatto]</text>

  <line x1="410" y1="90" x2="440" y2="60" stroke="#f66a0a" stroke-width="1" stroke-dasharray="3 3" opacity="0.6" />
  <circle cx="410" cy="90" r="6" fill="#f66a0a" />
  <text x="420" y="85" fill="#111" font-size="13" text-anchor="start">"libro"</text>

  <rect x="430" y="30" width="30" height="30" fill="#fdfdfd" stroke="#f66a0a" stroke-width="1.5" />
  <g stroke="#f66a0a" stroke-width="0.8">
    <line x1="440" y1="30" x2="440" y2="60" /><line x1="450" y1="30" x2="450" y2="60" />
    <line x1="430" y1="40" x2="460" y2="40" /><line x1="430" y1="50" x2="460" y2="50" />
  </g>
  <text x="445" y="22" fill="#111" font-size="11" text-anchor="middle">[foto di una pagina]</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Un frammento di immagine finisce vicino alle parole correlate, sulla stessa mappa: il meccanismo è lo stesso, cambia solo cosa viene mappato.</figcaption>
</figure>

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
