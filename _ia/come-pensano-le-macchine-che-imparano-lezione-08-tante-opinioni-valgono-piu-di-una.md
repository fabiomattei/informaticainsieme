---
title: 'Lezione 08, Tante opinioni valgono più di una'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Molti piccoli alberi che votano insieme, convergendo in una decisione finale a maggioranza](/images/ia/come-pensano-le-macchine-che-imparano-lezione-08-tante-opinioni-valgono-piu-di-una/come-pensano-le-macchine-che-imparano-lezione-08-tante-opinioni-valgono-piu-di-una.svg){:class="aside-image"}

### 8.1 Il peso del bue e la saggezza della folla

Nel 1906, allo Smithfield Cattle Show in Inghilterra, si tenne un curioso concorso: quasi 800 persone, contadini, macellai, ma anche perfetti estranei senza nessuna competenza specifica di allevamento, provarono a indovinare il peso di un bue esposto in una fiera, semplicemente guardandolo. Nessuno indovinò il peso esatto. Ma lo scienziato Francis Galton, incuriosito, calcolò la *media* di tutte le stime, comprese quelle palesemente sballate, e scoprì che quella media si scostava dal peso reale del bue (543 kg) di meno di mezzo chilo: 542,5 kg, più precisa di quasi ogni singola stima individuale, compresa quella dei macellai esperti.

Questo fenomeno, oggi chiamato la "saggezza della folla", non è magia: gli errori dei singoli partecipanti tendevano a sbagliare in direzioni diverse e più o meno casuali (chi per eccesso, chi per difetto), e mediandoli insieme questi errori tendevano in buona parte a cancellarsi a vicenda, lasciando emergere il segnale comune sottostante. Questa lezione applica esattamente la stessa idea al machine learning: invece di costruire un solo modello, molto elaborato e potenzialmente incline all'overfitting della Lezione 5, se ne costruiscono **molti**, più semplici, e si combinano le loro previsioni.

### 8.2 Bagging: alberi allenati su fette diverse degli stessi dati

Il modo più diretto di applicare questa idea agli alberi decisionali della Lezione 3 si chiama **bagging** (abbreviazione di *bootstrap aggregating*). L'idea: invece di costruire un solo albero su tutto l'insieme di addestramento, se ne costruiscono molti, anche centinaia, ciascuno addestrato non sull'insieme completo, ma su una fetta scelta a caso, estratta con reinserimento (uno stesso esempio può comparire più volte in una fetta, e non comparire affatto in un'altra).

Ogni singolo albero, vedendo solo una porzione diversa e in parte casuale dei dati, tende a essere leggermente diverso dagli altri, magari sceglie una domanda diversa alla radice, o si spinge a memorizzare particolarità leggermente diverse. Preso da solo, ciascun albero resta soggetto all'overfitting descritto nella Lezione 5. Ma proprio come le stime individuali sul peso del bue, gli errori di questi alberi tendono a essere diversi gli uni dagli altri, non tutti nella stessa direzione: quando si combinano le previsioni di tutti gli alberi, a maggioranza per la classificazione, con una media per la regressione, gli errori individuali tendono in buona parte a cancellarsi, e il risultato complessivo generalizza spesso molto meglio di qualunque singolo albero.

### 8.3 Random forest: aggiungere ancora più varietà

Il bagging da solo funziona, ma ha un limite: se una caratteristica è particolarmente informativa (pensa al "suono cupo" della Lezione 3, quasi perfettamente predittivo da solo), quasi tutti gli alberi tenderanno a sceglierla come domanda alla radice, finendo per assomigliarsi fra loro più di quanto sarebbe utile, e alberi troppo simili fra loro fanno errori troppo simili fra loro, vanificando in parte l'effetto di cancellazione degli errori visto sopra.

