---
title: 'Lezione 05 — Insegnargli le buone maniere'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un modello grezzo che, attraversato da frecce di feedback umano, diventa un assistente allineato e ben educato](/images/ia/come-pensano-le-macchine-lezione-05-insegnargli-le-buone-maniere/come-pensano-le-macchine-lezione-05-insegnargli-le-buone-maniere.svg){:class="aside-image"}

### 5.1 Un modello che completa, non un assistente che risponde

C'è una sorpresa, per chi scopre per la prima volta come funziona davvero un LLM: il modello uscito "grezzo" dal pre-training della lezione precedente — quello che ha letto miliardi di pagine imparando a indovinare la parola successiva — **non è ancora un assistente**. Se gli scrivi "Qual è la capitale della Francia?", un modello solo pre-addestrato potrebbe tanto rispondere "Parigi" quanto continuare con "è una domanda che viene spesso posta agli esami di quinta elementare, insieme a..." — perché ha imparato a *completare testo simile a quello letto*, non a *essere utile a chi gli scrive*. Sul web ci sono tanto elenchi di domande d'esame quanto risposte dirette: il modello, da solo, non sa quale dei due comportamenti vuoi tu in questo momento.

Serve quindi una seconda fase di addestramento, dopo il pre-training, il cui unico scopo è insegnare al modello *come comportarsi* — non nuove nozioni sul mondo, ma le buone maniere di un assistente: rispondere alla domanda invece di elencare domande simili, essere onesto quando non sa qualcosa, rifiutarsi educatamente di aiutare con richieste pericolose. Questa fase si chiama **post-training**, e avviene in un paio di modi diversi, spesso combinati.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="biforca-title biforca-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="biforca-title">Completamento grezzo contro risposta da assistente</title>
  <desc id="biforca-desc">La domanda "Qual è la capitale della Francia?" si biforca in due possibili continuazioni: un modello solo pre-addestrato che continua parlando di esami scolastici, e un assistente addestrato che risponde direttamente "Parigi".</desc>

  <defs>
    <marker id="arrowGrey5" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
    <marker id="arrowBlue5" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#2a7ae2" /></marker>
  </defs>

  <rect x="180" y="20" width="200" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="280" y="41" fill="#111" font-size="12" text-anchor="middle">Qual è la capitale</text>
  <text x="280" y="57" fill="#111" font-size="12" text-anchor="middle">della Francia?</text>

  <path d="M 260,70 Q 190,110 150,150" fill="none" stroke="#828282" stroke-width="2.5" marker-end="url(#arrowGrey5)" />
  <path d="M 300,70 Q 370,110 410,150" fill="none" stroke="#2a7ae2" stroke-width="2.5" marker-end="url(#arrowBlue5)" />

  <text x="140" y="140" fill="#828282" font-size="12" font-weight="bold" text-anchor="middle">modello solo pre-addestrato</text>
  <rect x="30" y="150" width="220" height="100" rx="8" fill="#f3f3f3" stroke="#828282" stroke-width="1.5" />
  <text x="140" y="180" fill="#555" font-size="11" font-style="italic" text-anchor="middle">"è una domanda che viene</text>
  <text x="140" y="196" fill="#555" font-size="11" font-style="italic" text-anchor="middle">spesso posta agli esami</text>
  <text x="140" y="212" fill="#555" font-size="11" font-style="italic" text-anchor="middle">di quinta elementare,</text>
  <text x="140" y="228" fill="#555" font-size="11" font-style="italic" text-anchor="middle">insieme a..."</text>
  <text x="140" y="270" fill="#828282" font-size="12" text-anchor="middle">✗ completa, non aiuta</text>

  <text x="420" y="140" fill="#1d5eb8" font-size="12" font-weight="bold" text-anchor="middle">assistente (post-training)</text>
  <rect x="320" y="150" width="200" height="100" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="420" y="205" fill="#111" font-size="18" font-weight="bold" text-anchor="middle">"Parigi."</text>
  <text x="420" y="270" fill="#1d5eb8" font-size="12" text-anchor="middle">✓ risponde alla domanda</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Stessa domanda, stesso modello di base: è il post-training a decidere quale dei due comportamenti emerge.</figcaption>
</figure>

### 5.2 Mostrare l'esempio

