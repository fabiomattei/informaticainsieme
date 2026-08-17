---
title: 'Lezione 02 — Impilare le decisioni: la rete a più piani'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Due ingressi collegati a due strati nascosti che convergono in un output](/images/ia/come-pensano-le-reti-neurali-lezione-02-impilare-le-decisioni/come-pensano-le-reti-neurali-lezione-02-impilare-le-decisioni.svg){:class="aside-image"}

### 2.1 Un solo controllore non basta

La Lezione 1 ha lasciato un problema in sospeso: le quattro palline ai quattro angoli del quadrato, rosse in diagonale e blu in diagonale, che nessuna linea retta riesce a separare. Un solo interruttore, per quanto ben allenato, non ce la fa — è un limite geometrico, non una questione di allenamento migliore. Ma Minsky e Papert stessi, nel bel mezzo di un libro che elencava limiti, avevano lasciato cadere un suggerimento: e se, invece di un solo controllore, ne mettessimo più di uno in fila?

### 2.2 L'idea: più controllori, poi uno che li riassume

Immagina di sostituire il singolo interruttore-ombrello con due controllori intermedi, ciascuno che guarda gli stessi indizi ma con una propria regola, seguiti da un terzo controllore finale che guarda *le decisioni dei primi due* — non più gli indizi originali — per decidere l'output finale. Ogni controllore intermedio può tracciare la propria linea retta di separazione; il controllore finale, combinando due decisioni già prese, produce un confine complessivo che può piegarsi, spezzarsi, incurvarsi — non più vincolato a essere una singola linea dritta.

Vediamolo concretamente sul problema delle quattro palline. Chiamiamo i due indizi X e Y, ciascuno acceso o spento (1 o 0): pallina rossa in (0,0) e (1,1), pallina blu in (0,1) e (1,0) — esattamente lo schema XOR della Lezione 1. Costruiamo due controllori intermedi:

- **Controllore A**: si accende se *almeno uno* dei due indizi è acceso (una regola "OR").
- **Controllore B**: si accende se *non sono entrambi* accesi insieme (una regola "non-AND", cioè NAND).

E un controllore finale che guarda A e B:

- **Controllore finale**: dice "rosso" (output 1) se *sia A sia B* sono accesi insieme (una regola "AND").

Verifica tu stesso, punto per punto, che questa combinazione produce esattamente lo schema che cercavamo — lo faremo insieme nel "Prova tu" di questa lezione. Il punto essenziale: nessuno dei tre controllori, da solo, potrebbe risolvere l'XOR — ciascuno traccia solo una linea dritta — ma la loro combinazione a due piani sì.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="rete-title rete-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="rete-title">La struttura a due piani</title>
  <desc id="rete-desc">Gli input X e Y si collegano entrambi ai due controllori intermedi A e B, che a loro volta si collegano al controllore finale. La struttura da sola, senza i valori delle celle, che sono l'oggetto dell'esercizio di questa lezione.</desc>

  <text x="60" y="30" fill="#828282" font-size="12" text-anchor="middle">input</text>
  <text x="240" y="30" fill="#828282" font-size="12" text-anchor="middle">strato nascosto</text>
  <text x="420" y="30" fill="#828282" font-size="12" text-anchor="middle">output</text>

  <g stroke="#c9c9c9" stroke-width="1.5">
    <line x1="80" y1="100" x2="220" y2="80" /><line x1="80" y1="100" x2="220" y2="240" />
    <line x1="80" y1="220" x2="220" y2="80" /><line x1="80" y1="220" x2="220" y2="240" />
    <line x1="260" y1="80" x2="400" y2="160" /><line x1="260" y1="240" x2="400" y2="160" />
  </g>

  <circle cx="60" cy="100" r="20" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="60" y="105" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">X</text>
  <circle cx="60" cy="220" r="20" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="60" y="225" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">Y</text>

  <circle cx="240" cy="80" r="26" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="240" y="76" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">A</text>
  <text x="240" y="90" fill="#1d5eb8" font-size="9" text-anchor="middle">(OR)</text>

  <circle cx="240" cy="240" r="26" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="240" y="236" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">B</text>
  <text x="240" y="250" fill="#1d5eb8" font-size="9" text-anchor="middle">(NAND)</text>

  <circle cx="420" cy="160" r="28" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="420" y="156" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">Finale</text>
  <text x="420" y="170" fill="#c85506" font-size="9" text-anchor="middle">(AND)</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni collegamento porta un segnale acceso/spento: i valori esatti sono l'oggetto del "Prova tu" qui sotto.</figcaption>
</figure>

