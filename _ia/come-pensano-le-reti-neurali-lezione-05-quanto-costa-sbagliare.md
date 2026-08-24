---
title: 'Lezione 05, Quanto costa sbagliare'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un errore piccolo produce una loss bassa, un errore grande una loss alta](/images/ia/come-pensano-le-reti-neurali-lezione-05-quanto-costa-sbagliare/come-pensano-le-reti-neurali-lezione-05-quanto-costa-sbagliare.svg){:class="aside-image"}

### 5.1 Prima di correggersi, serve un numero

Le lezioni precedenti hanno mostrato come costruire una rete, interruttori, piani, stencil, bigliettini che si aggiornano, e come farle produrre un output a partire da un input. Manca ancora la domanda più importante di tutte: come si capisce se la rete sta imparando bene? Il primo passo è mettere un numero preciso su quanto la rete stia sbagliando in questo momento, una **funzione di perdita** (o *loss*), che deve essere piccola quando la rete va bene e grande quando va male. Le prossime due sezioni ne descrivono le due forme più comuni, ciascuna pensata per un tipo diverso di compito.

### 5.2 Il bersaglio delle freccette: sbagliare un numero

Quando l'output desiderato è un numero continuo, prevedere una temperatura, un prezzo, la posizione esatta di un oggetto in una foto, la penalità più naturale assomiglia a un bersaglio da freccette: quanto più la freccetta (la predizione) cade lontano dal centro (il valore vero), tanto peggio. La scelta standard, chiamata **errore quadratico medio**, non misura semplicemente la distanza dal centro, ma il *quadrato* di quella distanza: una freccetta che cade il doppio più lontano del centro rispetto a un'altra viene penalizzata quattro volte tanto, non due. Questo ha un effetto pratico preciso: errori piccoli vengono quasi ignorati, errori grandi vengono puniti in modo sproporzionatamente severo, esattamente il tipo di segnale che serve per correggere prima gli sbagli più gravi.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="mse-title mse-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="mse-title">Il bersaglio delle freccette</title>
  <desc id="mse-desc">Un bersaglio con due freccette: una a distanza 1 dal centro, una a distanza doppia. La penalità cresce con il quadrato della distanza, quindi la seconda freccetta riceve una penalità quattro volte più grande, non due.</desc>

  <circle cx="140" cy="140" r="100" fill="#fde8d6" />
  <circle cx="140" cy="140" r="75" fill="#fdfdfd" />
  <circle cx="140" cy="140" r="50" fill="#fde8d6" />
  <circle cx="140" cy="140" r="25" fill="#fdfdfd" />
  <circle cx="140" cy="140" r="8" fill="#c85506" />

  <line x1="140" y1="140" x2="170" y2="140" stroke="#2a7ae2" stroke-width="1.5" stroke-dasharray="3 2" />
  <circle cx="170" cy="140" r="6" fill="#2a7ae2" />
  <text x="155" y="130" fill="#1d5eb8" font-size="11" text-anchor="middle">d</text>

  <line x1="140" y1="140" x2="200" y2="140" stroke="#c85506" stroke-width="1.5" stroke-dasharray="3 2" />
  <circle cx="200" cy="140" r="6" fill="#c85506" />
  <text x="185" y="130" fill="#c85506" font-size="11" text-anchor="middle">2d</text>

  <text x="340" y="60" fill="#111" font-size="12" text-anchor="middle">penalità (distanza²)</text>
  <g stroke="#828282" stroke-width="1"><line x1="300" y1="200" x2="440" y2="200" /></g>
  <rect x="315" y="170" width="40" height="30" fill="#dceafc" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="335" y="165" fill="#111" font-size="12" text-anchor="middle">1</text>
  <rect x="385" y="80" width="40" height="120" fill="#fde8d6" stroke="#c85506" stroke-width="1.5" />
  <text x="405" y="75" fill="#111" font-size="12" text-anchor="middle">4</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Il doppio della distanza dal centro costa il quadruplo, non il doppio.</figcaption>
</figure>

### 5.3 Il test a crocette: sbagliare una categoria

