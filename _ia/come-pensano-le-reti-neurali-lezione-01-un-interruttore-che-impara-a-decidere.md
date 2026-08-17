---
title: 'Lezione 01 — Un interruttore che impara a decidere'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Tre ingressi pesati che convergono in un neurone e producono una decisione binaria](/images/ia/come-pensano-le-reti-neurali-lezione-01-un-interruttore-che-impara-a-decidere/come-pensano-le-reti-neurali-lezione-01-un-interruttore-che-impara-a-decidere.svg){:class="aside-image"}

### 1.1 Un interruttore con un'opinione

Immagina un interruttore un po' speciale. Non si limita ad accendere o spegnere una luce a comando: guarda alcuni indizi, li soppesa, e decide da solo se "accendersi" oppure no. Facciamo un esempio concreto: devi decidere se prendere l'ombrello uscendo di casa. Guardi due indizi — quanto è nuvoloso il cielo (da 0 a 10) e quanto è umida l'aria (da 0 a 10) — e ti fai un'idea sommandoli, ma non allo stesso modo: magari il cielo nuvoloso conta doppio rispetto all'umidità, perché è l'indizio più affidabile. Se il totale pesato supera una certa soglia, prendi l'ombrello; altrimenti no.

Questo è, in sostanza, il primo modello matematico di un neurone artificiale, proposto nel 1943 da Warren McCulloch e Walter Pitts: un'unità che somma segnali in ingresso, ciascuno con la propria importanza, e "scatta" se la somma supera una soglia. All'epoca i pesi — quanto contava ogni indizio — venivano scelti a mano da chi costruiva il modello. Mancava ancora l'ingrediente più interessante: far sì che l'interruttore imparasse da solo quanto pesare ogni indizio.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 600 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="percettrone-title percettrone-desc" style="width: 100%; max-width: 560px; height: auto; font-family: inherit;">
  <title id="percettrone-title">L'interruttore-ombrello</title>
  <desc id="percettrone-desc">Due indizi, cielo nuvoloso e umidità, entrano pesati (per 2 e per 1) in una somma. Se la somma supera 15, l'output è "sì ombrello", altrimenti "no".</desc>

  <defs>
    <marker id="arrowPerc" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="20" y="40" width="110" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="75" y="60" fill="#111" font-size="11" text-anchor="middle">Cielo nuvoloso</text>
  <text x="75" y="76" fill="#111" font-size="11" text-anchor="middle">(C)</text>

  <rect x="20" y="170" width="110" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="75" y="190" fill="#111" font-size="11" text-anchor="middle">Umidità</text>
  <text x="75" y="206" fill="#111" font-size="11" text-anchor="middle">(U)</text>

  <path d="M 130,70 L 200,120" fill="none" stroke="#2a7ae2" stroke-width="2" marker-end="url(#arrowPerc)" />
  <text x="155" y="90" fill="#1d5eb8" font-size="12" font-weight="bold">×2</text>

  <path d="M 130,195 L 200,160" fill="none" stroke="#2a7ae2" stroke-width="2" marker-end="url(#arrowPerc)" />
  <text x="155" y="185" fill="#1d5eb8" font-size="12" font-weight="bold">×1</text>

  <circle cx="230" cy="140" r="38" fill="#fdfdfd" stroke="#2a7ae2" stroke-width="2" />
  <text x="230" y="148" fill="#111" font-size="22" text-anchor="middle">&#931;</text>

  <path d="M 268,140 L 316,140" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowPerc)" />

  <rect x="320" y="105" width="110" height="70" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="375" y="135" fill="#111" font-size="12" text-anchor="middle">totale</text>
  <text x="375" y="152" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">&gt; 15 ?</text>

  <path d="M 430,140 L 468,140" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowPerc)" />

  <rect x="470" y="110" width="110" height="60" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="525" y="135" fill="#111" font-size="12" text-anchor="middle">🌂</text>
  <text x="525" y="155" fill="#111" font-size="11" text-anchor="middle">sì / no</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni indizio pesa in proporzione diversa: il cielo nuvoloso conta il doppio dell'umidità.</figcaption>
</figure>

### 1.2 Il percettrone: un interruttore che si corregge

Nel 1958 Frank Rosenblatt ebbe l'idea che rese questo modello davvero interessante: il **percettrone**, un interruttore capace di aggiustare da solo l'importanza data a ciascun indizio, imparando da esempi passati. Il meccanismo è sorprendentemente semplice. Ogni volta che il percettrone sbaglia una decisione, si corregge un po': se ha detto "no ombrello" e invece pioveva, aumenta un po' l'importanza data agli indizi che quel giorno erano alti (magari il cielo era molto nuvoloso: la prossima volta peserà di più il cielo nuvoloso); se ha detto "sì ombrello" e invece non pioveva, fa l'esatto opposto. Quando la decisione è già giusta, non cambia nulla — nessun bisogno di correggersi se non si è sbagliato.

