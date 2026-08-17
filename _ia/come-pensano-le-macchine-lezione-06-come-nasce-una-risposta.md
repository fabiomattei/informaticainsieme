---
title: 'Lezione 06 — Come nasce una risposta, parola per parola'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 6.1 Non una risposta, ma un mazzo di probabilità

Ecco un fraintendimento comune da sfatare subito: quando chiedi qualcosa a un chatbot, il modello **non calcola una risposta e basta**. A ogni singolo passo — per ogni singola parola che sta per scrivere — il modello produce internamente qualcosa di più simile a un mazzo di carte, dove ogni carta è una parola possibile e ha scritto sopra quanto è probabile che sia quella giusta. Per continuare "Il gatto si è arrampicato su un...", il mazzo potrebbe avere in cima "albero" con una probabilità alta, "tetto" poco più sotto, "armadio" ancora più giù, e così via fino a migliaia di parole con probabilità infinitesima ("frigorifero", "sottomarino"...).

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 340" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="mazzo-title mazzo-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="mazzo-title">Il mazzo di probabilità per la parola successiva</title>
  <desc id="mazzo-desc">Un ventaglio di carte per la frase "Il gatto si è arrampicato su un...": albero al 55%, tetto al 22%, armadio al 9%, frigorifero quasi 0%, più altre migliaia di carte quasi invisibili.</desc>

  <text x="260" y="24" fill="#111" font-size="14" text-anchor="middle">"Il gatto si è arrampicato su un..."</text>

  <g fill="none" stroke="#e3e3e3" stroke-width="1.5" opacity="0.7">
    <rect x="-40" y="-120" width="80" height="120" rx="8" transform="translate(260,320) rotate(-40)" />
    <rect x="-40" y="-120" width="80" height="120" rx="8" transform="translate(260,320) rotate(-32)" />
    <rect x="-40" y="-120" width="80" height="120" rx="8" transform="translate(260,320) rotate(32)" />
    <rect x="-40" y="-120" width="80" height="120" rx="8" transform="translate(260,320) rotate(40)" />
  </g>

  <g transform="translate(260,320) rotate(-22)">
    <rect x="-45" y="-150" width="90" height="150" rx="10" fill="#2a7ae2" stroke="#1d5eb8" stroke-width="1.5" />
    <text x="0" y="-85" fill="#fdfdfd" font-size="15" font-weight="bold" text-anchor="middle">albero</text>
    <text x="0" y="-65" fill="#fdfdfd" font-size="13" text-anchor="middle">55%</text>
  </g>

  <g transform="translate(260,320) rotate(-7)">
    <rect x="-40" y="-130" width="80" height="130" rx="10" fill="#6fa8e8" stroke="#3a7fd0" stroke-width="1.5" />
    <text x="0" y="-75" fill="#fdfdfd" font-size="14" font-weight="bold" text-anchor="middle">tetto</text>
    <text x="0" y="-57" fill="#fdfdfd" font-size="12" text-anchor="middle">22%</text>
  </g>

  <g transform="translate(260,320) rotate(7)">
    <rect x="-35" y="-110" width="70" height="110" rx="10" fill="#a9c9f2" stroke="#7fa8dc" stroke-width="1.5" />
    <text x="0" y="-62" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">armadio</text>
    <text x="0" y="-46" fill="#111" font-size="11" text-anchor="middle">9%</text>
  </g>

  <g transform="translate(260,320) rotate(22)">
    <rect x="-30" y="-90" width="60" height="90" rx="10" fill="#e3e3e3" stroke="#c9c9c9" stroke-width="1.5" />
    <text x="0" y="-48" fill="#555" font-size="10" text-anchor="middle">frigorifero</text>
    <text x="0" y="-34" fill="#555" font-size="10" text-anchor="middle">~0%</text>
  </g>

  <text x="260" y="335" fill="#828282" font-size="11" text-anchor="middle">...e altre migliaia di carte, sempre più invisibili</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni carta è una parola possibile, con la sua probabilità: più è probabile, più grande e scura è la carta.</figcaption>
</figure>

Il vero problema non è calcolare questo mazzo di probabilità — quello lo abbiamo già visto, è il risultato di tutta la macchina delle lezioni precedenti. Il problema è: **una volta che hai il mazzo, quale carta scegli?**

### 6.2 La scelta più ovvia — e perché è noiosa

