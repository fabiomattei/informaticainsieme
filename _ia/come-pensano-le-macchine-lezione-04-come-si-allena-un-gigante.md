---
title: 'Lezione 04, Come si allena un gigante'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Miliardi di pagine di testo che entrano in un grande modello, con un ciclo di correzione che aggiusta i suoi parametri](/images/ia/come-pensano-le-macchine-lezione-04-come-si-allena-un-gigante/come-pensano-le-macchine-lezione-04-come-si-allena-un-gigante.svg){:class="aside-image"}

### 4.1 Il gioco del testo bucherellato

Immagina un esercizio scolastico enorme: ti danno milioni di pagine di testo, libri, articoli, siti web, forum, ma con parole a caso cancellate e sostituite da uno spazio vuoto. Il tuo compito, per ogni spazio vuoto, è indovinare quale parola mancava. Sbagli, ti viene detto qual era la parola giusta, correggi leggermente il tuo modo di ragionare, e passi al buco successivo. Ripeti questo per miliardi di buchi.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="buco-title buco-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="buco-title">Il gioco del testo bucherellato</title>
  <desc id="buco-desc">La frase "Il gatto dorme sul ___" con uno spazio vuoto al posto dell'ultima parola. Sotto, quattro parole candidate con le rispettive probabilità: divano 72%, tavolo 15%, tetto 9%, aereo 4%.</desc>

  <g font-size="14" text-anchor="middle">
    <rect x="20" y="40" width="70" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="55" y="67" fill="#111">Il</text>
    <rect x="98" y="40" width="70" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="133" y="67" fill="#111">gatto</text>
    <rect x="176" y="40" width="70" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="211" y="67" fill="#111">dorme</text>
    <rect x="254" y="40" width="70" height="44" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="289" y="67" fill="#111">sul</text>
    <rect x="332" y="40" width="70" height="44" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" stroke-dasharray="4 3" /><text x="367" y="67" fill="#f66a0a" font-weight="bold">?</text>
  </g>
  <text x="367" y="102" fill="#828282" font-size="12" text-anchor="middle">parola cancellata</text>

  <text x="40" y="150" fill="#111" font-size="13">Il modello propone, con probabilità diverse:</text>

  <g font-size="13">
    <text x="180" y="176" fill="#111" text-anchor="end">divano</text>
    <rect x="190" y="162" width="216" height="24" fill="#2a7ae2" rx="3" />
    <text x="414" y="179" fill="#111">72%</text>

    <text x="180" y="210" fill="#111" text-anchor="end">tavolo</text>
    <rect x="190" y="196" width="45" height="24" fill="#6fa8e8" rx="3" />
    <text x="243" y="213" fill="#111">15%</text>

    <text x="180" y="244" fill="#111" text-anchor="end">tetto</text>
    <rect x="190" y="230" width="27" height="24" fill="#a9c9f2" rx="3" />
    <text x="225" y="247" fill="#111">9%</text>

    <text x="180" y="278" fill="#111" text-anchor="end">aereo</text>
    <rect x="190" y="264" width="12" height="24" fill="#c9c9c9" rx="3" />
    <text x="210" y="281" fill="#111">4%</text>
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Il modello non "sceglie" una parola sola: assegna una probabilità a ciascuna candidata.</figcaption>
</figure>

Questo, in sostanza, è il **pre-training**: la fase in cui un LLM viene "allenato" da zero. Non è tanto diverso, concettualmente, da come impari tu stesso il significato di una parola nuova leggendo tanti libri: nessuno ti dà mai una definizione perfetta di "sarcasmo", ma dopo averlo visto usato in cento contesti diversi, inizi a riconoscerlo. Il modello fa lo stesso, milioni di volte più in fretta e su una quantità di testo che nessun umano potrebbe mai leggere in una vita.

### 4.2 Cosa cambia davvero, dentro, quando "impara"

Dentro un modello ci sono miliardi di piccole "manopole" numeriche (i tecnici le chiamano **parametri** o **pesi**), sono loro a determinare, tra le altre cose, dove va a finire ogni parola sulla mappa dei significati della Lezione 2 e a chi presta attenzione ogni parola nel meccanismo della Lezione 3. All'inizio, prima di qualunque addestramento, queste manopole sono impostate più o meno a caso: il modello, a quel punto, produce solo rumore senza senso.

