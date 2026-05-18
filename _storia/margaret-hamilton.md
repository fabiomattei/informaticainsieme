---
title: "Margaret Hamilton"
date: '2026-05-18'
author: Fabio Mattei
layout: page
---

## La madre dell'ingegneria del software

C'è una fotografia del 1969 che è diventata una delle immagini più iconiche della storia dell'informatica. Margaret Hamilton è in piedi accanto a una pila di fogli stampati alta quanto lei: è il codice sorgente del software di navigazione che avrebbe portato gli astronauti dell'Apollo 11 sulla Luna. La foto non era stata pianificata come statement — lei stava solo mostrando quanto lavoro ci fosse stato — ma è diventata il simbolo di qualcosa di più grande: la dimostrazione che il software, fino ad allora trattato come un dettaglio tecnico di seconda categoria, era in realtà la spina dorsale di una delle imprese più ambiziose che l'umanità avesse mai tentato.

Margaret Hamilton non si limitò a scrivere quel codice. Inventò il modo di pensare al software stesso.

## Dagli studi alla NASA, per caso

![Margaret Hamilton accanto allo stack del codice sorgente del software Apollo (Draper Laboratory, 1969, dominio pubblico)](/images/storia/margaret-hamilton/margaret-hamilton-apollo.png){:class="aside-image" style="max-width: 38%;"}

Margaret Hamilton nacque nel 1936 a Paoli, Indiana. Studiò matematica al Earlham College e si laureò nel 1958, con l'intenzione di proseguire con un dottorato in matematica astratta al Brandeis. Per finanziare gli studi, prese un lavoro temporaneo al MIT: avrebbe dovuto essere una parentesi. Divenne invece il centro della sua vita professionale.

Al MIT si trovò a lavorare con i primi computer disponibili, imparando a programmarli in modo autodidatta — non esistevano corsi formali, non esisteva una disciplina codificata, non esistevano manuali che dicessero come si faceva. Chi programmava in quegli anni lo faceva per tentativi, per intuizione, per una logica che si costruiva mentre si procedeva. Hamilton aveva quella logica in abbondanza.

Nel 1961 iniziò a lavorare sul progetto SAGE, un sistema di difesa aerea che usava computer per tracciare aerei nemici in tempo reale — uno dei primi sistemi software di grandi dimensioni mai costruiti. Fu lì che capì qualcosa di fondamentale: un sistema software complesso non si comporta come la somma delle sue parti. Ha una sua natura, suoi modi di fallire, sue sorprese. Trattarlo come matematica applicata non bastava.

## Il software che portò l'uomo sulla Luna

Nel 1965 la NASA stava cercando qualcuno per sviluppare il software di navigazione e controllo per il programma Apollo. Il MIT Instrumentation Laboratory ottenne il contratto, e Hamilton divenne la responsabile del progetto. Aveva ventinove anni.

Il compito era senza precedenti. Il software doveva girare su computer con risorse che oggi sarebbero considerate ridicolmente esigue — meno di 4 KB di RAM, un processore lento per gli standard di qualsiasi epoca — e doveva guidare tre astronauti nello spazio, in orbita lunare, e farli atterrare su un corpo celeste a 384.000 chilometri dalla Terra. Non c'era margine per l'errore. Non esistevano precedenti a cui guardare. Non esisteva nemmeno, in senso formale, la disciplina che avrebbe dovuto fare quel lavoro.

Hamilton si trovò a costruire il software e, allo stesso tempo, a costruire il modo di costruire il software. Ogni problema che incontrava diventava un principio, ogni errore diventava una regola. A poco a poco, quello che stava facendo prese una forma riconoscibile: era ingegneria. Era un'ingegneria del software.

Coniò lei stessa l'espressione — "software engineering" — in un'epoca in cui i colleghi informatici consideravano il termine eccessivamente pretenzioso. L'ingegneria era roba da chi costruiva ponti e motori, non da chi scriveva codice. Hamilton insistette: il software meritava la stessa rigore metodologico, la stessa attenzione alla struttura, gli stessi standard di qualità. Ci vollero anni perché il resto del mondo concordasse.

## Laureen e la lezione più importante

Un pomeriggio del 1968, Margaret portò con sé al lavoro sua figlia **Laureen**, come faceva spesso. Il laboratorio era aperto anche la notte e nei fine settimana, e trovare una babysitter non era sempre possibile: Laureen cresceva in mezzo ai computer, tra le schermate e i tecnici, in un ambiente che a lei sembrava normale e al mondo esterno sembrava fantascienza.

Quel pomeriggio, mentre la madre lavorava, Laureen si sedette alla consolle del simulatore di missione e cominciò a premere tasti. Premette quelli sbagliati, nella sequenza sbagliata, e fece andare in crash la simulazione. Il programma che aveva attivato per errore — P01, una routine di navigazione prelanciomissione — non avrebbe mai dovuto essere eseguito durante il volo. Ma nessuno aveva previsto quella possibilità: il software assumeva che gli astronauti sapessero esattamente cosa fare, e non era stato scritto nessun controllo per impedire quella sequenza.