La strategia più ovvia è: prendi sempre la carta più probabile. Si chiama scelta **golosa** (greedy). Sembra ragionevole, ma ha un difetto pratico fastidioso: tende a produrre testo ripetitivo e prevedibile, perché ogni volta che c'è un bivio (due parole quasi ugualmente probabili), il modello sceglie sempre e solo la stessa, cadendo facilmente in loop tipo "il gatto è un animale che è un animale che è un animale...". Inoltre, scegliere sempre e solo la carta più alta significa che, a parità di domanda, il modello darebbe **sempre esattamente la stessa risposta, parola per parola** — il che spiegherebbe perché a volte i chatbot sembrano un po' più "vivi" e vari di così.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 560 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="loop-title loop-desc" style="width: 100%; max-width: 520px; height: auto; font-family: inherit;">
  <title id="loop-title">Il loop della scelta golosa</title>
  <desc id="loop-desc">La sequenza di parole "è un animale che" si ripete in loop perché il modello, scegliendo sempre la parola più probabile, torna sempre allo stesso bivio.</desc>

  <defs>
    <marker id="arrowLoop" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M8,0 L0,4 L8,8 z" fill="#f66a0a" /></marker>
  </defs>

  <path d="M 404,45 Q 270,5 148,45" fill="none" stroke="#f66a0a" stroke-width="2" stroke-dasharray="5 3" marker-end="url(#arrowLoop)" />
  <text x="270" y="18" fill="#f66a0a" font-size="12" text-anchor="middle">la carta più probabile è sempre la stessa → si ripete</text>

  <rect x="140" y="52" width="272" height="56" rx="8" fill="none" stroke="#f66a0a" stroke-width="1.5" stroke-dasharray="4 3" />

  <g font-size="13" text-anchor="middle">
    <rect x="20" y="60" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="50" y="85" fill="#111">Il</text>
    <rect x="84" y="60" width="60" height="40" rx="6" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" /><text x="114" y="85" fill="#111">gatto</text>
    <rect x="148" y="60" width="60" height="40" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="178" y="85" fill="#111">è</text>
    <rect x="212" y="60" width="60" height="40" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="242" y="85" fill="#111">un</text>
    <rect x="276" y="60" width="60" height="40" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="306" y="85" fill="#111">animale</text>
    <rect x="340" y="60" width="60" height="40" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="370" y="85" fill="#111">che</text>
    <rect x="404" y="60" width="60" height="40" rx="6" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" /><text x="434" y="85" fill="#111">è</text>
    <rect x="468" y="60" width="60" height="40" rx="6" fill="#f3f3f3" stroke="#828282" stroke-width="1.5" stroke-dasharray="3 2" /><text x="498" y="85" fill="#828282">...</text>
  </g>

  <text x="280" y="140" fill="#828282" font-size="12" text-anchor="middle">"il gatto è un animale che è un animale che è un animale che..."</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Scegliendo sempre la carta più probabile, il modello ritorna sempre allo stesso bivio e ripete lo stesso blocco.</figcaption>
</figure>

### 6.3 Un dado invece di una scelta fissa

La strategia alternativa, usata dalla maggior parte dei chatbot che usi ogni giorno, è **pescare** dal mazzo invece di prendere sempre la carta in cima — ma pescare in modo che le carte più probabili abbiano più chance di uscire, esattamente come un'urna con palline pesate: se "albero" ha probabilità 60% e "tetto" ha probabilità 25%, immagina un'urna con 60 palline "albero", 25 palline "tetto", e così via per tutte le altre parole possibili, in proporzione alla loro probabilità. Estrai una pallina a caso: quasi sempre uscirà "albero" o "tetto", ma ogni tanto — non spessissimo, ma non mai — uscirà qualcos'altro, magari "ramo" o "cornicione". Questo si chiama **campionamento** (sampling), ed è il motivo per cui la stessa identica domanda, fatta due volte allo stesso chatbot, può produrre due risposte leggermente diverse.

### 6.4 La temperatura: quanto rischia il dado

Qui entra in gioco un parametro che forse hai già visto nominare, se hai usato strumenti più avanzati per parlare con un LLM: la **temperatura**. Pensala come una manopola che ridisegna l'urna prima di pescare:

