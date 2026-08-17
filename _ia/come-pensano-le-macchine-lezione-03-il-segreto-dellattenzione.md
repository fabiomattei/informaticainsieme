---
title: 'Lezione 03 — Il segreto dell''attenzione'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Frecce di spessore diverso dalla parola gli verso le parole precedenti della frase, a indicare quanto le ascolta](/images/ia/come-pensano-le-macchine-lezione-03-il-segreto-dellattenzione/come-pensano-le-macchine-lezione-03-il-segreto-dellattenzione.svg){:class="aside-image"}

### 3.1 A chi presti attenzione mentre leggi

Leggi questa frase: *"Marco ha detto a Luca che gli avrebbe prestato il libro."*

A chi si riferisce "gli"? Grammaticalmente potrebbe essere sia Marco sia Luca — ma tu, leggendo, probabilmente hai già deciso (quasi certamente "a Luca", perché ha senso che Marco presti *a lui*). Non l'hai deciso guardando "gli" isolato: l'hai deciso confrontandolo, in un istante, con "Marco", con "Luca", con "prestato" — pesando mentalmente quali parole della frase ti servono per sciogliere quell'ambiguità.

Per capire quanto sia sottile questo lavoro, prova un secondo esempio, dove basta cambiare *una sola parola* per ribaltare completamente la risposta:

- *"Il trofeo non entrava nella valigia perché era troppo **grande**."* A chi si riferisce "era"? Al trofeo.
- *"Il trofeo non entrava nella valigia perché era troppo **piccola**."* E ora? Alla valigia.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 660 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="trofeo-title trofeo-desc" style="width: 100%; max-width: 600px; height: auto; font-family: inherit;">
  <title id="trofeo-title">Una sola parola ribalta il riferimento</title>
  <desc id="trofeo-desc">Due varianti della stessa frase: con "grande" la freccia punta a trofeo, con "piccola" la freccia punta a valigia.</desc>

  <defs>
    <marker id="arrowO" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse">
      <path d="M0,0 L8,4 L0,8 z" fill="#f66a0a" />
    </marker>
  </defs>

  <path d="M 520,70 Q 310,15 95,70" fill="none" stroke="#f66a0a" stroke-width="3" marker-end="url(#arrowO)" />
  <rect x="40" y="70" width="110" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="95" y="100" fill="#111" font-size="14" text-anchor="middle">trofeo</text>
  <rect x="230" y="70" width="110" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="285" y="100" fill="#111" font-size="14" text-anchor="middle">valigia</text>
  <rect x="420" y="70" width="200" height="50" rx="8" fill="#fdfdfd" stroke="#f66a0a" stroke-width="1.5" />
  <text x="520" y="90" fill="#111" font-size="12" text-anchor="middle">...troppo</text>
  <text x="520" y="107" fill="#f66a0a" font-size="14" font-weight="bold" text-anchor="middle">grande</text>

  <path d="M 520,210 Q 405,265 285,210" fill="none" stroke="#f66a0a" stroke-width="3" marker-end="url(#arrowO)" />
  <rect x="40" y="210" width="110" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="95" y="240" fill="#111" font-size="14" text-anchor="middle">trofeo</text>
  <rect x="230" y="210" width="110" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="285" y="240" fill="#111" font-size="14" text-anchor="middle">valigia</text>
  <rect x="420" y="210" width="200" height="50" rx="8" fill="#fdfdfd" stroke="#f66a0a" stroke-width="1.5" />
  <text x="520" y="230" fill="#111" font-size="12" text-anchor="middle">...troppo</text>
  <text x="520" y="247" fill="#f66a0a" font-size="14" font-weight="bold" text-anchor="middle">piccola</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Stessa frase, una sola parola cambiata: il riferimento di "era" si ribalta da trofeo a valigia.</figcaption>
</figure>

