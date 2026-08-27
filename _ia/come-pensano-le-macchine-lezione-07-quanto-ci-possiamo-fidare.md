---
title: 'Lezione 07, Quanto ci possiamo fidare'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un quadrante a semicerchio mostra l'affidabilità di un chatbot, con la lancetta ferma a metà tra poco e molto affidabile](/images/ia/come-pensano-le-macchine-lezione-07-quanto-ci-possiamo-fidare/come-pensano-le-macchine-lezione-07-quanto-ci-possiamo-fidare.svg){:class="aside-image"}

### 7.1 Come si dà un voto a un chatbot?

Se due aziende dicono entrambe "il nostro modello è il più bravo", come si stabilisce chi ha ragione? Serve un modo oggettivo di misurare le capacità di un LLM, esattamente come si fa un'interrogazione a scuola per misurare quanto uno studente ha imparato. La soluzione più diffusa sono i **benchmark**: enormi raccolte di domande con risposta nota (problemi di matematica, quesiti di cultura generale, esercizi di programmazione, domande a scelta multipla su decine di materie), su cui si fa "sostenere l'esame" al modello e si conta quante risposte azzecca.

Sembra semplice e onesto. Ha però un tallone d'Achille pratico, sorprendentemente simile a un problema che conosci bene dalla tua vita scolastica.

### 7.2 Il problema del compito già visto

Immagina un'interrogazione in cui, per puro caso, le domande sono identiche a quelle di un compito che hai già svolto e corretto in classe la settimana prima. Prenderesti un voto altissimo, ma quel voto non misurerebbe affatto quanto hai capito l'argomento: misurerebbe solo quanto ti ricordi di quel compito specifico.

Lo stesso rischio vale, in modo ancora più insidioso, per un LLM: se il modello è stato addestrato leggendo praticamente "tutto il web", è più che plausibile che le domande di un benchmark famoso, insieme alle loro risposte corrette, fossero già presenti, magari alla lettera, da qualche parte nei miliardi di pagine di testo di addestramento. Un modello che ottiene un punteggio altissimo su un benchmark del genere potrebbe semplicemente **ricordarselo**, non saperlo ragionare da zero. Questo problema si chiama **contaminazione del test**, ed è uno dei motivi per cui i punteggi sui benchmark vanno sempre presi con un pizzico di scetticismo in più di quanto sembrerebbe a prima vista.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 340" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="venn-title venn-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="venn-title">La contaminazione del test</title>
  <desc id="venn-desc">Due cerchi che si sovrappongono: il testo di addestramento e le domande del benchmark. Nella zona di sovrapposizione, il modello ha già visto la domanda e la risposta durante l'addestramento.</desc>

  <circle cx="200" cy="180" r="140" fill="#2a7ae2" opacity="0.22" stroke="#2a7ae2" stroke-width="2" />
  <circle cx="340" cy="180" r="100" fill="#f66a0a" opacity="0.22" stroke="#f66a0a" stroke-width="2" />

  <text x="110" y="70" fill="#1d5eb8" font-size="13" font-weight="bold" text-anchor="middle">testo di addestramento</text>
  <text x="110" y="88" fill="#1d5eb8" font-size="11" text-anchor="middle">(miliardi di pagine)</text>

  <text x="410" y="90" fill="#c85506" font-size="13" font-weight="bold" text-anchor="middle">domande del</text>
  <text x="410" y="106" fill="#c85506" font-size="13" font-weight="bold" text-anchor="middle">benchmark</text>

  <rect x="225" y="150" width="130" height="60" rx="8" fill="#fdfdfd" stroke="#c85506" stroke-width="1.5" />
  <text x="290" y="172" fill="#111" font-size="11" font-weight="bold" text-anchor="middle">contaminazione:</text>
  <text x="290" y="187" fill="#111" font-size="11" text-anchor="middle">domanda (e risposta)</text>
  <text x="290" y="202" fill="#111" font-size="11" text-anchor="middle">già viste</text>

  <text x="405" y="180" fill="#c85506" font-size="11" text-anchor="middle">domande</text>
  <text x="405" y="195" fill="#c85506" font-size="11" text-anchor="middle">genuine</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Se una domanda del benchmark ricade nella zona di sovrapposizione, un punteggio alto misura la memoria, non il ragionamento.</figcaption>
</figure>

### 7.3 Chiedere a un altro modello di fare da giudice

