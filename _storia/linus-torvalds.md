---
title: "Linus Torvalds"
date: '2026-07-27'
author: Fabio Mattei
layout: page
---

## "Solo un hobby, non sarà grande come GNU"

Il 25 agosto 1991, uno studente finlandese di ventuno anni pubblicò su un gruppo di discussione Usenet un messaggio che si apriva con un tono quasi scusandosi: "Sto scrivendo un sistema operativo (gratuito), solo per hobby, non sarà grande e professionale come GNU." Quel sistema operativo si chiamava **Linux**. Oggi fa girare la maggioranza dei server del mondo, tutti gli smartphone Android, quasi tutti i supercomputer del pianeta, e un numero incalcolabile di dispositivi incorporati — dai router ai satelliti. Lo studente si chiamava **Linus Torvalds**, e la sua modestia iniziale si sarebbe rivelata, con il tempo, clamorosamente infondata.

## Il pezzo mancante di GNU

![Richard Stallman, fondatore del progetto GNU e della Free Software Foundation (foto di Anders Brenna, CC BY 3.0)](/images/storia/linus-torvalds/richard-stallman.jpg){:class="aside-image" style="max-width: 38%;"}

Per capire cosa fece Torvalds, bisogna prima capire cosa mancava. Nel 1983 **Richard Stallman**, programmatore al MIT, aveva lanciato il **Progetto GNU** — un acronimo ricorsivo per "GNU's Not Unix" — con l'obiettivo di costruire un sistema operativo completo, simile a Unix, ma interamente composto da software libero: codice che chiunque potesse eseguire, studiare, modificare e ridistribuire liberamente. Stallman fondò la **Free Software Foundation** nel 1985 e scrisse la **GNU General Public License** (GPL), una licenza che capovolgeva la logica del copyright tradizionale: chiunque distribuisse un programma coperto da GPL, comprese le sue modifiche, era obbligato a renderne disponibile il codice sorgente con la stessa libertà — un meccanismo che Stallman chiamò *copyleft*.

Nel giro di un decennio, il progetto GNU produsse quasi tutti i pezzi di un sistema operativo completo: il compilatore **GCC**, l'editor **Emacs**, la shell **Bash**, le utility di base. Mancava però il pezzo più critico: il **kernel**, il nucleo che gestisce direttamente l'hardware, la memoria e i processi. Il kernel ufficiale del progetto GNU, chiamato **Hurd**, aveva un'architettura ambiziosa — un design a microkernel — che si rivelò estremamente complessa da realizzare, e il suo sviluppo rallentò fino quasi a fermarsi.

## Un finlandese, un 386 e un sistema didattico troppo limitato

Linus Torvalds, studente di informatica all'Università di Helsinki, si era comprato un personal computer con processore Intel 386 e voleva un sistema operativo Unix-like che sfruttasse davvero le capacità di quell'hardware. L'unica opzione accessibile per uno studente era **Minix**, un piccolo sistema Unix-like scritto a scopo puramente didattico dal professore olandese **Andrew Tanenbaum**, pensato per essere insegnato nei corsi di sistemi operativi, non per un uso reale intensivo. Minix era deliberatamente limitato — Tanenbaum lo teneva volutamente semplice per ragioni pedagogiche — e non permetteva le estensioni che Torvalds avrebbe voluto.

Così Torvalds iniziò, per curiosità personale più che per ambizione dichiarata, a scrivere un proprio kernel da zero, studiando da vicino l'architettura del suo 386. Nell'annuncio del 1991 chiedeva semplicemente ai lettori del gruppo Usenet `comp.os.minix` cosa avrebbero voluto vedere in un sistema operativo gratuito, senza aspettarsi che il progetto sarebbe cresciuto oltre un piccolo esperimento personale.

## L'unione che mancava: GNU/Linux

![Linus Torvalds a LinuxCon, San Paolo, 2011 (foto di Beraldo Leal, CC BY 2.0)](/images/storia/linus-torvalds/linus-torvalds-2011.jpg){:style="max-width: 42%;"}

