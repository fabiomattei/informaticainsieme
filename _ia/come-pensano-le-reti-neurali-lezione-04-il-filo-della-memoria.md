---
title: 'Lezione 04 — Il filo della memoria'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 4.1 Il problema: sequenze che non hanno una lunghezza fissa

Sia la rete a più piani della Lezione 2 sia lo stencil della Lezione 3 si aspettano un input di dimensione fissa: un certo numero di indizi, un'immagine di una certa grandezza. Il testo, il parlato, una serie di misurazioni nel tempo non hanno questa comodità: una frase può avere cinque parole o cinquecento, e niente in una rete come quelle viste finora gestisce naturalmente input di lunghezza variabile, né tiene conto esplicitamente dell'ordine in cui le cose arrivano. Una rete della Lezione 2 applicata a una frase tratterebbe la prima e la cinquantesima parola come due numeri indipendenti in un elenco, non come due momenti di un racconto che si svolge nel tempo. Serve un'architettura pensata apposta per questo tipo di dato: le **reti neurali ricorrenti**.

### 4.2 L'idea: un amico che ti riassume mentre legge

Immagina un amico che legge un libro ad alta voce e, dopo ogni pagina, ti riassume a voce cosa ha capito finora — non ripetendo il libro intero, ma aggiornando un breve bigliettino mentale che tiene in testa. Ogni volta che gira pagina, non riparte da zero: combina il bigliettino di prima con la nuova pagina appena letta, e produce un bigliettino aggiornato. Questo bigliettino — che in una rete ricorrente si chiama **stato nascosto** — è, in linea di principio, un riassunto di *tutto* ciò che è stato letto fino a quel momento, non solo dell'ultima pagina: informazioni della pagina 3 possono, in teoria, sopravvivere nel bigliettino fino alla pagina 50, se sono state ritenute abbastanza importanti lungo il cammino.

Il dettaglio cruciale è che il tuo amico usa sempre lo stesso identico metodo di riassunto a ogni pagina — non inventa una strategia nuova ogni volta, ma applica la stessa identica regola, pagina dopo pagina, riusando gli stessi "criteri di riassunto" già visti nella Sezione 3.2 come idea di riuso, qui applicata non allo spazio di un'immagine ma al tempo di una lettura.

### 4.3 Allenarsi a riassumere bene

Anche un'architettura come questa va allenata, con lo stesso principio generale che la Lezione 6 tratterà in dettaglio: guardare l'errore finale e correggere, un po' alla volta, il modo in cui il bigliettino viene aggiornato. La particolarità qui è che, per farlo, bisogna "srotolare" mentalmente tutta la sequenza di aggiornamenti — trattarla come una catena lunga quanto il numero di pagine lette, con lo stesso identico metodo di riassunto ripetuto a ogni anello della catena — e poi far risalire la correzione all'indietro lungo l'intera catena, dall'ultima pagina fino alla prima. Questa procedura ha un nome tecnico, **backpropagation through time**, ma l'idea è la stessa "colpa che risale la catena" che vedremo, per una rete non ricorrente, nella Lezione 6.

### 4.4 Il bigliettino che si sporca

Qui emerge un problema pratico, ed è proprio quello che ti invito a sperimentare tu stesso nel "Prova tu" di questa lezione: più lunga è la sequenza, più il bigliettino tende a "sporcarsi". Ogni aggiornamento rimescola un po' il contenuto precedente con la nuova pagina, e ripetuto decine o centinaia di volte, questo rimescolamento tende a diluire i dettagli delle pagine più lontane — un po' come cercare di ricordare la settima voce di una lista della spesa dettata a voce, senza scriverla: le prime voci si confondono con quelle intermedie, e a volte sopravvivono solo le ultime, sentite di fresco. Il problema tecnico ha un nome, il **vanishing gradient nel tempo**: il segnale che dovrebbe insegnare alla rete "questo dettaglio della pagina 3 era importante" si affievolisce esponenzialmente attraversando decine di aggiornamenti successivi, fino quasi a sparire.

### 4.5 Un bigliettino più furbo: tenere un diario invece di un unico appunto

Due varianti più sofisticate di questa architettura — chiamate **LSTM** e **GRU** — affrontano il problema della Sezione 4.4 con un'idea intuitiva: invece di rimescolare sempre tutto il bigliettino a ogni pagina, la rete impara esplicitamente *cosa* vale la pena tenere dal bigliettino vecchio e *cosa* invece va sovrascritto con l'informazione nuova — un po' come un diario in cui certe righe restano intoccate pagina dopo pagina, e solo poche righe specifiche vengono aggiornate quando serve davvero. Questo meccanismo, chiamato **gating**, allevia molto il problema della Sezione 4.4 — il bigliettino resta leggibile per sequenze più lunghe, centinaia di pagine invece di poche decine — ma non lo elimina del tutto: anche un diario ben tenuto, dopo migliaia di pagine, comincia a perdere dettagli remoti.

### 4.6 Un problema diverso, che nessun diario può risolvere

C'è però un secondo limite, distinto dal primo, che nessuna variante di gating può aggirare, perché non dipende da *come* il bigliettino viene aggiornato ma dal fatto stesso che debba essere aggiornato *una pagina alla volta*: per sapere cosa dice il bigliettino dopo la pagina 50, devi prima sapere cosa diceva dopo la pagina 49, che a sua volta richiede la 48, e così via fino alla prima. Il tuo amico non può leggere due pagine contemporaneamente — anche avendo a disposizione altri cento amici pronti ad aiutarlo, la lettura di un libro di 500 pagine richiederebbe comunque 500 passaggi, uno dopo l'altro, nessuno saltabile. È esattamente questo vincolo — non il bigliettino che si sporca, già in parte curato dalla Sezione 4.5 — il motivo per cui, quando è diventato possibile allenare su quantità di testo enormi con hardware capace di lavorare su migliaia di cose insieme, questo tipo di architettura è diventato il vero collo di bottiglia da superare. La Lezione 8 riprenderà esattamente questo secondo problema.

---

> **Prova tu — Il telefono senza fili**
>
> Gioca (o immagina di giocare) al gioco del telefono senza fili con una catena di sei persone, in fila. Il messaggio di partenza, sussurrato alla prima persona, è: *"Il gatto grigio di Marta ha dormito tutto il pomeriggio sul davanzale della finestra."*
>
> Ogni persona può sussurrare all'orecchio del vicino solo *quello che ricorda* del messaggio ricevuto — non può farselo ripetere, non può scriverlo. Prova a immaginare (o a far provare a cinque amici) come si trasforma il messaggio, persona dopo persona.
>
> 1. Quali parti del messaggio ti aspetti sopravvivano meglio: i dettagli all'inizio ("il gatto grigio di Marta"), quelli del mezzo ("ha dormito tutto il pomeriggio") o quelli alla fine ("sul davanzale della finestra")? Perché?
> 2. Se una delle sei persone, invece di rimescolare tutto alla rinfusa, si impegnasse a ripetere **parola per parola** solo il soggetto della frase ("il gatto grigio di Marta") lasciando che il resto si trasformi liberamente — che tipo di meccanismo della Sezione 4.5 starebbe imitando?
> 3. Con sei persone il messaggio arriva già abbastanza deformato. Cosa ti aspetti succeda con una catena di sessanta persone invece di sei — e quale dei due problemi discussi in questa lezione (bigliettino che si sporca, oppure lettura obbligatoriamente una pagina alla volta) sta peggiorando in quel caso?

---

*Continua con la [Lezione 05 — Quanto costa sbagliare]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-05-quanto-costa-sbagliare.md %}.html)*
