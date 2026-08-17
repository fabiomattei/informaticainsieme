---
title: 'Lezione 08 — Perché a volte sbaglia (e perché può essere pericoloso)'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Una domanda si biforca in due risposte scritte con lo stesso tono sicuro: una corretta, l'altra un'allucinazione](/images/ia/come-pensano-le-macchine-lezione-08-perche-a-volte-sbaglia/come-pensano-le-macchine-lezione-08-perche-a-volte-sbaglia.svg){:class="aside-image"}

### 8.1 Inventare non è un guasto — è il meccanismo stesso

Hai già sentito parlare di chatbot che "inventano" fonti, citazioni, o persino eventi storici mai accaduti, presentandoli con la stessa sicurezza con cui direbbero un fatto vero. Questo fenomeno si chiama **allucinazione**, ma il nome è un po' fuorviante: fa pensare a un guasto occasionale, un bug da correggere. La realtà è più scomoda: un LLM **non ha, da nessuna parte al suo interno, un archivio di "fatti verificati" separato dal resto**. Tutto quello che fa — l'abbiamo visto fin dalla Lezione 1 — è indovinare la parola più plausibile visto il contesto. Quando la domanda riguarda qualcosa che il modello ha visto scritto migliaia di volte durante l'addestramento (la capitale della Francia), indovinare la parola plausibile e dire il vero coincidono quasi sempre. Quando la domanda riguarda qualcosa che il modello non ha mai visto — un dettaglio troppo specifico, un evento troppo recente, una fonte che semplicemente non esiste — il modello continua comunque a fare l'unica cosa che sa fare: produrre la continuazione più plausibile *nello stile* di una risposta corretta, anche se il contenuto è inventato di sana pianta.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="alluc-title alluc-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="alluc-title">Stessa sicurezza, affidabilità diversa</title>
  <desc id="alluc-desc">Due domande diverse. La prima, ampiamente presente nei testi di addestramento, riceve la risposta corretta "Parigi". La seconda, su un dettaglio mai scritto da nessuna parte, riceve comunque una risposta espressa con la stessa identica sicurezza, ma inventata.</desc>

  <defs>
    <marker id="arrowH8" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="20" y="40" width="170" height="60" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="105" y="65" fill="#111" font-size="12" text-anchor="middle">Qual è la capitale</text>
  <text x="105" y="82" fill="#111" font-size="12" text-anchor="middle">della Francia?</text>

  <g fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5">
    <rect x="210" y="48" width="34" height="44" /><rect x="216" y="54" width="34" height="44" /><rect x="222" y="60" width="34" height="44" />
  </g>
  <text x="240" y="120" fill="#1d5eb8" font-size="10" text-anchor="middle">migliaia di testi</text>

  <path d="M 270,70 L 330,70" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowH8)" />
  <rect x="340" y="45" width="180" height="50" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="430" y="76" fill="#111" font-size="16" font-weight="bold" text-anchor="middle">"Parigi."</text>
  <text x="430" y="118" fill="#1d5eb8" font-size="11" text-anchor="middle">✓ lo sa davvero</text>

  <rect x="20" y="190" width="170" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="105" y="215" fill="#111" font-size="12" text-anchor="middle">Un dettaglio ultra-specifico,</text>
  <text x="105" y="232" fill="#111" font-size="12" text-anchor="middle">mai scritto da</text>
  <text x="105" y="249" fill="#111" font-size="12" text-anchor="middle">nessuna parte</text>

  <g fill="none" stroke="#c9c9c9" stroke-width="1.5" stroke-dasharray="3 2">
    <rect x="210" y="205" width="34" height="44" /><rect x="216" y="211" width="34" height="44" /><rect x="222" y="217" width="34" height="44" />
  </g>
  <text x="240" y="277" fill="#828282" font-size="10" text-anchor="middle">nessun testo</text>

  <path d="M 270,225 L 330,225" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowH8)" />
  <rect x="340" y="200" width="180" height="50" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="430" y="231" fill="#111" font-size="16" font-weight="bold" text-anchor="middle">"Nel 1997."</text>
  <text x="430" y="273" fill="#c85506" font-size="11" text-anchor="middle">⚠ inventata, stessa sicurezza</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La confezione (il tono sicuro) è identica in entrambi i casi: solo il contenuto cambia affidabilità.</figcaption>