Molte domande interessanti, "questa risposta è ben scritta?", "questa spiegazione è chiara?", non hanno una singola risposta giusta da confrontare meccanicamente, come invece succede con "quanto fa 12×7?". Per queste, si è diffusa una tecnica un po' sorprendente: usare un **secondo LLM come giudice**, mostrandogli due risposte diverse alla stessa domanda e chiedendogli quale preferisce, un po' come chiedere a un insegnante esperto di leggere due temi e dire quale è meglio scritto, invece di contare solo gli errori di grammatica. Funziona sorprendentemente bene, ma eredita gli stessi limiti e le stesse preferenze "di gusto" (a volte anche gli stessi pregiudizi) del modello-giudice scelto, e i modelli-giudice, come vedremo nella Lezione 8, non sono affatto immuni dal preferire risposte lunghe e sicure di sé anche quando sono sbagliate.

Un'alternativa complementare sono le **arene**: piattaforme dove persone in carne e ossa confrontano alla cieca le risposte di due chatbot diversi alla stessa domanda, senza sapere quale modello ha prodotto quale risposta, e votano quella che preferiscono. Su tantissimi confronti, emerge una classifica, più simile a una classifica di gradimento del pubblico che a un esame oggettivo, ma proprio per questo utile a catturare aspetti (tono, utilità percepita) che un semplice punteggio su un test a crocette non cattura.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="giudice-title giudice-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="giudice-title">Giudice-LLM contro arena</title>
  <desc id="giudice-desc">Entrambi i metodi partono dalla stessa domanda con due risposte A e B. Nel giudice-LLM, un secondo modello legge entrambe e sceglie. Nell'arena, persone vere confrontano alla cieca e votano.</desc>

  <defs>
    <marker id="arrowG7" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="140" y="20" width="280" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="280" y="49" fill="#111" font-size="13" text-anchor="middle">Stessa domanda, due risposte: A e B</text>

  <path d="M 260,70 Q 200,90 140,110" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowG7)" />
  <path d="M 300,70 Q 360,90 420,110" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowG7)" />

  <text x="140" y="100" fill="#7b4fd1" font-size="15" font-weight="bold" text-anchor="middle">Giudice-LLM</text>
  <text x="420" y="100" fill="#2c7f3f" font-size="15" font-weight="bold" text-anchor="middle">Arena</text>

  <rect x="40" y="110" width="200" height="60" rx="8" fill="#e6dcfb" stroke="#7b4fd1" stroke-width="1.5" />
  <text x="140" y="136" fill="#111" font-size="12" text-anchor="middle">Un secondo LLM legge</text>
  <text x="140" y="152" fill="#111" font-size="12" text-anchor="middle">entrambe le risposte</text>

  <path d="M 140,170 L 140,200" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowG7)" />

  <rect x="40" y="200" width="200" height="60" rx="8" fill="#e6dcfb" stroke="#7b4fd1" stroke-width="1.5" />
  <text x="140" y="226" fill="#111" font-size="12" text-anchor="middle">Sceglie quale preferisce</text>
  <text x="140" y="242" fill="#111" font-size="12" text-anchor="middle">(gusto, chiarezza...)</text>

  <text x="140" y="285" fill="#828282" font-size="11" text-anchor="middle">veloce, ma eredita i pregiudizi</text>
  <text x="140" y="299" fill="#828282" font-size="11" text-anchor="middle">del modello-giudice</text>

  <rect x="320" y="110" width="200" height="60" rx="8" fill="#dcf3e4" stroke="#3aa655" stroke-width="1.5" />
  <text x="420" y="136" fill="#111" font-size="12" text-anchor="middle">Persone vere confrontano</text>
  <text x="420" y="152" fill="#111" font-size="12" text-anchor="middle">alla cieca</text>

  <path d="M 420,170 L 420,200" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowG7)" />

  <rect x="320" y="200" width="200" height="60" rx="8" fill="#dcf3e4" stroke="#3aa655" stroke-width="1.5" />
  <text x="420" y="226" fill="#111" font-size="12" text-anchor="middle">Votano quella</text>
  <text x="420" y="242" fill="#111" font-size="12" text-anchor="middle">che preferiscono</text>

  <text x="420" y="285" fill="#828282" font-size="11" text-anchor="middle">più lento, ma cattura il</text>
  <text x="420" y="299" fill="#828282" font-size="11" text-anchor="middle">gradimento reale del pubblico</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Due modi di giudicare senza una risposta oggettiva, con compromessi opposti tra velocità e affidabilità.</figcaption>
</figure>

### 7.4 Capacità che sembrano spuntare dal nulla

Un fenomeno molto discusso, e spesso raccontato in modo un po' troppo magico, è quello delle cosiddette **capacità emergenti**: alcuni compiti (ad esempio la capacità di risolvere un problema aritmetico a più passaggi) sembrano restare a un livello di prestazione piatto e mediocre man mano che un modello cresce di dimensione, finché, superata una certa soglia di grandezza, il punteggio schizza improvvisamente verso l'alto, come se il modello avesse acquisito una capacità completamente nuova di colpo.

