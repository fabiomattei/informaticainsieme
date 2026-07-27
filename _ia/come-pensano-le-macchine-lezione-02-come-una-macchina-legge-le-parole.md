---
title: 'Lezione 02 — Come una macchina legge le parole'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 2.1 Un computer non sa cosa sia "gatto"

Prova a fermarti un attimo su un fatto scomodo: un computer non ha idea di cosa significhi la parola "gatto". Non ha mai visto un gatto, non ha mai sentito le fusa, non collega quella parola a niente — a meno che tu non gliela traduca in qualcosa che sa maneggiare davvero: i **numeri**. Tutto quello che un LLM fa, in fondo, è aritmetica su numeri. Il primo problema pratico, allora, è: come si trasforma un testo — fatto di lettere, spazi, punteggiatura — in numeri, senza perdere per strada il significato?

### 2.2 Spezzare in pezzi Lego

Il primo passo si chiama **tokenizzazione**: il testo viene tagliato in pezzetti — chiamati *token* — che poi verranno trasformati in numeri. La cosa forse sorprendente è che questi pezzetti spesso *non* sono parole intere.

Immagina di avere a disposizione un insieme fisso di "mattoncini Lego" — diciamo qualche decina di migliaia di pezzi diversi — con cui costruire qualsiasi testo possibile, in qualsiasi lingua. Le parole comuni ("il", "casa", "andare") probabilmente hanno un mattoncino tutto per loro, perché compaiono così spesso che vale la pena dedicargli un pezzo apposito. Ma una parola rara, o inventata, o straniera — "sesquipedale", "ChatGPT", "supercalifragilistichespiralidoso" — quasi certamente **non** ha un mattoncino dedicato: viene spezzata in due, tre, quattro pezzi più piccoli che, uniti, la ricompongono. Un po' come costruire "castello" con i mattoncini "cas" + "tel" + "lo" quando non hai il pezzo "castello" intero nella scatola.

Questo spiega, tra l'altro, una stranezza che forse hai notato usando un chatbot: certe parole "costano" di più di altre. Una parola rara o in una lingua poco rappresentata nei testi di addestramento viene spesso spezzata in molti più pezzi di una parola comune in inglese — ed essendo il "prezzo" di un chatbot spesso legato al numero di pezzetti processati, questo ha conseguenze molto concrete, non solo teoriche.

### 2.3 Una mappa dei significati

Spezzare in pezzetti risolve solo metà del problema: abbiamo dei pezzi, ma sono ancora "pezzi di lettere", non numeri che catturano il significato. Il passo successivo — e qui sta il cuore dell'idea — è associare a ogni pezzetto un punto in una specie di **mappa dei significati**: invece di longitudine e latitudine, questa mappa ha centinaia di "coordinate", ma l'idea di fondo è la stessa di una mappa geografica. Parole con significato simile finiscono vicine sulla mappa; parole senza relazione finiscono lontane.

Per farti un'idea con una mappa fittizia a sole due coordinate (una mappa vera ne usa centinaia, ma il principio non cambia): "gatto" e "cane" — entrambi animali domestici — potrebbero finire vicini, tipo alle coordinate (3, 5) e (4, 5). "Automobile" starebbe da tutt'altra parte, diciamo (9, 1). E "gattino" — piccolo di gatto — starebbe vicinissimo a "gatto", magari (3, 6).

Il punto cruciale, e forse il più sorprendente: **nessuno disegna questa mappa a mano**. Il modello la costruisce da solo, durante l'addestramento, spostando pian piano ogni parola sulla mappa in modo che l'unica cosa che conta — indovinare bene la parola successiva — funzioni sempre meglio. Se spostare "gatto" più vicino a "cane" aiuta a indovinare meglio le frasi del testo di addestramento, il modello lo fa. Nessuno gli ha insegnato che sono entrambi animali: lo ha dedotto vedendo che compaiono spesso in contesti simili ("il mio ___ dorme tutto il giorno", "porto il ___ dal veterinario").

### 2.4 Fare i conti con i significati

La conseguenza più sorprendente di avere i significati come punti su una mappa è che ci puoi fare *aritmetica*. Non aritmetica sulle lettere — aritmetica sulle coordinate. Il caso più famoso, diventato quasi un aneddoto classico nel campo: prendi il punto di "re", sottrai il punto di "uomo", aggiungi il punto di "donna". Il punto più vicino al risultato, sulla mappa costruita da un modello ben addestrato, tende a essere... "regina".

Detto a parole: "re" sta a "uomo" come "?" sta a "donna" — e il modello, senza che nessuno gli abbia insegnato la parola "monarchia" o "genere", risponde correttamente solo perché ha organizzato la mappa in modo che questa relazione geometrica rispecchi una relazione di significato reale. Non funziona sempre alla perfezione, e su mappe più piccole o mal costruite l'aneddoto è più pulito in teoria che in pratica — ma il principio regge, ed è uno dei modi più diretti per toccare con mano che quei numeri non sono arbitrari: hanno una geometria che *significa* qualcosa.

### 2.5 Anche l'ordine conta

Un ultimo dettaglio, facile da dimenticare: "il cane morde il postino" e "il postino morde il cane" usano esattamente le stesse parole — ma vorresti sicuramente che il modello le distinguesse. Per questo, oltre alla posizione sulla mappa dei significati, a ogni pezzetto viene attaccata anche un'informazione che dice "sei il primo token della frase", "sei il secondo", e così via — una specie di numeretto di posizione cucito addosso a ogni pezzo, che il modello impara a leggere insieme al significato. Torneremo su come questo numeretto di posizione entra in gioco proprio nel meccanismo di attenzione, nella prossima lezione.

---

> **Prova tu — Il cruciverba dei vettori**
>
> Ecco una mappa dei significati giocattolo, con solo due coordinate (orizzontale, verticale) e otto parole già posizionate:
>
> | Parola | Coordinate |
> |---|---|
> | uomo | (2, 6) |
> | donna | (2, 2) |
> | re | (8, 6) |
> | regina | (?, ?) |
> | ragazzo | (1, 7) |
> | ragazza | (1, 3) |
> | principe | (7, 7) |
> | principessa | (?, ?) |
>
> La relazione geometrica che lega "uomo" a "donna" è: stessa coordinata orizzontale (2), coordinata verticale che scende da 6 a 2 (cioè: **−4**).
>
> 1. Applica la stessa relazione ("re" meno "uomo" più "donna") per calcolare le coordinate mancanti di "regina", partendo da "re" = (8, 6).
> 2. Controlla che la stessa identica relazione, applicata a "principe" = (7, 7), ti dia coordinate ragionevoli per "principessa" — coerenti, cioè, con lo stesso spostamento verticale di −4 che hai usato sopra.
> 3. Bonus: guardando solo le coordinate orizzontali (2, 2, 8, 1, 1, 7), riesci a indovinare *cosa* rappresenta quell'asse — che caratteristica condivisa separa il gruppo (2,2,1,1) dal gruppo (8,7)?
>
> Soluzioni e ragionamento in Appendice A.

---

*Continua con la [Lezione 03 — Il segreto dell'attenzione]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-03-il-segreto-dellattenzione.md %}.html)*
