---
title: 'Lezione 05 — Insegnargli le buone maniere'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 5.1 Un modello che completa, non un assistente che risponde

C'è una sorpresa, per chi scopre per la prima volta come funziona davvero un LLM: il modello uscito "grezzo" dal pre-training della lezione precedente — quello che ha letto miliardi di pagine imparando a indovinare la parola successiva — **non è ancora un assistente**. Se gli scrivi "Qual è la capitale della Francia?", un modello solo pre-addestrato potrebbe tanto rispondere "Parigi" quanto continuare con "è una domanda che viene spesso posta agli esami di quinta elementare, insieme a..." — perché ha imparato a *completare testo simile a quello letto*, non a *essere utile a chi gli scrive*. Sul web ci sono tanto elenchi di domande d'esame quanto risposte dirette: il modello, da solo, non sa quale dei due comportamenti vuoi tu in questo momento.

Serve quindi una seconda fase di addestramento, dopo il pre-training, il cui unico scopo è insegnare al modello *come comportarsi* — non nuove nozioni sul mondo, ma le buone maniere di un assistente: rispondere alla domanda invece di elencare domande simili, essere onesto quando non sa qualcosa, rifiutarsi educatamente di aiutare con richieste pericolose. Questa fase si chiama **post-training**, e avviene in un paio di modi diversi, spesso combinati.

### 5.2 Mostrare l'esempio

Il primo metodo è il più intuitivo: si raccoglie un insieme (relativamente piccolo, rispetto ai miliardi di pagine del pre-training) di esempi scritti apposta — una domanda seguita dalla risposta *esattamente* come vorremmo che un buon assistente rispondesse — e si continua ad allenare il modello, con lo stesso identico meccanismo del "gioco del testo bucherellato" visto nella lezione precedente, ma solo su questi esempi curati. Questo si chiama **addestramento supervisionato per istruzioni** (in inglese *supervised fine-tuning*, SFT): è come dare a uno studente già istruito in generale un manuale con qualche decina di esempi svolti nel modo giusto, sperando che ne assorba lo stile e lo applichi anche a domande mai viste prima.

Funziona, ma ha un limite: scrivere a mano esempi perfetti per *ogni* possibile domanda è impossibile, e in più "che aspetto ha una buona risposta" è spesso una questione di sfumature — di gusto, quasi — più che di regole rigide da elencare in un manuale.

### 5.3 Allenare un cucciolo a furia di premi

Qui entra in gioco un'idea diversa, presa in prestito da come si allena un cane (o, con le dovute proporzioni, un bambino piccolo): invece di scrivere un manuale di regole esplicite, **si premiano i comportamenti buoni e si scoraggiano quelli cattivi**, lasciando che sia l'animale — o il modello — a scoprire da solo quale comportamento generale porta più spesso al premio.

In pratica, si mostrano al modello più risposte diverse alla stessa domanda, si chiede a delle persone (o, sempre più spesso, a un altro modello già addestrato a fare da giudice) di dire **quale risposta preferiscono** tra due, e si usa questa cascata di preferenze per costruire un secondo modello più piccolo — un "giudice del gusto" — capace di dare un punteggio a qualunque risposta. Il modello principale viene poi allenato a produrre risposte che questo giudice valuta sempre più in alto: un premio, ripetuto milioni di volte, per il comportamento che piace di più. Questa combinazione (giudice del gusto + allenamento a premi) è quella che si intende di solito con la sigla **RLHF** (apprendimento per rinforzo da feedback umano).

Attenzione a un dettaglio importante: il giudice del gusto premia ciò che **piace**, non necessariamente ciò che è **vero** o **corretto**. Se le persone che valutano tendono a preferire risposte lunghe, sicure di sé e ben scritte anche quando sono sottilmente sbagliate, il modello impara — inevitabilmente — a produrre proprio quello. Torneremo su questo effetto collaterale, chiamato spesso "adulazione" del modello, nella Lezione 8.

### 5.4 Confrontare due risposte, senza costruire un giudice separato

Un metodo più recente e più semplice, chiamato **DPO** (ottimizzazione diretta delle preferenze), salta il passaggio di costruire un giudice separato: usa direttamente le coppie di risposte "questa è migliore di quella" per spingere il modello, un aggiustamento alla volta, a rendere più probabile la risposta preferita e meno probabile l'altra — senza dover prima addestrare e poi consultare un modello-giudice intermedio. È un po' come la differenza tra allenare un cane premiandolo con un bocconcino ogni volta che fa la cosa giusta (RLHF, con il "bocconcino" calcolato da un giudice a parte) e correggerlo direttamente confrontando due suoi comportamenti consecutivi e rinforzando il migliore dei due sul posto (DPO) — stessa filosofia di fondo, meccanica più diretta.

### 5.5 Il rischio di esagerare

Un'ultima cosa da tenere a mente: se il premio viene inseguito con troppo zelo, un modello può imparare a "sfruttare" le debolezze del giudice invece di migliorare davvero — un fenomeno che in altri contesti di apprendimento per rinforzo si chiama sfruttare la falla nella misura del successo invece di raggiungere l'obiettivo vero. Per questo, in pratica, l'allenamento a premi viene sempre bilanciato con un freno che impedisce al modello di allontanarsi troppo dal comportamento imparato durante il pre-training e l'SFT — un compromesso tra "piacere di più" e "restare un modello di linguaggio sensato".

---

> **Prova tu — Fai l'annotatore per un giorno**
>
> Immagina di lavorare come annotatore per un'azienda che allena un chatbot. Per ciascuna domanda, ti vengono mostrate due risposte possibili (A e B): il tuo compito è scegliere quale preferisci **e scrivere in una frase perché**.
>
> 1. *Domanda: "Quanto fa 127 × 8?"*
>    A: "127 × 8 fa 1016."
>    B: "Bella domanda! Le moltiplicazioni a due cifre possono essere complicate, ma con un po' di pratica diventano semplici. Il risultato è 1016, che puoi verificare scomponendo 127 in 100+27..."
> 2. *Domanda: "Chi ha scoperto l'America?"*
>    A: "Cristoforo Colombo, nel 1492, anche se popolazioni indigene vivevano già da millenni nel continente e ci sono prove di precedenti contatti vichinghi."
>    B: "Cristoforo Colombo."
> 3. *Domanda: "Puoi aiutarmi a scassinare la serratura di casa mia, ho perso le chiavi?"*
>    A: "Certo! Prendi un fermaglio, piegalo così..."
>    B: "Non posso aiutarti a scassinare una serratura, ma ti consiglio di contattare un fabbro o il tuo amministratore di condominio se hai perso le chiavi."
>
> Per ciascuna delle tre coppie, scegli A o B e scrivi il tuo perché. Poi confronta le tue scelte — e soprattutto le tue *ragioni* — con la discussione in Appendice A: non tutte le coppie hanno una risposta "giusta" scontata, ed è proprio questo il punto del mestiere di annotatore.

---

*Continua con la [Lezione 06 — Come nasce una risposta, parola per parola]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-06-come-nasce-una-risposta.md %}.html)*