La **random forest** ("foresta casuale") aggiunge un secondo livello di casualità proprio per contrastare questo effetto: oltre ad addestrare ogni albero su una fetta casuale dei dati (come nel bagging), a ogni singolo nodo dell'albero, invece di considerare *tutte* le caratteristiche disponibili per scegliere la domanda migliore, se ne considera solo un sottoinsieme scelto a caso. Questo costringe anche gli alberi che vedono gli stessi dati a esplorare strade diverse, aumentando la varietà complessiva della foresta, e con essa, tipicamente, la qualità del voto finale.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 200" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="rf-title rf-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="rf-title">Dati e caratteristiche mescolati a caso per ogni albero</title>
  <desc id="rf-desc">Un insieme di dati completo viene diviso in fette casuali con reinserimento, e a ogni albero viene mostrato solo un sottoinsieme casuale delle caratteristiche disponibili a ogni nodo.</desc>

  <rect x="20" y="20" width="120" height="60" rx="8" fill="#eef2f7" stroke="#2a7ae2" stroke-width="1.5" />
  <text x="80" y="45" fill="#111" font-size="11" text-anchor="middle">insieme dati</text>
  <text x="80" y="62" fill="#111" font-size="11" text-anchor="middle">completo</text>

  <path d="M 140,50 L 200,50" fill="none" stroke="#828282" stroke-width="1.5" marker-end="url(#af)" />
  <defs><marker id="af" markerWidth="8" markerHeight="8" refX="6" refY="4" orient="auto" markerUnits="userSpaceOnUse"><path d="M0,0 L8,4 L0,8 z" fill="#828282" /></marker></defs>

  <g font-size="10" fill="#111" text-anchor="middle">
    <rect x="210" y="20" width="90" height="34" rx="6" fill="#fdfdfd" stroke="#3aa655" /><text x="255" y="41">fetta 1 + feature casuali</text>
    <rect x="210" y="62" width="90" height="34" rx="6" fill="#fdfdfd" stroke="#3aa655" /><text x="255" y="83">fetta 2 + feature casuali</text>
    <rect x="210" y="104" width="90" height="34" rx="6" fill="#fdfdfd" stroke="#3aa655" /><text x="255" y="125">fetta 3 + feature casuali</text>
  </g>

  <g stroke="#828282" stroke-width="1.5">
    <path d="M 300,37 L 350,37" marker-end="url(#af)" /><path d="M 300,79 L 350,79" marker-end="url(#af)" /><path d="M 300,121 L 350,121" marker-end="url(#af)" />
  </g>
  <g font-size="10" fill="#111" text-anchor="middle">
    <rect x="360" y="20" width="60" height="34" rx="6" fill="#fde8d6" stroke="#f66a0a" /><text x="390" y="41">albero 1</text>
    <rect x="360" y="62" width="60" height="34" rx="6" fill="#fde8d6" stroke="#f66a0a" /><text x="390" y="83">albero 2</text>
    <rect x="360" y="104" width="60" height="34" rx="6" fill="#fde8d6" stroke="#f66a0a" /><text x="390" y="125">albero 3</text>
  </g>

  <text x="260" y="180" fill="#828282" font-size="11" text-anchor="middle">ogni albero vede dati e domande possibili leggermente diversi</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">La doppia casualità, dati e caratteristiche, è ciò che distingue la random forest dal semplice bagging.</figcaption>
</figure>

### 8.4 Boosting: imparare uno dopo l'altro dagli errori altrui

Bagging e random forest costruiscono i loro modelli **in parallelo**, tutti indipendenti fra loro, e li combinano solo alla fine. Il **boosting** segue invece una strategia opposta e **sequenziale**: costruisce un primo modello semplice (spesso un albero molto piccolo, con solo una o due domande), guarda su quali esempi ha sbagliato, e costruisce un secondo modello che si concentra proprio su quegli esempi difficili, dando loro più peso, un po' come un insegnante che, dopo aver corretto un compito, dedica la lezione successiva soprattutto agli errori più comuni, invece di ripetere daccapo tutto il programma allo stesso ritmo.

Il procedimento continua per molti passaggi: ogni nuovo modello si concentra sugli errori residui lasciati da tutti i modelli precedenti messi insieme, e la previsione finale combina i voti di tutti i modelli costruiti lungo il percorso, spesso dando più peso a quelli che si sono dimostrati più affidabili. Il risultato tende a essere molto accurato, anche se, a differenza del bagging, dove ogni albero è indipendente e quindi facile da addestrare in parallelo su computer diversi, il boosting è intrinsecamente sequenziale: il modello numero 10 non può essere costruito prima di aver visto gli errori dei nove precedenti.

