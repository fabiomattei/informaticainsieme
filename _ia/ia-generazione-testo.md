---
title: 'Generazione di testo'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
modified_date: '2026-07-25T00:00:00+02:00'
---

## Introduzione

I sistemi **LLM (Large Language Model)** sono modelli di intelligenza artificiale addestrati su enormi quantità di
testo per comprendere e generare linguaggio naturale in modo simile a un essere umano. Sono in grado di rispondere
a domande, scrivere testi, tradurre, riassumere e sostenere conversazioni su una vasta gamma di argomenti,
adattandosi al tono e al contenuto della richiesta.

## Come funzionano

Alla base degli LLM c'è l'architettura **Transformer**. Il testo viene scomposto in unità dette **token** (parole o
parti di parola) e, durante l'addestramento, il modello impara a prevedere il token successivo più probabile data
una sequenza di testo. Ripetendo questo processo su miliardi di frasi, il modello acquisisce sia una
rappresentazione statistica del linguaggio sia nozioni di fatto contenute nei testi di addestramento.

Dopo questa fase iniziale (**pre-training**), i modelli vengono ulteriormente affinati con tecniche come il
*fine-tuning* supervisionato e l'**apprendimento per rinforzo dal feedback umano (RLHF)**, che li rendono più utili,
coerenti e allineati alle richieste nelle conversazioni.

## Modelli e applicazioni

È utile distinguere tra due livelli:

* **Modelli di base (LLM)**: i motori linguistici veri e propri, sviluppati dalle grandi aziende di IA. Esempi:
  GPT (OpenAI), Claude (Anthropic), Gemini (Google), Llama (Meta, open source), Mistral (Mistral AI, open source).
* **Applicazioni costruite sopra gli LLM**: strumenti pensati per compiti specifici — ad esempio la scrittura di
  articoli o testi pubblicitari — che internamente si appoggiano a uno di questi modelli tramite API. Esempi:
  Jasper AI, Writesonic, Rytr, Copy.ai.

## Problematiche

* Sono **energivori**: l'addestramento e l'uso su larga scala richiedono grandi quantità di elettricità e risorse
  di calcolo.
* Richiedono **enormi quantità di dati** per l'addestramento, il che solleva questioni di diritto d'autore e
  qualità delle fonti utilizzate.
* L'uso di testi altrui per l'addestramento è oggetto di **cause legali e dibattiti** ancora aperti su copyright e
  compenso agli autori originali.
* Non offrono **garanzie di veridicità** sui contenuti generati.
* Sollevano questioni di **privacy**, dato che i dati di addestramento provengono spesso dal web senza un consenso
  esplicito degli autori.

## Limiti

* **Ragionamento logico e matematico**: i primi LLM davano spesso risposte scorrette su problemi di logica o
  calcolo, perché si limitano a prevedere il testo più probabile piuttosto che "ragionare" in senso umano. I
  modelli più recenti, detti **modelli di ragionamento** (es. la serie o di OpenAI, Claude con extended thinking,
  Gemini con reasoning), affrontano questi problemi generando esplicitamente passaggi intermedi prima della
  risposta finale, migliorando molto l'affidabilità su questo tipo di compiti — pur senza essere infallibili.
* **Allucinazioni**: il modello può restituire un'affermazione che sembra corretta e sicura di sé, ma che contiene
  informazioni inventate. Il limite persiste anche nei modelli più recenti, sebbene in forma ridotta.

![Limite chat GPT](/images/ia/limitichatgpt.png){:class="aside-image"}

## Domande

Quali problemi pensi che questo genere di algoritmi possa risolvere per la società?
Quali per te?
Quali per le persone che ti circondano?