Ripetendo questa correzione su tanti giorni passati (di cui conosciamo già se poi ha piovuto o no), il percettrone via via si aggiusta. E Rosenblatt dimostrò qualcosa di notevole: se esiste *un modo qualunque* di tracciare una linea netta che separa perfettamente i giorni di pioggia dai giorni di sole guardando quei due indizi, questa procedura di correzione, ripetuta abbastanza volte, trova sempre quella linea.

### 1.3 Il limite: quando una linea sola non basta

C'è però un problema, e per vederlo conviene un esempio ancora più semplice: quattro palline colorate messe ai quattro angoli di un quadrato, due rosse e due blu, ma disposte in diagonale — rossa in alto a sinistra, blu in alto a destra, blu in basso a sinistra, rossa in basso a destra. Prova a tracciare una singola linea retta che separi tutte le rosse da tutte le blu. Non ci riuscirai: qualunque retta tu disegni, finisce sempre per lasciare una pallina del colore sbagliato dalla parte sbagliata.

Questo schema non è un capriccio inventato apposta: corrisponde esattamente a un problema logico chiamato **XOR** ("o esclusivo"), che risponde "sì" se esattamente uno fra due segnali è attivo, "no" se sono entrambi attivi o entrambi spenti — e nessun singolo percettrone, per quanto ben allenato, può risolverlo. Non è un difetto della procedura di correzione della Sezione 1.2: è un limite geometrico invalicabile, perché un solo interruttore può tracciare solo confini dritti, mai un confine "storto" come richiederebbe questo problema.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 400 380" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="xor-title xor-desc" style="width: 100%; max-width: 340px; height: auto; font-family: inherit;">
  <title id="xor-title">Il problema XOR: nessuna retta separa le palline</title>
  <desc id="xor-desc">Quattro palline ai quattro angoli di un quadrato: rossa in alto a sinistra, blu in alto a destra, blu in basso a sinistra, rossa in basso a destra. Una linea retta tentativo lascia sempre almeno una pallina dalla parte sbagliata.</desc>

  <rect x="100" y="100" width="200" height="200" fill="none" stroke="#e3e3e3" stroke-width="1.5" stroke-dasharray="4 3" />

  <line x1="60" y1="200" x2="340" y2="200" stroke="#f66a0a" stroke-width="2.5" stroke-dasharray="6 4" />
  <text x="345" y="205" fill="#f66a0a" font-size="11">tentativo</text>

  <circle cx="100" cy="100" r="16" fill="#c85506" />
  <circle cx="300" cy="100" r="16" fill="#2a7ae2" />
  <circle cx="100" cy="300" r="16" fill="#2a7ae2" />
  <circle cx="300" cy="300" r="16" fill="#c85506" />

  <g stroke="#c85506" stroke-width="3" stroke-linecap="round">
    <line x1="88" y1="88" x2="112" y2="112" /><line x1="112" y1="88" x2="88" y2="112" />
    <line x1="88" y1="288" x2="112" y2="312" /><line x1="112" y1="288" x2="88" y2="312" />
  </g>

  <text x="200" y="345" fill="#828282" font-size="12" text-anchor="middle">qualunque retta tracci,</text>
  <text x="200" y="362" fill="#828282" font-size="12" text-anchor="middle">lascia sempre una pallina sbagliata</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Le due palline rosse sono in diagonale: nessuna linea dritta le separa entrambe dalle blu.</figcaption>
</figure>

### 1.4 Un inverno di delusione, e un indizio già nel problema

Nel 1969 Marvin Minsky e Seymour Papert pubblicarono un'analisi che metteva nero su bianco questo limite, con l'XOR come esempio principale. L'effetto pratico fu enorme e per certi versi ingiusto verso l'idea in sé: i finanziamenti alla ricerca sulle reti neurali si prosciugarono per gran parte degli anni '70, un periodo che si ricorda come il primo "inverno dell'intelligenza artificiale". Eppure lo stesso libro di Minsky e Papert osservava — quasi di passaggio — che impilare più interruttori in fila avrebbe potuto superare il limite. L'osservazione era corretta, ma mancava ancora un pezzo cruciale: nessuno sapeva ancora, all'epoca, *come* allenare in modo efficiente più interruttori collegati insieme. Quel pezzo mancante — che vedremo nella Lezione 6 — sarebbe arrivato solo vent'anni più tardi.

### 1.5 Da un interruttore netto a una manopola che sente le sfumature