- **Temperatura bassa** (vicina a zero): l'urna viene "schiacciata" per esasperare ancora di più le differenze — la pallina più probabile arriva quasi a riempire l'urna da sola, le altre quasi scompaiono. Il risultato è un testo prevedibile, quasi deterministico, adatto a compiti dove vuoi la risposta "più probabile e sicura" (un calcolo, un fatto preciso).
- **Temperatura alta**: l'urna viene invece "appiattita" — anche le palline meno probabili diventano quasi altrettanto numerose di quella in testa. Il risultato è un testo più sorprendente, creativo, a volte bizzarro o meno coerente: utile per scrivere una poesia o una storia di fantasia, rischioso per un fatto storico dove vuoi precisione.

Una tecnica complementare, usata quasi sempre insieme alla temperatura, è restringere prima il mazzo alle sole carte più plausibili — scartando in partenza la lunghissima coda di parole con probabilità quasi nulla ("frigorifero", "sottomarino") — e pescare solo tra quelle rimaste. È come giocare a carte con un mazzo ridotto alle carte sensate per quella mano, invece che con l'intero mazzo da 52 sempre in tavola: evita che, per pura sfortuna nell'estrazione, esca ogni tanto qualcosa di completamente assurdo.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 290" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="urne-title urne-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="urne-title">Tre urne, tre temperature</title>
  <desc id="urne-desc">Tre urne che mostrano la stessa distribuzione di probabilità (albero, tetto, ramo, frigorifero) ridisegnata a tre temperature diverse: originale, bassa (schiacciata verso albero) e alta (più appiattita tra le quattro parole).</desc>

  <text x="100" y="40" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">originale</text>
  <text x="260" y="40" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">temperatura bassa</text>
  <text x="420" y="40" fill="#111" font-size="13" font-weight="bold" text-anchor="middle">temperatura alta</text>

  <!-- urna 1: originale 60/25/10/5 -->
  <rect x="40" y="116" width="120" height="84" fill="#2a7ae2" />
  <rect x="40" y="81" width="120" height="35" fill="#f66a0a" />
  <rect x="40" y="67" width="120" height="14" fill="#3aa655" />
  <rect x="40" y="60" width="120" height="7" fill="#c9c9c9" />
  <rect x="40" y="60" width="120" height="140" rx="10" fill="none" stroke="#828282" stroke-width="2" />
  <text x="100" y="163" fill="#fdfdfd" font-size="13" font-weight="bold" text-anchor="middle">60%</text>

  <!-- urna 2: bassa 80/15/4/1 -->
  <rect x="200" y="88" width="120" height="112" fill="#2a7ae2" />
  <rect x="200" y="67" width="120" height="21" fill="#f66a0a" />
  <rect x="200" y="61.4" width="120" height="5.6" fill="#3aa655" />
  <rect x="200" y="60" width="120" height="1.4" fill="#c9c9c9" />
  <rect x="200" y="60" width="120" height="140" rx="10" fill="none" stroke="#828282" stroke-width="2" />
  <text x="260" y="150" fill="#fdfdfd" font-size="13" font-weight="bold" text-anchor="middle">80%</text>

  <!-- urna 3: alta 32/28/22/18 -->
  <rect x="360" y="155.2" width="120" height="44.8" fill="#2a7ae2" />
  <rect x="360" y="116" width="120" height="39.2" fill="#f66a0a" />
  <rect x="360" y="85.2" width="120" height="30.8" fill="#3aa655" />
  <rect x="360" y="60" width="120" height="25.2" fill="#c9c9c9" />
  <rect x="360" y="60" width="120" height="140" rx="10" fill="none" stroke="#828282" stroke-width="2" />
  <text x="420" y="182" fill="#fdfdfd" font-size="13" font-weight="bold" text-anchor="middle">32%</text>

  <g font-size="12" text-anchor="start">
    <rect x="40" y="230" width="14" height="14" fill="#2a7ae2" /><text x="60" y="242" fill="#111">albero</text>
    <rect x="140" y="230" width="14" height="14" fill="#f66a0a" /><text x="160" y="242" fill="#111">tetto</text>
    <rect x="230" y="230" width="14" height="14" fill="#3aa655" /><text x="250" y="242" fill="#111">ramo</text>
    <rect x="320" y="230" width="14" height="14" fill="#c9c9c9" /><text x="340" y="242" fill="#111">frigorifero</text>
  </g>
  <text x="260" y="268" fill="#828282" font-size="11" text-anchor="middle">(numeri illustrativi, diversi da quelli dell'esercizio qui sotto)</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Bassa temperatura schiaccia l'urna verso la parola già più probabile; alta temperatura la appiattisce verso le altre.</figcaption>
</figure>

### 6.5 Perché la stessa domanda dà risposte diverse

Mettendo insieme i pezzi: ogni parola della risposta di un chatbot nasce da (1) il calcolo del mazzo di probabilità, frutto di tutto ciò che abbiamo visto nelle lezioni precedenti, seguito da (2) un'estrazione — più o meno rischiosa a seconda della temperatura — da quel mazzo. Ripetuto parola dopo parola, questo processo costruisce l'intera risposta un pezzo alla volta, senza che il modello "sappia" in anticipo l'intera frase che sta per scrivere: proprio come te, quando parli a braccio, spesso non sai esattamente come finirà la frase che hai appena iniziato.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="ciclo-title ciclo-desc" style="width: 100%; max-width: 400px; height: auto; font-family: inherit;">
  <title id="ciclo-title">Il ciclo di generazione, parola per parola</title>
  <desc id="ciclo-desc">Un ciclo di quattro passi: calcola il mazzo di probabilità, pesca una parola secondo la temperatura, aggiungila alla risposta, passa alla parola successiva, e ricomincia.</desc>

  <defs>
    <marker id="arrowCiclo" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker>
  </defs>

  <path d="M 220,80 L 260,80" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowCiclo)" />
  <path d="M 350,120 L 350,260" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowCiclo)" />
  <path d="M 260,300 L 220,300" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowCiclo)" />
  <path d="M 130,260 L 130,120" fill="none" stroke="#828282" stroke-width="2" marker-end="url(#arrowCiclo)" />

  <rect x="40" y="40" width="180" height="80" rx="8" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="130" y="75" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">1. Calcola il mazzo</text>
  <text x="130" y="93" fill="#111" font-size="12" text-anchor="middle">di probabilità</text>

  <rect x="260" y="40" width="180" height="80" rx="8" fill="#fde8d6" stroke="#f66a0a" stroke-width="1.5" />
  <text x="350" y="75" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">2. Pesca una parola</text>
  <text x="350" y="93" fill="#111" font-size="12" text-anchor="middle">(secondo la temperatura)</text>

  <rect x="260" y="260" width="180" height="80" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="350" y="295" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">3. Aggiungi la parola</text>
  <text x="350" y="313" fill="#111" font-size="12" text-anchor="middle">alla risposta</text>

  <rect x="40" y="260" width="180" height="80" rx="8" fill="#fdfdfd" stroke="#828282" stroke-width="1.5" />
  <text x="130" y="295" fill="#111" font-size="12" font-weight="bold" text-anchor="middle">4. Passa alla parola</text>
  <text x="130" y="313" fill="#111" font-size="12" text-anchor="middle">successiva</text>

  <text x="240" y="195" fill="#828282" font-size="12" text-anchor="middle">si ripete</text>
  <text x="240" y="210" fill="#828282" font-size="12" text-anchor="middle">finché la risposta</text>
  <text x="240" y="225" fill="#828282" font-size="12" text-anchor="middle">non è completa</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Nessuna frase intera viene "pensata" in anticipo: emerge un passo alla volta, ripetendo lo stesso ciclo.</figcaption>
