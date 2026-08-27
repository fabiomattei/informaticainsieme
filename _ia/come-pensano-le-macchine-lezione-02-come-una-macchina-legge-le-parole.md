---
title: 'Lezione 02, Come una macchina legge le parole'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![La parola castello che entra in un tokenizzatore, ne escono i token cas-tel-lo, trasformati poi in vettori di numeri](/images/ia/come-pensano-le-macchine-lezione-02-come-una-macchina-legge-le-parole/come-pensano-le-macchine-lezione-02-come-una-macchina-legge-le-parole.svg){:class="aside-image"}

### 2.1 Un computer non sa cosa sia "gatto"

Prova a fermarti un attimo su un fatto scomodo: un computer non ha idea di cosa significhi la parola "gatto". Non ha mai visto un gatto, non ha mai sentito le fusa, non collega quella parola a niente, a meno che tu non gliela traduca in qualcosa che sa maneggiare davvero: i **numeri**. Tutto quello che un LLM fa, in fondo, è aritmetica su numeri. Il primo problema pratico, allora, è: come si trasforma un testo, fatto di lettere, spazi, punteggiatura, in numeri, senza perdere per strada il significato?

### 2.2 Spezzare in pezzi Lego

Il primo passo si chiama **tokenizzazione**: il testo viene tagliato in pezzetti, chiamati *token*, che poi verranno trasformati in numeri. La cosa forse sorprendente è che questi pezzetti spesso *non* sono parole intere.

Immagina di avere a disposizione un insieme fisso di "mattoncini Lego", diciamo qualche decina di migliaia di pezzi diversi, con cui costruire qualsiasi testo possibile, in qualsiasi lingua. Le parole comuni ("il", "casa", "andare") probabilmente hanno un mattoncino tutto per loro, perché compaiono così spesso che vale la pena dedicargli un pezzo apposito. Ma una parola rara, o inventata, o straniera, "sesquipedale", "ChatGPT", "supercalifragilistichespiralidoso", quasi certamente **non** ha un mattoncino dedicato: viene spezzata in due, tre, quattro pezzi più piccoli che, uniti, la ricompongono. Un po' come costruire "castello" con i mattoncini "cas" + "tel" + "lo" quando non hai il pezzo "castello" intero nella scatola.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 620 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="lego-title lego-desc" style="width: 100%; max-width: 560px; height: auto; font-family: inherit;">
  <title id="lego-title">Mattoncini di tokenizzazione</title>
  <desc id="lego-desc">In alto, la parola comune "il" come un unico mattoncino intero. In basso, la parola rara "castello" spezzata in tre mattoncini più piccoli: cas, tel, lo.</desc>

  <!-- parola comune: un mattoncino intero -->
  <g>
    <circle cx="250" cy="60" r="8" fill="#2a7ae2" />
    <circle cx="310" cy="60" r="8" fill="#2a7ae2" />
    <circle cx="370" cy="60" r="8" fill="#2a7ae2" />
    <rect x="220" y="60" width="180" height="60" rx="6" fill="#2a7ae2" stroke="#1d5eb8" stroke-width="1.5" />
    <text x="310" y="98" fill="#fdfdfd" font-size="20" font-weight="bold" text-anchor="middle">il</text>
  </g>
  <text x="310" y="150" fill="#111" font-size="13" text-anchor="middle">parola comune → un mattoncino intero</text>

  <!-- parola rara: spezzata in tre mattoncini -->
  <g>
    <circle cx="190" cy="200" r="8" fill="#2a7ae2" />
    <circle cx="240" cy="200" r="8" fill="#2a7ae2" />
    <rect x="155" y="200" width="120" height="60" rx="6" fill="#2a7ae2" stroke="#1d5eb8" stroke-width="1.5" />
    <text x="215" y="238" fill="#fdfdfd" font-size="17" font-weight="bold" text-anchor="middle">cas</text>

    <circle cx="305" cy="200" r="8" fill="#f66a0a" />
    <circle cx="355" cy="200" r="8" fill="#f66a0a" />
    <rect x="275" y="200" width="110" height="60" rx="6" fill="#f66a0a" stroke="#c85506" stroke-width="1.5" />
    <text x="330" y="238" fill="#fdfdfd" font-size="17" font-weight="bold" text-anchor="middle">tel</text>

    <circle cx="425" cy="200" r="8" fill="#3aa655" />
    <rect x="395" y="200" width="80" height="60" rx="6" fill="#3aa655" stroke="#2c7f3f" stroke-width="1.5" />
    <text x="435" y="238" fill="#fdfdfd" font-size="17" font-weight="bold" text-anchor="middle">lo</text>
  </g>
  <text x="490" y="238" fill="#111" font-size="16" font-style="italic" text-anchor="start">= castello</text>
  <text x="310" y="290" fill="#111" font-size="13" text-anchor="middle">parola rara → spezzata in più mattoncini, poi ricomposta</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Le parole comuni occupano un mattoncino intero; quelle rare vengono ricostruite unendo pezzi più piccoli.</figcaption>
