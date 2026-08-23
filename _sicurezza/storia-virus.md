---
title: 'La storia dei virus informatici'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

![Tappe della storia dei virus informatici](/images/sicurezza/storia-virus/timeline.svg){:class="aside-image"}

Il primo programma capace di replicarsi da solo non fu scritto per fare danni. Fu scritto per dimostrare che si poteva fare. Da quel momento, la storia dei virus informatici è la storia di un'idea — codice che copia se stesso — che è passata da curiosità accademica a strumento di crimine organizzato e persino di guerra tra stati.

### Creeper e Reaper: la preistoria (1971)

Nel 1971 **Bob Thomas**, un ricercatore della BBN Technologies, scrisse **Creeper**: un programma sperimentale che girava su ARPANET, l'antenata di Internet, e si spostava da un computer all'altro mostrando il messaggio "I'M THE CREEPER: CATCH ME IF YOU CAN". Non rubava dati, non danneggiava nulla. Era un esperimento sulla mobilità del codice.

Poco dopo, **Ray Tomlinson** — lo stesso che inventò l'email — scrisse **Reaper**, un programma pensato apposta per inseguire e cancellare Creeper. Fu, di fatto, il primo antivirus della storia: nato prima ancora che il termine "virus informatico" esistesse.

### Elk Cloner: il primo virus "in the wild" (1982)

Undici anni dopo, un quindicenne di nome **Rich Skrenta** scrisse **Elk Cloner** per gli Apple II della sua scuola. Si diffondeva tramite floppy disk: ogni volta che un dischetto infetto avviava il computer, il virus si copiava sulla memoria della macchina e infettava ogni altro floppy inserito successivamente. Al cinquantesimo avvio, mostrava una breve poesia sullo schermo invece di fare danni reali. Fu il primo virus a diffondersi al di fuori del laboratorio in cui era nato, "in the wild" come si dice in gergo.

Il termine **"virus"** applicato al software arrivò poco dopo, nel 1984, quando il ricercatore **Fred Cohen** lo usò formalmente nella sua tesi di dottorato per descrivere un programma capace di infettare altri programmi copiandovi al loro interno una versione, eventualmente modificata, di se stesso.

### Brain: il primo virus per PC (1986)

Nel 1986 due fratelli pakistani, **Basit e Amjad Farooq Alvi**, scrissero **Brain**, il primo virus per il sistema operativo MS-DOS dei personal computer IBM. Si diffondeva tramite floppy disk e, curiosamente, non era pensato per nuocere: i fratelli gestivano un negozio di software e volevano tracciare le copie pirata dei loro programmi. Il virus lasciava persino il proprio nome, indirizzo e numero di telefono nel codice. Nonostante le intenzioni, Brain si diffuse ben oltre il Pakistan, raggiungendo migliaia di computer nel mondo.

### Il Morris Worm: la prima crisi di Internet (1988)

Il 2 novembre 1988, **Robert Tappan Morris**, uno studente laureato alla Cornell University, rilasciò un programma pensato — a suo dire — per misurare le dimensioni di Internet. Un errore nel codice lo rese molto più aggressivo del previsto: il **Morris Worm** si copiava ripetutamente sulla stessa macchina già infetta, rallentando i sistemi fino a bloccarli. In poche ore infettò circa il 10% dei computer collegati a Internet dell'epoca, circa 6.000 macchine.

Fu il primo incidente di sicurezza informatica a fare notizia sui giornali nazionali statunitensi. Morris fu il primo condannato negli Stati Uniti in base al *Computer Fraud and Abuse Act*. L'incidente portò alla creazione del **CERT** (Computer Emergency Response Team), il primo team dedicato alla risposta agli incidenti informatici, tuttora un modello replicato in tutto il mondo.

### Dagli anni '90 alla posta elettronica

Negli anni '90 i virus si diffondevano ancora prevalentemente via floppy disk, ma un nuovo veicolo stava per cambiare tutto: l'email. Nel 1999 **Melissa**, un virus macro nascosto in un documento Word, si diffuse mandando automaticamente una copia di se stesso ai primi 50 contatti della rubrica di ogni vittima infetta. Fu tra i primi a dimostrare quanto velocemente un virus potesse propagarsi sfruttando la fiducia delle persone verso un allegato inviato da un conoscente.