C'è un secondo problema, più tecnico ma altrettanto importante, che ha richiesto di ripensare l'interruttore stesso. Un interruttore che scatta di netto — o acceso o spento, senza vie di mezzo — non lascia capire *quanto* fosse vicino a cambiare idea: due situazioni in cui l'interruttore dice "no" con sicurezza opposta (una appena sotto soglia, una lontanissima) restano indistinguibili una volta scattata la decisione finale. Per costruire interruttori capaci di correggersi in modo più fine — specialmente quando ce ne sono tanti collegati in fila, come vedremo nella prossima lezione — serve una manopola che si muova con gradualità, non un tasto che scatta di colpo: qualcosa che, invece di saltare bruscamente da spento ad acceso, scivoli con dolcezza da un estremo all'altro, passando per ogni sfumatura intermedia.

Le reti neurali moderne usano proprio manopole così, chiamate **funzioni di attivazione**. Alcune assomigliano a una diapositiva liscia a forma di "S", che scivola gradualmente da spento ad acceso passando per ogni grado intermedio di certezza. Altre — oggi le più usate — restano completamente spente finché il segnale in ingresso non supera lo zero, e da quel punto in poi salgono in modo diretto e proporzionale, senza mai appiattirsi: una specie di valvola che non lascia passare nulla finché non viene spinta, ma poi risponde in modo pulito e prevedibile a quanto viene spinta. Qual è il punto in comune fra tutte queste manopole, così diverse da un interruttore a scatto? Nessuna di esse ha "spigoli nascosti": si può sempre dire, in ogni punto, se il segnale sta crescendo, calando, o restando fermo — un dettaglio che sembra tecnico, ma che nella Lezione 6 si rivelerà l'ingrediente decisivo per insegnare a un'intera fila di interruttori a correggersi insieme.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="attiv-title attiv-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="attiv-title">Da un interruttore a scatto a una manopola sfumata</title>
  <desc id="attiv-desc">Tre curve a confronto: la funzione a scatto, che salta bruscamente da spento ad acceso; la funzione a S, che scivola con gradualità; e la ReLU, spenta fino a zero e poi lineare.</desc>

  <text x="100" y="20" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">a scatto</text>
  <g stroke="#828282" stroke-width="1"><line x1="30" y1="190" x2="170" y2="190" /><line x1="100" y1="40" x2="100" y2="190" /></g>
  <path d="M30,190 L100,190 L100,60 L170,60" fill="none" stroke="#c85506" stroke-width="2.5" />
  <text x="100" y="210" fill="#828282" font-size="10" text-anchor="middle">spento o acceso, senza vie di mezzo</text>

  <text x="280" y="20" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">a forma di "S"</text>
  <g stroke="#828282" stroke-width="1"><line x1="210" y1="190" x2="350" y2="190" /><line x1="280" y1="40" x2="280" y2="190" /></g>
  <path d="M210,172 C240,172 250,120 280,120 C310,120 320,68 350,68" fill="none" stroke="#2a7ae2" stroke-width="2.5" />
  <text x="280" y="210" fill="#828282" font-size="10" text-anchor="middle">scivola con gradualità</text>

  <text x="460" y="20" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">ReLU</text>
  <g stroke="#828282" stroke-width="1"><line x1="390" y1="190" x2="530" y2="190" /><line x1="460" y1="40" x2="460" y2="190" /></g>
  <path d="M390,180 L460,180 L530,80" fill="none" stroke="#3aa655" stroke-width="2.5" />
  <text x="460" y="210" fill="#828282" font-size="10" text-anchor="middle">spenta, poi sale in modo pulito</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Tutte e due le manopole sfumate, a differenza dello scatto, non hanno "spigoli nascosti".</figcaption>
</figure>

### 1.6 Perché serve proprio questa sfumatura