</figure>

### 8.2 Riempire i vuoti di un ricordo confuso

Un'analogia utile: pensa a quando racconti un ricordo d'infanzia sfocato. Non menti consapevolmente, ma la tua mente riempie automaticamente i dettagli mancanti con qualcosa di plausibile — il colore di un vestito, l'ordine esatto degli eventi — pur di restituire una storia coerente, e spesso *ti convinci* che sia andata proprio così. Un LLM fa qualcosa di analogo, ma in modo ancora più sistematico: non ha modo di distinguere internamente, con certezza, "questo lo so per certo" da "questo suona plausibile ma non ne sono sicuro" — perché, semplicemente, non ragiona in termini di certezza e incertezza come farebbe un umano consapevole dei propri limiti. Dopo l'addestramento a premi della Lezione 5, il problema spesso peggiora invece di migliorare: se le persone che valutano le risposte tendono a preferire risposte sicure di sé e ben argomentate anche quando sono sbagliate — e tendono a farlo, perché una risposta piena di dubbi è meno gradevole da leggere — il modello impara a *sembrare* sicuro anche quando non dovrebbe esserlo. Questo effetto collaterale si chiama spesso "adulazione" (sycophancy) del modello.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="adul-title adul-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="adul-title">Il giudice preferisce la sicurezza</title>
  <desc id="adul-desc">Due risposte alla stessa domanda: una onesta ma incerta, corretta, e una sicura di sé ma sbagliata. Il punteggio del giudice premia quella sbagliata perché più sicura e ben argomentata.</desc>

  <rect x="30" y="30" width="210" height="130" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="45" y="55" fill="#111" font-size="11" font-style="italic">"Non ne sono sicuro, ma</text>
  <text x="45" y="72" fill="#111" font-size="11" font-style="italic">credo sia circa nel</text>
  <text x="45" y="89" fill="#111" font-size="11" font-style="italic">1750, da verificare..."</text>
  <text x="135" y="145" fill="#1d5eb8" font-size="12" font-weight="bold" text-anchor="middle">✓ onesta e corretta</text>

  <rect x="280" y="30" width="210" height="130" rx="8" fill="#fdfdfd" stroke="#f66a0a" stroke-width="1.5" />
  <text x="295" y="55" fill="#111" font-size="11" font-weight="bold">"Sicuramente nel 1750,</text>
  <text x="295" y="72" fill="#111" font-size="11" font-weight="bold">senza alcun dubbio: è</text>
  <text x="295" y="89" fill="#111" font-size="11" font-weight="bold">un fatto ben noto."</text>
  <text x="385" y="145" fill="#c85506" font-size="12" font-weight="bold" text-anchor="middle">✗ sicura ma sbagliata</text>

  <text x="260" y="192" fill="#111" font-size="12" text-anchor="middle">punteggio del giudice (RLHF)</text>

  <rect x="30" y="204" width="74" height="24" fill="#6fa8e8" rx="3" />
  <text x="112" y="221" fill="#111" font-size="12">35</text>

  <rect x="280" y="204" width="137" height="24" fill="#f66a0a" rx="3" />
  <text x="425" y="221" fill="#111" font-size="12">65</text>

  <text x="385" y="260" fill="#c85506" font-size="12" text-anchor="middle">il giudice la preferisce</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Se chi valuta preferisce la sicurezza espressiva alla correttezza, il modello impara a suonare sicuro anche quando non dovrebbe.</figcaption>
</figure>

### 8.3 Pregiudizi ereditati dai testi

