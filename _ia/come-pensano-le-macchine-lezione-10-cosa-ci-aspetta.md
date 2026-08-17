---
title: 'Lezione 10 — Cosa ci aspetta'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Una freccia parte da "oggi" e attraversa alcune tappe future fino a un punto interrogativo all'orizzonte](/images/ia/come-pensano-le-macchine-lezione-10-cosa-ci-aspetta/come-pensano-le-macchine-lezione-10-cosa-ci-aspetta.svg){:class="aside-image"}

### 10.1 Un gigante non entra in tasca

I modelli più capaci di cui senti parlare sono enormi — servono computer specializzati grandi come armadi, non certo lo smartphone in tasca. Ma molti usi pratici (un assistente vocale sempre acceso sul telefono, un correttore che funziona anche senza connessione internet) hanno bisogno esattamente del contrario: qualcosa di piccolo, veloce, capace di girare su un dispositivo con poca memoria e poca batteria. Da qui nasce un intero filone di tecniche per **rendere un modello più piccolo senza perdere troppo delle sue capacità** — un compromesso che, come vedremo, richiama da vicino i rendimenti decrescenti della Lezione 4: si può alleggerire parecchio prima che le prestazioni comincino a peggiorare sul serio.

### 10.2 Il maestro e l'allievo

La prima tecnica si chiama **distillazione**, e l'analogia con la scuola è quasi perfetta: un modello enorme già addestrato (il "maestro") viene usato per generare tantissimi esempi di risposte di alta qualità, e un modello molto più piccolo (l'"allievo") viene allenato a imitare quelle risposte — non a ripartire da zero leggendo l'intero web come ha fatto il maestro, ma a copiarne il comportamento su una quantità di esempi molto più gestibile. Un allievo ben distillato può arrivare a comportarsi sorprendentemente vicino al maestro su molti compiti comuni, pur avendo una frazione delle sue dimensioni — un po' come un apprendista che, guardando lavorare un artigiano esperto per mille ore, impara a riprodurne bene i gesti più frequenti, senza dover reinventare da solo l'intero mestiere.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="distill-title distill-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="distill-title">Il maestro e l'allievo</title>
  <desc id="distill-desc">Un modello maestro, molto grande, genera tanti esempi di risposte di alta qualità. Un modello allievo, molto più piccolo, viene allenato a imitare quegli esempi.</desc>

  <defs>
    <marker id="arrowDist" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="30" y="30" width="150" height="240" rx="10" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
  <text x="105" y="140" fill="#111" font-size="16" font-weight="bold" text-anchor="middle">Maestro</text>
  <text x="105" y="160" fill="#111" font-size="12" text-anchor="middle">(modello enorme,</text>
  <text x="105" y="176" fill="#111" font-size="12" text-anchor="middle">già addestrato)</text>

  <path d="M 180,150 L 218,150" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowDist)" />
  <text x="200" y="130" fill="#828282" font-size="10" text-anchor="middle">genera esempi</text>

  <g fill="#fdfdfd" stroke="#828282" stroke-width="1.2">
    <rect x="222" y="120" width="60" height="42" rx="4" />
    <rect x="230" y="128" width="60" height="42" rx="4" />
    <rect x="238" y="136" width="60" height="42" rx="4" />
  </g>
  <text x="268" y="190" fill="#111" font-size="10" text-anchor="middle">esempi di alta qualità</text>

  <path d="M 300,157 L 378,157" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowDist)" />
  <text x="340" y="140" fill="#828282" font-size="10" text-anchor="middle">imita</text>

  <rect x="380" y="122" width="150" height="70" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="2" />
  <text x="455" y="150" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">Allievo</text>
  <text x="455" y="167" fill="#111" font-size="11" text-anchor="middle">(modello piccolo)</text>

  <text x="280" y="250" fill="#828282" font-size="12" text-anchor="middle">l'allievo copia il comportamento, non rilegge l'intero web</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Una frazione delle dimensioni, ma un comportamento sorprendentemente vicino su molti compiti comuni.</figcaption>
</figure>

### 10.3 Alleggerire senza buttare via il necessario

