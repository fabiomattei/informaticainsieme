---
title: 'Lezione 06 — La colpa che risale la catena'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 6.1 Il forward pass da solo non basta

La Lezione 2 ha mostrato come una fila di controllori — uno strato dopo l'altro — trasformi un input in un output, passando l'informazione in avanti, piano per piano. Ma nel "Prova tu" di quella lezione i pesi erano già dati, scelti a tavolino perché il risultato tornasse giusto. Nella realtà nessuno sceglie i pesi a mano: la rete deve trovarli da sola, correggendosi progressivamente in base agli errori commessi, esattamente come l'interruttore-ombrello della Lezione 1. La domanda di questa lezione è: quando l'errore finale viene misurato (Lezione 5) su una rete con centinaia o migliaia di pesi sparsi su più piani, *come si distribuisce la colpa* fra tutti quei pesi, capendo di quanto e in che direzione correggere ciascuno?

### 6.2 Un'idea semplice: passare la colpa all'indietro, un piano alla volta

Immagina una catena di artigiani in una linea di montaggio, ciascuno che riceve il pezzo dal precedente, lo modifica un po', e lo passa al successivo — il forward pass della Lezione 2. Quando il prodotto finale risulta difettoso, il modo più sensato di correggere la linea non è ricominciare a indovinare da zero le regolazioni di ogni singolo artigiano: è far risalire la segnalazione del difetto all'indietro lungo la stessa catena, un artigiano alla volta, partendo dall'ultimo. L'ultimo artigiano riceve la notizia "il pezzo finale era così e così sbagliato" e capisce subito quanto correggere la propria fase. Ma deve anche passare al penultimo un messaggio equivalente: "il pezzo che *mi* hai passato tu era, di riflesso, così e così sbagliato" — permettendogli a sua volta di correggersi e di passare un messaggio simile ancora più indietro. Questa procedura, applicata piano per piano fino al primo artigiano della catena, è l'intero contenuto operativo della **backpropagation** (o "retropropagazione dell'errore").

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="backprop-title backprop-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="backprop-title">Avanti e indietro lungo la catena</title>
  <desc id="backprop-desc">Tre artigiani in fila. Il forward pass scorre da sinistra a destra producendo l'output. Il backward pass fa risalire la colpa da destra a sinistra, un artigiano alla volta.</desc>

  <defs>
    <marker id="arrowFwd" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#2a7ae2" /></marker>
    <marker id="arrowBwd" markerWidth="8" markerHeight="8" refX="2" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M8,0 L0,4 L8,8 z" fill="#f66a0a" /></marker>
  </defs>

  <circle cx="30" cy="110" r="16" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="30" y="150" fill="#828282" font-size="10" text-anchor="middle">input</text>

  <rect x="70" y="80" width="110" height="60" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="125" y="114" fill="#111" font-size="12" text-anchor="middle">artigiano 1</text>
  <rect x="220" y="80" width="110" height="60" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="275" y="114" fill="#111" font-size="12" text-anchor="middle">artigiano 2</text>
  <rect x="370" y="80" width="110" height="60" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="425" y="114" fill="#111" font-size="12" text-anchor="middle">artigiano 3</text>

  <circle cx="520" cy="110" r="16" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="520" y="150" fill="#828282" font-size="10" text-anchor="middle">errore</text>

  <g stroke="#2a7ae2" stroke-width="2.5" marker-end="url(#arrowFwd)">
    <line x1="47" y1="92" x2="68" y2="92" /><line x1="182" y1="92" x2="218" y2="92" /><line x1="332" y1="92" x2="368" y2="92" /><line x1="482" y1="92" x2="503" y2="92" />
  </g>
  <text x="280" y="60" fill="#1d5eb8" font-size="12" text-anchor="middle">avanti (forward pass)</text>

  <g stroke="#f66a0a" stroke-width="2.5" marker-end="url(#arrowBwd)">
    <line x1="503" y1="130" x2="482" y2="130" /><line x1="368" y1="130" x2="332" y2="130" /><line x1="218" y1="130" x2="182" y2="130" /><line x1="68" y1="130" x2="47" y2="130" />
  </g>
  <text x="280" y="175" fill="#c85506" font-size="12" text-anchor="middle">la colpa che risale (backward pass)</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni artigiano riceve la colpa dal successivo, si corregge, e ne passa una versione tradotta al precedente.</figcaption>
</figure>

### 6.3 Cosa serve per calcolare "quanto è colpa mia"

