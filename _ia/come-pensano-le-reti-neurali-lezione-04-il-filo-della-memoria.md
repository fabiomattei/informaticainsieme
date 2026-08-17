---
title: 'Lezione 04 — Il filo della memoria'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 4.1 Il problema: sequenze che non hanno una lunghezza fissa

Sia la rete a più piani della Lezione 2 sia lo stencil della Lezione 3 si aspettano un input di dimensione fissa: un certo numero di indizi, un'immagine di una certa grandezza. Il testo, il parlato, una serie di misurazioni nel tempo non hanno questa comodità: una frase può avere cinque parole o cinquecento, e niente in una rete come quelle viste finora gestisce naturalmente input di lunghezza variabile, né tiene conto esplicitamente dell'ordine in cui le cose arrivano. Una rete della Lezione 2 applicata a una frase tratterebbe la prima e la cinquantesima parola come due numeri indipendenti in un elenco, non come due momenti di un racconto che si svolge nel tempo. Serve un'architettura pensata apposta per questo tipo di dato: le **reti neurali ricorrenti**.

### 4.2 L'idea: un amico che ti riassume mentre legge

Immagina un amico che legge un libro ad alta voce e, dopo ogni pagina, ti riassume a voce cosa ha capito finora — non ripetendo il libro intero, ma aggiornando un breve bigliettino mentale che tiene in testa. Ogni volta che gira pagina, non riparte da zero: combina il bigliettino di prima con la nuova pagina appena letta, e produce un bigliettino aggiornato. Questo bigliettino — che in una rete ricorrente si chiama **stato nascosto** — è, in linea di principio, un riassunto di *tutto* ciò che è stato letto fino a quel momento, non solo dell'ultima pagina: informazioni della pagina 3 possono, in teoria, sopravvivere nel bigliettino fino alla pagina 50, se sono state ritenute abbastanza importanti lungo il cammino.

Il dettaglio cruciale è che il tuo amico usa sempre lo stesso identico metodo di riassunto a ogni pagina — non inventa una strategia nuova ogni volta, ma applica la stessa identica regola, pagina dopo pagina, riusando gli stessi "criteri di riassunto" già visti nella Sezione 3.2 come idea di riuso, qui applicata non allo spazio di un'immagine ma al tempo di una lettura.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="rnn-title rnn-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="rnn-title">La rete srotolata nel tempo</title>
  <desc id="rnn-desc">Uno stato, il bigliettino mentale, si aggiorna a ogni pagina combinandosi con la nuova pagina letta. Lo stesso identico metodo di aggiornamento viene riusato a ogni passo.</desc>

  <defs>
    <marker id="arrowRnn" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <circle cx="20" cy="80" r="14" fill="none" stroke="#c9c9c9" stroke-width="1.5" stroke-dasharray="3 2" />
  <path d="M 34,80 L 48,80" stroke="#828282" stroke-width="2" marker-end="url(#arrowRnn)" fill="none" />

  <g>
    <circle cx="80" cy="80" r="30" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
    <circle cx="220" cy="80" r="30" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
    <circle cx="360" cy="80" r="30" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
    <circle cx="500" cy="80" r="30" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
  </g>
  <g fill="#111" font-size="11" text-anchor="middle">
    <text x="80" y="84">stato</text><text x="220" y="84">stato</text><text x="360" y="84">stato</text><text x="500" y="84">stato</text>
  </g>

  <path d="M 110,80 L 190,80" stroke="#828282" stroke-width="2" marker-end="url(#arrowRnn)" fill="none" />
  <path d="M 250,80 L 330,80" stroke="#828282" stroke-width="2" marker-end="url(#arrowRnn)" fill="none" />
  <path d="M 390,80 L 470,80" stroke="#828282" stroke-width="2" marker-end="url(#arrowRnn)" fill="none" />

  <g stroke="#828282" stroke-width="1.5">
    <path d="M 80,150 L 80,110" marker-end="url(#arrowRnn)" fill="none" />
    <path d="M 220,150 L 220,110" marker-end="url(#arrowRnn)" fill="none" />
    <path d="M 360,150 L 360,110" marker-end="url(#arrowRnn)" fill="none" />
    <path d="M 500,150 L 500,110" marker-end="url(#arrowRnn)" fill="none" />
  </g>
  <g fill="#fdfdfd" stroke="#828282" stroke-width="1.5">
    <rect x="45" y="150" width="70" height="36" rx="6" /><rect x="185" y="150" width="70" height="36" rx="6" />
    <rect x="325" y="150" width="70" height="36" rx="6" /><rect x="465" y="150" width="70" height="36" rx="6" />
  </g>
  <g fill="#111" font-size="11" text-anchor="middle">
    <text x="80" y="172">pagina 1</text><text x="220" y="172">pagina 2</text><text x="360" y="172">pagina 3</text><text x="500" y="172">pagina 4</text>
  </g>

  <text x="280" y="20" fill="#828282" font-size="11" text-anchor="middle">stesso identico metodo di aggiornamento riusato a ogni passo</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Il bigliettino non riparte mai da zero: porta con sé tutto ciò che è stato letto finora.</figcaption>
