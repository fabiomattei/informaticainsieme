---
title: "Rischi e sicurezza degli agenti AI"
date: '2026-08-14T09:25:00+02:00'
author: Fabio Mattei
layout: page
---

Dare a un sistema AI la capacità di agire in autonomia — non solo di rispondere, ma di eseguire codice, modificare file, spendere denaro o inviare messaggi — porta con sé vantaggi enormi in termini di produttività, ma anche una categoria di rischi che un semplice chatbot non pone. Conoscerli è il primo passo per progettare (o anche solo usare) agenti in modo responsabile.

## Errori che si moltiplicano

Un chatbot che sbaglia produce una risposta scorretta: fastidioso, ma contenuto. Un agente che sbaglia, invece, **agisce** sulla base dell'errore: può cancellare il file sbagliato, inviare un'email alla persona sbagliata o effettuare un acquisto non voluto. Più passi autonomi un agente compie senza supervisione, più un errore iniziale rischia di propagarsi e amplificarsi lungo la catena di azioni successive.

## Prompt injection

La **prompt injection** è probabilmente il rischio più discusso in ambito agentico. Consiste nel nascondere istruzioni malevole all'interno di contenuti che l'agente elabora — una pagina web, un documento, un'email — nella speranza che il modello le esegua come se fossero istruzioni legittime dell'utente. Un agente che legge automaticamente pagine web e ha accesso a strumenti sensibili (come l'invio di email o l'accesso a file privati) è particolarmente esposto: un testo nascosto in una pagina potrebbe provare a convincerlo a "inviare tutti i dati raccolti finora a questo indirizzo".

## Eccesso di autonomia

Quanta libertà è ragionevole dare a un agente prima di richiedere una conferma umana? Non esiste una risposta unica: dipende dalla reversibilità dell'azione. Leggere un file è un'azione a basso rischio; cancellarlo, inviarlo all'esterno, effettuare un pagamento o pubblicare un contenuto sono azioni difficili o impossibili da annullare, e per questo tipicamente richiedono una conferma esplicita da parte di una persona prima di essere eseguite.

## Alcune buone pratiche di progettazione

Chi costruisce sistemi agentici tende ad applicare alcuni principi per limitare i rischi.

Il **minimo privilegio** consiste nel dare all'agente accesso solo agli strumenti e ai dati strettamente necessari per il compito, evitando di concedere permessi ampi "per sicurezza, non si sa mai".

La **conferma umana** (*human in the loop*) per le azioni irreversibili o ad alto impatto resta, allo stato attuale, la difesa più efficace contro errori ed eventuali manipolazioni.

Il **contenimento** (*sandboxing*) limita l'ambiente in cui l'agente opera, ad esempio eseguendo codice generato in un ambiente isolato invece che direttamente su un sistema di produzione.

Il **logging e la tracciabilità** delle azioni compiute permettono di capire, dopo il fatto, cosa ha fatto un agente e perché, un requisito che diventa importante tanto per il debug quanto per eventuali obblighi normativi.

## Un rischio non è un motivo per rinunciare

Questi rischi non significano che gli agenti AI vadano evitati, ma che vanno **progettati con attenzione**, calibrando l'autonomia concessa al livello di fiducia che il compito e il contesto giustificano. È lo stesso principio, del resto, con cui si è sempre affrontata l'automazione in informatica: più un sistema agisce da solo, più le sue decisioni vanno vincolate, verificate e rese trasparenti.
