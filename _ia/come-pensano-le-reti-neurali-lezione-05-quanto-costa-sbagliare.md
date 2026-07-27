---
title: 'Lezione 05 — Quanto costa sbagliare'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 5.1 Prima di correggersi, serve un numero

Le lezioni precedenti hanno mostrato come costruire una rete — interruttori, piani, stencil, bigliettini che si aggiornano — e come farle produrre un output a partire da un input. Manca ancora la domanda più importante di tutte: come si capisce se la rete sta imparando bene? Il primo passo è mettere un numero preciso su quanto la rete stia sbagliando in questo momento — una **funzione di perdita** (o *loss*), che deve essere piccola quando la rete va bene e grande quando va male. Le prossime due sezioni ne descrivono le due forme più comuni, ciascuna pensata per un tipo diverso di compito.

### 5.2 Il bersaglio delle freccette: sbagliare un numero

Quando l'output desiderato è un numero continuo — prevedere una temperatura, un prezzo, la posizione esatta di un oggetto in una foto — la penalità più naturale assomiglia a un bersaglio da freccette: quanto più la freccetta (la predizione) cade lontano dal centro (il valore vero), tanto peggio. La scelta standard, chiamata **errore quadratico medio**, non misura semplicemente la distanza dal centro, ma il *quadrato* di quella distanza: una freccetta che cade il doppio più lontano del centro rispetto a un'altra viene penalizzata quattro volte tanto, non due. Questo ha un effetto pratico preciso: errori piccoli vengono quasi ignorati, errori grandi vengono puniti in modo sproporzionatamente severo — esattamente il tipo di segnale che serve per correggere prima gli sbagli più gravi.

### 5.3 Il test a crocette: sbagliare una categoria

Quando invece l'output è una scelta fra categorie diverse — che animale è nella foto, quale parola viene dopo — il bersaglio delle freccette è una scelta povera: tratterebbe allo stesso modo una risposta "quasi giusta ma poco sicura" e una "clamorosamente sbagliata", ignorando che la rete, alla fine, produce sempre una specie di percentuale di sicurezza per ciascuna categoria possibile (non una sola risposta secca, ma qualcosa come "70% gatto, 20% cane, 10% coniglio"). La scelta standard qui si chiama **cross-entropy**, e il modo più semplice per sentirla sulla pelle è immaginare un gioco a premi: rispondi con una percentuale di sicurezza sulla categoria che ritieni corretta, e vieni penalizzato in base a quanto quella percentuale era *bassa* — non in base a quanto la tua risposta finale era "diversa" dalla verità, ma in base a quanta sicurezza avevi riposto proprio lì dove serviva.

La tabella qui sotto — una versione semplificata, ma fedele nella forma, della vera formula matematica — mostra quanti "punti di sorpresa" guadagni in base alla percentuale di sicurezza che avevi assegnato alla risposta *effettivamente* corretta:

| Sicurezza sulla risposta corretta | Punti di sorpresa (penalità) |
|---|---|
| 90% | 1 |
| 70% | 2 |
| 50% | 3 |
| 20% | 8 |
| 10% | 12 |
| 1% | 20 |

Nota come i punti di sorpresa non crescano in modo proporzionale, ma sempre più ripidamente man mano che la sicurezza scende: passare da 90% a 70% costa solo un punto in più, ma passare da 10% a 1% ne costa otto in più. Punire la sicurezza mal riposta in modo così sproporzionato, invece che in modo proporzionale come farebbe il bersaglio delle freccette, è esattamente ciò che rende la cross-entropy la scelta giusta per un compito di categorie.

### 5.4 Loss e ultimo strato vanno scelti in coppia

Un dettaglio pratico da non sottovalutare: queste due misure dell'errore non si scelgono isolatamente, ma insieme a come è costruito l'ultimissimo strato della rete. Il test a crocette (cross-entropy) si accoppia naturalmente con un ultimo piano che produce percentuali di sicurezza fra 0 e 100 — ha bisogno che l'output sia già interpretabile come "quanto sono sicuro", non un numero qualsiasi. Il bersaglio delle freccette (errore quadratico medio) si accoppia invece naturalmente con un ultimo piano che produce un numero libero, senza vincoli — un output di temperatura o prezzo non ha motivo di essere schiacciato in un intervallo fisso. Scegliere la combinazione sbagliata — per esempio, il bersaglio delle freccette su un output che dovrebbe essere una percentuale — di solito non impedisce del tutto l'allenamento, ma lo rende più lento e meno stabile, un po' come guidare con le marce sbagliate: si arriva lo stesso, ma con molto più sforzo.

### 5.5 Un paesaggio con più di una valle buona

Vale la pena anticipare un'osservazione che tornerà utile più avanti: pensata come una superficie da esplorare — bassa dove la rete sbaglia poco, alta dove sbaglia molto — la funzione di perdita di una rete con milioni di impostazioni regolabili non è una singola valle con un unico punto più basso possibile, ma un paesaggio enorme con moltissime valli diverse, spesso quasi altrettanto basse l'una quanto l'altra. Reti allenate a partire da punti di partenza casuali diversi tipicamente finiscono in valli diverse — con impostazioni interne molto diverse fra loro — pur raggiungendo un livello di errore finale molto simile. Non esiste, in altre parole, un'unica "risposta perfetta" da trovare a tutti i costi: esistono molte soluzioni "ugualmente buone", una scoperta che renderà meno sorprendente, nelle lezioni successivi, il fatto che due reti allenate sullo stesso identico problema possano arrivare a impostazioni interne piuttosto diverse.

---

> **Prova tu — Due risposte "sicure al 70%"**
>
> Un modello deve classificare un'immagine fra tre categorie: gatto, cane, coniglio. La risposta corretta è **cane**.
>
> - **Predizione A**: 10% gatto, 70% cane, 20% coniglio.
> - **Predizione B**: 10% gatto, 20% cane, 70% coniglio.
>
> Entrambe le predizioni assegnano "70% di sicurezza" a *qualcosa* — solo che A la assegna alla categoria giusta, B a quella sbagliata.
>
> 1. Usando la tabella della Sezione 5.3, quanti punti di sorpresa guadagna la Predizione A? (Guarda la sicurezza che A assegna alla risposta corretta, cane.)
> 2. Quanti punti di sorpresa guadagna la Predizione B? (Guarda la sicurezza che B assegna alla risposta corretta, cane — non quella che B assegna, per sbaglio, a coniglio.)
> 3. Le due predizioni "sembrano" ugualmente sicure di sé (70% su qualcosa) — ma i punti di sorpresa sono uguali o molto diversi? Cosa ti dice questo sul motivo per cui il test a crocette non guarda "quanto sei sicuro" in generale, ma "quanto sei sicuro esattamente della risposta giusta"?

---

*Continua con la [Lezione 06 — La colpa che risale la catena]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-06-la-colpa-che-risale-la-catena.md %}.html)*