Quando invece l'output è una scelta fra categorie diverse, che animale è nella foto, quale parola viene dopo, il bersaglio delle freccette è una scelta povera: tratterebbe allo stesso modo una risposta "quasi giusta ma poco sicura" e una "clamorosamente sbagliata", ignorando che la rete, alla fine, produce sempre una specie di percentuale di sicurezza per ciascuna categoria possibile (non una sola risposta secca, ma qualcosa come "70% gatto, 20% cane, 10% coniglio"). La scelta standard qui si chiama **cross-entropy**, e il modo più semplice per sentirla sulla pelle è immaginare un gioco a premi: rispondi con una percentuale di sicurezza sulla categoria che ritieni corretta, e vieni penalizzato in base a quanto quella percentuale era *bassa*, non in base a quanto la tua risposta finale era "diversa" dalla verità, ma in base a quanta sicurezza avevi riposto proprio lì dove serviva.

La tabella qui sotto, una versione semplificata, ma fedele nella forma, della vera formula matematica, mostra quanti "punti di sorpresa" guadagni in base alla percentuale di sicurezza che avevi assegnato alla risposta *effettivamente* corretta:

| Sicurezza sulla risposta corretta | Punti di sorpresa (penalità) |
|---|---|
| 90% | 1 |
| 70% | 2 |
| 50% | 3 |
| 20% | 8 |
| 10% | 12 |
| 1% | 20 |

Nota come i punti di sorpresa non crescano in modo proporzionale, ma sempre più ripidamente man mano che la sicurezza scende: passare da 90% a 70% costa solo un punto in più, ma passare da 10% a 1% ne costa otto in più. Punire la sicurezza mal riposta in modo così sproporzionato, invece che in modo proporzionale come farebbe il bersaglio delle freccette, è esattamente ciò che rende la cross-entropy la scelta giusta per un compito di categorie.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 480 280" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="crossentropy-title crossentropy-desc" style="width: 100%; max-width: 440px; height: auto; font-family: inherit;">
  <title id="crossentropy-title">I punti di sorpresa crescono sempre più ripidi</title>
  <desc id="crossentropy-desc">Sei barre, una per ciascuna sicurezza della tabella: 90% costa 1 punto, 70% costa 2, 50% costa 3, 20% costa 8, 10% costa 12, 1% costa 20. L'altezza cresce sempre più ripidamente man mano che la sicurezza scende.</desc>

  <g stroke="#828282" stroke-width="1"><line x1="30" y1="240" x2="450" y2="240" /></g>

  <rect x="40" y="232" width="50" height="8" fill="#dceafc" /><text x="65" y="215" fill="#111" font-size="12" text-anchor="middle">1</text>
  <rect x="110" y="224" width="50" height="16" fill="#a9c9f2" /><text x="135" y="207" fill="#111" font-size="12" text-anchor="middle">2</text>
  <rect x="180" y="216" width="50" height="24" fill="#6fa8e8" /><text x="205" y="199" fill="#111" font-size="12" text-anchor="middle">3</text>
  <rect x="250" y="176" width="50" height="64" fill="#f5b483" /><text x="275" y="159" fill="#111" font-size="12" text-anchor="middle">8</text>
  <rect x="320" y="144" width="50" height="96" fill="#f68f4d" /><text x="345" y="127" fill="#111" font-size="12" text-anchor="middle">12</text>
  <rect x="390" y="80" width="50" height="160" fill="#c85506" /><text x="415" y="63" fill="#111" font-size="12" text-anchor="middle">20</text>

  <g fill="#828282" font-size="11" text-anchor="middle">
    <text x="65" y="256">90%</text><text x="135" y="256">70%</text><text x="205" y="256">50%</text>
    <text x="275" y="256">20%</text><text x="345" y="256">10%</text><text x="415" y="256">1%</text>
  </g>
  <text x="240" y="274" fill="#111" font-size="12" text-anchor="middle">sicurezza assegnata alla risposta corretta</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Passare da 90% a 70% costa poco; passare da 10% a 1% costa molto di più.</figcaption>
</figure>

### 5.4 Loss e ultimo strato vanno scelti in coppia

Un dettaglio pratico da non sottovalutare: queste due misure dell'errore non si scelgono isolatamente, ma insieme a come è costruito l'ultimissimo strato della rete. Il test a crocette (cross-entropy) si accoppia naturalmente con un ultimo piano che produce percentuali di sicurezza fra 0 e 100, ha bisogno che l'output sia già interpretabile come "quanto sono sicuro", non un numero qualsiasi. Il bersaglio delle freccette (errore quadratico medio) si accoppia invece naturalmente con un ultimo piano che produce un numero libero, senza vincoli, un output di temperatura o prezzo non ha motivo di essere schiacciato in un intervallo fisso. Scegliere la combinazione sbagliata, per esempio, il bersaglio delle freccette su un output che dovrebbe essere una percentuale, di solito non impedisce del tutto l'allenamento, ma lo rende più lento e meno stabile, un po' come guidare con le marce sbagliate: si arriva lo stesso, ma con molto più sforzo.

