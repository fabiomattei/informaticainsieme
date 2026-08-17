---
title: 'Collaborare su GitHub: fork, pull request e issue'
date: '2026-08-17T11:30:00+02:00'
author: Fabio Mattei
layout: page
---

![Flusso di fork, clone locale e pull request verso il repository originale](/images/git/pull-request-e-collaborazione/pull-request-e-collaborazione.svg){:class="aside-image"}

Il vero punto di forza di GitHub è il modo in cui semplifica la collaborazione tra più persone, anche quando non hanno accesso diretto in scrittura allo stesso repository. Questa pagina spiega gli strumenti principali.

## Le pull request

Una **pull request** (spesso abbreviata "PR") è una richiesta di unire le modifiche presenti su un branch al branch principale (o a un altro branch di destinazione), passando però prima per una fase di **revisione**.

Il flusso tipico è questo:

1. si crea un branch dedicato alla modifica da fare (`git switch -c fix-bug-login`);
2. si lavora sul branch e si inviano i commit al remote (`git push -u origin fix-bug-login`);
3. su GitHub si apre una **pull request**, scegliendo il branch di origine e quello di destinazione, e descrivendo cosa fa la modifica e perché;
4. altre persone del team possono leggere le modifiche riga per riga, lasciare **commenti** su punti specifici del codice, chiedere correzioni o approvare;
5. una volta approvata (ed eventualmente dopo che dei controlli automatici sono passati con successo), la pull request viene **unita** (*merge*) al branch di destinazione, direttamente dal pulsante sulla pagina web.

Le pull request rendono possibile la **revisione del codice** (*code review*): nessuna modifica arriva sul branch principale senza che almeno un'altra persona l'abbia controllata, il che aiuta a individuare errori, migliorare la qualità del codice e diffondere la conoscenza del progetto tra i membri del team.

## Il fork

Un **fork** è una copia completa di un repository, creata sul proprio account GitHub, di cui si diventa proprietari a tutti gli effetti (si può modificare liberamente, senza bisogno di permessi sul repository originale).

Il fork è lo strumento tipico per contribuire a progetti **open source** di cui non si è collaboratori diretti: si crea un fork del progetto, lo si clona in locale, si apportano le modifiche desiderate, si invia (`push`) il proprio branch al fork sul proprio account, e infine si apre una pull request **dal fork verso il repository originale**. Chi mantiene il progetto originale può quindi valutare la proposta e, se la ritiene valida, unirla.

{% highlight text %}
repository originale  <--- pull request ---  tuo fork (sul tuo account)
                                                    ^
                                                    |  git push
                                                    |
                                              repository locale (sul tuo computer)
{% endhighlight %}

## Le issue

Una **issue** è una segnalazione: un bug da correggere, una funzionalità da aggiungere, una domanda da porre a chi mantiene il progetto. Ogni issue ha un titolo, una descrizione, e può essere discussa nei commenti, assegnata a una persona, etichettata (*label*) per categoria (`bug`, `enhancement`, `documentation`...) e collegata a una pull request che la risolve.

Le issue sono lo strumento principale con cui i progetti organizzano il lavoro da fare e tengono traccia dei problemi segnalati dagli utenti.

## Collegare issue e pull request

Scrivendo nella descrizione di una pull request una frase come:

{% highlight text %}
Chiude #42
{% endhighlight %}

(o, in inglese, `Closes #42`, `Fixes #42`), GitHub collega automaticamente la pull request alla issue numero 42 e, quando la pull request viene unita, chiude automaticamente la issue corrispondente.

## GitHub Flow: un flusso di lavoro semplice

Molti progetti seguono una convenzione informale nota come **GitHub Flow**:

1. il branch `main` rappresenta sempre il codice funzionante e pronto per essere distribuito;
2. ogni nuova modifica (funzionalità, correzione...) viene sviluppata su un branch dedicato, creato a partire da `main`;
3. il lavoro viene condiviso presto, aprendo una pull request anche quando non è ancora completo, per favorire il confronto;
4. dopo revisione e approvazione, la pull request viene unita a `main`.

![Più branch di funzionalità creati da main e uniti nuovamente dopo la revisione](/images/git/pull-request-e-collaborazione/github-flow.svg){:class="half-image"}

È un flusso semplice ed efficace, adatto sia a piccoli progetti personali sia a team più grandi, ed è il punto di partenza consigliato per chi comincia a collaborare su GitHub.
