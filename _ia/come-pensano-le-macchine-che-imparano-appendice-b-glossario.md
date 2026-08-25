---
title: 'Appendice B, Glossario'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Una scheda del glossario con un termine e la sua definizione, indicizzata alfabeticamente](/images/ia/come-pensano-le-macchine-che-imparano-appendice-b-glossario/come-pensano-le-macchine-che-imparano-appendice-b-glossario.svg){:class="aside-image"}

Termini in ordine alfabetico, con un rimando alla lezione dove sono spiegati per la prima volta. Nessuna definizione qui contiene formule: per gli stessi concetti applicati dentro una rete neurale, vedi il glossario di *Come Pensano le Reti Neurali*.

**Accuratezza**, la percentuale di previsioni corrette sul totale; può essere fuorviante quando le categorie sono molto sbilanciate. Lezione 6.

**Albero decisionale**, un modello che classifica un esempio ponendo una sequenza di domande sì/no, una alla volta, scelte automaticamente per separare al meglio gli esempi noti. Lezione 3.

**Apprendimento non supervisionato**, la famiglia di problemi in cui gli esempi non hanno un'etichetta nota, e il compito è trovare una struttura nascosta nei dati, come i gruppi trovati dal clustering. Lezione 1, Lezione 7.

**Apprendimento supervisionato**, la famiglia di problemi in cui ogni esempio di addestramento ha già una risposta giusta nota, usata per insegnare al modello a prevederla anche su casi nuovi. Lezione 1.

**Bagging**, allenare molti modelli (tipicamente alberi) su fette diverse, scelte a caso con reinserimento, degli stessi dati, per poi combinarne le previsioni a voto o a media. Lezione 8.

**Boosting**, costruire molti modelli in sequenza, ciascuno concentrato a correggere gli errori lasciati da tutti quelli costruiti prima di lui. Lezione 8.

**Caratteristica (feature)**, un indizio osservabile su un esempio, usato dal modello per fare le sue previsioni: il suono di un'anguria, l'età di una bicicletta, il prezzo di un oggetto. Lezione 1.

**Classificazione**, il compito di prevedere un'etichetta a categorie (matura o non matura, spam o non spam), non un numero. Lezione 1.

**Clustering**, il compito di raggruppare esempi simili fra loro senza etichette di riferimento. Lezione 7.

**Distanza**, una misura di quanto due esempi, rappresentati come punti nello spazio delle caratteristiche, sono diversi fra loro; più due punti sono vicini, più gli esempi si somigliano. Lezione 2.

**Etichetta (label)**, la risposta giusta che un modello supervisionato deve imparare a prevedere, nota per ogni esempio di addestramento ma non per gli esempi nuovi. Lezione 1.

**Falso negativo**, un esempio realmente positivo che il modello ha previsto, per errore, come negativo (una malattia mancata, uno spam non riconosciuto). Lezione 6.

**Falso positivo**, un esempio realmente negativo che il modello ha previsto, per errore, come positivo (un falso allarme). Lezione 6.

**Foglia**, un nodo finale di un albero decisionale, che non pone altre domande ma restituisce direttamente una decisione. Lezione 3.

**Funzione di perdita (loss)**, un numero che misura complessivamente quanto un modello sbaglia sui dati che ha visto; più è basso, meglio il modello si adatta a quei dati. Lezione 4.

**Insieme di addestramento (training set)**, l'insieme di esempi usato per costruire, o correggere, un modello. Lezione 1, Lezione 5.

**Insieme di test**, un insieme di esempi tenuto completamente da parte, mai usato durante l'addestramento, su cui si misura una volta sola quanto un modello generalizza a casi nuovi. Lezione 5.

**Insieme di validazione**, un insieme di esempi, distinto sia dal training sia dal test, usato per confrontare più modelli o più impostazioni durante lo sviluppo, senza consumare l'insieme di test. Lezione 5.

