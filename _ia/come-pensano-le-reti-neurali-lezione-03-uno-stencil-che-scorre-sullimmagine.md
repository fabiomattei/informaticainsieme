---
title: 'Lezione 03 — Uno stencil che scorre sull''immagine'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 3.1 Il problema: un'immagine non è un elenco di numeri qualsiasi

La rete a più piani della Lezione 2 tratta il proprio input come un elenco piatto di numeri, senza distinzioni. Va benissimo per due indizi come "quanto è nuvoloso" e "quanto è umido" — ma cosa succede se l'input è un'immagine, diciamo un quadrato di 256 per 256 puntini di colore (pixel)? Una rete come quella della Lezione 2 la vedrebbe come un unico elenco di oltre 65mila numeri in fila, senza alcuna nozione che il puntino in una certa posizione sia spazialmente vicino al puntino appena accanto. Questo crea due problemi seri.

Primo: collegare ogni controllore a *tutti* i 65mila puntini richiederebbe milioni di connessioni regolabili già per un solo piano — un costo enorme, e un rischio altissimo di "imparare a memoria" invece che a capire (il problema anticipato nella Sezione 2.6). Secondo, più sottile: una rete così allenata a riconoscere un gatto in alto a sinistra di una foto non avrebbe alcuna garanzia di riconoscere lo stesso identico gatto se comparisse in basso a destra, perché ogni puntino avrebbe un'importanza propria e indipendente da tutte le altre. Manca quella che si chiama **invarianza traslazionale**: la capacità di riconoscere un motivo locale ovunque appaia nell'immagine, senza doverlo re-imparare posizione per posizione.

### 3.2 L'idea: uno stencil riusabile

Le **reti convoluzionali** risolvono entrambi i problemi con un'unica idea, presa in prestito da qualcosa che forse hai già usato con carta e forbici: uno stencil, o un timbro di gomma. Invece di guardare l'intera immagine in un colpo solo, un piccolo stencil — diciamo, grande quanto un francobollo — osserva solo una piccola porzione dell'immagine alla volta, e lo stesso identico stencil viene fatto scorrere su ogni porzione della griglia, una alla volta. Questo doppio vincolo — guardare solo un pezzetto per volta, e riusare sempre lo stesso stencil — riduce drasticamente il numero di impostazioni da regolare (uno stencil piccolo ha pochissimi numeri, indipendentemente da quanto è grande l'immagine intera) e garantisce l'invarianza traslazionale quasi per costruzione: lo stesso stencil che riconosce un bordo verticale in un angolo dell'immagine riconosce lo stesso identico bordo, con la stessa sensibilità, ovunque compaia.

### 3.3 Come funziona lo stencil in pratica

Immagina un pezzetto di immagine in bianco e nero, rappresentato come una griglia di numeri (0 per il nero, 1 per il bianco), e uno stencil altrettanto piccolo — una griglia di numeri anch'essa, pensata per "accendersi" quando incontra un certo motivo, per esempio un bordo verticale netto fra chiaro e scuro. L'operazione è semplice da descrivere passo passo: appoggia lo stencil in un angolo dell'immagine, moltiplica ogni numero dello stencil per il numero dell'immagine che gli sta esattamente sotto, somma tutti quei prodotti in un unico numero, e scrivi quel numero in una nuova griglia — la **mappa delle caratteristiche** — nella posizione corrispondente. Poi sposta lo stencil di una casella e ripeti, fino ad aver coperto l'intera immagine.