</figure>

Questo spiega, tra l'altro, una stranezza che forse hai notato usando un chatbot: certe parole "costano" di più di altre. Una parola rara o in una lingua poco rappresentata nei testi di addestramento viene spesso spezzata in molti più pezzi di una parola comune in inglese, ed essendo il "prezzo" di un chatbot spesso legato al numero di pezzetti processati, questo ha conseguenze molto concrete, non solo teoriche.

### 2.3 Una mappa dei significati

Spezzare in pezzetti risolve solo metà del problema: abbiamo dei pezzi, ma sono ancora "pezzi di lettere", non numeri che catturano il significato. Il passo successivo, e qui sta il cuore dell'idea, è associare a ogni pezzetto un punto in una specie di **mappa dei significati**: invece di longitudine e latitudine, questa mappa ha centinaia di "coordinate", ma l'idea di fondo è la stessa di una mappa geografica. Parole con significato simile finiscono vicine sulla mappa; parole senza relazione finiscono lontane.

Per farti un'idea con una mappa fittizia a sole due coordinate (una mappa vera ne usa centinaia, ma il principio non cambia): "gatto" e "cane", entrambi animali domestici, potrebbero finire vicini, tipo alle coordinate (3, 5) e (4, 5). "Automobile" starebbe da tutt'altra parte, diciamo (9, 1). E "gattino", piccolo di gatto, starebbe vicinissimo a "gatto", magari (3, 6).

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="mappa-title mappa-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="mappa-title">Mappa dei significati a due coordinate</title>
  <desc id="mappa-desc">Grafico cartesiano con quattro parole posizionate come punti: gatto (3,5), cane (4,5), gattino (3,6) e automobile (9,1). Gatto, cane e gattino sono vicini tra loro; automobile è isolata.</desc>

  <!-- griglia -->
  <g stroke="#e3e3e3" stroke-width="1">
    <line x1="50" y1="20" x2="50" y2="370" />
    <line x1="140" y1="20" x2="140" y2="370" />
    <line x1="230" y1="20" x2="230" y2="370" />
    <line x1="320" y1="20" x2="320" y2="370" />
    <line x1="410" y1="20" x2="410" y2="370" />
    <line x1="500" y1="20" x2="500" y2="370" />
    <line x1="50" y1="370" x2="500" y2="370" />
    <line x1="50" y1="300" x2="500" y2="300" />
    <line x1="50" y1="230" x2="500" y2="230" />
    <line x1="50" y1="160" x2="500" y2="160" />
    <line x1="50" y1="90"  x2="500" y2="90" />
    <line x1="50" y1="20"  x2="500" y2="20" />
  </g>

  <!-- assi -->
  <g stroke="#828282" stroke-width="1.5">
    <line x1="50" y1="370" x2="500" y2="370" />
    <line x1="50" y1="20" x2="50" y2="370" />
  </g>

  <!-- etichette tick -->
  <g fill="#828282" font-size="12" text-anchor="middle">
    <text x="50" y="388">0</text>
    <text x="140" y="388">2</text>
    <text x="230" y="388">4</text>
    <text x="320" y="388">6</text>
    <text x="410" y="388">8</text>
    <text x="500" y="388">10</text>
  </g>
  <g fill="#828282" font-size="12" text-anchor="end">
    <text x="42" y="374">0</text>
    <text x="42" y="304">2</text>
    <text x="42" y="234">4</text>
    <text x="42" y="164">6</text>
    <text x="42" y="94">8</text>
    <text x="42" y="24">10</text>
  </g>

  <!-- titoli assi -->
  <text x="275" y="410" fill="#111" font-size="13" text-anchor="middle">coordinata orizzontale</text>
  <text x="16" y="195" fill="#111" font-size="13" text-anchor="middle" transform="rotate(-90 16 195)">coordinata verticale</text>

  <!-- linee di vicinanza -->
  <g stroke="#2a7ae2" stroke-width="1" stroke-dasharray="4 4" opacity="0.5">
    <line x1="185" y1="195" x2="230" y2="195" />
    <line x1="185" y1="195" x2="185" y2="160" />
  </g>

  <!-- punti -->
  <circle cx="185" cy="195" r="6" fill="#2a7ae2" />
  <text x="175" y="215" fill="#111" font-size="14" text-anchor="end">gatto (3, 5)</text>

  <circle cx="230" cy="195" r="6" fill="#2a7ae2" />
  <text x="240" y="190" fill="#111" font-size="14" text-anchor="start">cane (4, 5)</text>

  <circle cx="185" cy="160" r="6" fill="#2a7ae2" />
  <text x="185" y="145" fill="#111" font-size="14" text-anchor="middle">gattino (3, 6)</text>

  <circle cx="455" cy="335" r="6" fill="#f66a0a" />
  <text x="455" y="320" fill="#111" font-size="14" text-anchor="middle">automobile (9, 1)</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Gatto, cane e gattino finiscono vicini sulla mappa; automobile resta isolata, molto più lontana.</figcaption>