Un gruppo di ricercatori ha però mostrato qualcosa di interessante: buona parte di questi "salti improvvisi" **sono in realtà un'illusione creata dal modo in cui si misura**, non un vero salto nel comportamento del modello. Se un compito viene valutato in modo "tutto o niente" (la risposta finale è giusta o sbagliata, punto), un modello che migliora gradualmente e con continuità, sbagliando un dettaglio in meno a ogni passaggio interno del ragionamento, può restare bloccato a "risposta finale sbagliata" per molto tempo, per poi passare improvvisamente a "risposta finale giusta" nel momento esatto in cui l'ultimo dettaglio residuo viene azzeccato. Misurando invece con un metro più fine (quanti passaggi intermedi del ragionamento sono corretti, non solo il verdetto finale) la stessa identica curva risulta liscia e graduale, non a scalino. La capacità "emergente", insomma, spesso non emerge affatto dal nulla: emergeva già gradualmente, ma la misura usata era troppo grossolana per accorgersene.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 340" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="emerg-title emerg-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="emerg-title">L'illusione delle capacità emergenti</title>
  <desc id="emerg-desc">Due curve sullo stesso grafico, dimensione del modello sull'asse orizzontale e punteggio sull'asse verticale. La curva arancione, misurata tutto o niente, resta piatta e poi salta improvvisamente. La curva blu, misurata a grana fine sugli stessi dati, cresce in modo graduale e liscio.</desc>

  <g stroke="#828282" stroke-width="1.5">
    <line x1="60" y1="280" x2="460" y2="280" />
    <line x1="60" y1="280" x2="60" y2="40" />
  </g>
  <text x="260" y="305" fill="#111" font-size="13" text-anchor="middle">dimensione del modello →</text>
  <text x="25" y="160" fill="#111" font-size="13" text-anchor="middle" transform="rotate(-90 25 160)">punteggio sul compito →</text>

  <line x1="275" y1="40" x2="275" y2="280" stroke="#828282" stroke-width="1.5" stroke-dasharray="4 3" />
  <text x="275" y="32" fill="#828282" font-size="11" text-anchor="middle">soglia</text>

  <path d="M60,270 C150,230 350,110 460,60" fill="none" stroke="#2a7ae2" stroke-width="3" />
  <path d="M60,275 C150,272 220,270 250,268 C270,220 285,90 300,70 C350,65 400,62 460,60" fill="none" stroke="#f66a0a" stroke-width="3" />

  <g font-size="11">
    <line x1="75" y1="52" x2="100" y2="52" stroke="#2a7ae2" stroke-width="3" />
    <text x="106" y="56" fill="#111">misura a grana fine (passaggi corretti)</text>
    <line x1="75" y1="70" x2="100" y2="70" stroke="#f66a0a" stroke-width="3" />
    <text x="106" y="74" fill="#111">misura tutto-o-niente (verdetto finale)</text>
  </g>

  <text x="330" y="130" fill="#c85506" font-size="12" text-anchor="start">← sembra un salto improvviso</text>
  <text x="95" y="200" fill="#1d5eb8" font-size="12" text-anchor="start">in realtà cresce</text>
  <text x="95" y="215" fill="#1d5eb8" font-size="12" text-anchor="start">già gradualmente</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Stessi dati, stesso modello: il "salto" appare solo con il metro grossolano, non con quello fine.</figcaption>
</figure>

### 7.5 Insegnargli a "pensare a voce alta"