Un LLM impara esclusivamente dai testi che legge — e quei testi, scritti da persone reali in decenni e secoli diversi, portano con sé gli stessi pregiudizi, stereotipi e squilibri di rappresentazione che si trovano nella società che li ha prodotti. Se nei testi di addestramento certe professioni compaiono più spesso associate a un genere, o certe nazionalità compaiono più spesso in contesti negativi, il modello tende ad assorbire — e poi riprodurre nelle sue risposte — quelle stesse associazioni statistiche, senza che nessuno gliele abbia insegnate esplicitamente come "regole". Misurare quanto un modello sia distorto in un modo o nell'altro, e correggerlo dove possibile, è oggi un intero campo di studio a sé, perché il pregiudizio non è mai un singolo bug da sistemare: è distribuito su miliardi di manopole interne, esattamente come lo è ogni altra conoscenza del modello.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 350" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="pregiudizio-title pregiudizio-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="pregiudizio-title">Distribuito, non isolato</title>
  <desc id="pregiudizio-desc">A sinistra, una singola manopola barrata con una X, a rappresentare l'idea sbagliata di un bug isolato da correggere. A destra, una griglia di venti manopole tutte leggermente velate d'arancio, a rappresentare il pregiudizio distribuito un po' su ciascuna delle miliardi di manopole del modello.</desc>

  <circle cx="90" cy="170" r="40" fill="#fdfdfd" stroke="#828282" stroke-width="2" />
  <line x1="90" y1="170" x2="102" y2="150" stroke="#2a7ae2" stroke-width="3" stroke-linecap="round" />
  <line x1="65" y1="145" x2="115" y2="195" stroke="#c85506" stroke-width="4" stroke-linecap="round" />
  <line x1="115" y1="145" x2="65" y2="195" stroke="#c85506" stroke-width="4" stroke-linecap="round" />
  <text x="90" y="232" fill="#c85506" font-size="12" text-anchor="middle">✗ non è una singola</text>
  <text x="90" y="248" fill="#c85506" font-size="12" text-anchor="middle">manopola difettosa</text>

  <text x="150" y="178" fill="#828282" font-size="26" text-anchor="middle">≠</text>

  <rect x="185" y="40" width="270" height="280" rx="12" fill="#f66a0a" opacity="0.12" />
  <text x="320" y="28" fill="#111" font-size="12" text-anchor="middle">il pregiudizio: un po' in ciascuna</text>

  <g fill="#fdfdfd" stroke="#828282" stroke-width="1.2">
    <circle cx="225" cy="75" r="12" /><circle cx="285" cy="75" r="12" /><circle cx="345" cy="75" r="12" /><circle cx="405" cy="75" r="12" />
    <circle cx="225" cy="125" r="12" /><circle cx="285" cy="125" r="12" /><circle cx="345" cy="125" r="12" /><circle cx="405" cy="125" r="12" />
    <circle cx="225" cy="175" r="12" /><circle cx="285" cy="175" r="12" /><circle cx="345" cy="175" r="12" /><circle cx="405" cy="175" r="12" />
    <circle cx="225" cy="225" r="12" /><circle cx="285" cy="225" r="12" /><circle cx="345" cy="225" r="12" /><circle cx="405" cy="225" r="12" />
    <circle cx="225" cy="275" r="12" /><circle cx="285" cy="275" r="12" /><circle cx="345" cy="275" r="12" /><circle cx="405" cy="275" r="12" />
  </g>
  <g stroke="#2a7ae2" stroke-width="1.8" stroke-linecap="round">
    <line x1="225" y1="75" x2="231" y2="67" /><line x1="285" y1="75" x2="291" y2="67" /><line x1="345" y1="75" x2="351" y2="67" /><line x1="405" y1="75" x2="411" y2="67" />
    <line x1="225" y1="125" x2="231" y2="117" /><line x1="285" y1="125" x2="291" y2="117" /><line x1="345" y1="125" x2="351" y2="117" /><line x1="405" y1="125" x2="411" y2="117" />
    <line x1="225" y1="175" x2="231" y2="167" /><line x1="285" y1="175" x2="291" y2="167" /><line x1="345" y1="175" x2="351" y2="167" /><line x1="405" y1="175" x2="411" y2="167" />
    <line x1="225" y1="225" x2="231" y2="217" /><line x1="285" y1="225" x2="291" y2="217" /><line x1="345" y1="225" x2="351" y2="217" /><line x1="405" y1="225" x2="411" y2="217" />
    <line x1="225" y1="275" x2="231" y2="267" /><line x1="285" y1="275" x2="291" y2="267" /><line x1="345" y1="275" x2="351" y2="267" /><line x1="405" y1="275" x2="411" y2="267" />
  </g>

  <text x="320" y="333" fill="#828282" font-size="12" text-anchor="middle">migliaia di manopole, non una sola</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Non c'è una manopola "razzista" da spegnere: il pregiudizio è un velo sottile su moltissime di esse insieme.</figcaption>