</figure>

Il punto cruciale, e forse il più sorprendente: **nessuno disegna questa mappa a mano**. Il modello la costruisce da solo, durante l'addestramento, spostando pian piano ogni parola sulla mappa in modo che l'unica cosa che conta, indovinare bene la parola successiva, funzioni sempre meglio. Se spostare "gatto" più vicino a "cane" aiuta a indovinare meglio le frasi del testo di addestramento, il modello lo fa. Nessuno gli ha insegnato che sono entrambi animali: lo ha dedotto vedendo che compaiono spesso in contesti simili ("il mio ___ dorme tutto il giorno", "porto il ___ dal veterinario").

### 2.4 Fare i conti con i significati

La conseguenza più sorprendente di avere i significati come punti su una mappa è che ci puoi fare *aritmetica*. Non aritmetica sulle lettere, aritmetica sulle coordinate. Il caso più famoso, diventato quasi un aneddoto classico nel campo: prendi il punto di "re", sottrai il punto di "uomo", aggiungi il punto di "donna". Il punto più vicino al risultato, sulla mappa costruita da un modello ben addestrato, tende a essere... "regina".

Detto a parole: "re" sta a "uomo" come "?" sta a "donna", e il modello, senza che nessuno gli abbia insegnato la parola "monarchia" o "genere", risponde correttamente solo perché ha organizzato la mappa in modo che questa relazione geometrica rispecchi una relazione di significato reale. Non funziona sempre alla perfezione, e su mappe più piccole o mal costruite l'aneddoto è più pulito in teoria che in pratica, ma il principio regge, ed è uno dei modi più diretti per toccare con mano che quei numeri non sono arbitrari: hanno una geometria che *significa* qualcosa.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="vettori-title vettori-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="vettori-title">re − uomo + donna = regina</title>
  <desc id="vettori-desc">Quattro punti su una mappa a due coordinate: uomo (2,3), re (8,5), donna (2,1), regina (8,3). Le frecce uomo→re e donna→regina sono parallele e della stessa lunghezza, a formare un parallelogramma.</desc>

  <!-- griglia -->
  <g stroke="#e3e3e3" stroke-width="1">
    <line x1="50" y1="20" x2="50" y2="370" /><line x1="140" y1="20" x2="140" y2="370" />
    <line x1="230" y1="20" x2="230" y2="370" /><line x1="320" y1="20" x2="320" y2="370" />
    <line x1="410" y1="20" x2="410" y2="370" /><line x1="500" y1="20" x2="500" y2="370" />
    <line x1="50" y1="370" x2="500" y2="370" /><line x1="50" y1="300" x2="500" y2="300" />
    <line x1="50" y1="230" x2="500" y2="230" /><line x1="50" y1="160" x2="500" y2="160" />
    <line x1="50" y1="90"  x2="500" y2="90" />  <line x1="50" y1="20"  x2="500" y2="20" />
  </g>

  <!-- assi -->
  <g stroke="#828282" stroke-width="1.5">
    <line x1="50" y1="370" x2="500" y2="370" />
    <line x1="50" y1="20" x2="50" y2="370" />
  </g>
  <g fill="#828282" font-size="12" text-anchor="middle">
    <text x="50" y="388">0</text><text x="140" y="388">2</text><text x="230" y="388">4</text>
    <text x="320" y="388">6</text><text x="410" y="388">8</text><text x="500" y="388">10</text>
  </g>
  <g fill="#828282" font-size="12" text-anchor="end">
    <text x="42" y="374">0</text><text x="42" y="304">2</text><text x="42" y="234">4</text>
    <text x="42" y="164">6</text><text x="42" y="94">8</text><text x="42" y="24">10</text>
  </g>

  <defs>
    <marker id="arrowVett" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse">
      <path d="M0,0 L8,4 L0,8 z" fill="#2a7ae2" />
    </marker>
  </defs>

  <!-- contorno del parallelogramma -->
  <g stroke="#828282" stroke-width="1.5" stroke-dasharray="4 4" fill="none">
    <line x1="140" y1="265" x2="140" y2="335" />
    <line x1="410" y1="195" x2="410" y2="265" />
  </g>

  <!-- le due frecce: stesso spostamento, applicato due volte -->
  <path d="M 140,265 L 405,197" fill="none" stroke="#2a7ae2" stroke-width="3" marker-end="url(#arrowVett)" />
  <path d="M 140,335 L 405,267" fill="none" stroke="#2a7ae2" stroke-width="3" marker-end="url(#arrowVett)" />

  <!-- punti -->
  <circle cx="140" cy="265" r="6" fill="#2a7ae2" />
  <text x="130" y="255" fill="#111" font-size="14" text-anchor="end">uomo (2, 3)</text>

  <circle cx="410" cy="195" r="6" fill="#2a7ae2" />
  <text x="420" y="190" fill="#111" font-size="14" text-anchor="start">re (8, 5)</text>

  <circle cx="140" cy="335" r="6" fill="#2a7ae2" />
  <text x="130" y="355" fill="#111" font-size="14" text-anchor="end">donna (2, 1)</text>

  <circle cx="410" cy="265" r="7" fill="#f66a0a" />
  <text x="420" y="285" fill="#f66a0a" font-size="14" font-weight="bold" text-anchor="start">regina (8, 3)</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Lo stesso spostamento ("+6 orizzontale, +2 verticale") che porta da uomo a re, applicato a donna, atterra su regina.</figcaption>