"Era" occupa esattamente la stessa posizione in entrambe le frasi — la terza parola dalla fine, se conti a ritroso. Se un lettore, o un programma, decidesse a chi si riferisce "era" guardando solo *quante parole indietro* si trova il candidato più probabile — una regola fissa tipo "guarda sempre due parole prima" — sbaglierebbe una delle due frasi di sicuro, perché la posizione da guardare non cambia mai, ma il *significato* dell'ultima parola sì. Contare le parole non basta: bisogna capire cosa dicono, e aggiustare il tiro frase per frase.

Questo — guardare le altre parole di un testo e decidere quanto ciascuna conta, in base al *contenuto* e non solo alla posizione, per capire quella che hai davanti — è esattamente ciò che fa il meccanismo di **attenzione**, il cuore di tutti i modelli linguistici moderni (i cosiddetti *Transformer*, di cui abbiamo parlato nella Lezione 1). Ricordi il "numeretto di posizione" cucito a ogni parola, di cui parlavamo alla fine della Lezione 2? È proprio qui che entra in gioco davvero: l'attenzione lo usa *insieme* al significato di ogni parola, mai al posto suo — è per questo che una regola basata sulla sola posizione, come quella appena vista, non basta mai da sola. La buona notizia è che l'idea, spogliata della matematica, resta semplice come questi due esempi.

### 3.2 Un cartellino per ogni parola

Immagina un'aula durante un lavoro di gruppo. Ogni studente porta appeso al collo un cartellino che descrive **cosa può offrire** ("so risolvere equazioni", "conosco bene la storia romana", "ho la penna rossa").

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 170" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="cartellini-title cartellini-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="cartellini-title">Tre cartellini appesi al collo</title>
  <desc id="cartellini-desc">Tre cartellini rettangolari appesi a un laccetto, ciascuno con scritto cosa lo studente sa offrire: "so risolvere equazioni", "conosco bene la storia romana", "ho la penna rossa".</desc>

  <g stroke="#828282" stroke-width="2">
    <line x1="100" y1="0" x2="100" y2="30" />
    <line x1="280" y1="0" x2="280" y2="30" />
    <line x1="460" y1="0" x2="460" y2="30" />
  </g>
  <g fill="#828282">
    <circle cx="100" cy="15" r="5" />
    <circle cx="280" cy="15" r="5" />
    <circle cx="460" cy="15" r="5" />
  </g>

  <g fill="#fdfdfd" stroke="#2a7ae2" stroke-width="2">
    <rect x="20" y="30" width="160" height="110" rx="10" />
    <rect x="200" y="30" width="160" height="110" rx="10" />
    <rect x="380" y="30" width="160" height="110" rx="10" />
  </g>
  <g fill="none" stroke="#828282" stroke-width="1.5">
    <circle cx="100" cy="45" r="4" fill="#fdfdfd" />
    <circle cx="280" cy="45" r="4" fill="#fdfdfd" />
    <circle cx="460" cy="45" r="4" fill="#fdfdfd" />
  </g>

  <g fill="#111" font-size="15" text-anchor="middle">
    <text x="100" y="80">so risolvere</text>
    <text x="100" y="102">equazioni</text>

    <text x="280" y="80">conosco bene la</text>
    <text x="280" y="102">storia romana</text>

    <text x="460" y="80">ho la penna</text>
    <text x="460" y="102">rossa</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni studente porta un cartellino che descrive cosa può offrire al gruppo.</figcaption>
</figure>

Quando uno studente ha un dubbio, si fa mentalmente una **domanda** ("mi serve aiuto con un'equazione") e scorre con lo sguardo i cartellini in giro per la stanza, decidendo — in base a quanto ogni cartellino risponde alla sua domanda — quanto ascoltare ciascun compagno. Non è un sì/no secco: magari ascolta per l'80% chi ha scritto "so risolvere equazioni" e per il 20% chi ha scritto qualcos'altro di vagamente utile, ignorando quasi del tutto chi ha solo la penna rossa.

Un modello con attenzione fa esattamente questo, per ogni singola parola del testo, contemporaneamente:

- ogni parola si pone una **domanda** ("di cosa ho bisogno per essere capita meglio?"),
- ogni parola espone un **cartellino** che descrive cosa offre,
- e ogni parola, confrontando la propria domanda con i cartellini di tutte le altre, decide quanto "ascoltare" ciascuna — un pizzico da questa, tanto da quella, pochissimo da un'altra ancora.