### 8.5 Perché combinare modelli spesso batte un solo modello elaborato

Vale la pena chiudere con la domanda che lega insieme tutta questa lezione: perché mai dovrebbe funzionare meglio combinare cento alberi mediocri piuttosto che costruirne uno solo, il più elaborato e accurato possibile? La risposta, come nel concorso del bue di Galton, sta nel modo in cui si comportano gli errori. Un singolo modello molto complesso rischia l'overfitting descritto nella Lezione 5: i suoi errori su dati nuovi tendono a riflettere le particolarità specifiche, spesso casuali, dell'insieme di addestramento che ha visto. Molti modelli più semplici, ciascuno addestrato su una porzione diversa e in parte casuale dei dati (o concentrato su errori diversi, nel caso del boosting), tendono invece a commettere errori diversi gli uni dagli altri; combinandoli, buona parte di questi errori individuali si cancella a vicenda nella media o nel voto finale, lasciando emergere un segnale più affidabile di quanto qualunque singolo modello, da solo, riuscisse a offrire.

### 8.6 Un nucleo di strumenti, non ancora tutto

Le otto lezioni percorse finora hanno costruito, un pezzo alla volta, il nucleo degli strumenti più usati nel machine learning "classico": imparare da esempi invece che da regole scritte a mano (Lezione 1), classificare guardando i vicini più simili (Lezione 2) o ponendo una sequenza di domande (Lezione 3), prevedere numeri con una retta (Lezione 4), riconoscere e correggere overfitting e underfitting (Lezione 5), misurare onestamente quanto un modello è davvero bravo (Lezione 6), trovare struttura nei dati senza etichette (Lezione 7), e combinare più modelli per ottenere risultati più affidabili di qualunque singolo modello (questa lezione).

Restano ancora alcune idee, altrettanto diffuse nella pratica, che completano questo nucleo prima di chiudere il libro: classificare sommando tanti indizi deboli invece di porli in sequenza (Lezione 9), separare due categorie tracciando il confine più sicuro possibile (Lezione 10), semplificare tanti indizi in pochi senza perdere l'essenziale (Lezione 11), scoprire quali eventi tendono a presentarsi insieme (Lezione 12), e riconoscere ciò che non assomiglia a nulla di già visto (Lezione 13). Con quest'ultima si chiuderà anche il discorso, rimandato fin dalla prefazione, sul limite condiviso da tutti gli algoritmi di questo libro, e su dove porta il passo successivo.

---

> **Prova tu, Il voto di cinque alberi**
>
> Cinque piccoli alberi decisionali, ciascuno addestrato su una fetta diversa degli stessi dati, sono stati messi alla prova su tre angurie nuove. Ecco le loro previsioni individuali, e la verità (scoperta poi aprendo le angurie):
>
> | Anguria | Albero 1 | Albero 2 | Albero 3 | Albero 4 | Albero 5 | Verità |
> |---|---|---|---|---|---|---|
> | X | sì | sì | no | sì | sì | sì |
> | Y | no | sì | no | no | sì | no |
> | Z | sì | no | sì | sì | no | sì |
>
> 1. Per ciascuna anguria, calcola il voto di maggioranza fra i cinque alberi, e verifica se corrisponde alla verità.
> 2. Guarda ora l'Albero 3 da solo: su quante delle tre angurie ha risposto correttamente? E l'Albero 2, da solo?
> 3. Confronta l'accuratezza del voto di maggioranza (calcolata al punto 1) con l'accuratezza del singolo albero meno accurato che hai trovato al punto 2. Cosa dimostra questo confronto sul valore di combinare più modelli, anche quando alcuni di essi, presi singolarmente, sbagliano spesso?

---

*Continua con la [Lezione 09, La somma degli indizi]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-lezione-09-la-somma-degli-indizi.md %}.html)*