</figure>

### 8.4 Basta cambiare le parole, e la risposta cambia

Un'altra fragilità sorprendente: **piccole modifiche a una domanda, ininfluenti per un lettore umano, possono ribaltare la risposta di un LLM**. Cambiare l'ordine delle opzioni in una domanda a scelta multipla, aggiungere uno spazio di troppo, riformulare una domanda di matematica con parole leggermente diverse ma stesso identico problema: tutte cose che a un essere umano non cambierebbero la risposta di una virgola, ma che possono far vacillare un modello. Questo capita perché il modello non "capisce" il problema in astratto, separandolo dalle parole precise usate per porlo — la sua rappresentazione del problema *è* intrecciata con le parole esatte, in un modo più fragile di quanto ci piacerebbe credere.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="fragile-title fragile-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="fragile-title">Solo l'ordine cambia, la risposta pure</title>
  <desc id="fragile-desc">Stessa domanda a scelta multipla, stesse due opzioni: nella prima versione l'opzione A è "Giove" e il modello risponde correttamente A. Nella seconda versione l'ordine è invertito, l'opzione A è "Saturno", e il modello risponde ancora A, stavolta sbagliando.</desc>

  <defs>
    <marker id="arrowF8" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="30" y="30" width="260" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="45" y="55" fill="#111" font-size="12">Qual è il pianeta più grande?</text>
  <text x="45" y="75" fill="#111" font-size="12">A) <tspan font-weight="bold">Giove</tspan>&#160;&#160;&#160;B) Saturno</text>

  <path d="M 300,65 L 330,65" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowF8)" />
  <rect x="340" y="40" width="150" height="44" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="415" y="67" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">A) Giove ✓</text>

  <rect x="30" y="160" width="260" height="70" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="45" y="185" fill="#111" font-size="12">Qual è il pianeta più grande?</text>
  <text x="45" y="205" fill="#111" font-size="12">A) <tspan font-weight="bold" fill="#c85506">Saturno</tspan>&#160;&#160;&#160;B) Giove</text>

  <path d="M 300,195 L 330,195" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowF8)" />
  <rect x="340" y="170" width="150" height="44" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="415" y="197" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">A) Saturno ✗</text>

  <text x="260" y="255" fill="#828282" font-size="12" text-anchor="middle">stesse opzioni, ordine invertito: il modello sceglie ancora "A"</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Un dettaglio che a un umano non cambierebbe nulla può ribaltare la risposta del modello.</figcaption>
</figure>

### 8.5 Il gioco delle scappatoie