Proviamo a mettere numeri plausibili (inventati, ma realistici) sulla frase di apertura. Quando il modello elabora "gli", la sua domanda assomiglia a "chi ha appena ricevuto un'azione, in questa frase?". Confrontandola con i cartellini delle parole già lette, potrebbe arrivare a una ripartizione di questo tipo:

| Parola ascoltata | Quanto "gli" la ascolta | Perché (il "cartellino" di quella parola) |
|---|---|---|
| Luca | 60% | "sono il destinatario del verbo 'detto'" |
| prestato | 25% | "sono l'azione futura di cui si parla" |
| Marco | 10% | "sono chi parla" |
| a, che, ha | 5% in tutto | poco rilevanti per la domanda di "gli" |

Il risultato, per "gli", non è più il suo significato isolato, ma una miscela pesata secondo queste percentuali: un po' di sé stesso, molto di "Luca", un bel po' di "prestato", pochissimo del resto. Dopo essere passato attraverso questo meccanismo, "gli" porta con sé — mischiate nella sua rappresentazione numerica — tracce forti di "Luca" e di "prestato", e tracce deboli di "Marco". Il bello è che nessuno scrive a mano le regole per decidere chi ascoltare, né tantomeno le percentuali esatte: il modello impara, allenandosi su miliardi di frasi, a costruire domande e cartellini tali che l'ascolto giusto emerga da solo.

### 3.3 Non si può sbirciare il futuro