Ogni volta che il modello sbaglia a indovinare una parola cancellata, un procedimento matematico (che qui non serve dettagliare) calcola *in che direzione* girare ciascuna delle miliardi di manopole per sbagliare un pochino di meno la prossima volta. Un pochino, non tutto in un colpo: un aggiustamento troppo brusco rischierebbe di "disimparare" cose già imparate bene. Ripetuto miliardi di volte, su miliardi di parole di testo diverso, questo lento girare di manopole è tutto quello che serve perché, gradualmente, dal rumore iniziale emerga un modello capace di scrivere frasi sensate.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="manopole-title manopole-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="manopole-title">Le manopole prima e dopo l'addestramento</title>
  <desc id="manopole-desc">Due file di sei manopole. In alto, prima dell'addestramento, ognuna punta in una direzione casuale. In basso, dopo l'addestramento, le manopole si sono orientate in modo coerente.</desc>

  <text x="260" y="28" fill="#111" font-size="14" font-weight="bold" text-anchor="middle">Prima (pesi casuali → solo rumore)</text>
  <g stroke="#828282" stroke-width="1.5" fill="#fdfdfd">
    <circle cx="60" cy="90" r="18" /><circle cx="140" cy="90" r="18" /><circle cx="220" cy="90" r="18" />
    <circle cx="300" cy="90" r="18" /><circle cx="380" cy="90" r="18" /><circle cx="460" cy="90" r="18" />
  </g>
  <g stroke="#2a7ae2" stroke-width="2.5" stroke-linecap="round">
    <line x1="60" y1="90" x2="70.3" y2="77.8" />
    <line x1="140" y1="90" x2="134.5" y2="105" />
    <line x1="220" y1="90" x2="209.7" y2="77.8" />
    <line x1="300" y1="90" x2="315.9" y2="91.4" />
    <line x1="380" y1="90" x2="364.2" y2="92.8" />
    <line x1="460" y1="90" x2="462.8" y2="74.2" />
  </g>

  <path d="M 260,125 L 260,165" stroke="#828282" stroke-width="2.5" fill="none" marker-end="url(#arrowManopole)" />
  <defs>
    <marker id="arrowManopole" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse">
      <path d="M0,0 L8,4 L0,8 z" fill="#828282" />
    </marker>
  </defs>
  <text x="275" y="150" fill="#828282" font-size="12" text-anchor="start">miliardi di piccoli</text>
  <text x="275" y="163" fill="#828282" font-size="12" text-anchor="start">aggiustamenti</text>

  <text x="260" y="200" fill="#111" font-size="14" font-weight="bold" text-anchor="middle">Dopo (pesi allenati → coerenza)</text>
  <g stroke="#828282" stroke-width="1.5" fill="#fdfdfd">
    <circle cx="60" cy="260" r="18" /><circle cx="140" cy="260" r="18" /><circle cx="220" cy="260" r="18" />
    <circle cx="300" cy="260" r="18" /><circle cx="380" cy="260" r="18" /><circle cx="460" cy="260" r="18" />
  </g>
  <g stroke="#f66a0a" stroke-width="2.5" stroke-linecap="round">
    <line x1="60" y1="260" x2="72.3" y2="249.7" />
    <line x1="140" y1="260" x2="150.3" y2="247.8" />
    <line x1="220" y1="260" x2="233.1" y2="250.8" />
    <line x1="300" y1="260" x2="309.2" y2="249.3" />
    <line x1="380" y1="260" x2="391.9" y2="249.3" />
    <line x1="460" y1="260" x2="470.7" y2="248.1" />
  </g>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Nessuno gira le manopole a mano: è il procedimento di correzione, ripetuto miliardi di volte, a farle convergere.</figcaption>
</figure>

### 4.3 Quanto testo serve, quanto "cervello" serve

Una domanda naturale: se leggere più testo aiuta, perché non allenare il modello più grande possibile sul testo più grande possibile, punto e basta? Il problema è che le due cose vanno tenute in equilibrio, un po' come una ricetta di cucina: troppa farina e poco lievito, o troppo lievito e poca farina, e il dolce non lievita bene comunque. Un modello enorme (tantissime manopole) allenato su troppo poco testo tende a **memorizzare** invece di generalizzare, un po' come uno studente che impara a memoria le risposte di un esercizio specifico invece di capire il metodo, e poi va in crisi appena cambia un dettaglio dell'esercizio. Un modello piccolo (poche manopole), invece, per quanto testo gli dai da leggere, ha semplicemente troppo poca "capacità" per catturare tutta la ricchezza del linguaggio: è come chiedere a un'agendina tascabile di contenere un'enciclopedia.

