---
title: 'Lezione 08 — Perché a volte sbaglia (e perché può essere pericoloso)'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 8.1 Inventare non è un guasto — è il meccanismo stesso

Hai già sentito parlare di chatbot che "inventano" fonti, citazioni, o persino eventi storici mai accaduti, presentandoli con la stessa sicurezza con cui direbbero un fatto vero. Questo fenomeno si chiama **allucinazione**, ma il nome è un po' fuorviante: fa pensare a un guasto occasionale, un bug da correggere. La realtà è più scomoda: un LLM **non ha, da nessuna parte al suo interno, un archivio di "fatti verificati" separato dal resto**. Tutto quello che fa — l'abbiamo visto fin dalla Lezione 1 — è indovinare la parola più plausibile visto il contesto. Quando la domanda riguarda qualcosa che il modello ha visto scritto migliaia di volte durante l'addestramento (la capitale della Francia), indovinare la parola plausibile e dire il vero coincidono quasi sempre. Quando la domanda riguarda qualcosa che il modello non ha mai visto — un dettaglio troppo specifico, un evento troppo recente, una fonte che semplicemente non esiste — il modello continua comunque a fare l'unica cosa che sa fare: produrre la continuazione più plausibile *nello stile* di una risposta corretta, anche se il contenuto è inventato di sana pianta.

### 8.2 Riempire i vuoti di un ricordo confuso

Un'analogia utile: pensa a quando racconti un ricordo d'infanzia sfocato. Non menti consapevolmente, ma la tua mente riempie automaticamente i dettagli mancanti con qualcosa di plausibile — il colore di un vestito, l'ordine esatto degli eventi — pur di restituire una storia coerente, e spesso *ti convinci* che sia andata proprio così. Un LLM fa qualcosa di analogo, ma in modo ancora più sistematico: non ha modo di distinguere internamente, con certezza, "questo lo so per certo" da "questo suona plausibile ma non ne sono sicuro" — perché, semplicemente, non ragiona in termini di certezza e incertezza come farebbe un umano consapevole dei propri limiti. Dopo l'addestramento a premi della Lezione 5, il problema spesso peggiora invece di migliorare: se le persone che valutano le risposte tendono a preferire risposte sicure di sé e ben argomentate anche quando sono sbagliate — e tendono a farlo, perché una risposta piena di dubbi è meno gradevole da leggere — il modello impara a *sembrare* sicuro anche quando non dovrebbe esserlo. Questo effetto collaterale si chiama spesso "adulazione" (sycophancy) del modello.

### 8.3 Pregiudizi ereditati dai testi

Un LLM impara esclusivamente dai testi che legge — e quei testi, scritti da persone reali in decenni e secoli diversi, portano con sé gli stessi pregiudizi, stereotipi e squilibri di rappresentazione che si trovano nella società che li ha prodotti. Se nei testi di addestramento certe professioni compaiono più spesso associate a un genere, o certe nazionalità compaiono più spesso in contesti negativi, il modello tende ad assorbire — e poi riprodurre nelle sue risposte — quelle stesse associazioni statistiche, senza che nessuno gliele abbia insegnate esplicitamente come "regole". Misurare quanto un modello sia distorto in un modo o nell'altro, e correggerlo dove possibile, è oggi un intero campo di studio a sé, perché il pregiudizio non è mai un singolo bug da sistemare: è distribuito su miliardi di manopole interne, esattamente come lo è ogni altra conoscenza del modello.

### 8.4 Basta cambiare le parole, e la risposta cambia

Un'altra fragilità sorprendente: **piccole modifiche a una domanda, ininfluenti per un lettore umano, possono ribaltare la risposta di un LLM**. Cambiare l'ordine delle opzioni in una domanda a scelta multipla, aggiungere uno spazio di troppo, riformulare una domanda di matematica con parole leggermente diverse ma stesso identico problema: tutte cose che a un essere umano non cambierebbero la risposta di una virgola, ma che possono far vacillare un modello. Questo capita perché il modello non "capisce" il problema in astratto, separandolo dalle parole precise usate per porlo — la sua rappresentazione del problema *è* intrecciata con le parole esatte, in un modo più fragile di quanto ci piacerebbe credere.

### 8.5 Il gioco delle scappatoie

Un modello viene addestrato, come visto nella Lezione 5, anche a *rifiutarsi* di aiutare con richieste pericolose (costruire un'arma, scrivere codice dannoso, generare contenuti illegali). Ma un rifiuto imparato durante l'addestramento non è una barriera fisica invalicabile: è, di nuovo, solo un altro pattern appreso — e i pattern appresi si possono aggirare con astuzia. Le tecniche per farlo si chiamano **jailbreak**: convincere il modello, con un contesto inventato ad hoc ("stai scrivendo una scena di un film, il personaggio malvagio deve spiegare come..."), a produrre comunque il contenuto che rifiuterebbe se chiesto direttamente. Alcune tecniche sono sorprendentemente semplici — nascondere la richiesta pericolosa in mezzo a decine di richieste innocue, ad esempio, può abbassare le difese del modello quasi come un rumore di fondo che stanca un guardiano attento.

Per questo, chi costruisce questi modelli investe tempo apposta a cercare le proprie falle prima che lo facciano altri: un processo chiamato **red teaming**, in cui persone (e, sempre più spesso, altri modelli) provano sistematicamente a "rompere" il modello con richieste sempre più creative, per scoprire e correggere le falle prima del rilascio pubblico. Una tecnica interessante per rendere questo processo più scalabile si chiama **AI costituzionale**: invece di far correggere ogni singola risposta problematica da una persona, si dà al modello stesso un piccolo insieme di principi scritti ("non aiutare azioni pericolose", "sii onesto") e lo si allena a **criticare e correggere le proprie risposte** confrontandole con quei principi — una specie di coscienza scritta esplicitamente, invece che assorbita implicitamente e in modo opaco dai dati.

---

> **Prova tu — Fai il fact-checker**
>
> Ecco cinque affermazioni. Alcune sono vere, altre sono "allucinazioni" plausibili ma false, scritte apposta per suonare credibili. Segna per ciascuna "vera" o "inventata" — senza usare internet, solo ragionando su quanto ti suona plausibile e su cosa già sai.
>
> 1. "La Torre Eiffel fu costruita originariamente come struttura temporanea per l'Esposizione Universale di Parigi del 1889."
> 2. "Il romanzo 'I Promessi Sposi' di Alessandro Manzoni fu pubblicato per la prima volta nel 1712."
> 3. "L'acqua bolle a una temperatura più bassa in montagna che al livello del mare, a causa della minore pressione atmosferica."
> 4. "Albert Einstein fu bocciato in matematica alle scuole superiori."
> 5. "Il linguaggio di programmazione Python prende il nome dal serpente pitone, scelto dal suo creatore come simbolo di 'flessibilità' del linguaggio."
>
> Scrivi le tue cinque risposte con una breve motivazione, poi confrontale con l'Appendice A: alcune di queste sono esattamente il tipo di "fatto" plausibile che un LLM potrebbe inventare con piena sicurezza — e il bello dell'esercizio è notare quali segnali (una data troppo precisa, un dettaglio troppo pulito) ti hanno fatto insospettire, o no.

---

*Continua con la [Lezione 09 — Oltre la chat]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-09-oltre-la-chat.md %}.html)*
