---
title: "Tool use e function calling"
date: '2026-08-14T09:20:00+02:00'
author: Fabio Mattei
layout: page
---

Alla base di ogni agente AI c'è una capacità tecnica precisa: quella di far scegliere e usare al modello linguistico degli strumenti esterni. Questa capacità va sotto il nome di **tool use** (uso di strumenti) o **function calling** (chiamata di funzioni), e capirne il meccanismo aiuta a togliere un po' di "magia" da come funzionano davvero gli agenti.

## Il meccanismo di base

Un modello linguistico, da solo, produce soltanto testo: non può davvero "cliccare" da nessuna parte o eseguire codice. Il function calling funziona così: a chi sviluppa l'applicazione viene chiesto di descrivere al modello quali funzioni ha a disposizione, specificando nome, scopo e parametri di ciascuna (ad esempio una funzione `cerca_meteo(citta)` che restituisce le previsioni per una città). Quando il modello riceve una richiesta dell'utente e riconosce che la risposta richiede quella funzione, non genera direttamente una risposta testuale: genera invece una **struttura dati** (tipicamente JSON) che indica quale funzione vuole chiamare e con quali argomenti.

A questo punto è il codice dell'applicazione, non il modello, a eseguire realmente la funzione richiesta e a restituire il risultato al modello, che lo userà per formulare la risposta finale in linguaggio naturale. Il modello, in altre parole, **decide** quale strumento usare e con quali parametri, ma non lo esegue mai direttamente: l'esecuzione resta sempre sotto il controllo del codice che ospita il modello, un dettaglio importante anche dal punto di vista della sicurezza.

## Un esempio passo per passo

Un utente chiede: "Che tempo fa domani a Milano?" Il modello riconosce di avere a disposizione uno strumento `cerca_meteo` e genera una richiesta di chiamata con l'argomento `citta: "Milano"`. L'applicazione intercetta questa richiesta, esegue realmente la funzione (magari interrogando un servizio meteo esterno) e restituisce il risultato al modello, ad esempio "18°C, nuvoloso". Il modello, infine, trasforma questo dato grezzo in una risposta naturale: "Domani a Milano ci saranno 18°C con cielo nuvoloso."

## Perché serve descrivere bene gli strumenti

La qualità con cui uno strumento viene descritto al modello — nome chiaro, spiegazione precisa di cosa fa, parametri ben documentati — influisce direttamente sull'affidabilità dell'agente. Una descrizione ambigua può portare il modello a scegliere lo strumento sbagliato, a passare parametri scorretti o a non capire quando uno strumento non è affatto necessario. Progettare buoni strumenti per un agente è, per certi versi, simile a progettare una buona API per altri sviluppatori: chiarezza e precisione contano quanto la funzionalità stessa.

## Dal singolo strumento all'agente

Il tool use da solo, con una sola chiamata di funzione per risposta, è già utile in tanti assistenti. Diventa **informatica agentica** vera e propria quando il ciclo si ripete più volte di seguito: il modello chiama uno strumento, osserva il risultato, decide se serve chiamarne un altro, e così via, fino a raggiungere l'obiettivo assegnato — il meccanismo descritto nella pagina [Cos'è un agente AI]({{ site.baseurl }}{% link _agentica/cosa-e-un-agente-ai.md %}.html).