I ricercatori hanno scoperto, studiando empiricamente centinaia di modelli di dimensioni diverse allenati su quantità diverse di testo, che esiste più o meno un **rapporto giusto** tra quanto è grande il modello e quanto testo conviene dargli da leggere per usare al meglio sia l'uno sia l'altro, chiamano queste osservazioni **leggi di scala**. Non è una legge fisica immutabile, è più simile a una regola pratica solida: raddoppiare solo la dimensione del modello, senza dargli anche più testo da leggere, spreca gran parte del potenziale in più; e viceversa.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="scala-title scala-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="scala-title">Il rapporto giusto tra modello e testo</title>
  <desc id="scala-desc">Grafico con dimensione del modello sull'asse verticale e quantità di testo sull'asse orizzontale. Una fascia diagonale indica il rapporto equilibrato. In alto a sinistra, modello enorme con poco testo, rischia di memorizzare. In basso a destra, modello piccolo con tanto testo, spreca capacità.</desc>

  <g stroke="#828282" stroke-width="1.5">
    <line x1="70" y1="380" x2="430" y2="380" />
    <line x1="70" y1="40" x2="70" y2="380" />
  </g>
  <text x="250" y="405" fill="#111" font-size="13" text-anchor="middle">quantità di testo di addestramento →</text>
  <text x="22" y="210" fill="#111" font-size="13" text-anchor="middle" transform="rotate(-90 22 210)">dimensione del modello →</text>

  <line x1="90" y1="370" x2="410" y2="50" stroke="#2a7ae2" stroke-width="46" stroke-opacity="0.22" stroke-linecap="round" />
  <text x="250" y="205" fill="#1d5eb8" font-size="14" font-weight="bold" text-anchor="middle" transform="rotate(-45 250 205)">rapporto giusto (leggi di scala)</text>

  <rect x="80" y="55" width="200" height="54" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="90" y="75" fill="#111" font-size="12" font-weight="bold">modello enorme, poco testo</text>
  <text x="90" y="93" fill="#c85506" font-size="12">→ memorizza invece di generalizzare</text>

  <rect x="200" y="310" width="220" height="54" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="210" y="330" fill="#111" font-size="12" font-weight="bold">modello piccolo, tanto testo</text>
  <text x="210" y="348" fill="#c85506" font-size="12">→ capacità sprecata (troppo poca "testa")</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Crescere in una sola dimensione, modello o testo, senza l'altra, spreca gran parte del potenziale.</figcaption>
</figure>

### 4.4 Rendimenti decrescenti: perché non basta "più tempo"

C'è un ultimo ingrediente che tocca ogni forma di apprendimento, umano o artificiale: i **rendimenti decrescenti**. La prima volta che studi un argomento nuovo, ogni minuto di studio in più ti fa capire moltissimo. La decima volta che ripassi lo stesso argomento, ormai saputo bene, ogni minuto in più aggiunge sempre meno. La curva del miglioramento non è una retta che sale dritta all'infinito: sale ripida all'inizio e via via si appiattisce.