Due tecniche complementari intervengono direttamente sul modello già addestrato, invece che sull'addestramento stesso. La **potatura** (pruning) individua le manopole interne (i parametri della Lezione 4) che contribuiscono pochissimo al comportamento finale — un po' come potare i rami secchi di una pianta, che non servono più a portare linfa ma occupano comunque spazio — e le rimuove, riducendo le dimensioni del modello. La **quantizzazione** invece non rimuove nulla, ma **arrotonda** ogni numero interno a una precisione inferiore: invece di conservare un numero con dodici cifre decimali per ogni manopola, se ne tengono solo due o tre — un po' come arrotondare i prezzi in un preventivo a spanne invece che al millesimo di centesimo. Il modello occupa molta meno memoria e gira più veloce, e sorprendentemente le sue risposte restano quasi indistinguibili da quelle del modello originale a piena precisione: gran parte di quella precisione extra, evidentemente, non stava contribuendo poi molto al risultato finale.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="alleggerire-title alleggerire-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="alleggerire-title">Potatura e quantizzazione</title>
  <desc id="alleggerire-desc">A sinistra, una griglia di manopole con alcune rimosse (potatura). A destra, un numero con molte cifre decimali arrotondato a sole due cifre (quantizzazione).</desc>

  <defs>
    <marker id="arrowQuant" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <line x1="260" y1="20" x2="260" y2="250" stroke="#e3e3e3" stroke-width="1.5" />

  <text x="140" y="25" fill="#111" font-size="14" font-weight="bold" text-anchor="middle">Potatura</text>
  <g fill="#fdfdfd" stroke="#828282" stroke-width="1.2">
    <circle cx="50" cy="50" r="12" /><circle cx="185" cy="50" r="12" />
    <circle cx="50" cy="95" r="12" /><circle cx="95" cy="95" r="12" /><circle cx="140" cy="95" r="12" /><circle cx="185" cy="95" r="12" />
    <circle cx="95" cy="140" r="12" /><circle cx="185" cy="140" r="12" />
    <circle cx="95" cy="185" r="12" /><circle cx="140" cy="185" r="12" /><circle cx="185" cy="185" r="12" />
  </g>
  <g fill="none" stroke="#c9c9c9" stroke-width="1.2" stroke-dasharray="3 2">
    <circle cx="95" cy="50" r="12" /><circle cx="140" cy="50" r="12" />
    <circle cx="140" cy="140" r="12" />
    <circle cx="50" cy="140" r="12" /><circle cx="50" cy="185" r="12" />
  </g>
  <text x="140" y="225" fill="#828282" font-size="11" text-anchor="middle">manopole poco utili rimosse</text>

  <text x="390" y="25" fill="#111" font-size="14" font-weight="bold" text-anchor="middle">Quantizzazione</text>
  <text x="390" y="110" fill="#111" font-size="13" text-anchor="middle">3,14159265358979</text>
  <path d="M 390,125 L 390,150" stroke="#828282" stroke-width="2" marker-end="url(#arrowQuant)" fill="none" />
  <text x="390" y="185" fill="#111" font-size="24" font-weight="bold" text-anchor="middle">3,14</text>
  <text x="390" y="225" fill="#828282" font-size="11" text-anchor="middle">meno memoria, quasi la stessa risposta</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Due modi di alleggerire un modello già addestrato, senza rifare l'addestramento.</figcaption>
</figure>

### 10.4 Insegnargli un nuovo trucco senza rifare tutta la scuola