C'è una regola in più, e ha una logica ferrea: quando il modello sta cercando di indovinare la parola numero 10 di una frase, non può "ascoltare" le parole 11, 12, 13 — perché, semplicemente, al momento di indovinare non esistono ancora. È un po' come leggere un giallo: puoi rileggere quante volte vuoi le pagine già lette, ma non puoi sbirciare l'ultima pagina per scoprire chi è l'assassino mentre sei ancora alla lezione 3 — sarebbe imbrogliare, e soprattutto non ti insegnerebbe a *dedurre* nulla. Per questo, ogni parola può ascoltare solo sé stessa e le parole che la precedono, mai quelle che vengono dopo. Questo vincolo si chiama **mascheramento causale**, e serve anche a un secondo scopo pratico: permette al modello, durante l'addestramento, di esercitarsi a indovinare *ogni* parola di un intero testo contemporaneamente — un'unica lettura che allena simultaneamente la previsione della parola 2, della parola 3, della parola 4, e così via — invece di dover rileggere il testo da capo una volta per ogni parola da indovinare.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 440 460" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="maschera-title maschera-desc" style="width: 100%; max-width: 400px; height: auto; font-family: inherit;">
  <title id="maschera-title">Matrice del mascheramento causale</title>
  <desc id="maschera-desc">Griglia 5×5: riga = parola che guarda, colonna = parola osservata. Le celle sotto la diagonale (parole precedenti) sono consentite, la diagonale è l'auto-attenzione, le celle sopra la diagonale (parole future) sono bloccate.</desc>

  <defs>
    <pattern id="hatch" width="6" height="6" patternTransform="rotate(45)" patternUnits="userSpaceOnUse">
      <rect width="6" height="6" fill="#f3f3f3" />
      <line x1="0" y1="0" x2="0" y2="6" stroke="#cfcfcf" stroke-width="2" />
    </pattern>
  </defs>

  <text x="220" y="16" fill="#111" font-size="13" text-anchor="middle">colonna = parola osservata</text>
  <text x="14" y="220" fill="#111" font-size="13" text-anchor="middle" transform="rotate(-90 14 220)">riga = parola che guarda</text>

  <!-- etichette colonne -->
  <g fill="#111" font-size="12">
    <text x="157" y="70" text-anchor="end" transform="rotate(-40 157 70)">Marco</text>
    <text x="211" y="70" text-anchor="end" transform="rotate(-40 211 70)">ha</text>
    <text x="265" y="70" text-anchor="end" transform="rotate(-40 265 70)">detto</text>
    <text x="319" y="70" text-anchor="end" transform="rotate(-40 319 70)">a</text>
    <text x="373" y="70" text-anchor="end" transform="rotate(-40 373 70)">Luca</text>
  </g>
  <!-- etichette righe -->
  <g fill="#111" font-size="12" text-anchor="end">
    <text x="120" y="112">Marco</text>
    <text x="120" y="166">ha</text>
    <text x="120" y="220">detto</text>
    <text x="120" y="274">a</text>
    <text x="120" y="328">Luca</text>
  </g>

  <!-- riga Marco (i=0): solo sé stessa -->
  <rect x="130" y="80" width="54" height="54" fill="#2a7ae2" stroke="#fdfdfd" stroke-width="2" />
  <rect x="184" y="80" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />
  <rect x="238" y="80" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />
  <rect x="292" y="80" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />
  <rect x="346" y="80" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />

  <!-- riga ha (i=1) -->
  <rect x="130" y="134" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="184" y="134" width="54" height="54" fill="#2a7ae2" stroke="#fdfdfd" stroke-width="2" />
  <rect x="238" y="134" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />
  <rect x="292" y="134" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />
  <rect x="346" y="134" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />

  <!-- riga detto (i=2) -->
  <rect x="130" y="188" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="184" y="188" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="238" y="188" width="54" height="54" fill="#2a7ae2" stroke="#fdfdfd" stroke-width="2" />
  <rect x="292" y="188" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />
  <rect x="346" y="188" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />

  <!-- riga a (i=3) -->
  <rect x="130" y="242" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="184" y="242" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="238" y="242" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="292" y="242" width="54" height="54" fill="#2a7ae2" stroke="#fdfdfd" stroke-width="2" />
  <rect x="346" y="242" width="54" height="54" fill="url(#hatch)" stroke="#fdfdfd" stroke-width="2" />

  <!-- riga Luca (i=4) -->
  <rect x="130" y="296" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="184" y="296" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="238" y="296" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="292" y="296" width="54" height="54" fill="#dceafc" stroke="#fdfdfd" stroke-width="2" />
  <rect x="346" y="296" width="54" height="54" fill="#2a7ae2" stroke="#fdfdfd" stroke-width="2" />

  <!-- legenda -->
  <g font-size="13" fill="#111">
    <rect x="130" y="380" width="20" height="20" fill="#dceafc" stroke="#828282" />
    <text x="158" y="395">consentito (parola precedente)</text>
    <rect x="130" y="408" width="20" height="20" fill="#2a7ae2" />
    <text x="158" y="423">sé stessa</text>
    <rect x="130" y="436" width="20" height="20" fill="url(#hatch)" stroke="#828282" />
    <text x="158" y="451">bloccato (parola futura)</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni riga può "vedere" solo le celle a sinistra della diagonale: sé stessa e le parole già lette, mai quelle future.</figcaption>
</figure>

### 3.4 Più occhi valgono più di uno

Un solo giro di "domanda e cartellini" per parola sarebbe piuttosto limitante: nella frase di prima, servirebbe capire contemporaneamente *chi fa cosa* (Marco presta), *a chi* (a Luca), e *di cosa si parla* (il libro) — relazioni diverse, che convivono nella stessa frase. La soluzione è far girare **più meccanismi di attenzione in parallelo**, ciascuno con le proprie domande e i propri cartellini, un po' come guardare la stessa scena attraverso più lenti colorate diverse contemporaneamente: una lente evidenzia "chi fa l'azione", un'altra "a chi è diretta", un'altra ancora "di cosa si parla materialmente".

Torniamo all'esempio del trofeo e della valigia per vedere perché serve più di una lente insieme. Da sola, una lente puramente grammaticale ("qual è il soggetto della frase precedente?") non basta a scegliere tra "trofeo" e "valigia": entrambi sono candidati sintatticamente validi. Serve una seconda lente, che confronta il significato dell'aggettivo finale con le proprietà tipiche degli oggetti coinvolti — una valigia, di solito, è quella che *contiene*; un trofeo è quello che *viene contenuto*. È questa seconda lente a far pendere la bilancia: "grande" punta al trofeo, "piccola" punta alla valigia. Nessuna delle due lenti, da sola, risolverebbe sempre il problema — è la combinazione a farlo.

