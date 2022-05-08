---
id: 845
title: 'Architettura di una applicazione WEB'
date: '2021-11-18T08:04:34+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=845'
---

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/architetturaweb-1024x456.png)</figure>La struttura di una applicazione web è basata sul concetto di client/server (cliente servitore).

Il client è rappresentato da un software che permette all’utente di utilizzare l’applicazione in oggetto. Si tratta di un browser qualsiasi come Firefox, Chrome, Edge, Safari, etc…

Digitando l’indirizzo internete dell’applicazione web in oggetto il client si connette al server. Il server è da un lato una macchina fisica, tangibile, cui il client si collega, dall’altra un software che si occupa di raccogliere le richieste specifiche del client.

#### Pagine Statiche

Una pagina statica è una pagina che *appare sempre uguale a chiunque la richiede*.

Ad esempio se il client richiede la home page (pagina principale) del sito www.libero.it, questa apparirà sempre uguale per tutti gli utenti. A questa pagina corrisponderà sul server un file specifico di tipo html che viene spedito sempre uguale a tutti i richiedenti.

Ricapitolando se il client richiede una pagina statica, consistente in un solo file html, il server manda al client il file contenente la pagina statica specifica richiesta.

#### Pagine dinamiche

Una pagina dinamica è una pagina che appare *personalizzata per ciacun utente*.

Ad esempio se un utente si collega a www.facebook.com questi vedrà il suo nome scritto in alto e i post dei suoi amici. Se un utente diverso si collega allo stesso sito vedrà il suo nome scritto in alto e post dei suoi amici che sono diversi dai post dell’utente precedente. Si parla in questo caso di pagina dinamica.

Per poter servire una pagina dinamica il programma server non può servire un semplice file html.

Per poter servire una pagina dinamica il server raccoglierà la richiesta dal client identificando l’utente, chiamerà un linguaggio che si occuperà a sua volta di gestire la logica e interrogare il database, contenete i dati relativi all’utente, con i risultati il linguaggio popolerà un template (pagina html con buchi da riempire) per restituire in fine all’utente la pagina completa.

Nelle prossime lezioni ci occuperemo di cominciare a creare pagine dinamiche.