Il numero che risulta a ogni posizione misura quanto fortemente il motivo cercato dallo stencil è presente proprio lì: alto se il motivo c'è, basso o nullo altrimenti. Uno strato di questo tipo, in pratica, non usa un solo stencil ma decine o centinaia in parallelo, ciascuno alla ricerca di un motivo diverso — un bordo orizzontale, uno diagonale, una macchia di un certo colore — producendo altrettante mappe delle caratteristiche, ognuna seguita, come nella Lezione 1, da una manopola sfumata che decide quanto "accendere" quella risposta.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="stencil-title stencil-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="stencil-title">Lo stencil che scorre sull'immagine</title>
  <desc id="stencil-desc">Un piccolo stencil 3×3 osserva una porzione dell'immagine alla volta e produce un numero nella mappa delle caratteristiche, poi scorre di una casella e ripete.</desc>

  <text x="130" y="25" fill="#111" font-size="13" text-anchor="middle">immagine</text>
  <g stroke="#e3e3e3" stroke-width="1">
    <line x1="40" y1="40" x2="220" y2="40" /><line x1="40" y1="70" x2="220" y2="70" /><line x1="40" y1="100" x2="220" y2="100" />
    <line x1="40" y1="130" x2="220" y2="130" /><line x1="40" y1="160" x2="220" y2="160" /><line x1="40" y1="190" x2="220" y2="190" /><line x1="40" y1="220" x2="220" y2="220" />
    <line x1="40" y1="40" x2="40" y2="220" /><line x1="70" y1="40" x2="70" y2="220" /><line x1="100" y1="40" x2="100" y2="220" />
    <line x1="130" y1="40" x2="130" y2="220" /><line x1="160" y1="40" x2="160" y2="220" /><line x1="190" y1="40" x2="190" y2="220" /><line x1="220" y1="40" x2="220" y2="220" />
  </g>
  <rect x="40" y="40" width="90" height="90" fill="none" stroke="#f66a0a" stroke-width="3" />
  <rect x="70" y="40" width="90" height="90" fill="none" stroke="#f66a0a" stroke-width="1.5" stroke-dasharray="5 3" opacity="0.6" />
  <text x="85" y="150" fill="#c85506" font-size="11" text-anchor="middle">scorre di una</text>
  <text x="85" y="163" fill="#c85506" font-size="11" text-anchor="middle">casella per volta</text>

  <defs>
    <marker id="arrowSten" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>
  <path d="M 230,85 Q 280,85 320,90" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowSten)" />

  <text x="384" y="25" fill="#111" font-size="12" text-anchor="middle">mappa delle caratteristiche</text>
  <g stroke="#e3e3e3" stroke-width="1">
    <line x1="340" y1="70" x2="428" y2="70" /><line x1="340" y1="92" x2="428" y2="92" /><line x1="340" y1="114" x2="428" y2="114" /><line x1="340" y1="136" x2="428" y2="136" /><line x1="340" y1="158" x2="428" y2="158" />
    <line x1="340" y1="70" x2="340" y2="158" /><line x1="362" y1="70" x2="362" y2="158" /><line x1="384" y1="70" x2="384" y2="158" /><line x1="406" y1="70" x2="406" y2="158" /><line x1="428" y1="70" x2="428" y2="158" />
  </g>
  <rect x="340" y="70" width="22" height="22" fill="#f66a0a" />
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Lo stesso identico stencil viene riusato su ogni porzione dell'immagine.</figcaption>
</figure>

### 3.4 Riassumere senza perdere l'essenziale

