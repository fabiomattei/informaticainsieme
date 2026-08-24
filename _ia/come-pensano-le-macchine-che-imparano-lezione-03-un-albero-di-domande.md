---
title: 'Lezione 03 — Un albero di domande'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un piccolo albero di domande sì/no che porta a decisioni finali](/images/ia/come-pensano-le-macchine-che-imparano-lezione-03-un-albero-di-domande/come-pensano-le-macchine-che-imparano-lezione-03-un-albero-di-domande.svg){:class="aside-image"}

### 3.1 Indovina chi, ma per angurie

Conosci di sicuro il meccanismo di "Indovina chi?": invece di chiedere subito "è Marco?", fai domande che dividono a metà i candidati rimasti — "porta gli occhiali?", "ha i capelli scuri?" — e a ogni risposta scarti circa metà delle possibilità restanti. In poche domande, ben scelte, arrivi al nome giusto molto più in fretta che tentando nomi a caso.

Un **albero decisionale** applica esattamente questa stessa logica alla classificazione di angurie, email o clienti: invece di guardare tutti gli indizi insieme come faceva k-NN nella Lezione 2, pone una sequenza di domande semplici, una alla volta, ciascuna con risposta sì/no (o comunque un numero ristretto di risposte), fino ad arrivare a una decisione finale. La differenza fondamentale rispetto a "Indovina chi?" è che qui nessuno sceglie le domande a mano: è l'algoritmo stesso, guardando gli esempi di addestramento, a decidere quali domande porre e in che ordine — proprio come nella Lezione 1, dove la regola nasceva dai dati e non dalla penna del programmatore.

### 3.2 Il vocabolario dell'albero

Un albero decisionale si legge dall'alto verso il basso. Il punto di partenza, in cima, si chiama **nodo radice**: la prima domanda che viene posta a ogni esempio, quella scelta come più utile fra tutte. Da ogni nodo partono dei **rami**, uno per ogni possibile risposta, che portano a un nuovo nodo — una nuova domanda — oppure direttamente a una **foglia**: un nodo finale che non pone più domande, ma restituisce la decisione (nel nostro caso: "matura" o "non matura").

Percorrere l'albero per un esempio nuovo significa semplicemente rispondere, una alla volta, alle domande che incontri partendo dalla radice, seguendo il ramo corrispondente alla tua risposta, finché non atterri su una foglia. La decisione scritta su quella foglia è la previsione del modello.

### 3.3 Come si sceglie la domanda migliore: l'idea di purezza

Il cuore dell'algoritmo — e la parte davvero interessante — è come si decide *quale* domanda porre a ogni nodo, fra tutte quelle possibili. L'idea guida è questa: fra tutte le domande disponibili, scegli quella che divide meglio gli esempi rimasti in gruppi il più possibile **puri**, cioè gruppi dove quasi tutti gli esempi condividono la stessa etichetta.

Facciamo un esempio concreto. Supponi di avere dieci angurie, sei mature e quattro non mature, tutte mischiate insieme in cima all'albero. Confrontiamo due domande candidate:

- La domanda "il suono è cupo?" divide le dieci angurie in due gruppi: uno con 5 mature e 1 non matura, l'altro con 1 matura e 3 non mature. Entrambi i gruppi sono piuttosto **puri**: nel primo dominano nettamente le mature, nel secondo dominano nettamente le non mature.
- La domanda "l'anguria pesa più di 5 kg?" divide invece le stesse dieci angurie in due gruppi quasi identici: 3 mature e 2 non mature da una parte, 3 mature e 2 non mature dall'altra. Entrambi i gruppi restano **mescolati** quasi quanto lo era il gruppo di partenza: la domanda non ha chiarito quasi nulla.

