---
title: 'Lezione 10 — Cosa ci aspetta'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 10.1 Un gigante non entra in tasca

I modelli più capaci di cui senti parlare sono enormi — servono computer specializzati grandi come armadi, non certo lo smartphone in tasca. Ma molti usi pratici (un assistente vocale sempre acceso sul telefono, un correttore che funziona anche senza connessione internet) hanno bisogno esattamente del contrario: qualcosa di piccolo, veloce, capace di girare su un dispositivo con poca memoria e poca batteria. Da qui nasce un intero filone di tecniche per **rendere un modello più piccolo senza perdere troppo delle sue capacità** — un compromesso che, come vedremo, richiama da vicino i rendimenti decrescenti della Lezione 4: si può alleggerire parecchio prima che le prestazioni comincino a peggiorare sul serio.

### 10.2 Il maestro e l'allievo

La prima tecnica si chiama **distillazione**, e l'analogia con la scuola è quasi perfetta: un modello enorme già addestrato (il "maestro") viene usato per generare tantissimi esempi di risposte di alta qualità, e un modello molto più piccolo (l'"allievo") viene allenato a imitare quelle risposte — non a ripartire da zero leggendo l'intero web come ha fatto il maestro, ma a copiarne il comportamento su una quantità di esempi molto più gestibile. Un allievo ben distillato può arrivare a comportarsi sorprendentemente vicino al maestro su molti compiti comuni, pur avendo una frazione delle sue dimensioni — un po' come un apprendista che, guardando lavorare un artigiano esperto per mille ore, impara a riprodurne bene i gesti più frequenti, senza dover reinventare da solo l'intero mestiere.

### 10.3 Alleggerire senza buttare via il necessario

Due tecniche complementari intervengono direttamente sul modello già addestrato, invece che sull'addestramento stesso. La **potatura** (pruning) individua le manopole interne (i parametri della Lezione 4) che contribuiscono pochissimo al comportamento finale — un po' come potare i rami secchi di una pianta, che non servono più a portare linfa ma occupano comunque spazio — e le rimuove, riducendo le dimensioni del modello. La **quantizzazione** invece non rimuove nulla, ma **arrotonda** ogni numero interno a una precisione inferiore: invece di conservare un numero con dodici cifre decimali per ogni manopola, se ne tengono solo due o tre — un po' come arrotondare i prezzi in un preventivo a spanne invece che al millesimo di centesimo. Il modello occupa molta meno memoria e gira più veloce, e sorprendentemente le sue risposte restano quasi indistinguibili da quelle del modello originale a piena precisione: gran parte di quella precisione extra, evidentemente, non stava contribuendo poi molto al risultato finale.

### 10.4 Insegnargli un nuovo trucco senza rifare tutta la scuola

Un'ultima tecnica di efficienza risolve un problema diverso: come specializzare un modello già addestrato per un compito nuovo (ad esempio, rispondere nello stile di una specifica azienda) senza dover rifare da capo l'intero, costosissimo addestramento. La tecnica, chiamata **LoRA**, aggiunge solo un piccolo numero di manopole nuove, "a fianco" di quelle originali che restano congelate — un po' come attaccare un post-it con qualche correzione specifica su un libro di testo già stampato, invece di ristamparlo da zero da un editore. Si allena solo il post-it, non l'intero libro: una frazione minuscola dei parametri totali, con un costo di gran lunga inferiore a un nuovo addestramento completo.

### 10.5 Aprire la scatola nera

Fin qui abbiamo descritto *cosa fa* un LLM, ma c'è un campo di ricerca — relativamente giovane e in rapida crescita — che si chiede una domanda più profonda: possiamo capire, guardando dentro le miliardi di manopole di un modello già addestrato, **cosa rappresenta davvero ciascuna di esse**? Questo si chiama **interpretabilità meccanicistica**, e uno dei risultati più curiosi (e più raccontati) riguarda un esperimento del 2024, ribattezzato informalmente "Golden Gate Claude": i ricercatori sono riusciti a isolare, dentro un modello reale, un singolo "concetto interno" che si attivava sistematicamente ogni volta che il testo riguardava il Golden Gate Bridge di San Francisco — non solo quando il ponte era citato per nome, ma anche in descrizioni indirette. Amplificando artificialmente questo singolo concetto molto oltre il normale, hanno ottenuto un modello ossessionato: qualunque domanda gli venisse posta ("come faccio la pasta al pomodoro?"), trovava un modo — a volte comico, a volte inquietante — di ricondurre la risposta al Golden Gate Bridge. L'esperimento è divertente da raccontare, ma il punto serio è profondo: dimostra che dentro la massa apparentemente indecifrabile di manopole di un modello esistono concetti isolabili e persino manipolabili individualmente — una prima crepa di luce dentro quella che per anni è stata descritta, un po' rassegnati, come una "scatola nera".

### 10.6 Cosa resta da capire

Chiudiamo con onestà: nessuno — nemmeno chi costruisce questi modelli — ha oggi una comprensione completa del perché certe capacità compaiono e altre no, di come evitare del tutto le allucinazioni della Lezione 8 senza sacrificare troppa creatività, o di cosa succederà rendendo questi sistemi ancora più grandi e ancora più capaci di agire da soli nel mondo, come accennato nella Lezione 9. Il campo si muove rapidissimo: quello che oggi è "stato dell'arte" sarà probabilmente superato nel giro di pochi anni, e alcune delle tecniche descritte in questo libro invecchieranno più in fretta di altre. Quello che difficilmente cambierà è il nucleo concettuale che hai visto costruirsi lezione dopo lezione: parole trasformate in numeri, attenzione che pesa il contesto, addestramento su enormi quantità di testo seguito da un affinamento sul comportamento desiderato, e una generazione, parola per parola, guidata da probabilità e da un pizzico di rischio calcolato. È un meccanismo più semplice di quanto il suo comportamento lasci intuire — e proprio per questo, capirlo un pezzo alla volta vale la fatica.

---

> **Prova tu — Le tue previsioni**
>
> Non c'è una risposta "giusta" da confrontare qui — solo la tua opinione, da mettere alla prova col tempo. Scrivi una risposta breve (2-3 frasi) a ciascuna domanda, con la data di oggi accanto:
>
> 1. Tra cinque anni, pensi che i chatbot saranno ancora basati sulla stessa idea di "indovinare la parola successiva" vista nella Lezione 1, o pensi che emergerà un'idea completamente diversa?
> 2. Pensi che il problema delle allucinazioni (Lezione 8) sarà in gran parte risolto, o pensi che sia una conseguenza inevitabile di come funziona il meccanismo, destinata a restare per sempre almeno in parte?
> 3. Pensi che tra cinque anni si parlerà ancora di "modelli enormi in un data center", o pensi che l'efficienza (Sezione 10.1) avrà reso normale avere modelli molto capaci direttamente sul telefono, senza bisogno di internet?
>
> In Appendice A trovi, non delle "risposte corrette" (nessuno le ha), ma un riassunto di cosa pensavano i ricercatori più esperti del settore nel momento in cui questo libro è stato scritto — utile termine di paragone quando rileggerai le tue risposte tra qualche anno.

---

*Continua con l'[Appendice A — Soluzioni ai giochi]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-appendice-a-soluzioni.md %}.html)*