</figure>

### 2.5 Anche l'ordine conta

Un ultimo dettaglio, facile da dimenticare: "il cane morde il postino" e "il postino morde il cane" usano esattamente le stesse parole, ma vorresti sicuramente che il modello le distinguesse. Per questo, oltre alla posizione sulla mappa dei significati, a ogni pezzetto viene attaccata anche un'informazione che dice "sei il primo token della frase", "sei il secondo", e così via, una specie di numeretto di posizione cucito addosso a ogni pezzo, che il modello impara a leggere insieme al significato. Torneremo su come questo numeretto di posizione entra in gioco proprio nel meccanismo di attenzione, nella prossima lezione.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="ordine-title ordine-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="ordine-title">Stesse parole, ordine diverso, ruoli diversi</title>
  <desc id="ordine-desc">Due frasi con le stesse cinque parole in ordine diverso: "il cane morde il postino" e "il postino morde il cane". Nella prima cane è il soggetto e postino l'oggetto; nella seconda i ruoli si scambiano.</desc>

  <!-- riga 1: il cane morde il postino -->
  <g font-size="11" fill="#828282" text-anchor="middle">
    <circle cx="60" cy="40" r="12" fill="#fdfdfd" stroke="#828282" /><text x="60" y="44">1</text>
    <circle cx="146" cy="40" r="12" fill="#fdfdfd" stroke="#828282" /><text x="146" y="44">2</text>
    <circle cx="232" cy="40" r="12" fill="#fdfdfd" stroke="#828282" /><text x="232" y="44">3</text>
    <circle cx="318" cy="40" r="12" fill="#fdfdfd" stroke="#828282" /><text x="318" y="44">4</text>
    <circle cx="404" cy="40" r="12" fill="#fdfdfd" stroke="#828282" /><text x="404" y="44">5</text>
  </g>
  <g font-size="14" text-anchor="middle">
    <rect x="20" y="56" width="80" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="60" y="83" fill="#111">il</text>
    <rect x="106" y="56" width="80" height="44" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" /><text x="146" y="83" fill="#111">cane</text>
    <rect x="192" y="56" width="80" height="44" rx="6" fill="#f0f0f0" stroke="#828282" stroke-width="1.5" /><text x="232" y="83" fill="#111">morde</text>
    <rect x="278" y="56" width="80" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="318" y="83" fill="#111">il</text>
    <rect x="364" y="56" width="80" height="44" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="404" y="83" fill="#111">postino</text>
  </g>
  <g font-size="12" text-anchor="middle">
    <text x="146" y="118" fill="#2a7ae2">soggetto</text>
    <text x="232" y="118" fill="#555">verbo</text>
    <text x="404" y="118" fill="#f66a0a">oggetto</text>
  </g>

  <text x="240" y="150" fill="#828282" font-size="13" text-anchor="middle">stesse parole, ordine diverso ↓</text>

  <!-- riga 2: il postino morde il cane -->
  <g font-size="11" fill="#828282" text-anchor="middle">
    <circle cx="60" cy="176" r="12" fill="#fdfdfd" stroke="#828282" /><text x="60" y="180">1</text>
    <circle cx="146" cy="176" r="12" fill="#fdfdfd" stroke="#828282" /><text x="146" y="180">2</text>
    <circle cx="232" cy="176" r="12" fill="#fdfdfd" stroke="#828282" /><text x="232" y="180">3</text>
    <circle cx="318" cy="176" r="12" fill="#fdfdfd" stroke="#828282" /><text x="318" y="180">4</text>
    <circle cx="404" cy="176" r="12" fill="#fdfdfd" stroke="#828282" /><text x="404" y="180">5</text>
  </g>
  <g font-size="14" text-anchor="middle">
    <rect x="20" y="192" width="80" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="60" y="219" fill="#111">il</text>
    <rect x="106" y="192" width="80" height="44" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" /><text x="146" y="219" fill="#111">postino</text>
    <rect x="192" y="192" width="80" height="44" rx="6" fill="#f0f0f0" stroke="#828282" stroke-width="1.5" /><text x="232" y="219" fill="#111">morde</text>
    <rect x="278" y="192" width="80" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="318" y="219" fill="#111">il</text>
    <rect x="364" y="192" width="80" height="44" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="404" y="219" fill="#111">cane</text>
  </g>
  <g font-size="12" text-anchor="middle">
    <text x="146" y="254" fill="#2a7ae2">soggetto</text>
    <text x="232" y="254" fill="#555">verbo</text>
    <text x="404" y="254" fill="#f66a0a">oggetto</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Stesse cinque parole, stesse posizioni numerate, ma "cane" e "postino" si scambiano i ruoli a seconda dell'ordine.</figcaption>