Un'ultima osservazione utile riguarda un trucco molto semplice quanto efficace: chiedere esplicitamente al modello di scrivere il proprio ragionamento passo passo, prima di dare la risposta finale ("pensiamoci con calma, passo per passo..."), invece di sparare subito il verdetto. Questa tecnica, chiamata **catena di pensiero** (chain-of-thought), spesso migliora sensibilmente l'accuratezza su problemi che richiedono più passaggi logici o aritmetici, un po' come quando un insegnante ti chiede di "mostrare i calcoli" invece di scrivere solo il risultato finale: non è che il foglio in sé ti renda più bravo, ma il fatto di scomporre il problema in passaggi più piccoli e verificabili aiuta a commettere meno errori lungo la strada. Alcuni modelli più recenti vengono ora allenati esplicitamente, con le tecniche della Lezione 5, a produrre ragionamenti interni lunghi ed estesi prima di rispondere, pagando in tempo di attesa quello che guadagnano in accuratezza.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 340" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="cot-title cot-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="cot-title">Risposta diretta contro catena di pensiero</title>
  <desc id="cot-desc">Un problema con più passaggi logici si biforca in due percorsi: la risposta diretta, un solo salto al verdetto finale, e la catena di pensiero, tre passaggi verificabili prima della risposta finale.</desc>

  <defs>
    <marker id="arrowCot" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <rect x="180" y="20" width="200" height="50" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="280" y="41" fill="#111" font-size="12" text-anchor="middle">Problema con più</text>
  <text x="280" y="57" fill="#111" font-size="12" text-anchor="middle">passaggi logici</text>

  <path d="M 240,70 Q 190,90 140,110" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowCot)" />
  <path d="M 320,70 Q 370,90 420,110" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowCot)" />

  <text x="140" y="100" fill="#828282" font-size="13" font-weight="bold" text-anchor="middle">risposta diretta</text>
  <rect x="60" y="110" width="160" height="60" rx="8" fill="#f3f3f3" stroke="#828282" stroke-width="1.5" />
  <text x="140" y="136" fill="#111" font-size="12" text-anchor="middle">Sparo subito</text>
  <text x="140" y="152" fill="#111" font-size="12" text-anchor="middle">il verdetto finale</text>
  <text x="140" y="195" fill="#c85506" font-size="11" text-anchor="middle">✗ più a rischio di errore</text>

  <text x="420" y="100" fill="#1d5eb8" font-size="13" font-weight="bold" text-anchor="middle">catena di pensiero</text>
  <rect x="340" y="110" width="160" height="40" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="420" y="134" fill="#111" font-size="11" text-anchor="middle">1. Scompongo il problema</text>

  <path d="M 420,150 L 420,162" stroke="#828282" stroke-width="1.5" marker-end="url(#arrowCot)" fill="none" />
  <rect x="340" y="164" width="160" height="40" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="420" y="188" fill="#111" font-size="11" text-anchor="middle">2. Risolvo un pezzo alla volta</text>

  <path d="M 420,204 L 420,216" stroke="#828282" stroke-width="1.5" marker-end="url(#arrowCot)" fill="none" />
  <rect x="340" y="218" width="160" height="40" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="420" y="242" fill="#111" font-size="11" text-anchor="middle">3. Verifico e combino</text>

  <path d="M 420,258 L 420,270" stroke="#828282" stroke-width="1.5" marker-end="url(#arrowCot)" fill="none" />
  <rect x="340" y="272" width="160" height="40" rx="6" fill="#dceafc" stroke="#2a7ae2" stroke-width="2" />
  <text x="420" y="297" fill="#111" font-size="11" font-weight="bold" text-anchor="middle">✓ Risposta finale</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Scomporre il problema in passaggi verificabili aiuta a commettere meno errori lungo la strada.</figcaption>
</figure>

---

> **Prova tu, Il quiz-trappola**
>
> Ecco un mini-benchmark di quattro domande. Il tuo compito non è rispondere alle domande, ma **fare da revisore**: per ciascuna, decidi se ti sembra "sospetta", cioè, se pensi sia plausibile che un modello l'abbia già vista, identica o quasi, da qualche parte nel suo testo di addestramento, oppure "genuina", pensata apposta per essere nuova.
>
> 1. "Qual è la capitale della Francia?"
> 2. "Se un fruttivendolo di Cuneo compra 47 casse da 23 mele ciascuna a 0,34 € a mela, e ne rivende i tre quarti maggiorando il prezzo del 22%, quanto guadagna in tutto, arrotondato al centesimo?"
> 3. "Quanto fa 2 + 2?"
> 4. "Descrivi in due frasi la trama del romanzo immaginario 'Il sentiero di vetro spezzato' di un autore inventato apposta per questo esercizio."
>
> Per ciascuna, scrivi "sospetta" o "genuina" e il perché. Suggerimento: pensa a quante volte, in tutto il web, potresti aspettarti di trovare scritta *esattamente* quella domanda (o una sua variante quasi identica) insieme alla risposta. Confronta il tuo ragionamento con l'Appendice A.

---

## Esercizi

1. Spiega con parole tue cosa significa "contaminazione del test" per un benchmark di un LLM, usando l'analogia dell'interrogazione con le domande già viste.
2. Descrivi la differenza fra usare un secondo LLM come giudice e usare un'arena con persone vere per confrontare due chatbot. Quale dei due limiti principali, la velocità contro i pregiudizi ereditati, ti sembra più preoccupante, e perché?
3. Spiega, con parole tue, perché alcune "capacità emergenti" potrebbero essere un'illusione dovuta al modo in cui vengono misurate, piuttosto che un vero salto improvviso nel comportamento del modello.
4. Descrivi cosa significa la tecnica della "catena di pensiero" (chain-of-thought), e perché chiedere al modello di ragionare passo passo spesso migliora l'accuratezza su problemi complessi.
5. Pensa a una domanda che ritieni "genuina" (poco probabile che un modello l'abbia già vista identica durante l'addestramento) e a una che ritieni "sospetta" (probabilmente già vista). Scrivile entrambe e spiega il tuo ragionamento.

---

*Continua con la [Lezione 08, Perché a volte sbaglia]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-08-perche-a-volte-sbaglia.md %}.html)*