**k (in k-NN)**, il numero di vicini più prossimi consultati per fare una previsione: piccolo lo rende sensibile a singoli esempi anomali, grande lo rende insensibile alla posizione specifica del punto nuovo. Lezione 2.

**k (in k-means)**, il numero di gruppi che l'algoritmo di clustering deve cercare, scelto in anticipo da chi usa l'algoritmo. Lezione 7.

**k-means**, l'algoritmo di clustering più diffuso, che alterna l'assegnazione di ogni punto al centro più vicino e il ricalcolo di ogni centro come media dei punti a lui assegnati, finché i centri smettono di muoversi. Lezione 7.

**k-NN (k-Nearest Neighbors)**, un algoritmo di classificazione che prevede l'etichetta di un punto nuovo guardando le etichette dei suoi k vicini più prossimi nello spazio delle caratteristiche, e votando a maggioranza. Lezione 2.

**Machine learning (apprendimento automatico)**, l'insieme di tecniche che permettono a un algoritmo di trovare da solo una regola a partire da esempi, invece di riceverla scritta a mano da un programmatore. Lezione 1.

**Matrice di confusione**, una tabella che incrocia le previsioni di un modello con la realtà, distinguendo veri positivi, falsi positivi, veri negativi e falsi negativi. Lezione 6.

**Modello**, la regola, trovata automaticamente da un algoritmo di apprendimento a partire dagli esempi, usata poi per fare previsioni su casi nuovi. Lezione 1.

**Naive Bayes**, un algoritmo di classificazione che combina molti indizi deboli, ciascuno trattato come indipendente dagli altri (l'assunzione "naive"), sommandone il contributo per decidere quale categoria è più probabile. Lezione 9.

**Nodo radice**, il primo nodo, in cima a un albero decisionale, che pone la prima domanda a ogni esempio. Lezione 3.

**Overfitting**, quando un modello smette di imparare la regola generale e comincia a memorizzare le particolarità specifiche degli esempi di addestramento, riconoscibile da un errore di test molto più alto di quello di addestramento. Lezione 5.

**Precisione**, fra tutte le volte che un modello ha previsto la categoria positiva, la frazione di volte in cui aveva ragione. Lezione 6.

**Purezza**, quanto gli esempi rimasti in un gruppo, dopo una domanda posta da un albero decisionale, condividono la stessa etichetta; una domanda buona produce gruppi il più puri possibile. Lezione 3.

**Ramo**, il collegamento fra un nodo di un albero decisionale e il nodo successivo, corrispondente a una possibile risposta alla domanda del nodo. Lezione 3.

**Random forest**, una raccolta di alberi decisionali allenati con bagging, a cui si aggiunge una seconda casualità: a ogni nodo, ogni albero considera solo un sottoinsieme casuale delle caratteristiche disponibili. Lezione 8.

**Regressione**, il compito di prevedere un numero (un prezzo, una temperatura), non una categoria. Lezione 1, Lezione 4.

**Regressione lineare**, un modello di regressione che prevede un numero come combinazione pesata delle caratteristiche in ingresso, rappresentabile come una retta (o un piano, con più caratteristiche) tracciata per adattarsi il più possibile ai dati noti. Lezione 4.

**Richiamo (recall)**, fra tutti gli esempi realmente positivi, la frazione che il modello è riuscito a individuare. Lezione 6.

**Spazio delle caratteristiche**, la mappa immaginaria in cui ogni esempio diventa un punto, e ogni caratteristica diventa una coordinata; la base geometrica su cui si misura la distanza fra esempi. Lezione 2.

**Underfitting**, quando un modello è troppo semplice per catturare nemmeno i pattern presenti negli esempi di addestramento, riconoscibile da un errore alto sia in addestramento sia in test. Lezione 5.

**Vero negativo**, un esempio realmente negativo, previsto correttamente come negativo dal modello. Lezione 6.

**Vero positivo**, un esempio realmente positivo, previsto correttamente come positivo dal modello. Lezione 6.
