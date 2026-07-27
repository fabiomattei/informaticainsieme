---
title: "John McCarthy e Marvin Minsky"
date: '2026-07-27'
author: Fabio Mattei
layout: page
---

## L'estate in cui nacque un nome

Nell'estate del 1956, un gruppo di una decina di ricercatori si riunì per circa due mesi al Dartmouth College, nel New Hampshire, per un workshop che i suoi stessi organizzatori descrivevano, nella proposta di finanziamento scritta l'anno precedente, con una frase di un ottimismo quasi disarmante: ogni aspetto dell'apprendimento e dell'intelligenza può, in linea di principio, essere descritto con tale precisione da permettere a una macchina di simularlo. Quel workshop non produsse alcuna macchina pensante. Ma diede un nome a un campo di ricerca che ancora non esisteva formalmente: **intelligenza artificiale**. I due nomi più associati a quella nascita sono **John McCarthy**, che coniò il termine, e **Marvin Minsky**, che ne fu tra i primi e più influenti artefici — e, decenni dopo, tra i primi a incrinarne l'ottimismo iniziale.

## La proposta di Dartmouth

![John McCarthy, inventore del LISP e del termine "intelligenza artificiale" (foto da Flickr, utente null0, CC BY-SA 2.0)](/images/storia/mccarthy-minsky-dartmouth/john-mccarthy-2006.jpg){:class="aside-image" style="max-width: 36%;"}

**John McCarthy**, giovane matematico allora professore a Dartmouth, insieme a **Marvin Minsky**, **Claude Shannon** — già celebre come padre della teoria dell'informazione — e **Nathaniel Rochester** di IBM, scrisse nel 1955 la proposta che avrebbe portato al finanziamento del workshop dell'anno successivo. Fu McCarthy a insistere sul nome "intelligenza artificiale", scegliendolo deliberatamente al posto di termini già in uso come "cibernetica", associato al lavoro di Norbert Wiener su sistemi di controllo e retroazione: voleva marcare una distinzione netta verso un approccio nuovo, basato sulla manipolazione simbolica del pensiero piuttosto che su modelli ispirati alla regolazione biologica.

Al workshop parteciparono, insieme ai quattro organizzatori, altri ricercatori destinati a diventare figure centrali del campo: **Allen Newell** e **Herbert Simon**, che portarono a Dartmouth il **Logic Theorist**, un programma già funzionante capace di dimostrare autonomamente teoremi di logica matematica — probabilmente il primo vero programma di intelligenza artificiale della storia, sviluppato prima ancora che il termine esistesse. L'incontro non produsse un'unica teoria condivisa, né tantomeno i risultati pratici che l'entusiasmo della proposta lasciava intendere. Ma battezzò il campo, ne fissò l'agenda per i decenni successivi, e mise nella stessa stanza le persone che lo avrebbero guidato.

## LISP: il linguaggio del pensiero simbolico

Nel 1958 McCarthy, nel frattempo passato al MIT, sviluppò **LISP** (List Processor), un linguaggio di programmazione pensato specificamente per la manipolazione di simboli — liste di dati e di istruzioni trattate secondo le stesse regole — invece che per il calcolo numerico puro a cui erano dedicati linguaggi come FORTRAN. LISP introdusse concetti che sarebbero diventati fondamentali per l'informatica in generale, ben oltre l'IA: la **ricorsione** come strumento di programmazione centrale, la **garbage collection** per la gestione automatica della memoria, le espressioni condizionali. È, dopo FORTRAN, il secondo linguaggio di programmazione ad alto livello ancora in uso attivo a distanza di oltre sessant'anni, e per decenni è stato il linguaggio di riferimento della ricerca sull'intelligenza artificiale.

McCarthy contribuì anche alla nascita del concetto di **time-sharing** — la possibilità che più utenti condividessero contemporaneamente le risorse di un unico calcolatore centrale — e negli anni successivi si dedicò a formalizzare la logica del ragionamento di senso comune, restando per tutta la carriera un sostenitore dell'approccio simbolico e logico all'IA, scettico verso i modelli che imitavano il funzionamento dei neuroni biologici.

