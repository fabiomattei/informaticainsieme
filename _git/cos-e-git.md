---
title: 'Cos''è Git e il controllo di versione'
date: '2026-08-17T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Differenza tra un sistema di versionamento centralizzato e uno distribuito come Git](/images/git/cos-e-git/cos-e-git.svg){:class="aside-image"}

**Git** è un **sistema di controllo di versione distribuito**: uno strumento che tiene traccia di tutte le modifiche fatte ai file di un progetto nel tempo, permettendo di tornare indietro, confrontare versioni diverse e far collaborare più persone sullo stesso codice senza pestarsi i piedi a vicenda.

È stato creato nel 2005 da **Linus Torvalds** (lo stesso creatore di Linux) per gestire lo sviluppo del kernel Linux, e oggi è lo strumento più usato al mondo per versionare il codice sorgente.

## Perché serve un sistema di controllo di versione

Prima di Git, molti sviluppatori "versionavano" i propri progetti a mano, con cartelle come:

{% highlight text %}
progetto_finale/
progetto_finale_2/
progetto_finale_DEFINITIVO/
progetto_finale_DEFINITIVO_uso_questo/
{% endhighlight %}

![Diverse cartelle con nomi progressivi come tentativo di versionare un progetto a mano](/images/git/cos-e-git/caos-cartelle.svg){:class="half-image"}

Un sistema del genere ha diversi problemi:

* non si capisce **cosa** è cambiato tra una versione e l'altra;
* non si sa **chi** ha fatto una modifica né **perché**;
* è quasi impossibile far lavorare **più persone insieme** sugli stessi file senza sovrascrivere il lavoro altrui;
* tornare a una versione precedente significa cercare a mano la cartella giusta, sperando di averla salvata.

Git risolve tutti questi problemi: registra ogni modifica in una **cronologia** consultabile in ogni momento, con l'autore, la data e una descrizione di cosa è cambiato.

## Sistemi centralizzati e sistemi distribuiti

I sistemi di controllo di versione si dividono in due grandi famiglie.

**Sistemi centralizzati** (es. Subversion, CVS): esiste un unico server che conserva l'intera cronologia del progetto. Ogni sviluppatore scarica solo la versione attuale dei file e, per vedere la cronologia o creare una nuova versione, deve essere connesso al server.

**Sistemi distribuiti** (Git, Mercurial): ogni sviluppatore possiede sul proprio computer una **copia completa** del progetto, cronologia compresa. Si può lavorare, creare nuove versioni e consultare la storia del progetto anche senza connessione a Internet; la sincronizzazione con gli altri avviene solo quando serve, scambiando dati con un repository condiviso (tipicamente ospitato su un servizio come GitHub).

Questo è il motivo per cui Git è così veloce e resiliente: non dipende da un server sempre raggiungibile, e ogni copia del progetto è di fatto un backup completo della sua storia.

## Git e GitHub non sono la stessa cosa

È un errore comune confondere i due:

* **Git** è il programma che gira sul proprio computer e gestisce il controllo di versione.
* **GitHub** è un servizio web (di proprietà di Microsoft) che ospita repository Git online, e aggiunge strumenti di collaborazione come issue, pull request e revisione del codice.

Git funziona perfettamente anche senza GitHub: si può usare Git in locale, oppure sincronizzarsi con altri servizi simili (GitLab, Bitbucket, un server aziendale...). GitHub è semplicemente il più diffuso.

Nelle prossime pagine vedremo prima i **concetti fondamentali** di Git, poi i **comandi da riga di comando** per usarlo nella pratica, e infine come muoversi nell'**interfaccia web di GitHub**.
