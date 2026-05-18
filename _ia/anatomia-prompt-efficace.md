---
title: 'Anatomia di un Prompt Efficace'
date: '2026-05-17T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

Quando si chiede qualcosa a un modello linguistico e la risposta arriva vaga, generica o completamente fuori bersaglio, la tentazione è di pensare che "l'AI non funziona". Quasi sempre, però, il problema non è il modello: è il prompt. Imparare a costruire un prompt efficace è un'abilità pratica, non un'arte oscura, e si impara in fretta una volta che si capisce di cosa è fatto.

## I Cinque Elementi di un Buon Prompt

Un prompt si compone di cinque elementi fondamentali. Non è necessario usarli tutti ogni volta — un ottimo punto di partenza è già **compito, contesto e formato** — ma conoscerli tutti permette di sapere cosa aggiungere quando la risposta non convince.

Il **ruolo** risponde alla domanda: *chi deve essere l'AI in questa interazione?* Assegnare un ruolo — "sei un copywriter esperto", "sei un insegnante di matematica delle medie" — non è una formula magica, ma orienta il modello verso un registro, un livello di dettaglio e un vocabolario coerenti con il compito. È la differenza tra parlare con un generico assistente e parlare con qualcuno che sa già in quale direzione muoversi.

Il **contesto** risponde alla domanda: *cosa deve sapere l'AI prima di rispondere?* Senza contesto il modello deve indovinare. Con il contesto giusto — il tipo di progetto, il pubblico di destinazione, i vincoli pratici — può adattare la risposta alla situazione reale invece di produrre qualcosa di astratto e riusabile da chiunque.

Il **compito** è il cuore del prompt: *cosa deve fare, concretamente?* Verbi precisi come "riassumi", "genera", "confronta", "elenca", "riscrivi" producono risultati molto più mirati di richieste aperte come "parlami di" o "aiutami con".

Il **formato** risponde alla domanda: *come deve presentarsi la risposta?* Una tabella, un elenco puntato, un paragrafo, duecento parole, un titolo seguito da tre varianti: specificarlo evita di ricevere muri di testo quando si voleva un riassunto, o viceversa.

Infine i **vincoli** definiscono ciò che non si vuole: *in italiano, senza tecnicismi, senza claim non verificabili, tono formale*. Sono spesso la differenza tra una risposta buona e una risposta davvero utilizzabile.

## La Formula in Pratica

Ecco un prompt costruito con tutti e cinque gli elementi:

> **[RUOLO]** Sei un copywriter esperto di e-commerce. **[CONTESTO]** Devo lanciare uno zaino da viaggio per nomadi digitali, prezzo €120. **[COMPITO]** Scrivi 3 varianti di descrizione prodotto. **[FORMATO]** Ogni variante max 60 parole, con un titolo accattivante. **[VINCOLI]** Tono ironico, niente claim non verificabili.

La stessa AI, con un prompt costruito così, produce qualcosa di professionale e direttamente utilizzabile invece di una risposta generica. Non cambia il modello: cambia la qualità dell'istruzione che gli viene data.

## Tre Tecniche da Tenere nello Zaino

Oltre alla struttura degli elementi, esistono tre approcci consolidati che vale la pena conoscere.

Il più semplice è lo **Zero-Shot**: si chiede e basta, senza esempi. Funziona bene per compiti chiari e ben definiti, come "traduci questo testo in inglese" o "elenca i vantaggi di questa soluzione". Quando il compito è ambiguo o richiede uno stile specifico, però, non basta.

In quel caso si ricorre al **Few-Shot**: si forniscono due o tre esempi del tipo di output desiderato, e il modello impara per imitazione. Se voglio che associa ogni animale a un'emoji e scrivo `cane → 🐕`, `gatto → 🐈`, il modello capisce il pattern e lo applica senza che io debba spiegarlo a parole. Gli esempi parlano più di molte istruzioni.

La terza tecnica è il **Chain-of-Thought**: si chiede esplicitamente al modello di ragionare passo dopo passo prima di rispondere. Aggiungere "pensa passo per passo, poi rispondi" a un prompt complesso migliora sensibilmente la qualità del ragionamento, in particolare per problemi matematici, logici o che richiedono deduzioni concatenate. Il modello, esternalizzando il ragionamento, commette meno errori di salto.

## Un Esempio Concreto: la Email Professionale

Consideriamo un caso pratico. Se chiedo semplicemente *"scrivi un'email per chiedere un rinvio"*, la risposta sarà inevitabilmente vaga: non sa a chi scrivo, perché chiedo il rinvio, che tono voglio usare, né quanto deve essere lunga. Il risultato è qualcosa che potrebbe andare bene per chiunque, il che significa che non va bene per nessuno in particolare.

Se invece scrivo *"sei un assistente professionale. Scrivi un'email al cliente Marco Rossi per posticipare la riunione di lunedì 10 a mercoledì 12. Tono cortese e diretto, max 100 parole, in italiano"*, la risposta è un'email pronta da inviare: oggetto, saluto, motivo, proposta alternativa, chiusura. La differenza non sta nel modello — sta in quanto spazio di interpretazione ho lasciato.

## Le Cinque Trappole Più Comuni

Chi inizia a usare i modelli linguistici cade spesso nelle stesse trappole. La prima è essere **troppo vago**: "aiutami con il marketing" non dà all'AI nessun punto di partenza, e la risposta sarà altrettanto vaga. La seconda è dimenticare il **contesto**: il modello non sa per chi stai scrivendo né in quale situazione ti trovi, e non può indovinarlo.

La terza trappola è concentrare **troppe richieste in un solo prompt**: dieci domande in un unico messaggio producono quasi sempre risposte superficiali su tutto. Meglio un prompt alla volta. La quarta è non specificare il **formato**: se non dici come vuoi la risposta, la ricevi nel modo in cui il modello preferisce darla, che non è necessariamente il modo in cui ti serve.

La quinta trappola, forse la più insidiosa, è **fidarsi ciecamente** dell'output. I modelli linguistici non verificano i fatti: producono testo plausibile, non testo vero. Fatti, numeri, citazioni e link vanno sempre controllati.

## Quello che Ogni Prompter Deve Tenere a Mente

Prima di usare qualunque output in modo serio, vale la pena ricordare alcune caratteristiche strutturali di questi modelli.

Le **allucinazioni** sono risposte inventate ma presentate con tono sicuro: citazioni che non esistono, statistiche false, link a pagine inesistenti. Non sono un difetto correggibile con un aggiornamento: sono una proprietà del modo in cui i modelli funzionano. Verificare è sempre necessario.

I **bias** sono pregiudizi presenti nei dati di addestramento che si riflettono negli output. Il modello non ragiona in modo neutro: riproduce pattern statistici, inclusi quelli distorti. Il pensiero critico non è facoltativo.

La **privacy** è un rischio concreto: incollare dati personali, sanitari, aziendali o riservati in un prompt significa trasmetterli a un servizio esterno. I dati sensibili non appartengono ai prompt.

Infine il **diritto d'autore**: l'output può essere ispirato o derivato da opere protette. Per uso commerciale è opportuno valutare i rischi, soprattutto per testi creativi o materiale visivo generato.