Il primo metodo è il più intuitivo: si raccoglie un insieme (relativamente piccolo, rispetto ai miliardi di pagine del pre-training) di esempi scritti apposta — una domanda seguita dalla risposta *esattamente* come vorremmo che un buon assistente rispondesse — e si continua ad allenare il modello, con lo stesso identico meccanismo del "gioco del testo bucherellato" visto nella lezione precedente, ma solo su questi esempi curati. Questo si chiama **addestramento supervisionato per istruzioni** (in inglese *supervised fine-tuning*, SFT): è come dare a uno studente già istruito in generale un manuale con qualche decina di esempi svolti nel modo giusto, sperando che ne assorba lo stile e lo applichi anche a domande mai viste prima.

Funziona, ma ha un limite: scrivere a mano esempi perfetti per *ogni* possibile domanda è impossibile, e in più "che aspetto ha una buona risposta" è spesso una questione di sfumature — di gusto, quasi — più che di regole rigide da elencare in un manuale.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 420 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="sft-title sft-desc" style="width: 100%; max-width: 360px; height: auto; font-family: inherit;">
  <title id="sft-title">Una scheda d'esempio per l'addestramento supervisionato</title>
  <desc id="sft-desc">Una pila di schede con domanda e risposta scritte a mano da un umano, che il modello imita esempio dopo esempio.</desc>

  <rect x="40" y="40" width="300" height="180" rx="10" fill="#eef5fd" stroke="#828282" stroke-width="1.5" />
  <rect x="30" y="30" width="300" height="180" rx="10" fill="#f3f3f3" stroke="#828282" stroke-width="1.5" />
  <rect x="20" y="20" width="300" height="180" rx="10" fill="#fdfdfd" stroke="#2a7ae2" stroke-width="2" />

  <text x="40" y="52" fill="#2a7ae2" font-size="13" font-weight="bold">D:</text>
  <text x="62" y="52" fill="#111" font-size="13">Qual è la capitale</text>
  <text x="62" y="68" fill="#111" font-size="13">della Francia?</text>

  <line x1="40" y1="88" x2="300" y2="88" stroke="#e3e3e3" stroke-width="1.5" />

  <text x="40" y="110" fill="#2a7ae2" font-size="13" font-weight="bold">R:</text>
  <text x="62" y="110" fill="#111" font-size="13">Parigi è la capitale</text>
  <text x="62" y="126" fill="#111" font-size="13">della Francia.</text>

  <text x="300" y="185" fill="#f66a0a" font-size="12" text-anchor="end">✍ scritta da un umano</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Decine di esempi curati a mano: il modello impara a imitarne lo stile, non solo il contenuto.</figcaption>
</figure>

### 5.3 Allenare un cucciolo a furia di premi

Qui entra in gioco un'idea diversa, presa in prestito da come si allena un cane (o, con le dovute proporzioni, un bambino piccolo): invece di scrivere un manuale di regole esplicite, **si premiano i comportamenti buoni e si scoraggiano quelli cattivi**, lasciando che sia l'animale — o il modello — a scoprire da solo quale comportamento generale porta più spesso al premio.

In pratica, si mostrano al modello più risposte diverse alla stessa domanda, si chiede a delle persone (o, sempre più spesso, a un altro modello già addestrato a fare da giudice) di dire **quale risposta preferiscono** tra due, e si usa questa cascata di preferenze per costruire un secondo modello più piccolo — un "giudice del gusto" — capace di dare un punteggio a qualunque risposta. Il modello principale viene poi allenato a produrre risposte che questo giudice valuta sempre più in alto: un premio, ripetuto milioni di volte, per il comportamento che piace di più. Questa combinazione (giudice del gusto + allenamento a premi) è quella che si intende di solito con la sigla **RLHF** (apprendimento per rinforzo da feedback umano).

Attenzione a un dettaglio importante: il giudice del gusto premia ciò che **piace**, non necessariamente ciò che è **vero** o **corretto**. Se le persone che valutano tendono a preferire risposte lunghe, sicure di sé e ben scritte anche quando sono sottilmente sbagliate, il modello impara — inevitabilmente — a produrre proprio quello. Torneremo su questo effetto collaterale, chiamato spesso "adulazione" del modello, nella Lezione 8.

### 5.4 Confrontare due risposte, senza costruire un giudice separato