Dopo che uno o più stencil hanno prodotto le loro mappe delle caratteristiche, è comune applicare un passaggio di "riassunto": si prendono piccole regioni della mappa (tipicamente blocchi di 2×2 caselle) e si conserva solo il valore più alto di ciascun blocco, scartando gli altri tre — un'operazione chiamata **max pooling**. L'effetto è duplice: la mappa diventa più piccola e più veloce da elaborare nei passaggi successivi, e la rete diventa un po' più tollerante a piccoli spostamenti (se il motivo cercato si sposta di un solo puntino dentro quel blocco di 2×2, il massimo del blocco spesso resta lo stesso).

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="pooling-title pooling-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="pooling-title">Max pooling: riassumere senza perdere l'essenziale</title>
  <desc id="pooling-desc">Una mappa 4×4 divisa in quattro blocchi 2×2, ciascuno colorato diversamente. Ogni blocco si riduce al suo valore più alto in una mappa 2×2 più piccola.</desc>

  <text x="120" y="25" fill="#111" font-size="12" text-anchor="middle">mappa delle caratteristiche</text>
  <rect x="40" y="40" width="80" height="80" fill="#dceafc" opacity="0.5" />
  <rect x="120" y="40" width="80" height="80" fill="#fde8d6" opacity="0.5" />
  <rect x="40" y="120" width="80" height="80" fill="#dcf3e4" opacity="0.5" />
  <rect x="120" y="120" width="80" height="80" fill="#e6dcfb" opacity="0.5" />
  <g stroke="#828282" stroke-width="1">
    <line x1="40" y1="80" x2="200" y2="80" /><line x1="80" y1="40" x2="80" y2="200" /><line x1="160" y1="40" x2="160" y2="200" /><line x1="40" y1="160" x2="200" y2="160" />
  </g>
  <rect x="40" y="40" width="160" height="160" fill="none" stroke="#828282" stroke-width="2" />
  <g font-size="14" fill="#111" text-anchor="middle">
    <text x="60" y="65">3</text><text x="100" y="65">5</text><text x="140" y="65">4</text><text x="180" y="65">1</text>
    <text x="60" y="105">1</text><text x="100" y="105">2</text><text x="140" y="105">0</text><text x="180" y="105">2</text>
    <text x="60" y="145">2</text><text x="100" y="145">6</text><text x="140" y="145">3</text><text x="180" y="145">1</text>
    <text x="60" y="185">3</text><text x="100" y="185">1</text><text x="140" y="185">5</text><text x="180" y="185">2</text>
  </g>

  <defs>
    <marker id="arrowPool" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>
  <path d="M 210,120 L 300,120" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowPool)" />
  <text x="255" y="105" fill="#828282" font-size="10" text-anchor="middle">tieni solo il massimo</text>

  <text x="360" y="25" fill="#111" font-size="12" text-anchor="middle">dopo il max pooling</text>
  <rect x="320" y="80" width="40" height="40" fill="#dceafc" />
  <rect x="360" y="80" width="40" height="40" fill="#fde8d6" />
  <rect x="320" y="120" width="40" height="40" fill="#dcf3e4" />
  <rect x="360" y="120" width="40" height="40" fill="#e6dcfb" />
  <g stroke="#828282" stroke-width="1.5" fill="none">
    <rect x="320" y="80" width="80" height="80" />
    <line x1="320" y1="120" x2="400" y2="120" /><line x1="360" y1="80" x2="360" y2="160" />
  </g>
  <g font-size="15" font-weight="bold" fill="#111" text-anchor="middle">
    <text x="340" y="106">5</text><text x="380" y="106">4</text>
    <text x="340" y="146">6</text><text x="380" y="146">5</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La mappa diventa più piccola, e tollera piccoli spostamenti del motivo cercato.</figcaption>
</figure>

### 3.5 Dai bordi agli oggetti: una gerarchia di motivi