### 2.3 Il piano intermedio ha un nome

Questa struttura a due piani ha un nome preciso: i due controllori di mezzo (A e B) formano uno **strato nascosto** — "nascosto" perché non osserviamo direttamente le loro decisioni, sono un passaggio intermedio che la rete usa internamente prima di arrivare all'output finale. Una rete costruita così — uno o più strati nascosti fra l'input e l'output, in cui ogni strato guarda solo l'output dello strato precedente — si chiama **rete feedforward multistrato** (in inglese, storicamente, *multi-layer perceptron*, anche se i controllori moderni usano le manopole sfumate della Lezione 1, non più l'interruttore a scatto netto del percettrone originale). "Feedforward" perché l'informazione scorre in una sola direzione, dall'input verso l'output, senza mai tornare indietro — una proprietà su cui torneremo nella Lezione 4, quando incontreremo un'architettura che invece un pezzo di informazione se lo tiene e lo riusa.

### 2.4 Quanto in profondità, quanto in larghezza

Due numeri descrivono un edificio come questo: quanti **piani** (strati) ha, e quanto è **largo** ogni piano (quanti controllori contiene). Una rete con un solo piano è semplicemente il singolo interruttore della Lezione 1; una rete "profonda" — da cui il termine "*deep learning*" — ne ha tipicamente più di due o tre. Non c'è una soglia precisa oltre cui una rete "diventa" profonda: è più una questione di convenzione storica, per contrasto con i modelli a un solo piano che l'hanno preceduta, che una linea netta da tracciare.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="edificio-title edificio-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="edificio-title">Profondità e larghezza</title>
  <desc id="edificio-desc">Due edifici: uno alto e stretto con quattro piani, a rappresentare la profondità; uno basso e largo con un solo piano ma molte finestre, a rappresentare la larghezza.</desc>

  <text x="110" y="30" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">profondità</text>
  <g fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5">
    <rect x="50" y="60" width="120" height="40" /><rect x="50" y="100" width="120" height="40" />
    <rect x="50" y="140" width="120" height="40" /><rect x="50" y="180" width="120" height="40" />
  </g>
  <g fill="#fdfdfd" stroke="#2a7ae2" stroke-width="1">
    <rect x="62" y="72" width="14" height="16" /><rect x="103" y="72" width="14" height="16" /><rect x="144" y="72" width="14" height="16" />
    <rect x="62" y="112" width="14" height="16" /><rect x="103" y="112" width="14" height="16" /><rect x="144" y="112" width="14" height="16" />
    <rect x="62" y="152" width="14" height="16" /><rect x="103" y="152" width="14" height="16" /><rect x="144" y="152" width="14" height="16" />
    <rect x="62" y="192" width="14" height="16" /><rect x="103" y="192" width="14" height="16" /><rect x="144" y="192" width="14" height="16" />
  </g>
  <text x="110" y="240" fill="#828282" font-size="11" text-anchor="middle">quanti piani (strati)</text>

  <text x="340" y="30" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">larghezza</text>
  <rect x="240" y="180" width="200" height="40" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <g fill="#fdfdfd" stroke="#f66a0a" stroke-width="1">
    <rect x="250" y="192" width="14" height="16" /><rect x="278" y="192" width="14" height="16" /><rect x="306" y="192" width="14" height="16" />
    <rect x="334" y="192" width="14" height="16" /><rect x="362" y="192" width="14" height="16" /><rect x="390" y="192" width="14" height="16" /><rect x="418" y="192" width="14" height="16" />
  </g>
  <text x="340" y="240" fill="#828282" font-size="11" text-anchor="middle">quanti controllori per piano</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Due modi diversi di far crescere una rete, con compromessi diversi.</figcaption>
</figure>

### 2.5 Perché aggiungere piani è spesso più efficiente che allargarli

Il risultato matematico citato alla fine della Lezione 1 garantisce che un solo piano intermedio, se abbastanza largo, basta in teoria per rappresentare quasi qualunque relazione fra input e output. In pratica, però, aggiungere piani è spesso più conveniente che allargare solo il primo: ogni piano aggiuntivo può riusare ciò che il piano precedente ha già "capito", componendo elementi semplici in elementi via via più complessi — invece di dover ricostruire tutto da zero con un solo strato enorme. Vedremo questa idea diventare molto più concreta e visiva nella Lezione 3, con reti che imparano a riconoscere prima bordi, poi forme, poi oggetti interi, un piano alla volta.

### 2.6 L'altra faccia della medaglia: quanto è grande non significa quanto è bravo

Più piani e più controllori per piano significano più impostazioni regolabili — la **capacità** della rete, cioè quanto complicate sono le relazioni che può, in linea di principio, rappresentare. Ma una capacità molto più grande di quanto il problema richieda ha un rischio concreto: la rete può arrivare a *memorizzare* le particolarità specifiche degli esempi su cui si è allenata, invece di imparare la regola generale sottostante — un fenomeno chiamato **overfitting**, che la Lezione 7 tratterà con i suoi segnali di riconoscimento e i suoi rimedi. Per ora basta tenere a mente che scegliere quanto grande costruire una rete non è mai una questione di "più piani sono sempre meglio": è un compromesso fra potere rappresentativo e capacità di funzionare bene anche su casi mai visti prima.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="overfit-title overfit-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="overfit-title">Capacità troppo grande: memorizzare invece di generalizzare</title>
  <desc id="overfit-desc">Stessi punti in entrambi i pannelli. A sinistra, un confine liscio separa le due categorie ignorando un punto rumoroso. A destra, un confine molto più capace si piega per includere anche quel punto, un segno di overfitting.</desc>

  <text x="120" y="25" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">confine che generalizza</text>
  <path d="M 150,20 C140,80 160,140 140,200" fill="none" stroke="#828282" stroke-width="2" />
  <circle cx="70" cy="60" r="7" fill="#2a7ae2" /><circle cx="90" cy="120" r="7" fill="#2a7ae2" /><circle cx="60" cy="170" r="7" fill="#2a7ae2" /><circle cx="95" cy="185" r="7" fill="#2a7ae2" />
  <circle cx="95" cy="100" r="7" fill="#f66a0a" />
  <circle cx="180" cy="80" r="7" fill="#f66a0a" /><circle cx="200" cy="150" r="7" fill="#f66a0a" /><circle cx="220" cy="60" r="7" fill="#f66a0a" /><circle cx="190" cy="190" r="7" fill="#f66a0a" />
  <text x="120" y="230" fill="#828282" font-size="11" text-anchor="middle">ignora un punto rumoroso</text>

  <text x="400" y="25" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">confine che memorizza</text>
  <path d="M 430,20 C420,60 460,80 440,100 C425,115 460,140 420,200" fill="none" stroke="#c85506" stroke-width="2" />
  <circle cx="350" cy="60" r="7" fill="#2a7ae2" /><circle cx="370" cy="120" r="7" fill="#2a7ae2" /><circle cx="340" cy="170" r="7" fill="#2a7ae2" /><circle cx="375" cy="185" r="7" fill="#2a7ae2" />
  <circle cx="375" cy="100" r="7" fill="#f66a0a" />
  <circle cx="460" cy="80" r="7" fill="#f66a0a" /><circle cx="480" cy="150" r="7" fill="#f66a0a" /><circle cx="500" cy="60" r="7" fill="#f66a0a" /><circle cx="470" cy="190" r="7" fill="#f66a0a" />
  <text x="400" y="230" fill="#828282" font-size="11" text-anchor="middle">si contorce per includerlo</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Una capacità eccessiva può inseguire ogni singolarità degli esempi visti, a scapito della regola generale.</figcaption>
</figure>

---

> **Prova tu — Risolvi l'XOR con due controllori**
>
> Verifica a mano che la combinazione della Sezione 2.2 funzioni davvero. Per ognuna delle quattro combinazioni di X e Y, calcola: **A** (acceso se almeno uno tra X, Y è acceso), **B** (acceso se non sono entrambi accesi), e il **controllore finale** (acceso se sia A sia B sono accesi). Confronta il risultato con la tabella delle palline della Lezione 1: rosso ("finale" acceso) quando X e Y sono *diversi*, blu ("finale" spento) quando sono *uguali*.
>
> | X | Y | A (OR) | B (NAND) | Finale (A AND B) | Che colore dovrebbe essere? |
> |---|---|---|---|---|---|
> | 0 | 0 | ? | ? | ? | blu |
> | 0 | 1 | ? | ? | ? | rosso |
> | 1 | 0 | ? | ? | ? | rosso |
> | 1 | 1 | ? | ? | ? | blu |
>
> Riempi le tre colonne con i "?" per tutte e quattro le righe. Il controllore finale riproduce esattamente la colonna dei colori attesi? Se una riga non torna, rileggi la definizione di A o B nella Sezione 2.2 — è un buon modo per sentire, con le tue mani, perché serva *proprio* questa coppia di regole intermedie e non un'altra.

---

*Continua con la [Lezione 03 — Uno stencil che scorre sull'immagine]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-03-uno-stencil-che-scorre-sullimmagine.md %}.html)*