</figure>

---

> **Prova tu, Il cruciverba dei vettori**
>
> Ecco una mappa dei significati giocattolo, con solo due coordinate (orizzontale, verticale) e otto parole già posizionate:
>
> | Parola | Coordinate |
> |---|---|
> | uomo | (2, 6) |
> | donna | (2, 2) |
> | re | (8, 6) |
> | regina | (?, ?) |
> | ragazzo | (1, 7) |
> | ragazza | (1, 3) |
> | principe | (7, 7) |
> | principessa | (?, ?) |
>
> La relazione geometrica che lega "uomo" a "donna" è: stessa coordinata orizzontale (2), coordinata verticale che scende da 6 a 2 (cioè: **−4**).
>
> 1. Applica la stessa relazione ("re" meno "uomo" più "donna") per calcolare le coordinate mancanti di "regina", partendo da "re" = (8, 6).
> 2. Controlla che la stessa identica relazione, applicata a "principe" = (7, 7), ti dia coordinate ragionevoli per "principessa", coerenti, cioè, con lo stesso spostamento verticale di −4 che hai usato sopra.
> 3. Bonus: guardando solo le coordinate orizzontali (2, 2, 8, 1, 1, 7), riesci a indovinare *cosa* rappresenta quell'asse, che caratteristica condivisa separa il gruppo (2,2,1,1) dal gruppo (8,7)?
>
> Soluzioni e ragionamento in Appendice A.

---

## Esercizi

1. Scegli una parola rara o inventata, per esempio un nome proprio poco comune o una parola straniera, e prova a immaginare come un tokenizzatore potrebbe spezzarla in pezzi più piccoli, come "cas" + "tel" + "lo" per "castello".
2. Su una mappa dei significati immaginaria a due coordinate, colloca, anche solo a parole con coordinate plausibili, quattro parole a tua scelta legate fra loro da un tema comune, per esempio quattro sport o quattro strumenti musicali, motivando quali finiscono più vicine e quali più lontane.
3. Spiega, con parole tue, perché nessuno "disegna a mano" la mappa dei significati, e come fa il modello a costruirla da solo durante l'addestramento.
4. Prova a costruire una tua analogia in stile "re − uomo + donna = regina", usando quattro parole diverse legate dalla stessa relazione, per esempio un mestiere e il suo equivalente al femminile o maschile. Spiega perché ti aspetteresti che funzioni.
5. Spiega perché le frasi "il cane morde il postino" e "il postino morde il cane" devono essere rappresentate in modo diverso dal modello, pur essendo fatte esattamente delle stesse cinque parole, e quale informazione aggiuntiva permette di distinguerle.

---

*Continua con la [Lezione 03, Il segreto dell'attenzione]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-03-il-segreto-dellattenzione.md %}.html)*
