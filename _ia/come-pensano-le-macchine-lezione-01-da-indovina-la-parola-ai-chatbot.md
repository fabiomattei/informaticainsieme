---
title: 'Lezione 01 — Da "indovina la parola" ai chatbot'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Evoluzione dai bigrammi alla rete ricorrente al Transformer con attenzione, fino al chatbot moderno](/images/ia/come-pensano-le-macchine-lezione-01-da-indovina-la-parola-ai-chatbot/come-pensano-le-macchine-lezione-01-da-indovina-la-parola-ai-chatbot.svg){:class="aside-image"}

### 1.1 L'amico che finisce le tue frasi

Conosci quella persona che, ogni volta che inizi una frase, la finisce al posto tuo? A volte indovina, a volte no, ma ci prova sempre. Ecco: un modello linguistico (in inglese *Large Language Model*, LLM — modello linguistico di grandi dimensioni) è esattamente questo, portato a un livello quasi assurdo. Legge quello che hai scritto finora e prova a indovinare cosa viene dopo. Una parola alla volta. Poi un'altra. Poi un'altra ancora — e mettendo insieme tutte queste piccole previsioni, una via l'altra, viene fuori un'intera risposta.

Sembra un trucco troppo semplice per spiegare conversazioni che sembrano intelligenti. Eppure è davvero così che funziona — il difficile non è l'idea, ma *come* si arriva a indovinare bene. E per capirlo, conviene vedere come ci si è arrivati passo passo, perché all'inizio andava malissimo.

### 1.2 Il primo trucco: contare le parole

Immagina di dover indovinare la parola dopo "il gatto" avendo a disposizione solo un libro enorme di frasi già scritte. La strategia più ingenua possibile: guarda l'ultima parola ("gatto") e conta, in tutto il libro, quale parola compare più spesso subito dopo. Magari è "dorme". Allora scommetti su "dorme".

