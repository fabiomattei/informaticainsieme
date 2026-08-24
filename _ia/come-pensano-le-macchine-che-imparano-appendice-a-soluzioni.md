---
title: 'Appendice A — Soluzioni ai giochi'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un elenco di spunte, una per ogni lezione con soluzione risolta](/images/ia/come-pensano-le-macchine-che-imparano-appendice-a-soluzioni/come-pensano-le-macchine-che-imparano-appendice-a-soluzioni.svg){:class="aside-image"}

Le soluzioni qui sotto sono organizzate per lezione. Per alcuni giochi non esiste un'unica risposta "corretta" — il valore dell'esercizio sta nel ragionamento, non nel risultato. Leggile solo dopo aver provato tu stesso: sbirciare prima toglie gran parte del divertimento.

### Soluzione — Lezione 1: impara a riconoscere l'anguria matura

1. Guardando la tabella, l'unica combinazione che porta a "sì" è suono cupo **insieme a** puntino intenso: entrambe le condizioni devono valere contemporaneamente. Un suono cupo da solo (con puntino pallido) non basta, e un puntino intenso da solo (con suono acuto) nemmeno.
2. L'anguria nuova ha suono cupo e puntino intenso: applicando la regola trovata al punto 1, la classifichi come **matura**.
3. La seconda anguria ha suono acuto e puntino intenso: la regola prevede **non matura**, esattamente lo stesso caso già presente nella tabella (riga 4). La fiducia in questa previsione è ragionevole, perché corrisponde a un esempio già osservato una volta con lo stesso identico esito — ma resta comunque basata su una sola osservazione, non su molte.

### Soluzione — Lezione 2: trova i vicini più vicini

1. Distanze da N=(8,6): A(8,7)=1, B(7,9)=4, C(2,3)=9, D(3,1)=10, E(9,8)=3.
2. Ordinando le distanze crescenti: A=1, E=3, B=4, C=9, D=10. Con k=3, i tre più vicini sono **A, E, B** — tutte e tre etichettate "sì". Il voto a maggioranza dà quindi **matura**.
3. Con k=5 (tutte e cinque le angurie), le etichette sono: A=sì, B=sì, C=no, D=no, E=sì → 3 sì contro 2 no, maggioranza ancora **matura**. La previsione non cambia perché, in questo caso specifico, i tre punti più vicini a N (A, E, B) sono esattamente le tre angurie mature, mentre le due più lontane (C, D) sono esattamente le due non mature: la separazione per distanza coincide perfettamente con la separazione per etichetta, quindi aggiungere le due angurie più lontane al voto (passando da k=3 a k=5) non fa che confermare ciò che i tre vicini più stretti già indicavano.

### Soluzione — Lezione 3: costruisci la radice dell'albero

1. La domanda "il suono è cupo?" divide le otto angurie in: cupo → 4 mature, 0 non mature (gruppo **perfettamente puro**); acuto → 0 mature, 4 non mature (gruppo **perfettamente puro**).
2. La domanda "il peso è pesante?" divide invece in: pesante → 2 mature, 2 non mature; leggero → 2 mature, 2 non mature — entrambi i gruppi restano **mescolati esattamente quanto il gruppo di partenza**, la domanda non ha chiarito nulla.
3. La domanda sul suono produce gruppi perfettamente puri, mentre quella sul peso non separa affatto le etichette: "il suono è cupo?" è nettamente la scelta migliore per il nodo radice, e in questo caso basterebbe da sola per classificare correttamente tutte e otto le angurie, senza bisogno di ulteriori domande.

### Soluzione — Lezione 4: confronta due regole di prezzo

1. Regola A (190 − 20 × età): età 1 → 170 (errore 10); età 2 → 150 (errore 0); età 4 → 110 (errore 20); età 5 → 90 (errore 0). Errore totale: 10+0+20+0 = **30**.
2. Regola B (170 − 15 × età): età 1 → 155 (errore 25); età 2 → 140 (errore 10); età 4 → 110 (errore 20); età 5 → 95 (errore 5). Errore totale: 25+10+20+5 = **60**.
3. La Regola A ha un errore totale (30) nettamente più basso della Regola B (60): rappresenta meglio i dati osservati, ed è quindi la retta da preferire fra le due candidate.

### Soluzione — Lezione 5: diagnostica due modelli