</figure>

---

> **Prova tu — L'urna a due temperature**
>
> Costruisci due urne giocattolo (bastano dei foglietti in un sacchetto, o carte da gioco) per completare la frase: **"Il gatto si è arrampicato su un..."**, con queste probabilità di partenza per quattro parole candidate:
>
> | Parola | Probabilità originale |
> |---|---|
> | albero | 60% |
> | tetto | 25% |
> | ramo | 10% |
> | frigorifero | 5% |
>
> **Urna a bassa temperatura**: rendi le proporzioni ancora più estreme a favore della parola già più probabile — ad esempio 85% albero, 12% tetto, 2% ramo, 1% frigorifero. Scrivi quante palline/foglietti useresti per ciascuna parola se l'urna ne contenesse 100 in totale.
>
> **Urna ad alta temperatura**: appiattisci invece le proporzioni, avvicinandole tutte a un quarto ciascuna (25% ciascuna). Scrivi di nuovo quante palline userebbe ciascuna parola su 100 totali.
>
> Ora rispondi: in quale delle due urne è più probabile — pur restando un evento raro — pescare "frigorifero"? E in quale urna, pescando 5 volte di fila (rimettendo ogni volta la pallina estratta), ti aspetti di ottenere quasi sempre la stessa identica parola? Ragionamento completo in Appendice A.

---

*Continua con la [Lezione 07 — Quanto ci possiamo fidare]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-07-quanto-ci-possiamo-fidare.md %}.html)*