Nel 2000 arrivò **ILOVEYOU**, un worm scritto da un programmatore filippino e mascherato da lettera d'amore in allegato. In pochi giorni infettò milioni di computer in tutto il mondo, causando danni stimati in miliardi di dollari tra pulizia dei sistemi e perdita di produttività. Restò per anni tra gli attacchi informatici più costosi mai registrati.

### I worm di rete: Code Red, Nimda, SQL Slammer (2001-2003)

Con la diffusione di Internet ad alta velocità, i worm smisero di aver bisogno dell'intervento umano — un doppio clic su un allegato — per diffondersi: iniziarono a sfruttare direttamente le vulnerabilità dei server esposti in rete. **Code Red** (2001) infettò decine di migliaia di server Microsoft IIS sfruttando una falla non ancora corretta. **Nimda**, lo stesso anno, combinava più tecniche di diffusione contemporaneamente — email, siti web compromessi, condivisioni di rete — diventando uno dei worm più veloci mai visti fino ad allora.

Nel 2003 **SQL Slammer** portò questa logica all'estremo: un programma di appena 376 byte capace di raddoppiare il numero di macchine infette ogni 8,5 secondi circa, infettando la maggior parte dei sistemi vulnerabili nel mondo in meno di dieci minuti. Rimane, ancora oggi, il worm a diffusione più rapida della storia.

### Stuxnet: quando un virus diventa un'arma (2010)

Nel 2010 la scoperta di **Stuxnet** cambiò per sempre la percezione di cosa un virus potesse fare. Non rubava carte di credito né spediva spam: era progettato per infiltrarsi nei sistemi di controllo industriale (SCADA) degli impianti di arricchimento dell'uranio iraniani, e alterare silenziosamente la velocità delle centrifughe fino a danneggiarle fisicamente, mostrando ai tecnici valori di funzionamento normali.

La complessità di Stuxnet — sfruttava quattro vulnerabilità sconosciute fino ad allora (**zero-day**) e componenti firmati con certificati digitali rubati — fece capire agli analisti che dietro non poteva esserci un singolo criminale, ma le risorse di uno stato. È generalmente considerato il primo caso pubblicamente noto di un'arma informatica costruita da governi per colpire un'infrastruttura fisica.

### L'era del ransomware (2017-oggi)

Nel maggio 2017, **WannaCry** colpì oltre 200.000 computer in 150 paesi in pochi giorni, bloccando ospedali, aziende e infrastrutture pubbliche cifrando i loro dati e chiedendo un riscatto in bitcoin per sbloccarli. Sfruttava **EternalBlue**, una vulnerabilità di Windows originariamente scoperta e tenuta segreta dalla NSA statunitense, poi trapelata online.

Poche settimane dopo, **NotPetya** si diffuse con la stessa vulnerabilità, ma con un obiettivo diverso: sembrava un ransomware, ma in realtà non permetteva di recuperare i dati in nessun modo, nemmeno pagando. Era un **wiper**, pensato per distruggere. Causò oltre 10 miliardi di dollari di danni globali, colpendo aziende come Maersk e Merck.

Da allora, il ransomware si è trasformato in un vero e proprio modello di business criminale: il **Ransomware-as-a-Service (RaaS)**, in cui gruppi specializzati sviluppano il malware e lo "affittano" ad altri criminali in cambio di una percentuale sui riscatti, spesso aggiungendo una seconda estorsione — minacciando di pubblicare i dati rubati oltre a cifrarli.

### Dalla curiosità al crimine industrializzato

In poco più di cinquant'anni, il virus informatico è passato da esperimento di laboratorio a strumento di crimine organizzato su scala globale, fino ad arma di stato. Le difese si sono evolute di pari passo: dai primi antivirus basati su firme — che riconoscevano un virus confrontandolo con un catalogo di virus già noti — ai moderni sistemi **EDR** (Endpoint Detection and Response), che osservano il comportamento dei programmi in tempo reale per individuare anche minacce mai viste prima.

I principi di difesa restano quelli visti in [Concetti base della sicurezza informatica]({{ site.baseurl }}{% link _sicurezza/concetti-base.md %}.html): aggiornare i sistemi, limitare i privilegi, diffidare degli allegati inattesi. La storia dei virus dimostra che nessuna di queste regole è mai stata scontata — ed è per questo che vale la pena raccontarla.