Questo si chiama modello a **bigrammi**: guarda solo *una* parola indietro per indovinare la prossima. Puoi immaginarlo come un'agenda telefonica gigante: cerchi "gatto", e sotto trovi l'elenco di tutte le parole viste dopo "gatto" nel libro, con quante volte ciascuna è comparsa. Scegli la più frequente (o, per essere un po' più creativo, ne peschi una a caso pesando le probabilità in base a quante volte è comparsa).

Facciamolo concretamente, su un librone giocattolo di sole quattro frasi:

1. Il gatto dorme in poltrona.
2. Il gatto dorme tutto il giorno.
3. Il gatto gioca con il filo.
4. Il gatto miagola forte.

Cerchi "gatto" nella tua agenda e trovi: "dorme" comparsa 2 volte su 4 (50%), "gioca" 1 volta (25%), "miagola" 1 volta (25%). Scommetti su "dorme" — è la voce più affollata alla pagina "gatto". Con un libro vero, lungo milioni di frasi invece di quattro, il principio non cambia: conti, e scommetti sul più frequente.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="bigramma-title bigramma-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="bigramma-title">L'agenda alla pagina "gatto"</title>
  <desc id="bigramma-desc">Dopo la parola "gatto", tre parole candidate con le rispettive frequenze: dorme 50%, gioca 25%, miagola 25%.</desc>

  <text x="30" y="30" fill="#111" font-size="14" text-anchor="start">dopo "gatto" →</text>

  <g font-size="13">
    <text x="120" y="66" fill="#111" text-anchor="end">dorme</text>
    <rect x="130" y="52" width="220" height="28" fill="#2a7ae2" rx="3" />
    <text x="360" y="70" fill="#111">50%</text>

    <text x="120" y="116" fill="#111" text-anchor="end">gioca</text>
    <rect x="130" y="102" width="110" height="28" fill="#6fa8e8" rx="3" />
    <text x="250" y="120" fill="#111">25%</text>

    <text x="120" y="166" fill="#111" text-anchor="end">miagola</text>
    <rect x="130" y="152" width="110" height="28" fill="#6fa8e8" rx="3" />
    <text x="250" y="170" fill="#111">25%</text>
  </g>

  <text x="240" y="205" fill="#828282" font-size="11" text-anchor="middle">su un librone di sole quattro frasi</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La voce più affollata alla pagina "gatto" vince la scommessa.</figcaption>
</figure>

Il problema è evidente anche da questo minuscolo esempio: "gatto" da solo dice pochissimo. Sa che il gatto probabilmente fa qualcosa legato al riposo, ma non sa *quale* delle quattro frasi hai in mente — e nella vita reale "il gatto nero attraversò la strada" e "il gatto di Marco dorme tutto il giorno" hanno in comune solo "il gatto": dopo, vanno in direzioni completamente diverse. Puoi allargare la memoria a due, tre, quattro parole indietro (**trigrammi**, **quadrigrammi**...), e infatti per decenni — ben prima dei chatbot di oggi — i correttori automatici e i primi traduttori funzionavano più o meno così. Ma ogni parola di memoria in più fa esplodere il numero di combinazioni possibili da contare, e anche con quattro o cinque parole di contesto il modello resta "miope": non si ricorda l'inizio di un discorso lungo, non capisce a chi si riferisce un "lui" comparso tre frasi prima.

### 1.3 Un po' di memoria in più

Il passo successivo è stato costruire modelli capaci di portarsi dietro un "riassunto" di tutto quello che hanno letto finora, non solo delle ultime parole — una specie di bigliettino mentale che si aggiorna a ogni nuova parola letta, invece di dover rileggere tutto da capo ogni volta. Questi modelli (chiamati **reti neurali ricorrenti**) hanno funzionato meglio dei semplici contatori, ma avevano un difetto tipico della memoria umana quando è sotto sforzo: più il discorso si allungava, più il bigliettino mentale si "sporcava", perdendo dettagli importanti comparsi molte parole prima. Era un po' come cercare di ricordare la settima cosa di una lista della spesa dettata a voce, senza scriverla: le prime voci si confondono, le ultime restano fresche.

Prendi una frase come questa: *"Il computer che Sara aveva comprato tre anni prima, dopo mesi passati a confrontare prezzi e recensioni online, un giorno d'estate si è improvvisamente spento e non si è più riacceso."* Quando arriva a "si è... spento", il modello deve ancora ricordare che il soggetto è "il computer" — comparso più di venti parole prima, con tutta una frase incidentale in mezzo a distrarlo. Un modello a bigrammi non ha nemmeno la possibilità di provarci: guarda solo l'ultima parola. Una rete ricorrente ci prova, portandosi dietro il bigliettino aggiornato parola dopo parola — ma su frasi lunghe come questa il bigliettino tende ad arrivare a destinazione già un po' sbiadito, e il modello rischia di "dimenticare" chi si sia davvero spento.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="sbiadisce-title sbiadisce-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="sbiadisce-title">Il ricordo di "computer" si affievolisce</title>
  <desc id="sbiadisce-desc">La lunga frase sul computer di Sara divisa in sei tratti. Sopra ciascuno, un'eco della parola "computer" via via più sbiadita, fino a diventare quasi invisibile vicino a "si è spento".</desc>

  <g font-size="10" text-anchor="middle" fill="#1d5eb8" font-weight="bold">
    <text x="45" y="45" opacity="1">computer</text>
    <text x="140" y="45" opacity="0.75">computer</text>
    <text x="235" y="45" opacity="0.55">computer</text>
    <text x="330" y="45" opacity="0.35">computer</text>
    <text x="425" y="45" opacity="0.18">computer</text>
    <text x="510" y="45" opacity="0.06">computer</text>
  </g>

  <g fill="#fdfdfd" stroke="#828282" stroke-width="1.5">
    <rect x="10" y="60" width="70" height="60" rx="6" /><rect x="105" y="60" width="70" height="60" rx="6" />
    <rect x="200" y="60" width="70" height="60" rx="6" /><rect x="295" y="60" width="70" height="60" rx="6" />
    <rect x="390" y="60" width="70" height="60" rx="6" /><rect x="480" y="60" width="70" height="60" rx="6" />
  </g>
  <g font-size="9" text-anchor="middle" fill="#111">
    <text x="45" y="88">Il</text><text x="45" y="100">computer</text>
    <text x="140" y="88">che Sara</text><text x="140" y="100">comprato</text>
    <text x="235" y="88">tre anni</text><text x="235" y="100">prima...</text>
    <text x="330" y="88">prezzi e</text><text x="330" y="100">recensioni</text>
    <text x="425" y="88">un giorno</text><text x="425" y="100">d'estate</text>
    <text x="515" y="88">si è</text><text x="515" y="100">spento</text>
  </g>

  <text x="280" y="160" fill="#c85506" font-size="12" text-anchor="middle">il ricordo sbiadisce parola dopo parola</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Più di venti parole più tardi, il bigliettino arriva a destinazione già un po' sbiadito.</figcaption>
</figure>

Serviva un'idea diversa: invece di comprimere tutto in un unico bigliettino che si aggiorna parola per parola, perché non lasciare che il modello **torni a guardare**, ogni volta che gli serve, esattamente le parole di prima che contano di più — qualunque punto del discorso siano, vicine o lontane?

### 1.4 Il salto: guardare tutta la frase in una volta

Questa idea — chiamata **attenzione** — è la svolta di cui parleremo per tutto il resto del libro, a partire dalla prossima lezione. Per ora basti dire questo: invece di leggere una parola alla volta portandosi dietro solo un riassunto compresso, un modello con attenzione può, per ogni parola, "guardare indietro" a tutte le altre parole del testo contemporaneamente e decidere — parola per parola — quali contano di più per capire quella lì.

Torna alla frase del computer di Sara: con l'attenzione, quando il modello arriva a "si è spento" non deve sperare che l'informazione "il soggetto è il computer" sia sopravvissuta intatta dentro un bigliettino compresso per oltre venti parole — può letteralmente tornare a guardare "computer", ovunque si trovi nella frase, e ricollegarlo a "spento" in quel preciso istante, ignorando quasi del tutto le parole incidentali in mezzo ("prezzi", "recensioni", "estate") che c'entrano poco. Niente più memoria che sbiadisce: solo uno sguardo mirato, rifatto da zero ogni volta che serve. Nella Lezione 3 vedremo esattamente *come* il modello decide quali parole guardare.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="salto-title salto-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="salto-title">L'attenzione salta dritta al bersaglio</title>
  <desc id="salto-desc">La stessa frase sul computer di Sara. Una freccia diretta collega "si è spento" a "Il computer", scavalcando le parole incidentali di mezzo, che restano sullo sfondo perché poco rilevanti.</desc>

  <defs>
    <marker id="arrowSalto" markerWidth="8" markerHeight="8" refX="2" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M8,0 L0,4 L8,8 z" fill="#2a7ae2" /></marker>
  </defs>
  <path d="M 515,55 Q 280,10 45,55" fill="none" stroke="#2a7ae2" stroke-width="2.5" marker-end="url(#arrowSalto)" />

  <rect x="10" y="60" width="70" height="60" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
  <g fill="#fdfdfd" stroke="#e3e3e3" stroke-width="1.5" opacity="0.7">
    <rect x="105" y="60" width="70" height="60" rx="6" /><rect x="200" y="60" width="70" height="60" rx="6" />
    <rect x="295" y="60" width="70" height="60" rx="6" /><rect x="390" y="60" width="70" height="60" rx="6" />
  </g>
  <rect x="480" y="60" width="70" height="60" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />

  <g font-size="9" text-anchor="middle">
    <text x="45" y="88" fill="#111" font-weight="bold">Il</text><text x="45" y="100" fill="#111" font-weight="bold">computer</text>
    <text x="140" y="88" fill="#aaa">che Sara</text><text x="140" y="100" fill="#aaa">comprato</text>
    <text x="235" y="88" fill="#aaa">tre anni</text><text x="235" y="100" fill="#aaa">prima...</text>
    <text x="330" y="88" fill="#aaa">prezzi e</text><text x="330" y="100" fill="#aaa">recensioni</text>
    <text x="425" y="88" fill="#aaa">un giorno</text><text x="425" y="100" fill="#aaa">d'estate</text>
    <text x="515" y="88" fill="#111" font-weight="bold">si è</text><text x="515" y="100" fill="#111" font-weight="bold">spento</text>
  </g>

  <text x="280" y="160" fill="#1d5eb8" font-size="12" text-anchor="middle">un collegamento diretto, ignorando quasi del tutto le parole di mezzo</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Niente più memoria che sbiadisce: solo uno sguardo mirato, rifatto da zero ogni volta che serve.</figcaption>
</figure>

Nel 2017 un gruppo di ricercatori pubblicò un articolo con un titolo quasi provocatorio: *"L'attenzione è tutto ciò che serve"*. La loro proposta, chiamata **Transformer**, buttava via del tutto l'idea del bigliettino che si aggiorna parola per parola e costruiva un intero modello attorno alla sola attenzione. Si è rivelata una delle idee più influenti dell'informatica recente: quasi tutti i chatbot che usi oggi sono Transformer.

### 1.5 Più letture, più bravo

C'è un secondo ingrediente, altrettanto importante quanto l'architettura: la scala. Un Transformer piccolo, allenato su pochi testi, indovina la parola successiva poco meglio di un modello a bigrammi. Lo stesso Transformer, allenato su *miliardi* di pagine web, libri, articoli — e reso più grande, con più "manopole" interne da regolare — comincia a mostrare comportamenti che sui piccoli modelli semplicemente non esistevano: rispondere a domande mai viste, seguire istruzioni, persino abbozzare un ragionamento.

Chiedi a entrambi: *"Ho 3 mele, ne mangio una, quante me ne restano?"* Un Transformer piccolo, allenato su poco testo, ha visto troppo poche volte frasi simili per aver "capito" cosa significhi contare — probabilmente continuerà con qualcosa di plausibile a orecchio ma sbagliato, tipo "...quante me ne restano da comprare per la torta". Un Transformer molto più grande, allenato su una quantità di testo enorme — comprese innumerevoli spiegazioni di semplici sottrazioni — molto spesso risponde correttamente "2". Nessuno dei due sta davvero "facendo di conto" come faresti tu: entrambi indovinano ancora la parola più plausibile, un pezzetto alla volta. Ma su un modello abbastanza grande, "la parola più plausibile dopo aver letto miliardi di esempi di sottrazioni spiegate" finisce per coincidere, quasi sempre, con la risposta giusta. Torneremo su questo salto, che i ricercatori chiamano "scala", nella Lezione 4 e ne discuteremo criticamente nella Lezione 7 — perché non è tutto oro quel che luccica, ed è più sottile di quanto sembri.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="scala-title scala-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="scala-title">Piccolo contro grande</title>
  <desc id="scala-desc">Stessa domanda posta a due modelli. Il Transformer piccolo, con poche manopole interne, risponde in modo plausibile ma sbagliato. Il Transformer grande, con molte più manopole, risponde correttamente "2".</desc>

  <text x="110" y="30" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">Transformer piccolo</text>
  <rect x="40" y="45" width="140" height="90" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <g fill="#828282">
    <circle cx="65" cy="65" r="3" /><circle cx="95" cy="70" r="3" /><circle cx="130" cy="60" r="3" /><circle cx="155" cy="80" r="3" />
    <circle cx="70" cy="100" r="3" /><circle cx="110" cy="105" r="3" /><circle cx="145" cy="115" r="3" /><circle cx="60" cy="120" r="3" />
  </g>
  <text x="110" y="155" fill="#828282" font-size="10" text-anchor="middle">poche manopole</text>
  <text x="110" y="185" fill="#111" font-size="10" text-anchor="middle">"Ho 3 mele, ne mangio una...</text>
  <rect x="30" y="205" width="160" height="46" rx="6" fill="#f3f3f3" stroke="#828282" stroke-width="1.5" />
  <text x="110" y="225" fill="#555" font-size="10" text-anchor="middle">"...quante me ne restano</text>
  <text x="110" y="238" fill="#555" font-size="10" text-anchor="middle">da comprare per la torta" ✗</text>

  <text x="400" y="30" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">Transformer grande</text>
  <rect x="310" y="45" width="180" height="120" rx="8" fill="#fdfdfd" stroke="#2a7ae2" stroke-width="1.5" />
  <g fill="#2a7ae2">
    <circle cx="335" cy="62" r="3" /><circle cx="360" cy="70" r="3" /><circle cx="390" cy="58" r="3" /><circle cx="420" cy="66" r="3" /><circle cx="450" cy="60" r="3" /><circle cx="470" cy="75" r="3" />
    <circle cx="330" cy="90" r="3" /><circle cx="355" cy="95" r="3" /><circle cx="380" cy="88" r="3" /><circle cx="410" cy="98" r="3" /><circle cx="440" cy="92" r="3" /><circle cx="465" cy="100" r="3" />
    <circle cx="340" cy="118" r="3" /><circle cx="365" cy="125" r="3" /><circle cx="395" cy="115" r="3" /><circle cx="425" cy="122" r="3" /><circle cx="455" cy="118" r="3" /><circle cx="475" cy="130" r="3" />
    <circle cx="325" cy="145" r="3" /><circle cx="350" cy="150" r="3" /><circle cx="385" cy="145" r="3" /><circle cx="415" cy="150" r="3" /><circle cx="445" cy="145" r="3" /><circle cx="470" cy="150" r="3" />
  </g>
  <text x="400" y="185" fill="#1d5eb8" font-size="10" text-anchor="middle">molte manopole</text>
  <text x="400" y="185" fill="#111" font-size="10" text-anchor="middle" dy="14">"Ho 3 mele, ne mangio una...</text>
  <rect x="330" y="205" width="140" height="46" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="400" y="234" fill="#111" font-size="20" font-weight="bold" text-anchor="middle">2 ✓</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Nessuno dei due "fa di conto": ma su un modello grande, la parola più plausibile coincide quasi sempre con quella giusta.</figcaption>
</figure>

Da qui in avanti, ogni volta che pensi a un LLM, tienilo a mente: un motore che indovina la parola successiva, così bravo — perché guarda tutto il contesto con attenzione, e perché ha letto una quantità di testo che nessun umano potrebbe leggere in mille vite — da sembrare capace di conversare.

---

> **Prova tu — Il modello a bigrammi improvvisato**
>
> Prendi questo minuscolo "libro" di quattro frasi già scritte:
>
> 1. Il cane corre nel parco.
> 2. Il cane dorme sul divano.
> 3. Il gatto dorme sul tappeto.
> 4. Il gatto guarda la finestra.
>
> Ora, **senza guardare oltre**, prova a fare da modello a bigrammi: data solo l'ultima parola, quale parola scommetteresti venga dopo?
>
> - Dopo "Il", quale parola è più frequente tra le quattro frasi?
> - Dopo "cane", che succede — puoi prevedere con sicurezza cosa viene dopo, o le frasi 1 e 2 vanno in direzioni diverse?
> - Dopo "dorme", stessa domanda: riesci a indovinare "sul divano" o "sul tappeto" guardando solo "dorme"?
>
> Prova a scrivere le tue tre risposte, poi confrontale con il ragionamento in Appendice A. Il punto dell'esercizio non è "sbagliare o no" — è sentire sulla tua pelle *quanta* informazione perdi guardando indietro una parola sola.

---

*Continua con la [Lezione 02 — Come una macchina legge le parole]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-02-come-una-macchina-legge-le-parole.md %}.html)*
