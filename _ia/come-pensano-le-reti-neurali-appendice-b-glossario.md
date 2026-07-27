---
title: 'Appendice B — Glossario'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

Termini in ordine alfabetico, con un rimando alla lezione dove sono spiegati per la prima volta. Nessuna definizione qui contiene formule: per la versione tecnica e formale di ciascun termine, vedi il glossario di *Come Imparano le Reti Neurali*.

**Adam** — variante evoluta del momentum che calibra il ritmo di correzione separatamente per ciascun peso della rete, in base a quanto è stato "agitato" storicamente il suo segnale di errore. Lezione 7.

**Attenzione** — la capacità di un modello di "guardare" direttamente qualunque altra posizione di una sequenza, in un solo passo, invece di doverla raggiungere passando per tutte le posizioni intermedie. Lezione 8.

**Backpropagation (retropropagazione dell'errore)** — la procedura con cui la colpa di un errore finale viene fatta risalire, un piano alla volta, dall'ultimo strato di una rete fino al primo, permettendo di correggere ogni peso in proporzione a quanto ha contribuito allo sbaglio. Lezione 6.

**Bigliettino mentale (stato nascosto)** — il riassunto compresso che una rete ricorrente si porta dietro leggendo una sequenza un elemento alla volta, aggiornato a ogni nuovo elemento letto. Lezione 4.

**Capacità** — quanto complicate sono, in linea di principio, le relazioni che una rete può rappresentare; dipende da quanti piani ha e quanto sono larghi. Lezione 2.

**Colpa (errore locale)** — il segnale, calcolato per ciascun piano durante la backpropagation, che dice quanto e in che direzione correggere i pesi di quel piano specifico. Lezione 6.

**Cross-entropy (test a crocette)** — il modo standard di misurare l'errore quando l'output è una scelta fra categorie; penalizza pesantemente la sicurezza riposta sulla categoria sbagliata. Lezione 5.

**Data augmentation** — aumentare artificialmente la varietà degli esempi di allenamento con piccole trasformazioni che non ne cambiano il significato, per rendere più difficile la memorizzazione. Lezione 7.

**Dropout** — spegnere temporaneamente, a caso, alcuni controllori di un piano durante l'allenamento, per impedire che facciano eccessivo affidamento gli uni sugli altri. Lezione 7.

**Early stopping** — fermare l'allenamento non appena le prestazioni sull'insieme di validazione smettono di migliorare, invece di proseguire fino a un numero di sessioni deciso in anticipo. Lezione 7.

**Errore quadratico medio (MSE)** — il modo standard di misurare l'errore quando l'output è un numero continuo; penalizza gli errori grandi in modo più che proporzionale rispetto a quelli piccoli. Lezione 5.

**Funzione di attivazione (manopola sfumata)** — la trasformazione che rende un controllore capace di rispondere con gradualità invece che con uno scatto netto acceso/spento, l'ingrediente che rende una rete a più piani più espressiva di un solo interruttore. Lezione 1.

**Funzione di perdita (loss)** — un numero che misura quanto una rete sta sbagliando in questo momento, piccolo quando va bene, grande quando va male. Lezione 5.

**Gating** — il meccanismo con cui LSTM e GRU decidono esplicitamente cosa mantenere del bigliettino mentale precedente e cosa sovrascrivere con l'informazione nuova. Lezione 4.

**Inizializzazione dei pesi** — la scelta dei valori di partenza di una rete prima dell'allenamento; deve essere casuale ma contenuta, mai tutta a zero. Lezione 7.

**Insieme di validazione** — un gruppo di esempi tenuto volutamente da parte, mai usato per correggere i pesi, su cui si controlla periodicamente se la rete sta davvero generalizzando. Lezione 7.

**Learning rate scheduling** — far variare, durante l'allenamento, la dimensione del passo di correzione: piccola all'inizio, poi più grande, poi di nuovo piccola verso la fine. Lezione 7.

**LSTM / GRU** — varianti delle reti ricorrenti che tengono un "diario" più selettivo del semplice bigliettino, alleviando (ma non eliminando) il problema della memoria che si sporca su sequenze lunghe. Lezione 4.

**Momentum** — accumulare una specie di velocità nella direzione di correzione seguita consistentemente nei passi recenti, invece di correggersi in modo indipendente ogni volta. Lezione 7.

**Overfitting** — quando una rete smette di imparare la regola generale e comincia a memorizzare le particolarità specifiche degli esempi di allenamento, riconoscibile da un errore di validazione che risale mentre quello di allenamento continua a scendere. Lezione 7.

**Percettrone** — il primo modello di neurone artificiale capace di correggere da solo i propri pesi da esempi passati, proposto da Rosenblatt nel 1958. Lezione 1.

**Pooling (max pooling)** — riassumere una piccola regione di una mappa di caratteristiche in un singolo valore (tipicamente il massimo), per ridurre la dimensione e aggiungere tolleranza a piccoli spostamenti. Lezione 3.

**Regolarizzazione** — penalizzare esplicitamente i pesi troppo grandi aggiungendo una "multa" alla funzione di perdita, per contenere il rischio di overfitting senza cambiare l'architettura della rete. Lezione 7.

**Rete convoluzionale (CNN)** — un'architettura pensata per immagini e altri dati a griglia, che usa piccoli stencil riusati su ogni porzione dell'input invece di collegare ogni controllore a tutti i punti dell'input. Lezione 3.

**Rete feedforward multistrato** — una rete organizzata in più piani, ciascuno che guarda solo l'output del piano precedente, capace di rappresentare confini di decisione curvi e non solo rette. Lezione 2.

**Rete neurale ricorrente (RNN)** — un'architettura pensata per sequenze di lunghezza variabile, che mantiene un bigliettino mentale aggiornato un elemento alla volta. Lezione 4.

**Sequenzialità** — il vincolo, tipico delle reti ricorrenti, per cui elaborare un elemento di una sequenza richiede di aver già elaborato tutti quelli precedenti, impedendo il calcolo in parallelo. Lezione 4, Lezione 8.

**Stencil (filtro, kernel)** — una piccola griglia di numeri fatta scorrere su un'immagine per riconoscere un motivo locale specifico, sempre lo stesso ovunque appaia. Lezione 3.

**Strato nascosto** — un piano intermedio di controllori, fra l'input e l'output di una rete, le cui decisioni non vengono osservate direttamente ma servono solo come passaggio interno. Lezione 2.

**Transformer** — l'architettura, proposta nel 2017, che elimina del tutto la lettura sequenziale delle reti ricorrenti costruendo un modello basato unicamente sull'attenzione. Lezione 8.

**Underfitting** — quando una rete non riesce a catturare nemmeno i pattern presenti negli esempi di allenamento, riconoscibile da un errore alto sia in allenamento sia in validazione. Lezione 7.

**Vanishing gradient nel tempo (bigliettino che si sporca)** — l'affievolirsi progressivo del segnale di correzione quando deve attraversare molti passaggi temporali successivi in una rete ricorrente, tipico di sequenze lunghe. Lezione 4.

**XOR** — un problema logico che risponde "sì" se esattamente uno fra due segnali è attivo; l'esempio classico che un solo percettrone non può risolvere, risolvibile invece impilando più controllori. Lezione 1, Lezione 2.

---

*Fine del libro.*
