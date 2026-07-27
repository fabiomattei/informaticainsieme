---
title: 'Lezione 01 — Un interruttore che impara a decidere'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 1.1 Un interruttore con un'opinione

Immagina un interruttore un po' speciale. Non si limita ad accendere o spegnere una luce a comando: guarda alcuni indizi, li soppesa, e decide da solo se "accendersi" oppure no. Facciamo un esempio concreto: devi decidere se prendere l'ombrello uscendo di casa. Guardi due indizi — quanto è nuvoloso il cielo (da 0 a 10) e quanto è umida l'aria (da 0 a 10) — e ti fai un'idea sommandoli, ma non allo stesso modo: magari il cielo nuvoloso conta doppio rispetto all'umidità, perché è l'indizio più affidabile. Se il totale pesato supera una certa soglia, prendi l'ombrello; altrimenti no.

Questo è, in sostanza, il primo modello matematico di un neurone artificiale, proposto nel 1943 da Warren McCulloch e Walter Pitts: un'unità che somma segnali in ingresso, ciascuno con la propria importanza, e "scatta" se la somma supera una soglia. All'epoca i pesi — quanto contava ogni indizio — venivano scelti a mano da chi costruiva il modello. Mancava ancora l'ingrediente più interessante: far sì che l'interruttore imparasse da solo quanto pesare ogni indizio.

### 1.2 Il percettrone: un interruttore che si corregge

Nel 1958 Frank Rosenblatt ebbe l'idea che rese questo modello davvero interessante: il **percettrone**, un interruttore capace di aggiustare da solo l'importanza data a ciascun indizio, imparando da esempi passati. Il meccanismo è sorprendentemente semplice. Ogni volta che il percettrone sbaglia una decisione, si corregge un po': se ha detto "no ombrello" e invece pioveva, aumenta un po' l'importanza data agli indizi che quel giorno erano alti (magari il cielo era molto nuvoloso: la prossima volta peserà di più il cielo nuvoloso); se ha detto "sì ombrello" e invece non pioveva, fa l'esatto opposto. Quando la decisione è già giusta, non cambia nulla — nessun bisogno di correggersi se non si è sbagliato.

Ripetendo questa correzione su tanti giorni passati (di cui conosciamo già se poi ha piovuto o no), il percettrone via via si aggiusta. E Rosenblatt dimostrò qualcosa di notevole: se esiste *un modo qualunque* di tracciare una linea netta che separa perfettamente i giorni di pioggia dai giorni di sole guardando quei due indizi, questa procedura di correzione, ripetuta abbastanza volte, trova sempre quella linea.

### 1.3 Il limite: quando una linea sola non basta

C'è però un problema, e per vederlo conviene un esempio ancora più semplice: quattro palline colorate messe ai quattro angoli di un quadrato, due rosse e due blu, ma disposte in diagonale — rossa in alto a sinistra, blu in alto a destra, blu in basso a sinistra, rossa in basso a destra. Prova a tracciare una singola linea retta che separi tutte le rosse da tutte le blu. Non ci riuscirai: qualunque retta tu disegni, finisce sempre per lasciare una pallina del colore sbagliato dalla parte sbagliata.

Questo schema non è un capriccio inventato apposta: corrisponde esattamente a un problema logico chiamato **XOR** ("o esclusivo"), che risponde "sì" se esattamente uno fra due segnali è attivo, "no" se sono entrambi attivi o entrambi spenti — e nessun singolo percettrone, per quanto ben allenato, può risolverlo. Non è un difetto della procedura di correzione della Sezione 1.2: è un limite geometrico invalicabile, perché un solo interruttore può tracciare solo confini dritti, mai un confine "storto" come richiederebbe questo problema.

### 1.4 Un inverno di delusione, e un indizio già nel problema

Nel 1969 Marvin Minsky e Seymour Papert pubblicarono un'analisi che metteva nero su bianco questo limite, con l'XOR come esempio principale. L'effetto pratico fu enorme e per certi versi ingiusto verso l'idea in sé: i finanziamenti alla ricerca sulle reti neurali si prosciugarono per gran parte degli anni '70, un periodo che si ricorda come il primo "inverno dell'intelligenza artificiale". Eppure lo stesso libro di Minsky e Papert osservava — quasi di passaggio — che impilare più interruttori in fila avrebbe potuto superare il limite. L'osservazione era corretta, ma mancava ancora un pezzo cruciale: nessuno sapeva ancora, all'epoca, *come* allenare in modo efficiente più interruttori collegati insieme. Quel pezzo mancante — che vedremo nella Lezione 6 — sarebbe arrivato solo vent'anni più tardi.

### 1.5 Da un interruttore netto a una manopola che sente le sfumature

