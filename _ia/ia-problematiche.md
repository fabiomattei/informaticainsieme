---
title: 'Problematiche'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

L'intelligenza artificiale, ed in particolare l'IA generativa, non solleva soltanto questioni tecniche ma anche economiche, legali, ambientali e industriali. Di seguito quattro problematiche aperte che accompagnano lo sviluppo di questi sistemi.

## Diritti di sfruttamento delle opere

I modelli generativi (testo, immagini, musica) vengono addestrati su enormi quantità di opere esistenti: libri, articoli, fotografie, quadri, canzoni. Gran parte di questo materiale è protetto da diritto d'autore e viene utilizzato senza che l'autore originale sia stato consultato o remunerato.

Questo ha portato a numerose cause legali. Alcuni esempi:

* Il **New York Times** ha citato in giudizio OpenAI e Microsoft sostenendo che ChatGPT sia stato addestrato utilizzando milioni di suoi articoli senza autorizzazione.
* **Getty Images** ha fatto causa a Stability AI (creatrice di Stable Diffusion) per l'uso non autorizzato del proprio archivio fotografico, tanto che alcune immagini generate riproducevano ancora il watermark di Getty.
* Numerosi illustratori e musicisti hanno denunciato che le loro opere sono state usate per addestrare modelli capaci di imitarne lo stile in modo indistinguibile dall'originale, senza alcun compenso.

Il nodo centrale è che addestrare un modello richiede una copia (temporanea o permanente) dell'opera, e le leggi sul diritto d'autore sono state scritte molto prima che questo tipo di utilizzo fosse immaginabile. Diversi Paesi stanno cercando di normare eccezioni per il "text and data mining" a fini di ricerca, ma l'uso commerciale resta un terreno controverso.

## A chi appartiene l'opera "creata"?

Una volta che un'IA genera un testo, un'immagine o un brano musicale a partire da un prompt, di chi è il risultato? Le posizioni legali non sono ancora uniformi:

* Negli **Stati Uniti**, l'Ufficio Copyright ha stabilito che un'opera generata interamente da un'IA, senza un contributo creativo umano sufficiente, **non è protetta da copyright** e cade quindi nel pubblico dominio.
* In **Europa** la normativa richiede che un'opera, per essere protetta, sia frutto della "creazione intellettuale dell'autore": una macchina non può essere considerata autore, e resta da chiarire quanto intervento umano (scelta del prompt, modifiche successive, selezione tra più risultati) sia sufficiente per rivendicare la paternità.
* Le aziende che offrono questi strumenti (OpenAI, Midjourney, ecc.) regolano la questione nei propri termini di servizio, assegnando solitamente all'utente i diritti di utilizzo commerciale del contenuto generato, ma questo non risolve la domanda più ampia sulla tutela legale dell'opera.

La questione non è solo teorica: se un'opera generata da IA non è tutelabile, chiunque potrebbe riutilizzarla, copiarla o rivenderla senza che l'autore del prompt possa opporsi.

## Energia

Addestrare un grande modello linguistico richiede l'esecuzione di calcoli su migliaia di GPU per settimane o mesi, in enormi data center che consumano quantità di energia paragonabili a quelle di intere città.

Alcuni dati per farsi un'idea della scala del problema:

* Si stima che l'addestramento di GPT-3 abbia consumato energia sufficiente per alimentare centinaia di abitazioni per un anno.
* Ogni singola interrogazione a un chatbot come ChatGPT consuma, secondo diverse stime, diverse volte l'energia di una normale ricerca su un motore di ricerca tradizionale.
* I data center dedicati all'IA richiedono anche enormi quantità di **acqua** per il raffreddamento dei server, con un impatto significativo sulle risorse idriche locali nelle zone in cui sono costruiti.

Questo pone un problema di sostenibilità: la crescita della domanda di IA generativa sta spingendo le grandi aziende tecnologiche a costruire nuove centrali elettriche (comprese centrali nucleari dedicate) pur di garantire l'energia necessaria ai propri data center.

## Hardware

L'addestramento e l'esecuzione di reti neurali di grandi dimensioni richiedono hardware specializzato, in particolare GPU (Graphics Processing Unit) capaci di eseguire un numero enorme di operazioni matematiche in parallelo.

Questa richiesta ha generato conseguenze rilevanti:

* **NVIDIA**, principale produttore di GPU per l'IA, è diventata una delle aziende con la capitalizzazione di mercato più alta al mondo, superando colossi storici come Apple e Microsoft in determinati periodi.
* La produzione dei chip più avanzati dipende in larga parte da una sola azienda, la taiwanese **TSMC**, rendendo la catena di approvvigionamento globale dell'IA vulnerabile a tensioni geopolitiche nell'area di Taiwan.
* Diversi governi (Stati Uniti, Cina, Unione Europea) hanno introdotto restrizioni all'esportazione di chip avanzati, considerando l'accesso a questo hardware una questione di sicurezza nazionale.
* La domanda di GPU ha superato per lungo tempo l'offerta disponibile, rendendo questi componenti scarsi e costosi anche per usi non legati all'IA, come il gaming.

L'accesso all'hardware più avanzato è quindi diventato un fattore competitivo determinante: solo poche aziende e pochi Paesi al mondo hanno le risorse economiche e infrastrutturali per addestrare i modelli più grandi.

## Domande

* È giusto che un'opera generata da un'IA, addestrata su milioni di opere altrui senza autorizzazione, non debba nulla agli autori originali?
* Chi dovrebbe possedere i diritti su un contenuto generato da un'IA: chi ha scritto il prompt, l'azienda che ha creato il modello, o nessuno dei due?
* Il costo ambientale dell'IA generativa (energia, acqua, hardware) è giustificato dai benefici che porta? Chi dovrebbe pagarne il prezzo?
* Se solo poche aziende al mondo possono permettersi l'hardware necessario per addestrare i modelli più avanzati, che tipo di concentrazione di potere si sta creando?