</figure>

### 4.3 Allenarsi a riassumere bene

Anche un'architettura come questa va allenata, con lo stesso principio generale che la Lezione 6 tratterà in dettaglio: guardare l'errore finale e correggere, un po' alla volta, il modo in cui il bigliettino viene aggiornato. La particolarità qui è che, per farlo, bisogna "srotolare" mentalmente tutta la sequenza di aggiornamenti — trattarla come una catena lunga quanto il numero di pagine lette, con lo stesso identico metodo di riassunto ripetuto a ogni anello della catena — e poi far risalire la correzione all'indietro lungo l'intera catena, dall'ultima pagina fino alla prima. Questa procedura ha un nome tecnico, **backpropagation through time**, ma l'idea è la stessa "colpa che risale la catena" che vedremo, per una rete non ricorrente, nella Lezione 6.

### 4.4 Il bigliettino che si sporca

Qui emerge un problema pratico, ed è proprio quello che ti invito a sperimentare tu stesso nel "Prova tu" di questa lezione: più lunga è la sequenza, più il bigliettino tende a "sporcarsi". Ogni aggiornamento rimescola un po' il contenuto precedente con la nuova pagina, e ripetuto decine o centinaia di volte, questo rimescolamento tende a diluire i dettagli delle pagine più lontane — un po' come cercare di ricordare la settima voce di una lista della spesa dettata a voce, senza scriverla: le prime voci si confondono con quelle intermedie, e a volte sopravvivono solo le ultime, sentite di fresco. Il problema tecnico ha un nome, il **vanishing gradient nel tempo**: il segnale che dovrebbe insegnare alla rete "questo dettaglio della pagina 3 era importante" si affievolisce esponenzialmente attraversando decine di aggiornamenti successivi, fino quasi a sparire.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="sporca-title sporca-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="sporca-title">Il bigliettino che si sporca</title>
  <desc id="sporca-desc">Una curva che scende rapidamente: quanto sopravvive un dettaglio del bigliettino, in funzione di quante pagine successive sono state lette, si affievolisce in modo esponenziale.</desc>

  <g stroke="#828282" stroke-width="1.5">
    <line x1="50" y1="220" x2="440" y2="220" />
    <line x1="50" y1="220" x2="50" y2="30" />
  </g>
  <text x="245" y="245" fill="#111" font-size="13" text-anchor="middle">pagine successive lette →</text>
  <text x="20" y="125" fill="#111" font-size="13" text-anchor="middle" transform="rotate(-90 20 125)">quanto sopravvive il dettaglio →</text>

  <path d="M50,40 C120,60 180,120 250,170 C300,200 350,212 440,218" fill="none" stroke="#c85506" stroke-width="3" />

  <circle cx="50" cy="40" r="9" fill="#c85506" opacity="1" />
  <circle cx="170" cy="95" r="9" fill="#c85506" opacity="0.6" />
  <circle cx="280" cy="180" r="9" fill="#c85506" opacity="0.3" />
  <circle cx="400" cy="215" r="9" fill="#c85506" opacity="0.1" />

  <text x="50" y="25" fill="#c85506" font-size="10" text-anchor="middle">pagina 3</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Più aggiornamenti attraversa, più un dettaglio si diluisce nel bigliettino — fino quasi a sparire.</figcaption>
</figure>

### 4.5 Un bigliettino più furbo: tenere un diario invece di un unico appunto