Perché ogni artigiano possa tradurre "il pezzo che ho prodotto era sbagliato di tot" in "devo correggere le mie regolazioni di tot", serve un ingrediente che la Lezione 1 aveva già preparato senza dirlo esplicitamente: la manopola sfumata di ogni controllore (Sezione 1.5) permette sempre di sapere, in ogni punto, quanto il suo output *reagirebbe* a un piccolo cambiamento del proprio input — se un ritocco minimo lo farebbe salire molto, poco, o quasi nulla. Questa "reattività locale" è esattamente il moltiplicatore che serve per tradurre la colpa ricevuta da un piano in due cose: quanto correggere i propri pesi, e quanta colpa (già tradotta, già pesata) passare indietro al piano precedente. È il motivo preciso per cui un interruttore a scatto netto — senza sfumature, Sezione 1.5 — non si sarebbe mai potuto correggere in una catena di più di un piano: non c'è "reattività locale" da misurare in un tasto che salta di colpo da spento ad acceso.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="reattivita-title reattivita-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="reattivita-title">La reattività locale della manopola sfumata</title>
  <desc id="reattivita-desc">La stessa curva a S della funzione di attivazione, con due punti evidenziati: uno sulla parte ripida, dove un piccolo ritocco cambia molto l'output, e uno sulla parte piatta, dove lo stesso ritocco cambia pochissimo.</desc>

  <g stroke="#828282" stroke-width="1"><line x1="40" y1="220" x2="440" y2="220" /><line x1="40" y1="220" x2="40" y2="40" /></g>
  <path d="M40,200 C110,200 130,50 240,50 C350,50 370,50 440,50" fill="none" stroke="#2a7ae2" stroke-width="2.5" />

  <line x1="150" y1="150" x2="210" y2="70" stroke="#f66a0a" stroke-width="2.5" />
  <circle cx="180" cy="110" r="6" fill="#f66a0a" />
  <text x="185" y="195" fill="#c85506" font-size="11" text-anchor="middle">reattività alta:</text>
  <text x="185" y="209" fill="#c85506" font-size="11" text-anchor="middle">un ritocco qui cambia molto</text>

  <line x1="370" y1="48" x2="430" y2="52" stroke="#3aa655" stroke-width="2.5" />
  <circle cx="400" cy="50" r="6" fill="#3aa655" />
  <text x="400" y="90" fill="#2c7f3f" font-size="11" text-anchor="middle">reattività quasi nulla:</text>
  <text x="400" y="104" fill="#2c7f3f" font-size="11" text-anchor="middle">un ritocco qui cambia pochissimo</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La pendenza della curva, in ogni punto, è esattamente la reattività locale usata per tradurre la colpa.</figcaption>
</figure>

### 6.4 Correggere costa quasi quanto decidere

Vale la pena notare un fatto pratico, che tornerà utile più avanti quando parleremo di reti molto grandi: far risalire la colpa lungo la catena richiede, a ogni anello, un lavoro di calcolo paragonabile a quello richiesto per produrre l'output in avanti (il forward pass). Non è un lavoro qualitativamente diverso o molto più pesante: è, grosso modo, un secondo passaggio della stessa entità del primo. Questo spiega perché allenare una rete costi, per ogni esempio visto, circa 2-3 volte il costo di farle semplicemente produrre una risposta — non un costo enormemente più grande, ma la somma di "decidere" più "capire di quanto correggersi", ripetuta per ogni esempio, molte volte durante l'allenamento.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 400 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="costo-title costo-desc" style="width: 100%; max-width: 360px; height: auto; font-family: inherit;">
  <title id="costo-title">Quanto costa correggersi</title>
  <desc id="costo-desc">Due barre: produrre solo una risposta costa un'unità di calcolo. Produrre una risposta e correggersi (forward più backward) costa circa 2-3 volte tanto.</desc>

  <line x1="30" y1="220" x2="370" y2="220" stroke="#828282" stroke-width="1.5" />

  <rect x="60" y="160" width="100" height="60" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="110" y="150" fill="#111" font-size="14" font-weight="bold" text-anchor="middle">1×</text>
  <text x="110" y="240" fill="#828282" font-size="11" text-anchor="middle">produce una risposta</text>

  <rect x="220" y="160" width="100" height="60" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <rect x="220" y="60" width="100" height="100" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="270" y="50" fill="#111" font-size="14" font-weight="bold" text-anchor="middle">~2-3×</text>
  <text x="270" y="240" fill="#828282" font-size="11" text-anchor="middle">produce e si corregge</text>

  <g font-size="11">
    <rect x="30" y="10" width="14" height="14" fill="#dceafc" stroke="#2a7ae2" /><text x="50" y="21" fill="#111">forward (decidere)</text>
    <rect x="190" y="10" width="14" height="14" fill="#fde8d6" stroke="#f66a0a" /><text x="210" y="21" fill="#111">backward (correggersi)</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Non un costo enormemente più grande: la somma di "decidere" e "capire come correggersi".</figcaption>
</figure>

---

> **Prova tu — Fai risalire la colpa lungo tre anelli**
>
> Una piccola rete a tre piani ha appena prodotto un output sbagliato. Il supervisore finale calcola: **colpa al piano 3 (output) = −4** (un numero negativo significa "avresti dovuto essere più alto di così").
>
> Per far risalire la colpa a ritroso, ogni anello della catena applica questa regola: *colpa del piano precedente = colpa ricevuta × peso di collegamento fra i due piani × reattività locale del piano precedente* (la "sensibilità" della sua manopola in quel punto, Sezione 6.3 — un numero fra 0 e 1 che ti viene già dato, non serve calcolarlo).
>
> Dati:
> - Peso di collegamento fra piano 2 e piano 3: **2**. Reattività locale del piano 2 in questo esempio: **0,5**.
> - Peso di collegamento fra piano 1 e piano 2: **1,5**. Reattività locale del piano 1 in questo esempio: **0,4**.
>
> 1. Calcola la colpa che risale al piano 2: colpa piano 3 × peso (piano2-piano3) × reattività locale piano 2.
> 2. Usando il risultato del punto 1, calcola la colpa che risale al piano 1: colpa piano 2 × peso (piano1-piano2) × reattività locale piano 1.
> 3. Guarda i tre numeri di colpa (piano 3, piano 2, piano 1) in fila. Stanno diventando più piccoli o più grandi man mano che risali la catena? Rileggi la Sezione 4.4 (il bigliettino che si sporca): cosa succederebbe a questi numeri se la catena avesse non 3 ma 30 anelli, e ogni reattività locale restasse intorno a 0,4-0,5 per tutta la catena?

---

*Continua con la [Lezione 07 — Allenarsi bene (e non a memoria)]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-07-allenarsi-bene-e-non-a-memoria.md %}.html)*