Ognuna di queste "teste" di attenzione, allenandosi, tende a specializzarsi per conto suo su un tipo di relazione — senza che nessuno gliel'abbia detto esplicitamente — e alla fine i risultati di tutte le teste vengono ricomposti insieme in un'unica rappresentazione più ricca.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 580 340" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="teste-title teste-desc" style="width: 100%; max-width: 560px; height: auto; font-family: inherit;">
  <title id="teste-title">Tre teste di attenzione, tre domande diverse</title>
  <desc id="teste-desc">La stessa frase esaminata da tre teste di attenzione diverse: una guarda al soggetto (Marco), una al destinatario (Luca), una all'azione (detto). Ogni testa disegna una freccia diversa da "gli".</desc>

  <defs>
    <marker id="arrowBlue" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#2a7ae2" /></marker>
    <marker id="arrowOrange" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#f66a0a" /></marker>
    <marker id="arrowGreen" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#3aa655" /></marker>
  </defs>

  <!-- Testa 1: soggetto (blu) -->
  <text x="10" y="45" fill="#2a7ae2" font-size="13" font-weight="bold">Testa 1</text>
  <text x="10" y="60" fill="#2a7ae2" font-size="11">soggetto</text>
  <path d="M 530,20 Q 335,-15 140,20" fill="none" stroke="#2a7ae2" stroke-width="3" marker-end="url(#arrowBlue)" />
  <g font-size="12" text-anchor="middle">
    <rect x="110" y="50" width="60" height="40" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" /><text x="140" y="75" fill="#111">Marco</text>
    <rect x="175" y="50" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="205" y="75" fill="#111">ha</text>
    <rect x="240" y="50" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="270" y="75" fill="#111">detto</text>
    <rect x="305" y="50" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="335" y="75" fill="#111">a</text>
    <rect x="370" y="50" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="400" y="75" fill="#111">Luca</text>
    <rect x="435" y="50" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="465" y="75" fill="#111">che</text>
    <rect x="500" y="50" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#2a7ae2" stroke-width="2.5" /><text x="530" y="75" fill="#2a7ae2" font-weight="bold">gli</text>
  </g>

  <!-- Testa 2: destinatario (arancio) -->
  <text x="10" y="150" fill="#f66a0a" font-size="13" font-weight="bold">Testa 2</text>
  <text x="10" y="165" fill="#f66a0a" font-size="11">destinatario</text>
  <path d="M 530,125 Q 465,90 400,125" fill="none" stroke="#f66a0a" stroke-width="3" marker-end="url(#arrowOrange)" />
  <g font-size="12" text-anchor="middle">
    <rect x="110" y="155" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="140" y="180" fill="#111">Marco</text>
    <rect x="175" y="155" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="205" y="180" fill="#111">ha</text>
    <rect x="240" y="155" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="270" y="180" fill="#111">detto</text>
    <rect x="305" y="155" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="335" y="180" fill="#111">a</text>
    <rect x="370" y="155" width="60" height="40" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="400" y="180" fill="#111">Luca</text>
    <rect x="435" y="155" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="465" y="180" fill="#111">che</text>
    <rect x="500" y="155" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#f66a0a" stroke-width="2.5" /><text x="530" y="180" fill="#f66a0a" font-weight="bold">gli</text>
  </g>

  <!-- Testa 3: azione (verde) -->
  <text x="10" y="255" fill="#3aa655" font-size="13" font-weight="bold">Testa 3</text>
  <text x="10" y="270" fill="#3aa655" font-size="11">azione</text>
  <path d="M 530,230 Q 400,195 270,230" fill="none" stroke="#3aa655" stroke-width="3" marker-end="url(#arrowGreen)" />
  <g font-size="12" text-anchor="middle">
    <rect x="110" y="260" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="140" y="285" fill="#111">Marco</text>
    <rect x="175" y="260" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="205" y="285" fill="#111">ha</text>
    <rect x="240" y="260" width="60" height="40" rx="6" fill="#dcf3e4" stroke="#3aa655" stroke-width="1.5" /><text x="270" y="285" fill="#111">detto</text>
    <rect x="305" y="260" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="335" y="285" fill="#111">a</text>
    <rect x="370" y="260" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="400" y="285" fill="#111">Luca</text>
    <rect x="435" y="260" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="465" y="285" fill="#111">che</text>
    <rect x="500" y="260" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#3aa655" stroke-width="2.5" /><text x="530" y="285" fill="#3aa655" font-weight="bold">gli</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Stessa frase, stessa parola "gli": tre teste con tre domande diverse pesano parole diverse.</figcaption>
