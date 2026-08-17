---
title: 'Esplorare un repository su GitHub'
date: '2026-08-17T11:15:00+02:00'
author: Fabio Mattei
layout: page
---

![Il grafico dei branch su GitHub e il dettaglio di un commit con le sue modifiche](/images/git/esplorare-un-repository/esplorare-un-repository.svg){:class="aside-image"}

Oltre a ospitare il codice, GitHub permette di consultare comodamente dal browser tutta la cronologia e la struttura di un repository, senza dover usare la riga di comando.

## La cronologia dei commit

Cliccando sul numero di commit mostrato sopra l'elenco dei file (o andando su Code → Commits) si apre l'elenco completo della cronologia, con autore, data e messaggio di ogni commit, dal più recente al più vecchio.

Cliccando su un singolo commit se ne vedono i dettagli: quali file sono stati modificati, e per ciascuno le righe rimosse (evidenziate in rosso) e quelle aggiunte (evidenziate in verde) — la stessa informazione che si otterrebbe da riga di comando con `git show`.

## Modificare un file direttamente dal browser

Aprendo un file di testo dall'elenco dei file, l'icona a forma di matita in alto a destra permette di modificarlo direttamente nel browser, senza clonare il repository in locale. Al termine, GitHub chiede di scrivere un messaggio di commit e di scegliere se applicare la modifica direttamente sul branch corrente oppure crearne uno nuovo e aprire una pull request (vedi la prossima pagina).

È una funzionalità comoda per correzioni rapide (una parola sbagliata nel README, un piccolo refuso), ma non sostituisce un vero ambiente di sviluppo per modifiche più corpose.

## I branch su GitHub

Il menu a tendina in alto a sinistra nella pagina del codice mostra il branch attualmente selezionato e permette di passare a un altro branch, oppure di crearne uno nuovo direttamente dal browser, digitando un nome che non esiste ancora.

La scheda **"Insights" → "Network"** (o il grafico dei commit) mostra una rappresentazione visiva di come i branch si sono sviluppati e uniti nel tempo, utile per farsi un'idea d'insieme della storia del progetto.

## Confrontare versioni

GitHub permette di confrontare due branch, due tag o due commit qualsiasi tra loro, per vedere esattamente cosa cambia dall'uno all'altro. Questo si fa dalla pagina **"Compare"**, raggiungibile aggiungendo `/compare` all'indirizzo del repository, oppure automaticamente quando si apre una pull request.

## Rilasci (Releases)

Quando un progetto raggiunge una versione stabile pronta per essere distribuita, è possibile creare una **Release**: uno snapshot del codice a un certo commit, a cui si associa un numero di versione (ad esempio `v1.0.0`), delle note di rilascio che ne descrivono le novità, ed eventualmente dei file scaricabili (ad esempio un programma già compilato).

Le release sono collegate ai **tag** di Git, delle etichette che si possono assegnare a un commit specifico per marcarlo come punto di riferimento importante nella cronologia.

![Un tag v1.0.0 assegnato a un commit specifico, alla base di una release su GitHub](/images/git/esplorare-un-repository/tag-release.svg){:class="half-image"}

## Cercare nel codice

La barra di ricerca in alto nella pagina di GitHub permette di cercare del testo all'interno dei file di un repository (o, con la lente di ricerca su tutto GitHub, in tutti i repository pubblici), utile per trovare velocemente dove è definita una funzione o dove viene usata una certa variabile in progetti di grandi dimensioni.