## Minsky, le reti neurali, e l'inverno che lui stesso contribuì a creare

![Marvin Minsky, cofondatore del MIT AI Lab (foto da Flickr, utente Steamtalks, CC BY-SA 2.0)](/images/storia/mccarthy-minsky-dartmouth/marvin-minsky.jpg){:class="aside-image" style="max-width: 36%;"}

La storia di Minsky con le reti neurali è più complessa e, in un certo senso, ironica. Da studente di dottorato ad Harvard, nel 1951, aveva costruito con un collega lo **SNARC**, una delle prime macchine capaci di simulare una rudimentale rete di neuroni artificiali — quaranta "neuroni" realizzati con valvole termoioniche e componenti elettromeccanici — capace di apprendere a risolvere semplici labirinti per rinforzo. Minsky conosceva quindi l'approccio connessionista dall'interno, non da estraneo.

Fu proprio per questo che il suo giudizio successivo pesò così tanto. Nel 1969, insieme a **Seymour Papert**, pubblicò *Perceptrons*, un libro che dimostrava matematicamente i limiti intrinseci dei **percettroni** a singolo strato — i modelli di rete neurale più diffusi all'epoca, incapaci in linea di principio di risolvere anche problemi logici relativamente semplici come lo XOR (l'operazione logica "o esclusivo"). L'analisi era tecnicamente corretta e riguardava specificamente i percettroni a singolo strato, non le reti neurali in generale — un'estensione a più strati avrebbe potuto in teoria superare quei limiti. Ma l'effetto pratico sulla comunità scientifica e sui finanziatori fu devastante: la ricerca sulle reti neurali perse quasi ogni finanziamento per oltre un decennio, un periodo che gli storici dell'IA chiamano ancora oggi il primo **inverno dell'intelligenza artificiale**. Le reti neurali multi-strato sarebbero tornate al centro della ricerca solo negli anni Ottanta, con la riscoperta dell'algoritmo di **retropropagazione dell'errore** (*backpropagation*) — la stessa tecnica, in forme evolute, alla base del deep learning di oggi.

Minsky, dal canto suo, proseguì una carriera dedicata a teorie più ampie sulla natura dell'intelligenza. Nel suo libro *The Society of Mind* (1986) propose che la mente non fosse un singolo algoritmo unificato, ma una società di piccoli processi semplici e specializzati — "agenti" — che interagendo tra loro producevano il comportamento intelligente complessivo. Fu anche tra i cofondatori, insieme a McCarthy, del **MIT Artificial Intelligence Laboratory** nel 1959, uno dei primi e più influenti centri di ricerca sull'IA al mondo.

## Un campo nato con un nome e un'agenda, non con una risposta

Il paradosso della conferenza di Dartmouth è che il suo contributo più duraturo non fu una scoperta tecnica, ma un atto quasi linguistico: dare un nome — "intelligenza artificiale" — a un insieme di domande che i partecipanti stessi non sapevano ancora come risolvere, e che in alcuni casi, come dimostra la parabola delle reti neurali tra Minsky del 1951 e Minsky del 1969, si sono rivelate più difficili e più cicliche di quanto l'ottimismo iniziale lasciasse intendere. Sia McCarthy (Turing Award nel 1971) sia Minsky (Turing Award nel 1969) sono stati insigniti del massimo riconoscimento dell'informatica mondiale per il loro contributo fondativo — un contributo che riguarda tanto le risposte che diedero quanto le domande, ancora aperte oggi, che seppero formulare per primi con tale chiarezza da dare un nome a un intero campo scientifico.

## Crediti immagini

- John McCarthy (Stanford, 2006): foto di null0 (Flickr), [licenza CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:John_McCarthy_Stanford.jpg)
- Marvin Minsky: foto di Steamtalks (Flickr), [licenza CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Marvin_Minsky.jpg)