Impilando più coppie stencil-e-riassunto, una rete convoluzionale costruisce tipicamente una scala di rappresentazioni sempre più astratte, un piano alla volta — proprio l'idea di "riusare ciò che il piano precedente ha già capito" anticipata nella Sezione 2.5, qui resa particolarmente concreta e visiva. I primi piani imparano stencil che rispondono a motivi elementari: bordi orizzontali, verticali, diagonali, transizioni di colore. I piani intermedi combinano quei motivi elementari in forme più composite: angoli, texture, curve semplici. I piani più profondi combinano quelle forme in parti riconoscibili di un oggetto — un occhio, una ruota — fino ad arrivare, negli ultimi piani (tipicamente piani densi come quelli della Lezione 2), a una decisione sull'immagine intera: "questo è un gatto".

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="gerarchia-title gerarchia-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="gerarchia-title">Dai bordi agli oggetti</title>
  <desc id="gerarchia-desc">Quattro fasi in sequenza: l'immagine grezza, i bordi elementari, le forme composite, e infine il riconoscimento dell'oggetto intero, "gatto".</desc>

  <defs>
    <marker id="arrowGer" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="20" y="50" width="110" height="90" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <g stroke="#c9c9c9" stroke-width="1"><line x1="35" y1="65" x2="115" y2="65" /><line x1="35" y1="85" x2="115" y2="85" /><line x1="35" y1="105" x2="115" y2="105" /><line x1="35" y1="125" x2="115" y2="125" />
  <line x1="55" y1="60" x2="55" y2="130" /><line x1="75" y1="60" x2="75" y2="130" /><line x1="95" y1="60" x2="95" y2="130" /></g>
  <text x="75" y="165" fill="#828282" font-size="11" text-anchor="middle">immagine grezza</text>

  <path d="M 132,95 L 158,95" stroke="#828282" stroke-width="2" marker-end="url(#arrowGer)" fill="none" />

  <rect x="160" y="50" width="110" height="90" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <g stroke="#2a7ae2" stroke-width="2"><line x1="180" y1="70" x2="180" y2="120" /><line x1="200" y1="65" x2="250" y2="65" /><line x1="195" y1="100" x2="245" y2="130" /></g>
  <text x="215" y="165" fill="#1d5eb8" font-size="11" text-anchor="middle">bordi</text>

  <path d="M 272,95 L 298,95" stroke="#828282" stroke-width="2" marker-end="url(#arrowGer)" fill="none" />

  <rect x="300" y="50" width="110" height="90" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <path d="M 325,120 Q 325,70 380,70 L 380,110" fill="none" stroke="#f66a0a" stroke-width="2.5" />
  <text x="355" y="165" fill="#c85506" font-size="11" text-anchor="middle">forme</text>

  <path d="M 412,95 L 438,95" stroke="#828282" stroke-width="2" marker-end="url(#arrowGer)" fill="none" />

  <rect x="440" y="50" width="100" height="90" rx="8" fill="#dcf3e4" stroke="#3aa655" stroke-width="1.5" />
  <text x="490" y="100" font-size="26" text-anchor="middle">🐱</text>
  <text x="490" y="165" fill="#2c7f3f" font-size="11" text-anchor="middle">"gatto" ✓</text>

  <text x="280" y="20" fill="#828282" font-size="11" text-anchor="middle">piani superficiali → piani profondi</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni piano riusa ciò che il precedente ha già capito, componendo elementi via via più complessi.</figcaption>
</figure>

---

> **Prova tu — Fai scorrere lo stencil**
>
> Ecco una mini-immagine 4×4 (1 = chiaro, 0 = scuro) e uno stencil 3×3 pensato per rilevare un bordo verticale (dove il "chiaro" finisce e comincia lo "scuro" andando da sinistra a destra):
>
> ```
> Immagine:        Stencil:
> 1 1 0 0          1  0  -1
> 1 1 0 0          1  0  -1
> 1 1 0 0          1  0  -1
> 1 1 0 0
> ```
>
> Lo stencil può appoggiarsi in quattro posizioni diverse sull'immagine (il suo angolo in alto a sinistra può stare in riga 1 o 2, colonna 1 o 2). Per ciascuna posizione: moltiplica ogni numero dello stencil per il numero dell'immagine esattamente sotto, e somma i nove prodotti.
>
> 1. Calcola il risultato per lo stencil appoggiato in alto a sinistra (righe 1-3, colonne 1-3 dell'immagine).
> 2. Calcola il risultato per lo stencil spostato di una colonna a destra (righe 1-3, colonne 2-4).
> 3. Senza rifare tutti i calcoli, indovina (e poi verifica) i risultati per le due posizioni rimanenti (righe 2-4). Suggerimento: l'immagine è identica riga dopo riga — cosa significa questo per il risultato dello stencil, che guarda solo colonne diverse a seconda della posizione orizzontale?
> 4. Che numero ti aspetteresti se l'immagine, invece di avere il bordo netto fra colonna 2 e colonna 3, fosse tutta uniformemente chiara (tutti 1)? Prova a ricalcolare per la posizione del punto 1 e confronta.

---

*Continua con la [Lezione 04 — Il filo della memoria]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-04-il-filo-della-memoria.md %}.html)*
