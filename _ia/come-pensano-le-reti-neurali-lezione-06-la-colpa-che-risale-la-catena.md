---
title: 'Lezione 06 — La colpa che risale la catena'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 6.1 Il forward pass da solo non basta

La Lezione 2 ha mostrato come una fila di controllori — uno strato dopo l'altro — trasformi un input in un output, passando l'informazione in avanti, piano per piano. Ma nel "Prova tu" di quella lezione i pesi erano già dati, scelti a tavolino perché il risultato tornasse giusto. Nella realtà nessuno sceglie i pesi a mano: la rete deve trovarli da sola, correggendosi progressivamente in base agli errori commessi, esattamente come l'interruttore-ombrello della Lezione 1. La domanda di questa lezione è: quando l'errore finale viene misurato (Lezione 5) su una rete con centinaia o migliaia di pesi sparsi su più piani, *come si distribuisce la colpa* fra tutti quei pesi, capendo di quanto e in che direzione correggere ciascuno?

### 6.2 Un'idea semplice: passare la colpa all'indietro, un piano alla volta

Immagina una catena di artigiani in una linea di montaggio, ciascuno che riceve il pezzo dal precedente, lo modifica un po', e lo passa al successivo — il forward pass della Lezione 2. Quando il prodotto finale risulta difettoso, il modo più sensato di correggere la linea non è ricominciare a indovinare da zero le regolazioni di ogni singolo artigiano: è far risalire la segnalazione del difetto all'indietro lungo la stessa catena, un artigiano alla volta, partendo dall'ultimo. L'ultimo artigiano riceve la notizia "il pezzo finale era così e così sbagliato" e capisce subito quanto correggere la propria fase. Ma deve anche passare al penultimo un messaggio equivalente: "il pezzo che *mi* hai passato tu era, di riflesso, così e così sbagliato" — permettendogli a sua volta di correggersi e di passare un messaggio simile ancora più indietro. Questa procedura, applicata piano per piano fino al primo artigiano della catena, è l'intero contenuto operativo della **backpropagation** (o "retropropagazione dell'errore").

### 6.3 Cosa serve per calcolare "quanto è colpa mia"

Perché ogni artigiano possa tradurre "il pezzo che ho prodotto era sbagliato di tot" in "devo correggere le mie regolazioni di tot", serve un ingrediente che la Lezione 1 aveva già preparato senza dirlo esplicitamente: la manopola sfumata di ogni controllore (Sezione 1.5) permette sempre di sapere, in ogni punto, quanto il suo output *reagirebbe* a un piccolo cambiamento del proprio input — se un ritocco minimo lo farebbe salire molto, poco, o quasi nulla. Questa "reattività locale" è esattamente il moltiplicatore che serve per tradurre la colpa ricevuta da un piano in due cose: quanto correggere i propri pesi, e quanta colpa (già tradotta, già pesata) passare indietro al piano precedente. È il motivo preciso per cui un interruttore a scatto netto — senza sfumature, Sezione 1.5 — non si sarebbe mai potuto correggere in una catena di più di un piano: non c'è "reattività locale" da misurare in un tasto che salta di colpo da spento ad acceso.

### 6.4 Correggere costa quasi quanto decidere

Vale la pena notare un fatto pratico, che tornerà utile più avanti quando parleremo di reti molto grandi: far risalire la colpa lungo la catena richiede, a ogni anello, un lavoro di calcolo paragonabile a quello richiesto per produrre l'output in avanti (il forward pass). Non è un lavoro qualitativamente diverso o molto più pesante: è, grosso modo, un secondo passaggio della stessa entità del primo. Questo spiega perché allenare una rete costi, per ogni esempio visto, circa 2-3 volte il costo di farle semplicemente produrre una risposta — non un costo enormemente più grande, ma la somma di "decidere" più "capire di quanto correggersi", ripetuta per ogni esempio, molte volte durante l'allenamento.

---

> **Prova tu — Fai risalire la colpa lungo tre anelli**
>
> Una piccola rete a tre piani ha appena prodotto un output sbagliato. Il supervisore finale calcola: **colpa al piano 3 (output) = −4** (un numero negativo significa "avresti dovuto essere più alto di così").
>
> Per far risalire la colpa a ritroso, ogni anello della catena applica questa regola: *colpa del piano precedente = colpa ricevuta × peso di collegamento fra i due piani × reattività locale del piano precedente* (la "sensibilità" della sua manopola in quel punto, Sezione 6.3 — un numero fra 0 e 1 che ti viene già dato, non serve calcolarlo).
>
> Dati:
> - Peso di collegamento fra piano 2 e piano 3: **2**. Reattività locale del piano 2 in questo esempio: **0,5**.
> - Peso di collegamento fra piano 1 e piano 2: **1,5**. Reattività locale del piano 1 in questo esempio: **0,4**.
>
> 1. Calcola la colpa che risale al piano 2: colpa piano 3 × peso (piano2-piano3) × reattività locale piano 2.
> 2. Usando il risultato del punto 1, calcola la colpa che risale al piano 1: colpa piano 2 × peso (piano1-piano2) × reattività locale piano 1.
> 3. Guarda i tre numeri di colpa (piano 3, piano 2, piano 1) in fila. Stanno diventando più piccoli o più grandi man mano che risali la catena? Rileggi la Sezione 4.4 (il bigliettino che si sporca): cosa succederebbe a questi numeri se la catena avesse non 3 ma 30 anelli, e ogni reattività locale restasse intorno a 0,4-0,5 per tutta la catena?

---

*Continua con la [Lezione 07 — Allenarsi bene (e non a memoria)]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-07-allenarsi-bene-e-non-a-memoria.md %}.html)*