Un'ultima tecnica di efficienza risolve un problema diverso: come specializzare un modello già addestrato per un compito nuovo (ad esempio, rispondere nello stile di una specifica azienda) senza dover rifare da capo l'intero, costosissimo addestramento. La tecnica, chiamata **LoRA**, aggiunge solo un piccolo numero di manopole nuove, "a fianco" di quelle originali che restano congelate — un po' come attaccare un post-it con qualche correzione specifica su un libro di testo già stampato, invece di ristamparlo da zero da un editore. Si allena solo il post-it, non l'intero libro: una frazione minuscola dei parametri totali, con un costo di gran lunga inferiore a un nuovo addestramento completo.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 400 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="lora-title lora-desc" style="width: 100%; max-width: 340px; height: auto; font-family: inherit;">
  <title id="lora-title">Un post-it su un libro già stampato</title>
  <desc id="lora-desc">Un libro rappresenta il modello originale, con un lucchetto a indicare che i pesi restano congelati. Un post-it giallo attaccato all'angolo rappresenta le poche manopole nuove di LoRA, le uniche che vengono allenate.</desc>

  <rect x="50" y="30" width="200" height="240" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <rect x="68" y="55" width="16" height="12" rx="2" fill="#828282" />
  <path d="M71,55 v-6 a5,5 0 0 1 10,0 v6" stroke="#828282" stroke-width="2" fill="none" />

  <g stroke="#e3e3e3" stroke-width="3">
    <line x1="70" y1="100" x2="230" y2="100" /><line x1="70" y1="115" x2="230" y2="115" />
    <line x1="70" y1="130" x2="230" y2="130" /><line x1="70" y1="145" x2="200" y2="145" />
  </g>

  <text x="150" y="200" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">Modello originale</text>
  <text x="150" y="218" fill="#111" font-size="11" text-anchor="middle">(pesi congelati 🔒)</text>

  <g transform="translate(275,55) rotate(8)">
    <rect x="-45" y="-40" width="90" height="80" fill="#fff2a8" stroke="#e0c200" stroke-width="1.5" />
    <line x1="-30" y1="-15" x2="30" y2="-15" stroke="#a08000" stroke-width="1.5" />
    <line x1="-30" y1="0" x2="20" y2="0" stroke="#a08000" stroke-width="1.5" />
    <line x1="-30" y1="15" x2="30" y2="15" stroke="#a08000" stroke-width="1.5" />
  </g>

  <text x="300" y="195" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">post-it: nuove</text>
  <text x="300" y="212" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">manopole (LoRA)</text>
  <text x="300" y="230" fill="#828282" font-size="11" text-anchor="middle">solo queste si allenano</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Non si ristampa il libro: si attacca un post-it, molto più economico da scrivere.</figcaption>
</figure>

### 10.5 Aprire la scatola nera

Fin qui abbiamo descritto *cosa fa* un LLM, ma c'è un campo di ricerca — relativamente giovane e in rapida crescita — che si chiede una domanda più profonda: possiamo capire, guardando dentro le miliardi di manopole di un modello già addestrato, **cosa rappresenta davvero ciascuna di esse**? Questo si chiama **interpretabilità meccanicistica**, e uno dei risultati più curiosi (e più raccontati) riguarda un esperimento del 2024, ribattezzato informalmente "Golden Gate Claude": i ricercatori sono riusciti a isolare, dentro un modello reale, un singolo "concetto interno" che si attivava sistematicamente ogni volta che il testo riguardava il Golden Gate Bridge di San Francisco — non solo quando il ponte era citato per nome, ma anche in descrizioni indirette. Amplificando artificialmente questo singolo concetto molto oltre il normale, hanno ottenuto un modello ossessionato: qualunque domanda gli venisse posta ("come faccio la pasta al pomodoro?"), trovava un modo — a volte comico, a volte inquietante — di ricondurre la risposta al Golden Gate Bridge. L'esperimento è divertente da raccontare, ma il punto serio è profondo: dimostra che dentro la massa apparentemente indecifrabile di manopole di un modello esistono concetti isolabili e persino manipolabili individualmente — una prima crepa di luce dentro quella che per anni è stata descritta, un po' rassegnati, come una "scatola nera".

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 340" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="golden-title golden-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="golden-title">Golden Gate Claude</title>
  <desc id="golden-desc">Tre domande completamente diverse tra loro convergono tutte verso un unico concetto interno amplificato, il Golden Gate Bridge, e la risposta finale finisce sempre per citarlo.</desc>

  <defs>
    <marker id="arrowGolden" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="20" y="30" width="170" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="105" y="59" fill="#111" font-size="11" text-anchor="middle">"Come faccio la pasta</text>
  <text x="105" y="72" fill="#111" font-size="11" text-anchor="middle">al pomodoro?"</text>

  <rect x="20" y="145" width="170" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="105" y="175" fill="#111" font-size="11" text-anchor="middle">"Che tempo fa oggi?"</text>

  <rect x="20" y="260" width="170" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="105" y="282" fill="#111" font-size="11" text-anchor="middle">"Aiutami con i compiti</text>
  <text x="105" y="296" fill="#111" font-size="11" text-anchor="middle">di matematica"</text>

  <path d="M 190,55 Q 240,90 262,140" fill="none" stroke="#828282" stroke-width="1.5" />
  <path d="M 190,170 L 255,170" fill="none" stroke="#828282" stroke-width="1.5" />
  <path d="M 190,285 Q 240,240 262,200" fill="none" stroke="#828282" stroke-width="1.5" />

  <circle cx="300" cy="170" r="62" fill="#f66a0a" opacity="0.18" />
  <circle cx="300" cy="170" r="45" fill="#f66a0a" />
  <text x="300" y="164" fill="#fdfdfd" font-size="12" font-weight="bold" text-anchor="middle">Golden Gate</text>
  <text x="300" y="180" fill="#fdfdfd" font-size="12" font-weight="bold" text-anchor="middle">Bridge</text>
  <text x="300" y="95" fill="#c85506" font-size="12" text-anchor="middle">concetto amplificato</text>

  <path d="M 345,170 L 398,170" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowGolden)" />

  <rect x="400" y="145" width="140" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="470" y="167" fill="#111" font-size="11" text-anchor="middle">🌉 la risposta finale</text>
  <text x="470" y="183" fill="#111" font-size="11" text-anchor="middle">cita comunque il ponte</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Qualunque fosse la domanda, il modello trovava un modo di ricondurla al Golden Gate Bridge.</figcaption>
