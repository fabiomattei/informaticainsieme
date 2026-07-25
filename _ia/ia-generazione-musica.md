---
title: 'Generazione di musica'
date: '2024-10-26T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

La **musica generata dall'intelligenza artificiale** è la creazione di brani musicali tramite algoritmi di apprendimento automatico, addestrati su grandi quantità di musica esistente per apprendere accordi, melodie, ritmi e stili. Una volta addestrati, questi modelli possono generare composizioni originali o imitare uno stile specifico a partire da un semplice prompt testuale.

![Musica](/images/ia/musica1.jpg){:class="aside-image"}

Alcuni strumenti attualmente disponibili:

* [Suno](https://suno.com/)
* [Udio](https://www.udio.com/)
* [Soundraw](https://soundraw.io/)
* [Loudly](https://www.loudly.com/)
* [Boomy](https://boomy.com/)

## Un po' di storia: dai sintetizzatori all'IA

La musica sintetica ha una storia lunga, molto precedente all'IA generativa. Nella seconda metà degli anni '60 si diffondono i **sintetizzatori**, dispositivi elettronici capaci di generare suoni senza bisogno di corde o ance vibranti come negli strumenti tradizionali.

Con l'arrivo del personal computer si diffondono le **DAW** (Digital Audio Workstation), software che permettono di registrare, montare e manipolare l'audio digitale con la stessa libertà con cui un programma di videoscrittura modifica un testo.

![Musica](/images/ia/musica2.jpg){:class="aside-image"}

Un altro passaggio importante è l'**Auto-Tune**, sviluppato nel 1996 dall'ingegnere **Andy Hildebrand** e lanciato sul mercato nel 1997. Hildebrand, che lavorava per Exxon analizzando dati sismici per individuare giacimenti petroliferi, si accorse che le stesse tecniche matematiche potevano essere usate per rilevare e correggere l'intonazione di una voce registrata. Nato per correggere le stonature, l'Auto-Tune è oggi usato anche come effetto sonoro riconoscibile in molti generi musicali.

Nessuno di questi strumenti è di per sé intelligenza artificiale: sintetizzatori, DAW e Auto-Tune restano strumenti che l'uomo controlla passo passo. La differenza con l'IA generativa è che quest'ultima **compone al posto dell'utente**, a partire da un'istruzione generica.

## Come funziona la generazione musicale con l'IA

I modelli di generazione musicale si basano su reti neurali addestrate su grandi archivi di brani. Le architetture più usate sono:

* **RNN e LSTM**: adatte a elaborare sequenze, sono utili perché in un brano musicale ogni nota dipende da quelle precedenti.
* **GAN (reti generative avversarie)**: due reti competono tra loro, una genera musica e l'altra ne valuta la qualità, spingendo la prima a migliorare.
* **Transformer**: la stessa architettura alla base dei modelli linguistici come ChatGPT viene usata, addestrata su audio anziché su testo, per generare musica direttamente in formato audio a partire da una descrizione testuale (è il principio su cui si basano strumenti come Suno e Udio).

## Dai primi esperimenti agli strumenti di oggi

Nel 2016 il progetto **Flow Machines** di Sony ha prodotto "Daddy's Car", uno dei primi brani pop in cui melodia e armonia sono state generate da un'intelligenza artificiale; testo, arrangiamento e produzione sono comunque rimasti opera di un compositore umano, Benoît Carré.

Negli anni successivi sono comparsi sistemi via via più autonomi. **AIVA** (Artificial Intelligence Virtual Artist) è stata riconosciuta ufficialmente come compositrice dalla SACEM, la società francese che gestisce i diritti d'autore, un passaggio simbolico importante per la legittimazione dell'IA nel settore musicale.

Con la diffusione dei modelli Transformer, strumenti come **Suno** e **Udio** permettono oggi a chiunque di generare, a partire da una semplice descrizione testuale, un brano completo di melodia, strumentazione e voce cantata in pochi secondi: un salto rispetto ai sistemi precedenti, che richiedevano competenze tecniche per essere utilizzati.

## Impatti su artisti e industria musicale

La musica generata dall'IA solleva diverse questioni per il settore:

* **Diritti d'autore**: dato che questi modelli sono addestrati su musica esistente, si pone il problema di come remunerare gli artisti le cui opere hanno contribuito all'addestramento (lo stesso nodo discusso in [Problematiche]({{ site.baseurl }}{% link _ia/ia-problematiche.md %}.html)).
* **Nuove opportunità**: alcuni musicisti utilizzano l'IA come strumento di lavoro, ad esempio per generare rapidamente basi strumentali o idee da cui partire, oppure offrono servizi basati su modelli musicali personalizzati.
* **Imitazione dello stile**: un modello può imparare a riprodurre lo stile riconoscibile di un artista specifico, sollevando questioni etiche simili a quelle del *deepfake* applicate alla voce e allo stile compositivo.
* **Creatività ed emozione**: l'IA genera musica sulla base di pattern statistici appresi da brani esistenti, mentre la musica umana nasce anche da esperienze personali e intenzioni comunicative. Per questo l'IA viene oggi vista più come uno strumento di assistenza alla creatività, che come un sostituto del compositore umano.

## Domande

* Se un brano ha la melodia generata da un'IA ma testo, arrangiamento e produzione sono di un artista umano, di chi è "davvero" la canzone?
* È corretto che un modello venga addestrato su migliaia di brani esistenti senza il consenso dei rispettivi autori, anche se poi genera qualcosa di "nuovo"?
* Uno strumento come Suno o Udio, che genera un brano completo in pochi secondi, è più simile a un compositore o a uno strumento musicale evoluto?
