---
title: "Agenti vs chatbot e assistenti"
date: '2026-08-14T09:05:00+02:00'
author: Fabio Mattei
layout: page
---

I termini "chatbot", "assistente AI" e "agente AI" vengono spesso usati come sinonimi, ma descrivono livelli diversi di autonomia. Capire questa differenza aiuta a scegliere lo strumento giusto per il problema giusto, invece di aspettarsi da un chatbot capacità che semplicemente non ha.

## Chatbot: rispondere a una domanda

Un **chatbot** classico riceve un messaggio e produce una risposta testuale. Il suo mondo comincia e finisce nella conversazione: non può consultare un sito web aggiornato, non può eseguire codice, non può modificare un file. È estremamente utile per rispondere a domande, riassumere testi o generare contenuti, ma resta passivo rispetto al mondo esterno.

## Assistente AI: rispondere con l'aiuto di strumenti

Un **assistente AI** aggiunge un primo livello di autonomia: può usare uno o pochi strumenti predefiniti, in genere in modo abbastanza controllato. Un assistente che cerca sul web prima di rispondere, o che consulta il calendario dell'utente per rispondere "sei libero venerdì?", rientra in questa categoria. La sequenza di azioni possibili è però limitata e spesso decisa in anticipo da chi ha costruito il sistema.

## Agente AI: pianificare ed eseguire in autonomia

Un **agente AI** compie un passo ulteriore: gli si affida un obiettivo, non una singola domanda, e decide da solo quali strumenti usare, in quale ordine, per quanti passi, correggendo la strategia se qualcosa non funziona come previsto. Un agente a cui si chiede "prepara un report sulle vendite del mese scorso" potrebbe autonomamente decidere di interrogare un database, elaborare i dati, generare un grafico e scrivere un documento finale, senza che nessuno gli dica esplicitamente questi passaggi uno per uno.

## Una tabella riassuntiva

| Caratteristica | Chatbot | Assistente | Agente |
|---|---|---|---|
| Input tipico | Una domanda | Una domanda + contesto | Un obiettivo |
| Uso di strumenti | Nessuno | Limitato e predefinito | Ampio e scelto autonomamente |
| Numero di passi | Uno | Pochi, prevedibili | Molti, decisi dinamicamente |
| Capacità di correggersi | Nessuna | Minima | Centrale nel funzionamento |
| Esempio | Rispondere a "cos'è la fotosintesi?" | "Cerca sul web le ultime notizie su X" | "Organizza un viaggio a Roma nel weekend" |

## Un confine sfumato

Questa non è una classificazione rigida: nella pratica esiste un continuum, e uno stesso prodotto può comportarsi in modo più simile a un assistente o a un agente a seconda di come viene configurato. Il punto centrale da ricordare è che **più un sistema può pianificare, agire e correggersi da solo, più ci si sposta verso l'informatica agentica** — con tutti i vantaggi in termini di produttività, ma anche i rischi che vedremo nella pagina dedicata a [rischi e sicurezza degli agenti]({{ site.baseurl }}{% link _agentica/rischi-sicurezza-agenti.md %}.html).
