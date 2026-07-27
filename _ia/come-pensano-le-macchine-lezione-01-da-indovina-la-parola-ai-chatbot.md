---
title: 'Lezione 01 — Da "indovina la parola" ai chatbot'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 1.1 L'amico che finisce le tue frasi

Conosci quella persona che, ogni volta che inizi una frase, la finisce al posto tuo? A volte indovina, a volte no, ma ci prova sempre. Ecco: un modello linguistico (in inglese *Large Language Model*, LLM — modello linguistico di grandi dimensioni) è esattamente questo, portato a un livello quasi assurdo. Legge quello che hai scritto finora e prova a indovinare cosa viene dopo. Una parola alla volta. Poi un'altra. Poi un'altra ancora — e mettendo insieme tutte queste piccole previsioni, una via l'altra, viene fuori un'intera risposta.

Sembra un trucco troppo semplice per spiegare conversazioni che sembrano intelligenti. Eppure è davvero così che funziona — il difficile non è l'idea, ma *come* si arriva a indovinare bene. E per capirlo, conviene vedere come ci si è arrivati passo passo, perché all'inizio andava malissimo.

### 1.2 Il primo trucco: contare le parole

Immagina di dover indovinare la parola dopo "il gatto" avendo a disposizione solo un libro enorme di frasi già scritte. La strategia più ingenua possibile: guarda l'ultima parola ("gatto") e conta, in tutto il libro, quale parola compare più spesso subito dopo. Magari è "dorme". Allora scommetti su "dorme".

Questo si chiama modello a **bigrammi**: guarda solo *una* parola indietro per indovinare la prossima. Puoi immaginarlo come un'agenda telefonica gigante: cerchi "gatto", e sotto trovi l'elenco di tutte le parole viste dopo "gatto" nel libro, con quante volte ciascuna è comparsa. Scegli la più frequente (o, per essere un po' più creativo, ne peschi una a caso pesando le probabilità in base a quante volte è comparsa).

Il problema è evidente: "gatto" da solo non basta quasi mai a sapere cosa viene dopo. "Il gatto nero attraversò la strada" e "il gatto di Marco dorme tutto il giorno" hanno in comune solo "il gatto" — dopo, vanno in direzioni completamente diverse. Puoi allargare la memoria a due, tre, quattro parole indietro (**trigrammi**, **quadrigrammi**...), e infatti per decenni — ben prima dei chatbot di oggi — i correttori automatici e i primi traduttori funzionavano più o meno così. Ma ogni parola di memoria in più fa esplodere il numero di combinazioni possibili da contare, e anche con quattro o cinque parole di contesto il modello resta "miope": non si ricorda l'inizio di un discorso lungo, non capisce a chi si riferisce un "lui" comparso tre frasi prima.

### 1.3 Un po' di memoria in più

Il passo successivo è stato costruire modelli capaci di portarsi dietro un "riassunto" di tutto quello che hanno letto finora, non solo delle ultime parole — una specie di bigliettino mentale che si aggiorna a ogni nuova parola letta, invece di dover rileggere tutto da capo ogni volta. Questi modelli (chiamati **reti neurali ricorrenti**) hanno funzionato meglio dei semplici contatori, ma avevano un difetto tipico della memoria umana quando è sotto sforzo: più il discorso si allungava, più il bigliettino mentale si "sporcava", perdendo dettagli importanti comparsi molte parole prima. Era un po' come cercare di ricordare la settima cosa di una lista della spesa dettata a voce, senza scriverla: le prime voci si confondono, le ultime restano fresche.

Serviva un'idea diversa: invece di comprimere tutto in un unico bigliettino che si aggiorna parola per parola, perché non lasciare che il modello **torni a guardare**, ogni volta che gli serve, esattamente le parole di prima che contano di più — qualunque punto del discorso siano, vicine o lontane?

### 1.4 Il salto: guardare tutta la frase in una volta

Questa idea — chiamata **attenzione** — è la svolta di cui parleremo per tutto il resto del libro, a partire dalla prossima lezione. Per ora basti dire questo: invece di leggere una parola alla volta portandosi dietro solo un riassunto compresso, un modello con attenzione può, per ogni parola, "guardare indietro" a tutte le altre parole del testo contemporaneamente e decidere — parola per parola — quali contano di più per capire quella lì.

Nel 2017 un gruppo di ricercatori pubblicò un articolo con un titolo quasi provocatorio: *"L'attenzione è tutto ciò che serve"*. La loro proposta, chiamata **Transformer**, buttava via del tutto l'idea del bigliettino che si aggiorna parola per parola e costruiva un intero modello attorno alla sola attenzione. Si è rivelata una delle idee più influenti dell'informatica recente: quasi tutti i chatbot che usi oggi sono Transformer.

### 1.5 Più letture, più bravo

C'è un secondo ingrediente, altrettanto importante quanto l'architettura: la scala. Un Transformer piccolo, allenato su pochi testi, indovina la parola successiva poco meglio di un modello a bigrammi. Lo stesso Transformer, allenato su *miliardi* di pagine web, libri, articoli — e reso più grande, con più "manopole" interne da regolare — comincia a mostrare comportamenti che sui piccoli modelli semplicemente non esistevano: rispondere a domande mai viste, seguire istruzioni, persino abbozzare un ragionamento. Torneremo su questo salto, che i ricercatori chiamano "scala", nella Lezione 4 e ne discuteremo criticamente nella Lezione 7 — perché non è tutto oro quel che luccica, ed è più sottile di quanto sembri.

Da qui in avanti, ogni volta che pensi a un LLM, tienilo a mente: un motore che indovina la parola successiva, così bravo — perché guarda tutto il contesto con attenzione, e perché ha letto una quantità di testo che nessun umano potrebbe leggere in mille vite — da sembrare capace di conversare.

---

> **Prova tu — Il modello a bigrammi improvvisato**
>
> Prendi questo minuscolo "libro" di quattro frasi già scritte:
>
> 1. Il cane corre nel parco.
> 2. Il cane dorme sul divano.
> 3. Il gatto dorme sul tappeto.
> 4. Il gatto guarda la finestra.
>
> Ora, **senza guardare oltre**, prova a fare da modello a bigrammi: data solo l'ultima parola, quale parola scommetteresti venga dopo?
>
> - Dopo "Il", quale parola è più frequente tra le quattro frasi?
> - Dopo "cane", che succede — puoi prevedere con sicurezza cosa viene dopo, o le frasi 1 e 2 vanno in direzioni diverse?
> - Dopo "dorme", stessa domanda: riesci a indovinare "sul divano" o "sul tappeto" guardando solo "dorme"?
>
> Prova a scrivere le tue tre risposte, poi confrontale con il ragionamento in Appendice A. Il punto dell'esercizio non è "sbagliare o no" — è sentire sulla tua pelle *quanta* informazione perdi guardando indietro una parola sola.

---

*Continua con la [Lezione 02 — Come una macchina legge le parole]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-02-come-una-macchina-legge-le-parole.md %}.html)*