</figure>

### 10.6 Cosa resta da capire

Chiudiamo con onestà: nessuno — nemmeno chi costruisce questi modelli — ha oggi una comprensione completa del perché certe capacità compaiono e altre no, di come evitare del tutto le allucinazioni della Lezione 8 senza sacrificare troppa creatività, o di cosa succederà rendendo questi sistemi ancora più grandi e ancora più capaci di agire da soli nel mondo, come accennato nella Lezione 9. Il campo si muove rapidissimo: quello che oggi è "stato dell'arte" sarà probabilmente superato nel giro di pochi anni, e alcune delle tecniche descritte in questo libro invecchieranno più in fretta di altre. Quello che difficilmente cambierà è il nucleo concettuale che hai visto costruirsi lezione dopo lezione: parole trasformate in numeri, attenzione che pesa il contesto, addestramento su enormi quantità di testo seguito da un affinamento sul comportamento desiderato, e una generazione, parola per parola, guidata da probabilità e da un pizzico di rischio calcolato. È un meccanismo più semplice di quanto il suo comportamento lasci intuire — e proprio per questo, capirlo un pezzo alla volta vale la fatica.

---

> **Prova tu — Le tue previsioni**
>
> Non c'è una risposta "giusta" da confrontare qui — solo la tua opinione, da mettere alla prova col tempo. Scrivi una risposta breve (2-3 frasi) a ciascuna domanda, con la data di oggi accanto:
>
> 1. Tra cinque anni, pensi che i chatbot saranno ancora basati sulla stessa idea di "indovinare la parola successiva" vista nella Lezione 1, o pensi che emergerà un'idea completamente diversa?
> 2. Pensi che il problema delle allucinazioni (Lezione 8) sarà in gran parte risolto, o pensi che sia una conseguenza inevitabile di come funziona il meccanismo, destinata a restare per sempre almeno in parte?
> 3. Pensi che tra cinque anni si parlerà ancora di "modelli enormi in un data center", o pensi che l'efficienza (Sezione 10.1) avrà reso normale avere modelli molto capaci direttamente sul telefono, senza bisogno di internet?
>
> In Appendice A trovi, non delle "risposte corrette" (nessuno le ha), ma un riassunto di cosa pensavano i ricercatori più esperti del settore nel momento in cui questo libro è stato scritto — utile termine di paragone quando rileggerai le tue risposte tra qualche anno.

---

*Continua con l'[Appendice A — Soluzioni ai giochi]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-appendice-a-soluzioni.md %}.html)*