C'è un secondo problema, più tecnico ma altrettanto importante, che ha richiesto di ripensare l'interruttore stesso. Un interruttore che scatta di netto — o acceso o spento, senza vie di mezzo — non lascia capire *quanto* fosse vicino a cambiare idea: due situazioni in cui l'interruttore dice "no" con sicurezza opposta (una appena sotto soglia, una lontanissima) restano indistinguibili una volta scattata la decisione finale. Per costruire interruttori capaci di correggersi in modo più fine — specialmente quando ce ne sono tanti collegati in fila, come vedremo nella prossima lezione — serve una manopola che si muova con gradualità, non un tasto che scatta di colpo: qualcosa che, invece di saltare bruscamente da spento ad acceso, scivoli con dolcezza da un estremo all'altro, passando per ogni sfumatura intermedia.

Le reti neurali moderne usano proprio manopole così, chiamate **funzioni di attivazione**. Alcune assomigliano a una diapositiva liscia a forma di "S", che scivola gradualmente da spento ad acceso passando per ogni grado intermedio di certezza. Altre — oggi le più usate — restano completamente spente finché il segnale in ingresso non supera lo zero, e da quel punto in poi salgono in modo diretto e proporzionale, senza mai appiattirsi: una specie di valvola che non lascia passare nulla finché non viene spinta, ma poi risponde in modo pulito e prevedibile a quanto viene spinta. Qual è il punto in comune fra tutte queste manopole, così diverse da un interruttore a scatto? Nessuna di esse ha "spigoli nascosti": si può sempre dire, in ogni punto, se il segnale sta crescendo, calando, o restando fermo — un dettaglio che sembra tecnico, ma che nella Lezione 6 si rivelerà l'ingrediente decisivo per insegnare a un'intera fila di interruttori a correggersi insieme.

### 1.6 Perché serve proprio questa sfumatura

Si potrebbe pensare: perché non impilare tanti interruttori netti, uno sopra l'altro, e lasciare che la loro combinazione faccia il lavoro sporco? Il problema è che impilare interruttori che si limitano a sommare segnali, senza alcuna manopola sfumata nel mezzo, non aggiunge davvero potere decisionale: è un po' come fotocopiare una fotocopia — il risultato resta piatto quanto l'originale, non importa quante volte lo ripeti. È proprio la manopola sfumata della Sezione 1.5, inserita fra un interruttore e il successivo, a rompere questa piattezza e a permettere a una fila di interruttori di rappresentare confini di decisione curvi, spezzati, complicati quanto serve — non solo la linea dritta del percettrone singolo.

Quanto complicati, esattamente? Un risultato matematico sorprendente — che qui citiamo senza dimostrare — garantisce che una fila di interruttori-con-manopola sufficientemente numerosa può avvicinarsi quanto si vuole a *qualunque* relazione ragionevole fra indizi in ingresso e decisione in uscita, per quanto complicata. Non dice quanti interruttori servano per un caso specifico (a volte moltissimi), né come trovare le impostazioni giuste — quello è il compito di allenamento che occuperà buona parte di questo libro — ma stabilisce che il limite non è "quanto sono espressive le reti neurali in teoria", bensì, in pratica, quanti dati, quanto calcolo, e quanto è bravo l'algoritmo che le allena.

---

> **Prova tu — Correggi l'interruttore dell'ombrello**
>
> Il tuo interruttore-ombrello decide guardando due indizi, ciascuno da 0 a 10: **C** (quanto è nuvoloso il cielo) e **U** (quanto è umida l'aria). Il punteggio è 2×C + 1×U (il cielo pesa doppio), e la soglia per dire "sì ombrello" è 15.
>
> Oggi: C = 6, U = 4. Punteggio: 2×6 + 1×4 = 16. L'interruttore dice **sì, ombrello** — ed effettivamente ha piovuto. Nessuna correzione necessaria, la decisione era giusta.
>
> Regola di correzione (solo se l'interruttore ha sbagliato): se ha detto "no" ma la risposta giusta era "sì", **aumenta di 1** il peso di ogni indizio che oggi valeva più di 5; se ha detto "sì" ma la risposta giusta era "no", **diminuisci di 1** il peso di ogni indizio che oggi valeva più di 5.
>
> Ora tocca a te. Domani: C = 3, U = 8, pesi ancora 2 e 1, soglia sempre 15.
>
> 1. Calcola il punteggio di domani. L'interruttore dice sì o no ombrello?
> 2. In realtà, domani pioverà. L'interruttore ha sbagliato? Se sì, applica la regola di correzione e scrivi i nuovi pesi.
> 3. Con i pesi corretti, ricalcola il punteggio di domani: la decisione ora è quella giusta?

---

*Continua con la [Lezione 02 — Impilare le decisioni: la rete a più piani]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-02-impilare-le-decisioni.md %}.html)*