</figure>

### 3.5 Uno strato sopra l'altro

Un modello reale non fa questo "guarda e ascolta" una volta sola: lo ripete decine o addirittura centinaia di volte, uno strato sopra l'altro, ciascuno seguito da un breve momento di elaborazione interna della singola parola (di cui non ci occuperemo nel dettaglio qui). Pensa a come rileggi un tuo tema prima di consegnarlo: al primo giro correggi refusi e piccoli errori di concordanza tra parole vicine; a un giro successivo controlli che ogni pronome si riferisca davvero a chi deve; solo all'ultimo giro valuti se l'argomento nel complesso si tiene, e se il tono resta coerente dall'inizio alla fine. Sono passate diverse, ognuna più "larga" della precedente.

Un modello fa qualcosa di simile, strato dopo strato, contemporaneamente su ogni parola del testo: a ogni strato la rappresentazione di ogni parola si arricchisce un altro po'. I primi strati tendono a cogliere relazioni semplici e vicine (che parola precede quale); gli strati più profondi arrivano a catturare relazioni astratte e a lungo raggio — tono del discorso, intenzione, coerenza con qualcosa detto molte frasi prima. Nessuno di questi livelli è programmato a mano: è tutto il prodotto dello stesso identico meccanismo di attenzione, ripetuto e impilato su scala enorme.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 400" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="strati-title strati-desc" style="width: 100%; max-width: 420px; height: auto; font-family: inherit;">
  <title id="strati-title">Quattro strati impilati</title>
  <desc id="strati-desc">Uno stack di quattro strati, dal basso verso l'alto: relazioni vicine, legami grammaticali, riferimenti, senso globale. A sinistra, il token "gli" si arricchisce salendo di strato in strato, da un'ipotesi incerta a un riferimento chiaro a Luca.</desc>

  <defs>
    <marker id="arrowUp" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <!-- frecce di collegamento tra gli strati -->
  <g stroke="#828282" stroke-width="2" marker-end="url(#arrowUp)">
    <line x1="290" y1="320" x2="290" y2="282" />
    <line x1="290" y1="220" x2="290" y2="182" />
    <line x1="290" y1="120" x2="290" y2="82" />
  </g>

  <!-- strato 1 (basso) -->
  <rect x="140" y="320" width="300" height="60" rx="8" fill="#eef5fd" stroke="#828282" stroke-width="1.5" />
  <text x="155" y="345" fill="#111" font-size="13" font-weight="bold">Strato 1 — relazioni vicine</text>
  <text x="155" y="363" fill="#555" font-size="12">che parola precede quale</text>

  <!-- strato 2 -->
  <rect x="140" y="220" width="300" height="60" rx="8" fill="#cfe4fb" stroke="#828282" stroke-width="1.5" />
  <text x="155" y="245" fill="#111" font-size="13" font-weight="bold">Strato 2 — legami grammaticali</text>
  <text x="155" y="263" fill="#555" font-size="12">soggetto, oggetto, concordanze</text>

  <!-- strato 3 -->
  <rect x="140" y="120" width="300" height="60" rx="8" fill="#9dc7f5" stroke="#828282" stroke-width="1.5" />
  <text x="155" y="145" fill="#111" font-size="13" font-weight="bold">Strato 3 — riferimenti</text>
  <text x="155" y="163" fill="#333" font-size="12">a chi punta un pronome</text>

  <!-- strato 4 (alto) -->
  <rect x="140" y="20" width="300" height="60" rx="8" fill="#2a7ae2" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="155" y="45" fill="#fdfdfd" font-size="13" font-weight="bold">Strato 4 — senso globale</text>
  <text x="155" y="63" fill="#eaf2fd" font-size="12">tono, coerenza, contesto ampio</text>

  <!-- token "gli" che si arricchisce salendo -->
  <g stroke="#828282" stroke-width="1.5" marker-end="url(#arrowUp)">
    <line x1="60" y1="335" x2="60" y2="267" />
    <line x1="60" y1="235" x2="60" y2="167" />
    <line x1="60" y1="135" x2="60" y2="67" />
  </g>
  <rect x="25" y="335" width="70" height="30" rx="15" fill="#eef5fd" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="60" y="355" fill="#111" font-size="13" text-anchor="middle">gli?</text>
  <rect x="25" y="235" width="70" height="30" rx="15" fill="#cfe4fb" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="60" y="255" fill="#111" font-size="12" text-anchor="middle">forse Luca</text>
  <rect x="25" y="135" width="70" height="30" rx="15" fill="#9dc7f5" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="60" y="155" fill="#111" font-size="12" text-anchor="middle">→ Luca</text>
  <rect x="25" y="35" width="70" height="30" rx="15" fill="#2a7ae2" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="60" y="55" fill="#fdfdfd" font-size="12" font-weight="bold" text-anchor="middle">→ Luca ✓</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Salendo di strato in strato, la rappresentazione di "gli" si arricchisce, da ipotesi incerta a riferimento chiaro.</figcaption>