Due varianti più sofisticate di questa architettura — chiamate **LSTM** e **GRU** — affrontano il problema della Sezione 4.4 con un'idea intuitiva: invece di rimescolare sempre tutto il bigliettino a ogni pagina, la rete impara esplicitamente *cosa* vale la pena tenere dal bigliettino vecchio e *cosa* invece va sovrascritto con l'informazione nuova — un po' come un diario in cui certe righe restano intoccate pagina dopo pagina, e solo poche righe specifiche vengono aggiornate quando serve davvero. Questo meccanismo, chiamato **gating**, allevia molto il problema della Sezione 4.4 — il bigliettino resta leggibile per sequenze più lunghe, centinaia di pagine invece di poche decine — ma non lo elimina del tutto: anche un diario ben tenuto, dopo migliaia di pagine, comincia a perdere dettagli remoti.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 240" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="gating-title gating-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="gating-title">Il diario coi cancelli</title>
  <desc id="gating-desc">Il bigliettino vecchio e la pagina nuova entrano in un cancello che decide cosa tenere intatto e cosa aggiornare. Il bigliettino nuovo mantiene quasi tutto il contenuto precedente, con solo poche righe sovrascritte.</desc>

  <rect x="20" y="30" width="120" height="90" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <g stroke="#828282" stroke-width="2"><line x1="35" y1="50" x2="125" y2="50" /><line x1="35" y1="70" x2="125" y2="70" /></g>
  <line x1="35" y1="90" x2="125" y2="90" stroke="#f66a0a" stroke-width="2" stroke-dasharray="4 3" />
  <line x1="35" y1="108" x2="125" y2="108" stroke="#828282" stroke-width="2" />
  <text x="80" y="135" fill="#828282" font-size="10" text-anchor="middle">bigliettino vecchio</text>

  <rect x="20" y="160" width="120" height="50" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="80" y="190" fill="#111" font-size="11" text-anchor="middle">pagina nuova</text>

  <defs>
    <marker id="arrowGate" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>
  <path d="M 145,75 L 195,110" stroke="#828282" stroke-width="2" marker-end="url(#arrowGate)" fill="none" />
  <path d="M 145,185 L 195,130" stroke="#828282" stroke-width="2" marker-end="url(#arrowGate)" fill="none" />

  <path d="M 200,90 L 260,90 L 280,120 L 260,150 L 200,150 Z" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="240" y="115" fill="#111" font-size="10" text-anchor="middle">cancello</text>
  <text x="240" y="129" fill="#111" font-size="9" text-anchor="middle">cosa tenere?</text>
  <text x="240" y="141" fill="#111" font-size="9" text-anchor="middle">cosa aggiornare?</text>

  <path d="M 285,120 L 335,120" stroke="#828282" stroke-width="2" marker-end="url(#arrowGate)" fill="none" />

  <rect x="340" y="75" width="120" height="90" rx="6" fill="#fdfdfd" stroke="#2a7ae2" stroke-width="1.5" />
  <g stroke="#828282" stroke-width="2"><line x1="355" y1="95" x2="445" y2="95" /><line x1="355" y1="115" x2="445" y2="115" /></g>
  <line x1="355" y1="135" x2="445" y2="135" stroke="#2a7ae2" stroke-width="2" />
  <line x1="355" y1="153" x2="445" y2="153" stroke="#828282" stroke-width="2" />
  <text x="400" y="180" fill="#828282" font-size="10" text-anchor="middle">bigliettino nuovo</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Solo poche righe vengono riscritte; il resto del diario resta intoccato, pagina dopo pagina.</figcaption>
</figure>

### 4.6 Un problema diverso, che nessun diario può risolvere

C'è però un secondo limite, distinto dal primo, che nessuna variante di gating può aggirare, perché non dipende da *come* il bigliettino viene aggiornato ma dal fatto stesso che debba essere aggiornato *una pagina alla volta*: per sapere cosa dice il bigliettino dopo la pagina 50, devi prima sapere cosa diceva dopo la pagina 49, che a sua volta richiede la 48, e così via fino alla prima. Il tuo amico non può leggere due pagine contemporaneamente — anche avendo a disposizione altri cento amici pronti ad aiutarlo, la lettura di un libro di 500 pagine richiederebbe comunque 500 passaggi, uno dopo l'altro, nessuno saltabile. È esattamente questo vincolo — non il bigliettino che si sporca, già in parte curato dalla Sezione 4.5 — il motivo per cui, quando è diventato possibile allenare su quantità di testo enormi con hardware capace di lavorare su migliaia di cose insieme, questo tipo di architettura è diventato il vero collo di bottiglia da superare. La Lezione 8 riprenderà esattamente questo secondo problema.

---

> **Prova tu — Il telefono senza fili**
>
> Gioca (o immagina di giocare) al gioco del telefono senza fili con una catena di sei persone, in fila. Il messaggio di partenza, sussurrato alla prima persona, è: *"Il gatto grigio di Marta ha dormito tutto il pomeriggio sul davanzale della finestra."*
>
> Ogni persona può sussurrare all'orecchio del vicino solo *quello che ricorda* del messaggio ricevuto — non può farselo ripetere, non può scriverlo. Prova a immaginare (o a far provare a cinque amici) come si trasforma il messaggio, persona dopo persona.
>
> 1. Quali parti del messaggio ti aspetti sopravvivano meglio: i dettagli all'inizio ("il gatto grigio di Marta"), quelli del mezzo ("ha dormito tutto il pomeriggio") o quelli alla fine ("sul davanzale della finestra")? Perché?
> 2. Se una delle sei persone, invece di rimescolare tutto alla rinfusa, si impegnasse a ripetere **parola per parola** solo il soggetto della frase ("il gatto grigio di Marta") lasciando che il resto si trasformi liberamente — che tipo di meccanismo della Sezione 4.5 starebbe imitando?
> 3. Con sei persone il messaggio arriva già abbastanza deformato. Cosa ti aspetti succeda con una catena di sessanta persone invece di sei — e quale dei due problemi discussi in questa lezione (bigliettino che si sporca, oppure lettura obbligatoriamente una pagina alla volta) sta peggiorando in quel caso?

---

*Continua con la [Lezione 05 — Quanto costa sbagliare]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-05-quanto-costa-sbagliare.md %}.html)*
