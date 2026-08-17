---
title: "Il protocollo MCP (Model Context Protocol)"
date: '2026-08-14T09:10:00+02:00'
author: Fabio Mattei
layout: page
---

Perché un agente AI possa agire nel mondo reale, deve potersi collegare a strumenti esterni: un motore di ricerca, un database, un sistema di file, un servizio aziendale. Per molto tempo, ogni applicazione ha dovuto costruire questi collegamenti in modo su misura, ripetendo lo stesso lavoro di integrazione per ogni combinazione di modello e strumento. Il **Model Context Protocol (MCP)** nasce per risolvere proprio questo problema, proponendo uno standard comune.

## Il problema che risolve

Immaginiamo di avere tre modelli linguistici diversi e cinque strumenti diversi (un database, un servizio meteo, un gestionale, eccetera). Senza uno standard, servirebbero fino a quindici integrazioni su misura, una per ogni combinazione. Con un protocollo condiviso come MCP, basta che ogni strumento parli MCP una volta sola, e ogni modello capace di usare MCP potrà collegarcisi senza bisogno di codice dedicato: è lo stesso principio che sta dietro a standard come USB o HTTP, applicato al collegamento tra modelli AI e dati/strumenti esterni.

## Come funziona, in breve

MCP definisce un modo comune per esporre tre tipi di risorse verso un modello:

I **tool** sono funzioni che il modello può invocare per compiere un'azione, ad esempio "cerca un cliente nel database" o "invia una email".

Le **resource** sono dati che il modello può leggere per avere contesto, come il contenuto di un file o il risultato di una query, senza necessariamente dover eseguire un'azione.

I **prompt** sono modelli di istruzioni predefiniti che uno strumento può suggerire al modello per completare un compito specifico in modo coerente.

Un **server MCP** espone queste risorse; un **client MCP** (integrato nell'applicazione che ospita il modello) le scopre e le usa quando serve. Il modello non deve sapere in anticipo quali strumenti esistono: li scopre dinamicamente parlando con i server MCP disponibili.

## Perché è rilevante per l'informatica agentica

Un agente diventa utile nella misura in cui può interagire con sistemi reali, non solo generare testo. MCP fornisce il "linguaggio comune" che permette a un agente di collegarsi a un numero crescente di strumenti — file locali, servizi cloud, applicazioni aziendali — senza che ogni combinazione richieda un'integrazione scritta a mano. Per questo motivo, dalla sua introduzione il protocollo è stato adottato rapidamente da numerosi produttori di modelli e strumenti, diventando uno dei mattoni fondamentali dell'ecosistema agentico attuale.

## Un parallelo utile

Si può pensare a MCP come a una presa elettrica standard: non importa quale elettrodomestico si collega (l'agente) o quale impianto elettrico c'è dietro il muro (lo strumento), finché entrambi rispettano la stessa forma della spina, la connessione funziona. Prima di standard come questo, ogni collegamento richiedeva un adattatore su misura.