</figure>

### 3.6 Mettiamo tutto insieme

Riavvolgiamo il nastro e seguiamo, dall'inizio alla fine, cosa succede a una sola parola — "gli" — attraverso l'intero processo descritto in questa lezione, così le sezioni precedenti smettono di essere idee separate e diventano un unico meccanismo.

1. **Domanda e cartellini (3.2).** "Gli" si pone una domanda ("chi ha appena ricevuto l'azione?") e la confronta con i cartellini di "Marco", "Luca", "prestato" e delle altre parole già lette, ottenendo una miscela pesata verso "Luca" e "prestato".
2. **Il vincolo del tempo (3.3).** Il confronto riguarda solo le parole *già lette* fino a quel momento: "libro", che arriva dopo, non esiste ancora per "gli" mentre il modello lo elabora.
3. **Più punti di vista insieme (3.4).** Il confronto non avviene una volta sola: più "teste" lo fanno in parallelo, ognuna con la propria domanda — una magari si concentra su "chi è il destinatario", un'altra su "qual è l'azione", una terza su qualcosa a cui noi umani non penseremmo nemmeno, ma che il modello ha scoperto essere utile.
4. **Ripetuto in profondità (3.5).** Tutto questo — non una volta, ma decine o centinaia di volte, strato sopra strato — raffina progressivamente la rappresentazione di "gli", fino a che, all'ultimo strato, il modello ha effettivamente "capito" (nel senso puramente statistico che userà per generare la parola successiva) che quel "gli" punta a Luca.

Nessuno di questi quattro passaggi è stato scritto a mano da un programmatore: sono tutti il prodotto di ciò che il modello ha imparato, macinando testo, su come distribuire l'attenzione in modo utile a indovinare la parola successiva. È proprio questa combinazione — domande e cartellini, uno sguardo solo al passato, più teste in parallelo, ripetuta strato dopo strato — a rendere possibile tutto ciò che vedremo nelle prossime lezioni: come si allena un modello di questo tipo (Lezione 4) e come, alla fine, genera davvero una risposta parola per parola (Lezione 6).

---