1. Il Modello X (1% di errore in addestramento, 34% in test) mostra un divario enorme fra i due errori: è il segno classico dell'**overfitting**. Il modello ha memorizzato le particolarità specifiche degli esempi di addestramento invece di imparare una regola che generalizza a casi nuovi.
2. Il Modello Y (28% in addestramento, 31% in test) ha un errore alto già sui dati che ha visto durante l'apprendimento, con un divario molto più piccolo rispetto al test: è il segno classico dell'**underfitting**. Il modello è troppo semplice per catturare nemmeno i pattern presenti nei dati di addestramento.
3. Un "Modello Z" ben bilanciato dovrebbe mostrare un errore di addestramento relativamente basso, un errore di test non troppo distante da quello di addestramento (un divario piccolo, non enorme come nel Modello X), ed entrambi gli errori a un livello complessivamente contenuto — non alto come nel Modello Y. In altre parole: sufficientemente complesso da imparare bene dai dati, ma non così complesso da smettere di generalizzare.

### Soluzione — Lezione 6: leggi una matrice di confusione

1. Accuratezza: (12 veri positivi + 174 veri negativi) / 200 = 186/200 = **93%**.
2. Precisione: 12 / (12+6) = 12/18 = **66,7%** (un terzo delle email segnalate come spam, in realtà, non lo era). Richiamo: 12 / (12+8) = 12/20 = **60%** (il modello individua solo 6 email spam su 10).
3. Il 93% di accuratezza sembra ottimo, ma precisione e richiamo raccontano una storia più onesta: un allarme su tre è un falso allarme, e il 40% dello spam reale (le 8 email realmente spam classificate come "non spam") finisce comunque nella posta in arrivo, invisibile e indistinguibile dalla posta legittima, esattamente il tipo di errore silenzioso che l'accuratezza da sola non fa emergere.

### Soluzione — Lezione 7: un giro di k-means a mano

1. Con centro A=3 e centro B=18: il prezzo 3 dista 0 da A e 15 da B → gruppo A; il prezzo 4 dista 1 da A e 14 da B → gruppo A; il prezzo 15 dista 12 da A e 3 da B → gruppo B; il prezzo 18 dista 15 da A e 0 da B → gruppo B; il prezzo 20 dista 17 da A e 2 da B → gruppo B.
2. Gruppo A = {3, 4}, media = 3,5. Gruppo B = {15, 18, 20}, media = (15+18+20)/3 = 53/3 ≈ **17,7**.
3. Riassegnando con i nuovi centri (3,5 e 17,7): il prezzo 3 resta più vicino a 3,5; il 4 resta più vicino a 3,5; il 15, il 18 e il 20 restano tutti più vicini a 17,7. Le assegnazioni sono identiche a quelle del punto 1: l'algoritmo è già arrivato a **convergenza** dopo un solo giro.

### Soluzione — Lezione 8: il voto di cinque alberi

1. Anguria X: 4 "sì" contro 1 "no" → maggioranza **sì**, corrisponde alla verità. Anguria Y: 3 "no" contro 2 "sì" → maggioranza **no**, corrisponde alla verità. Anguria Z: 3 "sì" contro 2 "no" → maggioranza **sì**, corrisponde alla verità. Il voto di maggioranza ha indovinato tutte e tre le angurie: **3 su 3**.
2. L'Albero 3, da solo, risponde: no (X, sbagliato), no (Y, corretto), sì (Z, corretto) → **2 su 3** corrette. L'Albero 2, da solo, risponde: sì (X, corretto), sì (Y, sbagliato), no (Z, sbagliato) → **1 su 3** corrette, il peggiore fra i cinque.
3. Il voto di maggioranza ottiene 3 corrette su 3 (100%), mentre il singolo albero meno accurato (l'Albero 2) ne indovina solo 1 su 3 (33%). Anche se ogni singolo albero, preso da solo, commette diversi errori, gli errori dei cinque alberi cadono su angurie diverse: dove l'Albero 2 sbaglia (Y e Z), gli altri quattro alberi in maggioranza hanno ragione, e viceversa. È esattamente il principio del concorso del bue di Galton della Sezione 8.1: combinare più opinioni imperfette, ma che sbagliano in modo diverso, produce un risultato più affidabile di qualunque opinione singola.

---

*Continua con l'[Appendice B — Glossario]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-appendice-b-glossario.md %}.html)*