Hamilton capì immediatamente l'implicazione: quello che aveva fatto Laureen avrebbe potuto farlo un astronauta stanco, distratto, sotto pressione. Gli esseri umani commettono errori. I sistemi che non lo prevedono non sono sistemi robusti: sono sistemi che aspettano il momento giusto per fallire.

Propose di aggiungere un controllo che impedisse l'esecuzione di P01 durante il volo. I supervisori dissero di no: gli astronauti sono professionisti addestrati, non commettono errori così grossolani, non vale la pena aggiungere complessità al codice. Hamilton aggiunse comunque una nota nel manuale di bordo. Tre anni dopo, durante la missione Apollo 8, un astronauta eseguì accidentalmente P01 durante il volo. Fu il manuale a salvare la situazione.

## I sistemi fault-tolerant e l'approccio sistema di sistemi

L'episodio di Laureen non fu un caso isolato ma il punto di partenza di una riflessione più profonda che avrebbe cambiato il modo di pensare al software critico.

Hamilton capì che tutti i sistemi, senza eccezione, sono soggetti a eventi inaspettati. Non si tratta di prevedere ogni possibile errore — è impossibile — ma di costruire sistemi capaci di rispondere agli errori in modo intelligente. Nacque così il concetto di **software fault-tolerant**: un software che non si blocca di fronte all'imprevisto, ma lo gestisce.

Il software che Hamilton progettò per l'Apollo era in grado di operare per priorità. In ogni momento sapeva quali funzioni erano essenziali e quali erano secondarie. Se le risorse di calcolo diventavano insufficienti — se il computer si trovava sovraccarico — il sistema era in grado di sospendere autonomamente le funzioni meno critiche per mantenere attive quelle indispensabili. Mai smettere completamente di operare: questo era il principio.

![Apollo Guidance Computer con il pannello DSKY (NASA, dominio pubblico)](/images/storia/margaret-hamilton/apollo-guidance-computer.jpg){:class="aside-image" style="max-width: 40%;"}

Quella logica salvò la missione Apollo 11 il 20 luglio 1969, a pochi minuti dall'allunaggio. Durante la discesa verso la superficie lunare, il computer di bordo cominciò a generare allarmi — codici di errore che nessuno aveva mai visto durante le simulazioni. Il codice era 1202: overflow del computer, troppe richieste di elaborazione in contemporanea. Nelle sale di controllo a Houston si bloccò il respiro.

Il computer non si bloccò. Il software di Hamilton riconobbe la situazione, scartò i task non essenziali — stava raccogliendo dati dal radar di rendezvous che in quel momento non servivano — e mantenne attive le funzioni di guida e navigazione. Gli allarmi continuarono a suonare, ma il sistema continuò a funzionare. Neil Armstrong atterrò sul Mare della Tranquillità con quattro secondi di carburante rimasto.

## Il sistema di sistemi

C'è un ultimo principio che Hamilton ha portato nell'ingegneria del software, forse il più difficile da far accettare: il **sistema di sistemi**.

Quando si costruisce qualcosa di complesso — un software di navigazione spaziale, ma anche un sistema bancario, un sistema di controllo del traffico aereo, qualsiasi infrastruttura critica — non si possono progettare le parti in isolamento e aspettarsi che, mettendole insieme, il tutto funzioni. Ogni componente può funzionare perfettamente preso da solo e generare comportamenti imprevedibili quando interagisce con gli altri. Il sistema ha proprietà emergenti che non appartengono a nessuna delle parti.

Ma c'è di più: gli esseri umani sono parte del sistema. Non sono utenti esterni che interagiscono con qualcosa di separato da loro: sono elementi interni con i loro comportamenti, i loro errori, le loro routine e le loro eccezioni. Laureen al simulatore non era un'anomalia: era il sistema che funzionava esattamente come funziona — con esseri umani dentro. Progettare il software senza tenerne conto non è un'omissione tecnica: è un errore concettuale.

Questo approccio, che Hamilton ha sviluppato e formalizzato nel corso di decenni, è oggi alla base di ogni metodologia seria di ingegneria dei sistemi critici. Si chiama **DBTF** — *Development Before the Fact* — ed è il framework che ha costruito dopo Apollo per portare queste idee fuori dalla NASA e nell'industria.

## Il riconoscimento

Nel 2003 la NASA le ha dedicato un premio intitolato proprio a lei: il **NASA Exceptional Space Act Award**, uno dei riconoscimenti più alti che l'agenzia possa conferire a un civile. Nel 2016 il presidente Barack Obama le ha consegnato la **Presidential Medal of Freedom**, la più alta onorificenza civile degli Stati Uniti.

Aveva ricevuto il brevetto intellettuale di un'intera disciplina. Il riconoscimento ufficiale arrivò con cinquant'anni di ritardo — ma arrivò mentre era ancora viva per riceverlo, il che la mette in una categoria fortunata rispetto ad altri pionieri di questa storia.