Ecco uno schema di partenza per il punto 1 dell'esercizio: la frase divisa in caselle, una per parola, con "gli" evidenziata. Disegna (su carta, o mentalmente) le frecce da "gli" verso le altre caselle, ricordando che non può puntare a "libro".

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 800 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="caselle-title caselle-desc" style="width: 100%; max-width: 720px; height: auto; font-family: inherit;">
  <title id="caselle-title">La frase divisa in una casella per parola</title>
  <desc id="caselle-desc">Undici caselle in fila, una per parola: Marco, ha, detto, a, Luca, che, gli, avrebbe, prestato, il, libro. La casella "gli" è evidenziata; sopra la fila c'è spazio libero per disegnare le frecce dell'attenzione.</desc>

  <g font-size="13" text-anchor="middle">
    <g>
      <rect x="20" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="51" y="170" fill="#111">Marco</text>
    </g>
    <g>
      <rect x="88" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="119" y="170" fill="#111">ha</text>
    </g>
    <g>
      <rect x="156" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="187" y="170" fill="#111">detto</text>
    </g>
    <g>
      <rect x="224" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="255" y="170" fill="#111">a</text>
    </g>
    <g>
      <rect x="292" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="323" y="170" fill="#111">Luca</text>
    </g>
    <g>
      <rect x="360" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="391" y="170" fill="#111">che</text>
    </g>
    <g>
      <rect x="428" y="140" width="62" height="50" rx="8" fill="#2a7ae2" stroke="#2a7ae2" stroke-width="1.5" />
      <text x="459" y="170" fill="#fdfdfd" font-weight="bold">gli</text>
    </g>
    <g>
      <rect x="496" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="527" y="170" fill="#111">avrebbe</text>
    </g>
    <g>
      <rect x="564" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="595" y="170" fill="#111">prestato</text>
    </g>
    <g>
      <rect x="632" y="140" width="62" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
      <text x="663" y="170" fill="#111">il</text>
    </g>
    <g>
      <rect x="700" y="140" width="62" height="50" rx="8" fill="#f3f3f3" stroke="#828282" stroke-width="1.5" stroke-dasharray="3 3" />
      <text x="731" y="170" fill="#828282">libro</text>
    </g>
  </g>

  <text x="459" y="30" fill="#828282" font-size="13" text-anchor="middle">(spazio per le tue frecce, da "gli" verso le caselle precedenti)</text>
  <text x="731" y="115" fill="#828282" font-size="20" text-anchor="middle">🚫</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">"gli" è evidenziata; "libro" è tratteggiata perché il mascheramento causale non permette di puntare lì.</figcaption>
</figure>

> **Prova tu — Disegna le frecce dell'attenzione**
>
> Ecco la frase di apertura della lezione: *"Marco ha detto a Luca che gli avrebbe prestato il libro."*
>
> 1. Scrivi la frase su un foglio, una parola per casella. Concentrati sulla parola "gli". Disegna delle frecce da "gli" verso le altre parole della frase, spesse in proporzione a quanto pensi che "gli" debba "ascoltarle" per essere interpretata correttamente. (Ricorda la regola del mascheramento causale: "gli" può guardare solo sé stessa e le parole *precedenti* — non "libro", che viene dopo.)
> 2. Ora prova con una seconda "testa" di attenzione: invece di chiederti "a chi si riferisce gli", chiediti "qual è l'azione principale della frase" e ridisegna le frecce da "gli" pensando a questa domanda diversa. Le frecce più spesse cambiano?
> 3. Prova con questa frase più ambigua, senza una risposta ovvia: *"Il professore ha restituito il compito allo studente perché era sbagliato."* A cosa si riferisce "era sbagliato" — al compito o al professore che l'ha corretto? Disegna le frecce secondo la tua interpretazione più naturale, poi confrontati con il ragionamento in Appendice A.
>
> Non esiste un'unica risposta "corretta" al 100% per il punto 3 — è proprio per questo che è un buon esempio: anche i modelli reali, di fronte a frasi ambigue, a volte "sbagliano" a distribuire l'attenzione, esattamente come farebbe un lettore umano distratto.

---

*Continua con la [Lezione 04 — Come si allena un gigante]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-04-come-si-allena-un-gigante.md %}.html)*