La prima domanda è chiaramente più utile della seconda, perché dopo averla posta sai molto di più su ogni singola anguria di quanto sapessi prima. L'algoritmo prova, in ogni nodo, tutte le domande possibili su tutte le caratteristiche disponibili, misura quanto ciascuna aumenta la purezza dei gruppi risultanti, e sceglie sempre la migliore. Il calcolo esatto che misura la purezza ha un nome tecnico (spesso *impurità di Gini* o *entropia*), ma qui basta tenere a mente l'idea sostanziale: una domanda buona è quella che separa il più possibile gli esempi di etichette diverse.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="pur-title pur-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="pur-title">Confronto fra una domanda buona e una domanda inutile</title>
  <desc id="pur-desc">Un gruppo misto di dieci angurie viene diviso da due domande diverse: una produce due gruppi puri, l'altra produce due gruppi ancora mescolati quanto l'originale.</desc>

  <text x="90" y="20" fill="#828282" font-size="11" text-anchor="middle">gruppo di partenza</text>
  <g transform="translate(30,30)">
    <circle cx="0" cy="0" r="6" fill="#3aa655" /><circle cx="16" cy="0" r="6" fill="#3aa655" /><circle cx="32" cy="0" r="6" fill="#3aa655" /><circle cx="48" cy="0" r="6" fill="#c85506" /><circle cx="64" cy="0" r="6" fill="#c85506" />
    <circle cx="0" cy="18" r="6" fill="#3aa655" /><circle cx="16" cy="18" r="6" fill="#3aa655" /><circle cx="32" cy="18" r="6" fill="#3aa655" /><circle cx="48" cy="18" r="6" fill="#c85506" /><circle cx="64" cy="18" r="6" fill="#c85506" />
  </g>

  <text x="230" y="70" fill="#2a7ae2" font-size="12" font-weight="bold" text-anchor="middle">"suono cupo?"</text>
  <path d="M 110,40 L 190,90" fill="none" stroke="#828282" stroke-width="1.5" />
  <path d="M 110,40 L 190,130" fill="none" stroke="#828282" stroke-width="1.5" />

  <g transform="translate(200,80)">
    <circle cx="0" cy="0" r="6" fill="#3aa655" /><circle cx="16" cy="0" r="6" fill="#3aa655" /><circle cx="32" cy="0" r="6" fill="#3aa655" /><circle cx="48" cy="0" r="6" fill="#3aa655" /><circle cx="64" cy="0" r="6" fill="#3aa655" /><circle cx="80" cy="0" r="6" fill="#c85506" />
  </g>
  <text x="330" y="86" fill="#828282" font-size="10">quasi tutto verde: puro</text>

  <g transform="translate(200,120)">
    <circle cx="0" cy="0" r="6" fill="#c85506" /><circle cx="16" cy="0" r="6" fill="#c85506" /><circle cx="32" cy="0" r="6" fill="#c85506" /><circle cx="48" cy="0" r="6" fill="#3aa655" />
  </g>
  <text x="330" y="126" fill="#828282" font-size="10">quasi tutto arancio: puro</text>

  <text x="230" y="175" fill="#c85506" font-size="12" font-weight="bold" text-anchor="middle">"pesa più di 5 kg?"</text>
  <path d="M 110,50 L 190,190" fill="none" stroke="#e3e3e3" stroke-width="1.5" />
  <path d="M 110,50 L 190,230" fill="none" stroke="#e3e3e3" stroke-width="1.5" />

  <g transform="translate(200,180)">
    <circle cx="0" cy="0" r="6" fill="#3aa655" /><circle cx="16" cy="0" r="6" fill="#3aa655" /><circle cx="32" cy="0" r="6" fill="#3aa655" /><circle cx="48" cy="0" r="6" fill="#c85506" /><circle cx="64" cy="0" r="6" fill="#c85506" />
  </g>
  <text x="330" y="186" fill="#828282" font-size="10">ancora mescolato</text>

  <g transform="translate(200,220)">
    <circle cx="0" cy="0" r="6" fill="#3aa655" /><circle cx="16" cy="0" r="6" fill="#3aa655" /><circle cx="32" cy="0" r="6" fill="#3aa655" /><circle cx="48" cy="0" r="6" fill="#c85506" /><circle cx="64" cy="0" r="6" fill="#c85506" />
  </g>
  <text x="330" y="226" fill="#828282" font-size="10">ancora mescolato</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Verde = matura, arancio = non matura. La prima domanda separa bene, la seconda quasi per niente.</figcaption>
</figure>

### 3.4 Costruire l'albero: una domanda dopo l'altra

L'algoritmo costruisce l'albero ripetendo lo stesso procedimento a ogni nodo, un livello alla volta. Alla radice, prova tutte le domande possibili su tutte le caratteristiche e sceglie la migliore, dividendo gli esempi nei due gruppi risultanti. Poi, dentro *ciascuno* di quei due gruppi, ripete da capo l'identica ricerca: quale domanda, fra quelle rimaste, separa meglio *questo* sottogruppo specifico? Non è detto sia la stessa domanda usata alla radice, né va posta nello stesso ordine in ogni ramo: ogni sottogruppo può avere la sua domanda più utile, diversa da quella degli altri rami.

Il procedimento si ferma, in un dato ramo, quando un gruppo diventa **completamente puro** (tutti gli esempi rimasti hanno la stessa etichetta: a quel punto non serve più fare domande, si può mettere direttamente una foglia), oppure quando si raggiunge un limite deciso in anticipo — per esempio, non far crescere l'albero oltre una certa profondità, o non dividere più un gruppo che contiene meno di un certo numero di esempi.

Questo secondo tipo di limite non è un dettaglio marginale: un albero lasciato crescere senza freni tende a porre domande sempre più specifiche, fino a isolare un singolo esempio strano in una foglia tutta sua — una domanda così particolare da valere solo per quell'unico caso, inutile per qualunque anguria nuova. Torneremo su questo problema, che ha un nome preciso, nella Lezione 5.

### 3.5 Un pregio degli alberi: si possono leggere

A differenza di k-NN, che non spiega mai *perché* prevede una certa etichetta — si limita a votare fra i vicini — un albero decisionale, una volta costruito, può essere letto e capito riga per riga, esattamente come una ricetta o un diagramma di flusso: "se il suono è cupo e il puntino giallo è pallido, allora non matura". Questa qualità si chiama **interpretabilità**, ed è la ragione per cui gli alberi decisionali restano popolari in settori dove serve poter spiegare una decisione — per esempio, perché una banca ha rifiutato un prestito — non solo prenderla correttamente.

---

> **Prova tu — Costruisci la radice dell'albero**
>
> Otto angurie, con due caratteristiche a categoria e la loro etichetta:
>
> | Anguria | Suono | Peso | Matura? |
> |---|---|---|---|
> | 1 | cupo | pesante | sì |
> | 2 | cupo | leggero | sì |
> | 3 | cupo | pesante | sì |
> | 4 | cupo | leggero | sì |
> | 5 | acuto | pesante | no |
> | 6 | acuto | leggero | no |
> | 7 | acuto | pesante | no |
> | 8 | acuto | leggero | no |
>
> 1. Se poni la domanda "il suono è cupo?", in quanti gruppi si dividono le otto angurie, e quante mature/non mature finiscono in ciascun gruppo?
> 2. Se poni invece la domanda "il peso è pesante?", quante mature/non mature finiscono in ciascun gruppo?
> 3. Quale delle due domande produce gruppi più puri? Sulla base di questo, quale useresti come domanda alla radice dell'albero?

---

*Continua con la [Lezione 04 — Tracciare la retta migliore]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-lezione-04-tracciare-la-retta-migliore.md %}.html)*
