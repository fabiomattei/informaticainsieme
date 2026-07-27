---
title: 'Lezione 07 — Quanto ci possiamo fidare'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 7.1 Come si dà un voto a un chatbot?

Se due aziende dicono entrambe "il nostro modello è il più bravo", come si stabilisce chi ha ragione? Serve un modo oggettivo di misurare le capacità di un LLM — esattamente come si fa un'interrogazione a scuola per misurare quanto uno studente ha imparato. La soluzione più diffusa sono i **benchmark**: enormi raccolte di domande con risposta nota (problemi di matematica, quesiti di cultura generale, esercizi di programmazione, domande a scelta multipla su decine di materie), su cui si fa "sostenere l'esame" al modello e si conta quante risposte azzecca.

Sembra semplice e onesto. Ha però un tallone d'Achille pratico, sorprendentemente simile a un problema che conosci bene dalla tua vita scolastica.

### 7.2 Il problema del compito già visto

Immagina un'interrogazione in cui, per puro caso, le domande sono identiche a quelle di un compito che hai già svolto e corretto in classe la settimana prima. Prenderesti un voto altissimo — ma quel voto non misurerebbe affatto quanto hai capito l'argomento: misurerebbe solo quanto ti ricordi di quel compito specifico.

Lo stesso rischio vale, in modo ancora più insidioso, per un LLM: se il modello è stato addestrato leggendo praticamente "tutto il web", è più che plausibile che le domande di un benchmark famoso — insieme alle loro risposte corrette — fossero già presenti, magari alla lettera, da qualche parte nei miliardi di pagine di testo di addestramento. Un modello che ottiene un punteggio altissimo su un benchmark del genere potrebbe semplicemente **ricordarselo**, non saperlo ragionare da zero. Questo problema si chiama **contaminazione del test**, ed è uno dei motivi per cui i punteggi sui benchmark vanno sempre presi con un pizzico di scetticismo in più di quanto sembrerebbe a prima vista.

### 7.3 Chiedere a un altro modello di fare da giudice

Molte domande interessanti — "questa risposta è ben scritta?", "questa spiegazione è chiara?" — non hanno una singola risposta giusta da confrontare meccanicamente, come invece succede con "quanto fa 12×7?". Per queste, si è diffusa una tecnica un po' sorprendente: usare un **secondo LLM come giudice**, mostrandogli due risposte diverse alla stessa domanda e chiedendogli quale preferisce — un po' come chiedere a un insegnante esperto di leggere due temi e dire quale è meglio scritto, invece di contare solo gli errori di grammatica. Funziona sorprendentemente bene, ma eredita gli stessi limiti e le stesse preferenze "di gusto" (a volte anche gli stessi pregiudizi) del modello-giudice scelto — e i modelli-giudice, come vedremo nella Lezione 8, non sono affatto immuni dal preferire risposte lunghe e sicure di sé anche quando sono sbagliate.

Un'alternativa complementare sono le **arene**: piattaforme dove persone in carne e ossa confrontano alla cieca le risposte di due chatbot diversi alla stessa domanda, senza sapere quale modello ha prodotto quale risposta, e votano quella che preferiscono. Su tantissimi confronti, emerge una classifica — più simile a una classifica di gradimento del pubblico che a un esame oggettivo, ma proprio per questo utile a catturare aspetti (tono, utilità percepita) che un semplice punteggio su un test a crocette non cattura.

### 7.4 Capacità che sembrano spuntare dal nulla

Un fenomeno molto discusso, e spesso raccontato in modo un po' troppo magico, è quello delle cosiddette **capacità emergenti**: alcuni compiti (ad esempio la capacità di risolvere un problema aritmetico a più passaggi) sembrano restare a un livello di prestazione piatto e mediocre man mano che un modello cresce di dimensione — finché, superata una certa soglia di grandezza, il punteggio schizza improvvisamente verso l'alto, come se il modello avesse acquisito una capacità completamente nuova di colpo.

Un gruppo di ricercatori ha però mostrato qualcosa di interessante: buona parte di questi "salti improvvisi" **sono in realtà un'illusione creata dal modo in cui si misura**, non un vero salto nel comportamento del modello. Se un compito viene valutato in modo "tutto o niente" (la risposta finale è giusta o sbagliata, punto), un modello che migliora gradualmente e con continuità — sbagliando un dettaglio in meno a ogni passaggio interno del ragionamento — può restare bloccato a "risposta finale sbagliata" per molto tempo, per poi passare improvvisamente a "risposta finale giusta" nel momento esatto in cui l'ultimo dettaglio residuo viene azzeccato. Misurando invece con un metro più fine (quanti passaggi intermedi del ragionamento sono corretti, non solo il verdetto finale) la stessa identica curva risulta liscia e graduale, non a scalino. La capacità "emergente", insomma, spesso non emerge affatto dal nulla: emergeva già gradualmente, ma la misura usata era troppo grossolana per accorgersene.

### 7.5 Insegnargli a "pensare a voce alta"

Un'ultima osservazione utile riguarda un trucco molto semplice quanto efficace: chiedere esplicitamente al modello di scrivere il proprio ragionamento passo passo, prima di dare la risposta finale ("pensiamoci con calma, passo per passo..."), invece di sparare subito il verdetto. Questa tecnica — chiamata **catena di pensiero** (chain-of-thought) — spesso migliora sensibilmente l'accuratezza su problemi che richiedono più passaggi logici o aritmetici, un po' come quando un insegnante ti chiede di "mostrare i calcoli" invece di scrivere solo il risultato finale: non è che il foglio in sé ti renda più bravo, ma il fatto di scomporre il problema in passaggi più piccoli e verificabili aiuta a commettere meno errori lungo la strada. Alcuni modelli più recenti vengono ora allenati esplicitamente, con le tecniche della Lezione 5, a produrre ragionamenti interni lunghi ed estesi prima di rispondere — pagando in tempo di attesa quello che guadagnano in accuratezza.

---

> **Prova tu — Il quiz-trappola**
>
> Ecco un mini-benchmark di quattro domande. Il tuo compito non è rispondere alle domande, ma **fare da revisore**: per ciascuna, decidi se ti sembra "sospetta" — cioè, se pensi sia plausibile che un modello l'abbia già vista, identica o quasi, da qualche parte nel suo testo di addestramento — oppure "genuina", pensata apposta per essere nuova.
>
> 1. "Qual è la capitale della Francia?"
> 2. "Se un fruttivendolo di Cuneo compra 47 casse da 23 mele ciascuna a 0,34 € a mela, e ne rivende i tre quarti maggiorando il prezzo del 22%, quanto guadagna in tutto, arrotondato al centesimo?"
> 3. "Quanto fa 2 + 2?"
> 4. "Descrivi in due frasi la trama del romanzo immaginario 'Il sentiero di vetro spezzato' di un autore inventato apposta per questo esercizio."
>
> Per ciascuna, scrivi "sospetta" o "genuina" e il perché. Suggerimento: pensa a quante volte, in tutto il web, potresti aspettarti di trovare scritta *esattamente* quella domanda (o una sua variante quasi identica) insieme alla risposta. Confronta il tuo ragionamento con l'Appendice A.

---

*Continua con la [Lezione 08 — Perché a volte sbaglia]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-08-perche-a-volte-sbaglia.md %}.html)*
