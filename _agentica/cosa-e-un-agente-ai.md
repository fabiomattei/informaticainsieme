---
title: "Cos'è un agente AI"
date: '2026-08-14T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

Un **agente AI** è un sistema costruito attorno a un modello linguistico che non si limita a rispondere a una domanda, ma è in grado di percepire un contesto, decidere quali azioni compiere e portarle a termine usando degli strumenti, il tutto per raggiungere un obiettivo che gli è stato affidato. La differenza rispetto a un semplice chatbot non sta nel modello usato, ma nel *ciclo* in cui viene inserito: un agente osserva, ragiona, agisce, osserva di nuovo il risultato della propria azione e decide il passo successivo, ripetendo questo ciclo finché il compito non è concluso o non serve l'intervento umano.

## Gli ingredienti di un agente

Un agente AI moderno è tipicamente composto da alcuni elementi che lavorano insieme:

Il **modello linguistico** è il "cervello" che interpreta il compito, pianifica i passi e decide quando ha bisogno di uno strumento esterno. Da solo, però, un modello linguistico non può fare nulla al di fuori della conversazione: non può leggere un file, chiamare un'API o eseguire codice.

Gli **strumenti** (tool) sono le mani dell'agente: funzioni che il modello può invocare per interagire con il mondo esterno, come cercare informazioni sul web, leggere e scrivere file, eseguire query su un database o chiamare un'API. È l'unione di modello e strumenti a trasformare un generatore di testo in un sistema capace di agire.

La **memoria** permette all'agente di ricordare cosa ha fatto nei passi precedenti all'interno dello stesso compito (memoria a breve termine) o anche tra sessioni diverse (memoria a lungo termine), evitando di ripetere le stesse domande o di perdere il filo di un lavoro complesso.

Il **ciclo di controllo** (spesso chiamato *agent loop*) è il meccanismo che orchestra il tutto: decide quando chiamare il modello, quando eseguire uno strumento, quando fermarsi perché il compito è concluso e quando invece serve chiedere conferma a un essere umano.

## Un esempio concreto

Immaginiamo di chiedere a un agente: "Trova il volo più economico per Roma la prossima settimana e prenotalo se costa meno di 100 euro". Un chatbot tradizionale potrebbe solo spiegare come cercare un volo. Un agente, invece, userebbe uno strumento di ricerca voli, confronterebbe i prezzi, verificherebbe la condizione posta ("meno di 100 euro") e, se soddisfatta, userebbe un secondo strumento per completare la prenotazione, riportando poi all'utente cosa ha fatto.

Questa capacità di concatenare più passi e più strumenti in autonomia, prendendo decisioni lungo il percorso, è ciò che distingue l'**informatica agentica** dalla semplice interazione domanda-risposta con un modello linguistico.

## Perché se ne parla tanto ora

Gli agenti AI non sono un'idea nuova nell'informatica: la ricerca sugli "agenti intelligenti" esiste da decenni nell'ambito dell'intelligenza artificiale classica. Quello che è cambiato negli ultimi anni è la qualità dei modelli linguistici, che oggi sono abbastanza affidabili da pianificare sequenze di azioni sensate, capire quando uno strumento ha fallito e correggere la rotta, rendendo finalmente pratico ciò che prima restava perlopiù teorico.