Si potrebbe pensare: perché non impilare tanti interruttori netti, uno sopra l'altro, e lasciare che la loro combinazione faccia il lavoro sporco? Il problema è che impilare interruttori che si limitano a sommare segnali, senza alcuna manopola sfumata nel mezzo, non aggiunge davvero potere decisionale: è un po' come fotocopiare una fotocopia — il risultato resta piatto quanto l'originale, non importa quante volte lo ripeti. È proprio la manopola sfumata della Sezione 1.5, inserita fra un interruttore e il successivo, a rompere questa piattezza e a permettere a una fila di interruttori di rappresentare confini di decisione curvi, spezzati, complicati quanto serve — non solo la linea dritta del percettrone singolo.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="fotocopia-title fotocopia-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="fotocopia-title">Fotocopia di una fotocopia, contro un confine curvo</title>
  <desc id="fotocopia-desc">Le stesse quattro palline del problema XOR. A sinistra, impilare interruttori senza manopola sfumata produce ancora solo un confine dritto, che fallisce. A destra, con la manopola sfumata, il confine può curvarsi e isolare correttamente le due palline rosse.</desc>

  <text x="140" y="55" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">senza sfumatura</text>
  <rect x="80" y="80" width="120" height="120" fill="none" stroke="#e3e3e3" stroke-width="1.5" stroke-dasharray="4 3" />
  <line x1="50" y1="140" x2="230" y2="140" stroke="#c85506" stroke-width="2.5" stroke-dasharray="6 4" />
  <circle cx="80" cy="80" r="12" fill="#c85506" /><circle cx="200" cy="80" r="12" fill="#2a7ae2" />
  <circle cx="80" cy="200" r="12" fill="#2a7ae2" /><circle cx="200" cy="200" r="12" fill="#c85506" />
  <text x="140" y="230" fill="#828282" font-size="11" text-anchor="middle">ancora solo una retta:</text>
  <text x="140" y="245" fill="#828282" font-size="11" text-anchor="middle">"fotocopia di una fotocopia"</text>

  <text x="420" y="55" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">con sfumatura</text>
  <rect x="360" y="80" width="120" height="120" fill="none" stroke="#e3e3e3" stroke-width="1.5" stroke-dasharray="4 3" />
  <path d="M 380,200 Q 400,220 420,200" fill="none" stroke="#2a7ae2" stroke-width="2.5" />
  <path d="M 360,80 Q 380,100 360,120" fill="none" stroke="#2a7ae2" stroke-width="2.5" />
  <path d="M 400,80 Q 420,60 440,80" fill="none" stroke="#2a7ae2" stroke-width="2.5" />
  <path d="M 480,160 Q 500,180 480,200" fill="none" stroke="#2a7ae2" stroke-width="2.5" />
  <circle cx="360" cy="80" r="12" fill="#c85506" /><circle cx="480" cy="80" r="12" fill="#2a7ae2" />
  <circle cx="360" cy="200" r="12" fill="#2a7ae2" /><circle cx="480" cy="200" r="12" fill="#c85506" />
  <text x="420" y="230" fill="#828282" font-size="11" text-anchor="middle">un confine curvo isola</text>
  <text x="420" y="245" fill="#828282" font-size="11" text-anchor="middle">correttamente le due rosse</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La manopola sfumata è ciò che permette a più interruttori impilati di disegnare confini curvi, non solo dritti.</figcaption>
</figure>

Quanto complicati, esattamente? Un risultato matematico sorprendente — che qui citiamo senza dimostrare — garantisce che una fila di interruttori-con-manopola sufficientemente numerosa può avvicinarsi quanto si vuole a *qualunque* relazione ragionevole fra indizi in ingresso e decisione in uscita, per quanto complicata. Non dice quanti interruttori servano per un caso specifico (a volte moltissimi), né come trovare le impostazioni giuste — quello è il compito di allenamento che occuperà buona parte di questo libro — ma stabilisce che il limite non è "quanto sono espressive le reti neurali in teoria", bensì, in pratica, quanti dati, quanto calcolo, e quanto è bravo l'algoritmo che le allena.

---

> **Prova tu — Correggi l'interruttore dell'ombrello**
>
> Il tuo interruttore-ombrello decide guardando due indizi, ciascuno da 0 a 10: **C** (quanto è nuvoloso il cielo) e **U** (quanto è umida l'aria). Il punteggio è 2×C + 1×U (il cielo pesa doppio), e la soglia per dire "sì ombrello" è 15.
>
> Oggi: C = 6, U = 4. Punteggio: 2×6 + 1×4 = 16. L'interruttore dice **sì, ombrello** — ed effettivamente ha piovuto. Nessuna correzione necessaria, la decisione era giusta.
>
> Regola di correzione (solo se l'interruttore ha sbagliato): se ha detto "no" ma la risposta giusta era "sì", **aumenta di 1** il peso di ogni indizio che oggi valeva più di 5; se ha detto "sì" ma la risposta giusta era "no", **diminuisci di 1** il peso di ogni indizio che oggi valeva più di 5.
>
> Ora tocca a te. Domani: C = 3, U = 8, pesi ancora 2 e 1, soglia sempre 15.
>
> 1. Calcola il punteggio di domani. L'interruttore dice sì o no ombrello?
> 2. In realtà, domani pioverà. L'interruttore ha sbagliato? Se sì, applica la regola di correzione e scrivi i nuovi pesi.
> 3. Con i pesi corretti, ricalcola il punteggio di domani: la decisione ora è quella giusta?

---

*Continua con la [Lezione 02 — Impilare le decisioni: la rete a più piani]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-02-impilare-le-decisioni.md %}.html)*