### 5.5 Un paesaggio con più di una valle buona

Vale la pena anticipare un'osservazione che tornerà utile più avanti: pensata come una superficie da esplorare, bassa dove la rete sbaglia poco, alta dove sbaglia molto, la funzione di perdita di una rete con milioni di impostazioni regolabili non è una singola valle con un unico punto più basso possibile, ma un paesaggio enorme con moltissime valli diverse, spesso quasi altrettanto basse l'una quanto l'altra. Reti allenate a partire da punti di partenza casuali diversi tipicamente finiscono in valli diverse, con impostazioni interne molto diverse fra loro, pur raggiungendo un livello di errore finale molto simile. Non esiste, in altre parole, un'unica "risposta perfetta" da trovare a tutti i costi: esistono molte soluzioni "ugualmente buone", una scoperta che renderà meno sorprendente, nelle lezioni successivi, il fatto che due reti allenate sullo stesso identico problema possano arrivare a impostazioni interne piuttosto diverse.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="paesaggio-title paesaggio-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="paesaggio-title">Un paesaggio con più valli</title>
  <desc id="paesaggio-desc">Un profilo di terreno con tre valli di profondità simile. Tre punti di partenza diversi, ciascuno rotola verso una valle diversa, ma tutte e tre le valli raggiungono un livello di errore finale molto simile.</desc>

  <path d="M20,80 C60,180 100,180 140,100 C180,40 220,190 260,195 C300,200 340,60 380,110 C420,160 460,170 500,90 L500,240 L20,240 Z" fill="#eef5fd" stroke="#2a7ae2" stroke-width="2" />

  <circle cx="60" cy="30" r="8" fill="#2a7ae2" />
  <path d="M60,38 Q80,100 100,175" fill="none" stroke="#2a7ae2" stroke-width="1.5" stroke-dasharray="4 3" />
  <circle cx="100" cy="178" r="7" fill="#2a7ae2" />

  <circle cx="250" cy="30" r="8" fill="#f66a0a" />
  <path d="M250,38 Q265,120 270,193" fill="none" stroke="#f66a0a" stroke-width="1.5" stroke-dasharray="4 3" />
  <circle cx="270" cy="196" r="7" fill="#f66a0a" />

  <circle cx="430" cy="30" r="8" fill="#3aa655" />
  <path d="M430,38 Q410,90 397,135" fill="none" stroke="#3aa655" stroke-width="1.5" stroke-dasharray="4 3" />
  <circle cx="395" cy="138" r="7" fill="#3aa655" />

  <text x="260" y="18" fill="#828282" font-size="11" text-anchor="middle">punti di partenza casuali diversi</text>
  <text x="260" y="228" fill="#1d5eb8" font-size="12" text-anchor="middle">valli diverse, errore finale simile</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Non c'è un'unica "risposta perfetta": esistono molte soluzioni ugualmente buone.</figcaption>
</figure>

---

> **Prova tu, Due risposte "sicure al 70%"**
>
> Un modello deve classificare un'immagine fra tre categorie: gatto, cane, coniglio. La risposta corretta è **cane**.
>
> - **Predizione A**: 10% gatto, 70% cane, 20% coniglio.
> - **Predizione B**: 10% gatto, 20% cane, 70% coniglio.
>
> Entrambe le predizioni assegnano "70% di sicurezza" a *qualcosa*, solo che A la assegna alla categoria giusta, B a quella sbagliata.
>
> 1. Usando la tabella della Sezione 5.3, quanti punti di sorpresa guadagna la Predizione A? (Guarda la sicurezza che A assegna alla risposta corretta, cane.)
> 2. Quanti punti di sorpresa guadagna la Predizione B? (Guarda la sicurezza che B assegna alla risposta corretta, cane, non quella che B assegna, per sbaglio, a coniglio.)
> 3. Le due predizioni "sembrano" ugualmente sicure di sé (70% su qualcosa), ma i punti di sorpresa sono uguali o molto diversi? Cosa ti dice questo sul motivo per cui il test a crocette non guarda "quanto sei sicuro" in generale, ma "quanto sei sicuro esattamente della risposta giusta"?

---

*Continua con la [Lezione 06, La colpa che risale la catena]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-06-la-colpa-che-risale-la-catena.md %}.html)*