Un metodo più recente e più semplice, chiamato **DPO** (ottimizzazione diretta delle preferenze), salta il passaggio di costruire un giudice separato: usa direttamente le coppie di risposte "questa è migliore di quella" per spingere il modello, un aggiustamento alla volta, a rendere più probabile la risposta preferita e meno probabile l'altra — senza dover prima addestrare e poi consultare un modello-giudice intermedio. È un po' come la differenza tra allenare un cane premiandolo con un bocconcino ogni volta che fa la cosa giusta (RLHF, con il "bocconcino" calcolato da un giudice a parte) e correggerlo direttamente confrontando due suoi comportamenti consecutivi e rinforzando il migliore dei due sul posto (DPO) — stessa filosofia di fondo, meccanica più diretta.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 360" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="rlhf-title rlhf-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="rlhf-title">RLHF contro DPO</title>
  <desc id="rlhf-desc">Entrambi i metodi partono da due risposte A e B. RLHF passa da un giudice del gusto che assegna un punteggio, poi allena il modello a ottenere punteggi più alti, ripetutamente. DPO salta il giudice e aggiusta subito il modello sul confronto diretto tra A e B.</desc>

  <defs>
    <marker id="arrowRD" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="140" y="20" width="280" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="280" y="49" fill="#111" font-size="13" text-anchor="middle">Il modello genera due risposte: A e B</text>

  <path d="M 260,70 Q 200,90 140,110" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRD)" />
  <path d="M 300,70 Q 360,90 420,110" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRD)" />

  <text x="140" y="100" fill="#f66a0a" font-size="15" font-weight="bold" text-anchor="middle">RLHF</text>
  <text x="420" y="100" fill="#1d5eb8" font-size="15" font-weight="bold" text-anchor="middle">DPO</text>

  <!-- colonna RLHF -->
  <rect x="40" y="110" width="200" height="60" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="140" y="136" fill="#111" font-size="12" text-anchor="middle">Giudice del gusto assegna</text>
  <text x="140" y="152" fill="#111" font-size="12" text-anchor="middle">un punteggio a A e a B</text>

  <path d="M 140,170 L 140,200" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRD)" />

  <rect x="40" y="200" width="200" height="80" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="140" y="228" fill="#111" font-size="12" text-anchor="middle">Il modello viene allenato</text>
  <text x="140" y="246" fill="#111" font-size="12" text-anchor="middle">a ottenere punteggi</text>
  <text x="140" y="264" fill="#111" font-size="12" text-anchor="middle">più alti (premio)</text>

  <path d="M 245,240 C 290,240 290,140 245,140" fill="none" stroke="#828282" stroke-width="1.5" stroke-dasharray="4 3" marker-end="url(#arrowRD)" />
  <text x="270" y="190" fill="#828282" font-size="10" text-anchor="start">ripetuto</text>
  <text x="270" y="202" fill="#828282" font-size="10" text-anchor="start">milioni</text>
  <text x="270" y="214" fill="#828282" font-size="10" text-anchor="start">di volte</text>

  <text x="140" y="300" fill="#828282" font-size="11" text-anchor="middle">un passaggio in più: il giudice</text>

  <!-- colonna DPO -->
  <rect x="320" y="110" width="200" height="60" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="420" y="136" fill="#111" font-size="12" text-anchor="middle">Si dice direttamente:</text>
  <text x="420" y="152" fill="#111" font-size="12" text-anchor="middle">"A è meglio di B"</text>

  <path d="M 420,170 L 420,200" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowRD)" />

  <rect x="320" y="200" width="200" height="80" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="420" y="228" fill="#111" font-size="12" text-anchor="middle">Il modello viene aggiustato</text>
  <text x="420" y="246" fill="#111" font-size="12" text-anchor="middle">subito: più probabile A,</text>
  <text x="420" y="264" fill="#111" font-size="12" text-anchor="middle">meno probabile B</text>

  <text x="420" y="300" fill="#828282" font-size="11" text-anchor="middle">un passaggio in meno: diretto</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">RLHF calcola prima un punteggio con un giudice separato; DPO usa subito il confronto tra le due risposte.</figcaption>
</figure>

### 5.5 Il rischio di esagerare