Torvalds rilasciò il suo kernel sotto licenza **GPL** — la stessa licenza del progetto GNU. Fu una scelta cruciale: significava che il kernel di Linux poteva essere combinato liberamente con tutti gli strumenti GNU già esistenti — il compilatore, la shell, le utility — per formare, finalmente, il sistema operativo completo e interamente libero che Stallman aveva immaginato quasi un decennio prima. Il kernel di Torvalds era il pezzo mancante; GNU forniva tutto il resto. Il sistema risultante viene indicato correttamente come **GNU/Linux**, anche se nell'uso comune il nome si è ristretto a "Linux".

La crescita fu resa possibile dal modello di sviluppo che Torvalds adottò: aperto, distribuito, con centinaia e poi migliaia di programmatori in tutto il mondo che contribuivano codice tramite Internet, coordinati da Torvalds stesso come *maintainer* finale. Il saggista e programmatore **Eric S. Raymond** descrisse questo modello nel celebre saggio del 1997 *The Cathedral and the Bazaar*, contrapponendo lo sviluppo centralizzato e gerarchico del software tradizionale — la cattedrale — a quello caotico, distribuito, sorprendentemente efficace del bazaar open source.

## Free software o open source? Una distinzione che conta

Nel 1998 un gruppo di sviluppatori, tra cui lo stesso Raymond, coniò il termine **open source** come alternativa più pragmatica e meno ideologicamente connotata a "software libero". La distinzione non è puramente terminologica: Stallman ha sempre insistito che la questione centrale sia la **libertà** dell'utente — poter vedere, modificare, ridistribuire il codice, un principio quasi etico — mentre "open source" enfatizza i vantaggi pratici del codice aperto: maggiore affidabilità, sicurezza verificabile, sviluppo più rapido attraverso la collaborazione. Torvalds si è sempre collocato più vicino a questo secondo campo, pragmatico e tecnico piuttosto che ideologico, pur riconoscendo il debito storico verso il progetto di Stallman.

## Git: un secondo contributo enorme

Nel 2005, un conflitto con l'azienda che forniva il sistema proprietario di controllo versione usato dal kernel Linux (BitKeeper) spinse Torvalds a scrivere, in poche settimane, un nuovo sistema di controllo versione distribuito: **Git**. Doveva servire solo a gestire lo sviluppo del kernel Linux. È diventato, negli anni successivi, lo standard universale con cui viene gestito il codice sorgente di praticamente ogni progetto software al mondo — una seconda invenzione, nata da un problema pratico specifico, che ha finito per avere un impatto quasi pari a quella originale.

## Il presente: onnipresenza invisibile

Oggi Linux fa girare oltre il 96% dei server web più trafficati al mondo, la quasi totalità dei cinquecento supercomputer più potenti del pianeta, e — tramite Android, che ne usa il kernel — la maggioranza degli smartphone in circolazione. È un'infrastruttura tanto pervasiva quanto invisibile: milioni di persone la usano ogni giorno senza sapere che il loro telefono, il loro router di casa o il servizio cloud a cui si affidano gira su un kernel iniziato come un hobby da uno studente finlandese che pensava di non stare costruendo niente di importante.

Torvalds ha mantenuto per oltre trent'anni la guida tecnica dello sviluppo del kernel, gestita oggi attraverso la **Linux Foundation**, uno stile di leadership diretto e a volte controverso per la sua franchezza, che nel 2018 lo portò a una pausa temporanea e a un'autocritica pubblica sui toni usati nelle discussioni tecniche. Ma il progetto che nel 1991 sembrava, nelle sue stesse parole, "solo un hobby", resta il caso di scuola più citato al mondo su cosa può nascere quando migliaia di sconosciuti collaborano liberamente su un problema comune.

## Crediti immagini

- Richard Stallman: foto di Anders Brenna, [licenza CC BY 3.0](https://creativecommons.org/licenses/by/3.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Richard_Stallman_by_Anders_Brenna_01.jpg)
- Linus Torvalds a LinuxCon (2011): foto di Beraldo Leal, [licenza CC BY 2.0](https://creativecommons.org/licenses/by/2.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Linus_Torvalds_-_Linuxcon2011.jpg)
