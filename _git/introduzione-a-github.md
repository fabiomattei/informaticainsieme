---
title: 'Introduzione a GitHub'
date: '2026-08-17T11:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Anatomia della pagina di un repository su GitHub: schede, branch, file e README](/images/git/introduzione-a-github/introduzione-a-github.svg){:class="aside-image"}

**GitHub** è un servizio web che ospita repository Git online e aggiunge, attorno al semplice controllo di versione, un intero ambiente di collaborazione: revisione del codice, gestione dei problemi da risolvere, automazioni e molto altro.

È il servizio di questo tipo più diffuso al mondo, usato sia da singoli sviluppatori per progetti personali sia da aziende e grandi progetti open source (Linux, VS Code, React e migliaia di altri hanno il proprio codice su GitHub).

## Creare un account

Per usare GitHub è necessario creare un account gratuito su [github.com](https://github.com), fornendo un nome utente, un'email e una password. Il piano gratuito è più che sufficiente per la maggior parte degli usi personali e didattici: permette di creare repository pubblici e privati senza limiti di numero.

## Creare un nuovo repository

Dalla propria pagina GitHub, il pulsante **"New"** (o l'icona `+` in alto a destra → "New repository") apre la schermata di creazione di un nuovo repository, dove si può scegliere:

* il **nome** del repository;
* una breve **descrizione**;
* se renderlo **pubblico** (visibile a chiunque) o **privato** (visibile solo a chi viene esplicitamente invitato);
* se inizializzarlo con un file **README**, un file **.gitignore** già pronto per il linguaggio usato, e una **licenza** open source.

Se il repository viene creato vuoto (senza README), GitHub mostra subito dopo la creazione le istruzioni per collegarlo a un repository locale già esistente, con i comandi `git remote add` e `git push` visti nella pagina precedente.

## La pagina principale di un repository

La pagina di un repository su GitHub è organizzata in alcune zone principali:

* una barra in alto con le schede **Code**, **Issues**, **Pull requests**, **Actions**, **Settings** e altre, a seconda dei permessi che si hanno sul repository;
* l'elenco dei **file e delle cartelle** del progetto, così come si trovano nel branch attualmente selezionato;
* un menu a tendina per scegliere il **branch** da visualizzare;
* il contenuto del file **README.md**, mostrato automaticamente sotto l'elenco dei file: è la prima cosa che chiunque visiti il repository vede, ed è il posto giusto per spiegare cos'è il progetto, come installarlo e come usarlo.

## Autenticazione da riga di comando

Da qualche anno GitHub non accetta più, per motivi di sicurezza, l'uso diretto della password quando si esegue `git push` da riga di comando. Le due modalità di autenticazione più comuni sono:

* un **Personal Access Token** (PAT), una chiave generata dalle impostazioni dell'account (Settings → Developer settings → Personal access tokens), da usare al posto della password quando richiesta;
* una **chiave SSH**, generata sul proprio computer e collegata al proprio account GitHub (Settings → SSH and GPG keys): una volta configurata, permette di autenticarsi automaticamente senza dover inserire credenziali a ogni operazione.

![Personal Access Token e chiave SSH, le due modalità di autenticazione da riga di comando](/images/git/introduzione-a-github/autenticazione.svg){:class="half-image"}

In alternativa, l'applicazione ufficiale **GitHub Desktop** offre un'interfaccia grafica che gestisce l'autenticazione automaticamente, utile per chi preferisce non usare la riga di comando.