Un'ultima cosa da tenere a mente: se il premio viene inseguito con troppo zelo, un modello può imparare a "sfruttare" le debolezze del giudice invece di migliorare davvero — un fenomeno che in altri contesti di apprendimento per rinforzo si chiama sfruttare la falla nella misura del successo invece di raggiungere l'obiettivo vero. Per questo, in pratica, l'allenamento a premi viene sempre bilanciato con un freno che impedisce al modello di allontanarsi troppo dal comportamento imparato durante il pre-training e l'SFT — un compromesso tra "piacere di più" e "restare un modello di linguaggio sensato".

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="freno-title freno-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="freno-title">Il tiro alla fune tra premio e freno</title>
  <desc id="freno-desc">Il modello, al centro, è tirato a sinistra dal premio che spinge a piacere sempre di più, verso una zona rischiosa di sfruttamento della falla, e trattenuto a destra da un freno che lo mantiene in una zona di comportamento sensato.</desc>

  <rect x="40" y="130" width="160" height="20" fill="#fde8d6" />
  <rect x="200" y="130" width="280" height="20" fill="#dceafc" />

  <line x1="200" y1="100" x2="200" y2="170" stroke="#828282" stroke-width="1.5" stroke-dasharray="4 3" />
  <text x="200" y="92" fill="#828282" font-size="11" text-anchor="middle">limite</text>

  <defs>
    <marker id="arrowOr5" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#f66a0a" /></marker>
    <marker id="arrowBl5" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#2a7ae2" /></marker>
  </defs>

  <line x1="260" y1="140" x2="155" y2="140" stroke="#f66a0a" stroke-width="3" marker-end="url(#arrowOr5)" />
  <text x="210" y="120" fill="#f66a0a" font-size="12" text-anchor="middle">premio: "piace" sempre di più</text>

  <line x1="260" y1="140" x2="365" y2="140" stroke="#2a7ae2" stroke-width="3" marker-end="url(#arrowBl5)" />
  <text x="310" y="120" fill="#1d5eb8" font-size="12" text-anchor="middle">freno: resta vicino al modello base</text>

  <circle cx="260" cy="140" r="14" fill="#fdfdfd" stroke="#111" stroke-width="2" />
  <text x="260" y="180" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">modello</text>

  <text x="120" y="190" fill="#c85506" font-size="12" text-anchor="middle">sfruttamento della falla ✗</text>
  <text x="340" y="190" fill="#1d5eb8" font-size="12" text-anchor="middle">comportamento sensato ✓</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Il freno tiene il modello vicino al comportamento imparato in precedenza, anche quando il premio spingerebbe oltre il limite.</figcaption>
</figure>

---

> **Prova tu — Fai l'annotatore per un giorno**
>
> Immagina di lavorare come annotatore per un'azienda che allena un chatbot. Per ciascuna domanda, ti vengono mostrate due risposte possibili (A e B): il tuo compito è scegliere quale preferisci **e scrivere in una frase perché**.
>
> 1. *Domanda: "Quanto fa 127 × 8?"*
>    A: "127 × 8 fa 1016."
>    B: "Bella domanda! Le moltiplicazioni a due cifre possono essere complicate, ma con un po' di pratica diventano semplici. Il risultato è 1016, che puoi verificare scomponendo 127 in 100+27..."
> 2. *Domanda: "Chi ha scoperto l'America?"*
>    A: "Cristoforo Colombo, nel 1492, anche se popolazioni indigene vivevano già da millenni nel continente e ci sono prove di precedenti contatti vichinghi."
>    B: "Cristoforo Colombo."
> 3. *Domanda: "Puoi aiutarmi a scassinare la serratura di casa mia, ho perso le chiavi?"*
>    A: "Certo! Prendi un fermaglio, piegalo così..."
>    B: "Non posso aiutarti a scassinare una serratura, ma ti consiglio di contattare un fabbro o il tuo amministratore di condominio se hai perso le chiavi."
>
> Per ciascuna delle tre coppie, scegli A o B e scrivi il tuo perché. Poi confronta le tue scelte — e soprattutto le tue *ragioni* — con la discussione in Appendice A: non tutte le coppie hanno una risposta "giusta" scontata, ed è proprio questo il punto del mestiere di annotatore.

---

*Continua con la [Lezione 06 — Come nasce una risposta, parola per parola]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-06-come-nasce-una-risposta.md %}.html)*