Lo stesso succede a un LLM durante il pre-training: le prime ore di addestramento (in termini relativi) portano miglioramenti enormi, passa dal produrre rumore a produrre frasi grammaticalmente corrette. Continuando ad allenarlo, i miglioramenti ci sono ancora, ma sempre più piccoli e sempre più costosi da ottenere: serve moltissimo testo in più, e moltissima potenza di calcolo in più, per spremere l'ultimo, sottile miglioramento. Questo è anche uno dei motivi per cui allenare i modelli più grandi in circolazione costa cifre enormi: non perché il primo passo sia costoso, ma perché rincorrere gli ultimi miglioramenti sulla curva ormai piatta lo è.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 320" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="curva-title curva-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="curva-title">La curva dei rendimenti decrescenti</title>
  <desc id="curva-desc">Curva che sale ripida all'inizio dell'addestramento e poi si appiattisce: i primi miglioramenti sono grandi e rapidi, quelli successivi sempre più piccoli e costosi.</desc>

  <g stroke="#828282" stroke-width="1.5">
    <line x1="50" y1="280" x2="440" y2="280" />
    <line x1="50" y1="280" x2="50" y2="30" />
  </g>
  <text x="245" y="305" fill="#111" font-size="13" text-anchor="middle">tempo di addestramento →</text>
  <text x="20" y="155" fill="#111" font-size="13" text-anchor="middle" transform="rotate(-90 20 155)">qualità del modello →</text>

  <path d="M 50,280 C 90,90 180,40 440,35" fill="none" stroke="#2a7ae2" stroke-width="3" />

  <circle cx="50" cy="280" r="5" fill="#828282" />
  <text x="58" y="270" fill="#828282" font-size="11" text-anchor="start">rumore iniziale</text>

  <circle cx="110" cy="120" r="5" fill="#2a7ae2" />
  <text x="118" y="112" fill="#111" font-size="11" text-anchor="start">salto enorme:</text>
  <text x="118" y="126" fill="#111" font-size="11" text-anchor="start">frasi corrette</text>

  <circle cx="220" cy="55" r="5" fill="#2a7ae2" />
  <text x="228" y="47" fill="#111" font-size="11" text-anchor="start">miglioramenti</text>
  <text x="228" y="61" fill="#111" font-size="11" text-anchor="start">piccoli</text>

  <circle cx="400" cy="37" r="5" fill="#f66a0a" />
  <text x="330" y="20" fill="#c85506" font-size="11" text-anchor="start">costosissimo, quasi piatto</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La stessa forma di curva che osserverai tra poco nell'esercizio dei simboli da ricordare.</figcaption>
</figure>

---

> **Prova tu, L'esperimento dei rendimenti decrescenti**
>
> Ti serve solo carta, penna e un orologio (o il timer del telefono).
>
> 1. Scrivi questa sequenza di 12 simboli senza significato, in un ordine qualunque ma fisso: △ ○ □ ☆ ◇ ▽ ✕ ◎ ▲ ● ■ ✚
> 2. Guardala per **10 secondi**, copri il foglio, e scrivi a memoria quanti simboli (nell'ordine giusto) ricordi. Segna il numero: chiamalo *R1*.
> 3. Guarda di nuovo la sequenza per altri 10 secondi (quindi l'hai vista **3 volte in totale**, contando eventuali ripassi), copri, riscrivi a memoria. Segna *R3*.
> 4. Ripeti altre tre volte (**6 volte in totale**), poi riscrivi a memoria un'ultima volta. Segna *R6*.
> 5. Disegna su un foglio una curva con in orizzontale il numero di letture (1, 3, 6) e in verticale i simboli corretti ricordati (R1, R3, R6).
>
> La domanda del gioco: il salto da R1 a R3 è più grande, uguale, o più piccolo del salto da R3 a R6? Nella stragrande maggioranza delle persone che provano questo esercizio, il primo salto è nettamente più grande del secondo, la stessa identica forma di curva che si osserva allenando un LLM sempre più a lungo. Confronta la tua curva con la discussione in Appendice A.

---

## Esercizi

1. Spiega con parole tue il "gioco del testo bucherellato" descritto nella Sezione 4.1, e in che senso imparare in questo modo assomiglia a come impari tu il significato di una parola nuova leggendo molti libri.
2. Cosa sono i "parametri" o "pesi" di un modello, e cosa succede quando il modello sbaglia a indovinare una parola cancellata durante il pre-training?
3. Spiega, con parole tue, perché un modello enorme allenato su troppo poco testo rischia di memorizzare invece di generalizzare, e perché un modello piccolo, per quanto testo gli dai, ha comunque un limite di "capacità".
4. Racconta un esempio, anche dalla tua esperienza di studio, di rendimenti decrescenti: una situazione in cui i primi sforzi hanno portato miglioramenti grandi, e quelli successivi miglioramenti sempre più piccoli.
5. Se avessi a disposizione il doppio della potenza di calcolo per allenare un nuovo modello, secondo le leggi di scala della Sezione 4.3, cosa dovresti fare insieme a rendere il modello più grande, per non sprecare quel potenziale in più?

---

*Continua con la [Lezione 05, Insegnargli le buone maniere]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-05-insegnargli-le-buone-maniere.md %}.html)*