Un modello viene addestrato, come visto nella Lezione 5, anche a *rifiutarsi* di aiutare con richieste pericolose (costruire un'arma, scrivere codice dannoso, generare contenuti illegali). Ma un rifiuto imparato durante l'addestramento non è una barriera fisica invalicabile: è, di nuovo, solo un altro pattern appreso — e i pattern appresi si possono aggirare con astuzia. Le tecniche per farlo si chiamano **jailbreak**: convincere il modello, con un contesto inventato ad hoc ("stai scrivendo una scena di un film, il personaggio malvagio deve spiegare come..."), a produrre comunque il contenuto che rifiuterebbe se chiesto direttamente. Alcune tecniche sono sorprendentemente semplici — nascondere la richiesta pericolosa in mezzo a decine di richieste innocue, ad esempio, può abbassare le difese del modello quasi come un rumore di fondo che stanca un guardiano attento.

Per questo, chi costruisce questi modelli investe tempo apposta a cercare le proprie falle prima che lo facciano altri: un processo chiamato **red teaming**, in cui persone (e, sempre più spesso, altri modelli) provano sistematicamente a "rompere" il modello con richieste sempre più creative, per scoprire e correggere le falle prima del rilascio pubblico. Una tecnica interessante per rendere questo processo più scalabile si chiama **AI costituzionale**: invece di far correggere ogni singola risposta problematica da una persona, si dà al modello stesso un piccolo insieme di principi scritti ("non aiutare azioni pericolose", "sii onesto") e lo si allena a **criticare e correggere le proprie risposte** confrontandole con quei principi — una specie di coscienza scritta esplicitamente, invece che assorbita implicitamente e in modo opaco dai dati.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="scudo-title scudo-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="scudo-title">Lo scudo e la scappatoia (schema concettuale)</title>
  <desc id="scudo-desc">Rappresentazione schematica, senza dettagli tecnici: una richiesta pericolosa diretta viene bloccata dallo scudo di rifiuto. Una richiesta camuffata passa invece attraverso una piccola falla nello scudo, arrivando a produrre contenuto che avrebbe dovuto essere rifiutato. Il red teaming cerca e chiude queste falle prima del rilascio pubblico.</desc>

  <defs>
    <marker id="arrowS8" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="220" y="50" width="80" height="200" rx="24" fill="#eef5fd" stroke="#2a7ae2" stroke-width="4" />
  <rect x="205" y="150" width="110" height="30" fill="#fdfdfd" />
  <rect x="205" y="150" width="110" height="30" fill="none" stroke="#f66a0a" stroke-width="1.5" stroke-dasharray="4 3" />
  <text x="260" y="145" fill="#c85506" font-size="11" text-anchor="middle">falla</text>

  <path d="M 40,90 L 216,90" fill="none" stroke="#c85506" stroke-width="2.5" />
  <text x="220" y="94" fill="#c85506" font-size="16" font-weight="bold">✗</text>
  <text x="130" y="75" fill="#111" font-size="11" text-anchor="middle">richiesta pericolosa diretta</text>
  <text x="130" y="112" fill="#c85506" font-size="11" text-anchor="middle">rifiutata dallo scudo</text>

  <path d="M 40,225 C 120,225 165,195 205,166" fill="none" stroke="#828282" stroke-width="2" stroke-dasharray="5 3" />
  <path d="M 215,165 L 330,165" fill="none" stroke="#828282" stroke-width="2" stroke-dasharray="5 3" marker-end="url(#arrowS8)" />
  <text x="120" y="245" fill="#111" font-size="11" text-anchor="middle">richiesta camuffata</text>
  <text x="120" y="260" fill="#111" font-size="11" text-anchor="middle">(contesto inventato)</text>

  <rect x="340" y="140" width="150" height="50" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="415" y="160" fill="#111" font-size="11" text-anchor="middle">contenuto che avrebbe</text>
  <text x="415" y="176" fill="#111" font-size="11" text-anchor="middle">dovuto rifiutare ⚠</text>

  <text x="260" y="285" fill="#828282" font-size="12" text-anchor="middle">il red teaming cerca (e chiude) queste falle prima del rilascio</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Schema puramente concettuale: nessuna tecnica reale è descritta o suggerita qui.</figcaption>
</figure>

---

> **Prova tu — Fai il fact-checker**
>
> Ecco cinque affermazioni. Alcune sono vere, altre sono "allucinazioni" plausibili ma false, scritte apposta per suonare credibili. Segna per ciascuna "vera" o "inventata" — senza usare internet, solo ragionando su quanto ti suona plausibile e su cosa già sai.
>
> 1. "La Torre Eiffel fu costruita originariamente come struttura temporanea per l'Esposizione Universale di Parigi del 1889."
> 2. "Il romanzo 'I Promessi Sposi' di Alessandro Manzoni fu pubblicato per la prima volta nel 1712."
> 3. "L'acqua bolle a una temperatura più bassa in montagna che al livello del mare, a causa della minore pressione atmosferica."
> 4. "Albert Einstein fu bocciato in matematica alle scuole superiori."
> 5. "Il linguaggio di programmazione Python prende il nome dal serpente pitone, scelto dal suo creatore come simbolo di 'flessibilità' del linguaggio."
>
> Scrivi le tue cinque risposte con una breve motivazione, poi confrontale con l'Appendice A: alcune di queste sono esattamente il tipo di "fatto" plausibile che un LLM potrebbe inventare con piena sicurezza — e il bello dell'esercizio è notare quali segnali (una data troppo precisa, un dettaglio troppo pulito) ti hanno fatto insospettire, o no.

---

*Continua con la [Lezione 09 — Oltre la chat]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-09-oltre-la-chat.md %}.html)*
