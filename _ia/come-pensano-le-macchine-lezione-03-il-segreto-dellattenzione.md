---
title: 'Lezione 03 — Il segreto dell''attenzione'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 3.1 A chi presti attenzione mentre leggi

Leggi questa frase: *"Marco ha detto a Luca che gli avrebbe prestato il libro."*

A chi si riferisce "gli"? Grammaticalmente potrebbe essere sia Marco sia Luca — ma tu, leggendo, probabilmente hai già deciso (quasi certamente "a Luca", perché ha senso che Marco presti *a lui*). Non l'hai deciso guardando "gli" isolato: l'hai deciso confrontandolo, in un istante, con "Marco", con "Luca", con "prestato" — pesando mentalmente quali parole della frase ti servono per sciogliere quell'ambiguità.

Per capire quanto sia sottile questo lavoro, prova un secondo esempio, dove basta cambiare *una sola parola* per ribaltare completamente la risposta:

- *"Il trofeo non entrava nella valigia perché era troppo **grande**."* A chi si riferisce "era"? Al trofeo.
- *"Il trofeo non entrava nella valigia perché era troppo **piccola**."* E ora? Alla valigia.

"Era" occupa esattamente la stessa posizione in entrambe le frasi — la terza parola dalla fine, se conti a ritroso. Se un lettore, o un programma, decidesse a chi si riferisce "era" guardando solo *quante parole indietro* si trova il candidato più probabile — una regola fissa tipo "guarda sempre due parole prima" — sbaglierebbe una delle due frasi di sicuro, perché la posizione da guardare non cambia mai, ma il *significato* dell'ultima parola sì. Contare le parole non basta: bisogna capire cosa dicono, e aggiustare il tiro frase per frase.

Questo — guardare le altre parole di un testo e decidere quanto ciascuna conta, in base al *contenuto* e non solo alla posizione, per capire quella che hai davanti — è esattamente ciò che fa il meccanismo di **attenzione**, il cuore di tutti i modelli linguistici moderni (i cosiddetti *Transformer*, di cui abbiamo parlato nella Lezione 1). Ricordi il "numeretto di posizione" cucito a ogni parola, di cui parlavamo alla fine della Lezione 2? È proprio qui che entra in gioco davvero: l'attenzione lo usa *insieme* al significato di ogni parola, mai al posto suo — è per questo che una regola basata sulla sola posizione, come quella appena vista, non basta mai da sola. La buona notizia è che l'idea, spogliata della matematica, resta semplice come questi due esempi.

### 3.2 Un cartellino per ogni parola

Immagina un'aula durante un lavoro di gruppo. Ogni studente porta appeso al collo un cartellino che descrive **cosa può offrire** ("so risolvere equazioni", "conosco bene la storia romana", "ho la penna rossa"). Quando uno studente ha un dubbio, si fa mentalmente una **domanda** ("mi serve aiuto con un'equazione") e scorre con lo sguardo i cartellini in giro per la stanza, decidendo — in base a quanto ogni cartellino risponde alla sua domanda — quanto ascoltare ciascun compagno. Non è un sì/no secco: magari ascolta per l'80% chi ha scritto "so risolvere equazioni" e per il 20% chi ha scritto qualcos'altro di vagamente utile, ignorando quasi del tutto chi ha solo la penna rossa.

Un modello con attenzione fa esattamente questo, per ogni singola parola del testo, contemporaneamente:

- ogni parola si pone una **domanda** ("di cosa ho bisogno per essere capita meglio?"),
- ogni parola espone un **cartellino** che descrive cosa offre,
- e ogni parola, confrontando la propria domanda con i cartellini di tutte le altre, decide quanto "ascoltare" ciascuna — un pizzico da questa, tanto da quella, pochissimo da un'altra ancora.

Proviamo a mettere numeri plausibili (inventati, ma realistici) sulla frase di apertura. Quando il modello elabora "gli", la sua domanda assomiglia a "chi ha appena ricevuto un'azione, in questa frase?". Confrontandola con i cartellini delle parole già lette, potrebbe arrivare a una ripartizione di questo tipo:

| Parola ascoltata | Quanto "gli" la ascolta | Perché (il "cartellino" di quella parola) |
|---|---|---|
| Luca | 60% | "sono il destinatario del verbo 'detto'" |
| prestato | 25% | "sono l'azione futura di cui si parla" |
| Marco | 10% | "sono chi parla" |
| a, che, ha | 5% in tutto | poco rilevanti per la domanda di "gli" |

Il risultato, per "gli", non è più il suo significato isolato, ma una miscela pesata secondo queste percentuali: un po' di sé stesso, molto di "Luca", un bel po' di "prestato", pochissimo del resto. Dopo essere passato attraverso questo meccanismo, "gli" porta con sé — mischiate nella sua rappresentazione numerica — tracce forti di "Luca" e di "prestato", e tracce deboli di "Marco". Il bello è che nessuno scrive a mano le regole per decidere chi ascoltare, né tantomeno le percentuali esatte: il modello impara, allenandosi su miliardi di frasi, a costruire domande e cartellini tali che l'ascolto giusto emerga da solo.

### 3.3 Non si può sbirciare il futuro

C'è una regola in più, e ha una logica ferrea: quando il modello sta cercando di indovinare la parola numero 10 di una frase, non può "ascoltare" le parole 11, 12, 13 — perché, semplicemente, al momento di indovinare non esistono ancora. È un po' come leggere un giallo: puoi rileggere quante volte vuoi le pagine già lette, ma non puoi sbirciare l'ultima pagina per scoprire chi è l'assassino mentre sei ancora alla lezione 3 — sarebbe imbrogliare, e soprattutto non ti insegnerebbe a *dedurre* nulla. Per questo, ogni parola può ascoltare solo sé stessa e le parole che la precedono, mai quelle che vengono dopo. Questo vincolo si chiama **mascheramento causale**, e serve anche a un secondo scopo pratico: permette al modello, durante l'addestramento, di esercitarsi a indovinare *ogni* parola di un intero testo contemporaneamente — un'unica lettura che allena simultaneamente la previsione della parola 2, della parola 3, della parola 4, e così via — invece di dover rileggere il testo da capo una volta per ogni parola da indovinare.

### 3.4 Più occhi valgono più di uno

Un solo giro di "domanda e cartellini" per parola sarebbe piuttosto limitante: nella frase di prima, servirebbe capire contemporaneamente *chi fa cosa* (Marco presta), *a chi* (a Luca), e *di cosa si parla* (il libro) — relazioni diverse, che convivono nella stessa frase. La soluzione è far girare **più meccanismi di attenzione in parallelo**, ciascuno con le proprie domande e i propri cartellini, un po' come guardare la stessa scena attraverso più lenti colorate diverse contemporaneamente: una lente evidenzia "chi fa l'azione", un'altra "a chi è diretta", un'altra ancora "di cosa si parla materialmente".

Torniamo all'esempio del trofeo e della valigia per vedere perché serve più di una lente insieme. Da sola, una lente puramente grammaticale ("qual è il soggetto della frase precedente?") non basta a scegliere tra "trofeo" e "valigia": entrambi sono candidati sintatticamente validi. Serve una seconda lente, che confronta il significato dell'aggettivo finale con le proprietà tipiche degli oggetti coinvolti — una valigia, di solito, è quella che *contiene*; un trofeo è quello che *viene contenuto*. È questa seconda lente a far pendere la bilancia: "grande" punta al trofeo, "piccola" punta alla valigia. Nessuna delle due lenti, da sola, risolverebbe sempre il problema — è la combinazione a farlo.

Ognuna di queste "teste" di attenzione, allenandosi, tende a specializzarsi per conto suo su un tipo di relazione — senza che nessuno gliel'abbia detto esplicitamente — e alla fine i risultati di tutte le teste vengono ricomposti insieme in un'unica rappresentazione più ricca.

### 3.5 Uno strato sopra l'altro

Un modello reale non fa questo "guarda e ascolta" una volta sola: lo ripete decine o addirittura centinaia di volte, uno strato sopra l'altro, ciascuno seguito da un breve momento di elaborazione interna della singola parola (di cui non ci occuperemo nel dettaglio qui). Pensa a come rileggi un tuo tema prima di consegnarlo: al primo giro correggi refusi e piccoli errori di concordanza tra parole vicine; a un giro successivo controlli che ogni pronome si riferisca davvero a chi deve; solo all'ultimo giro valuti se l'argomento nel complesso si tiene, e se il tono resta coerente dall'inizio alla fine. Sono passate diverse, ognuna più "larga" della precedente.

Un modello fa qualcosa di simile, strato dopo strato, contemporaneamente su ogni parola del testo: a ogni strato la rappresentazione di ogni parola si arricchisce un altro po'. I primi strati tendono a cogliere relazioni semplici e vicine (che parola precede quale); gli strati più profondi arrivano a catturare relazioni astratte e a lungo raggio — tono del discorso, intenzione, coerenza con qualcosa detto molte frasi prima. Nessuno di questi livelli è programmato a mano: è tutto il prodotto dello stesso identico meccanismo di attenzione, ripetuto e impilato su scala enorme.

### 3.6 Mettiamo tutto insieme

Riavvolgiamo il nastro e seguiamo, dall'inizio alla fine, cosa succede a una sola parola — "gli" — attraverso l'intero processo descritto in questa lezione, così le sezioni precedenti smettono di essere idee separate e diventano un unico meccanismo.

1. **Domanda e cartellini (3.2).** "Gli" si pone una domanda ("chi ha appena ricevuto l'azione?") e la confronta con i cartellini di "Marco", "Luca", "prestato" e delle altre parole già lette, ottenendo una miscela pesata verso "Luca" e "prestato".
2. **Il vincolo del tempo (3.3).** Il confronto riguarda solo le parole *già lette* fino a quel momento: "libro", che arriva dopo, non esiste ancora per "gli" mentre il modello lo elabora.
3. **Più punti di vista insieme (3.4).** Il confronto non avviene una volta sola: più "teste" lo fanno in parallelo, ognuna con la propria domanda — una magari si concentra su "chi è il destinatario", un'altra su "qual è l'azione", una terza su qualcosa a cui noi umani non penseremmo nemmeno, ma che il modello ha scoperto essere utile.
4. **Ripetuto in profondità (3.5).** Tutto questo — non una volta, ma decine o centinaia di volte, strato sopra strato — raffina progressivamente la rappresentazione di "gli", fino a che, all'ultimo strato, il modello ha effettivamente "capito" (nel senso puramente statistico che userà per generare la parola successiva) che quel "gli" punta a Luca.

Nessuno di questi quattro passaggi è stato scritto a mano da un programmatore: sono tutti il prodotto di ciò che il modello ha imparato, macinando testo, su come distribuire l'attenzione in modo utile a indovinare la parola successiva. È proprio questa combinazione — domande e cartellini, uno sguardo solo al passato, più teste in parallelo, ripetuta strato dopo strato — a rendere possibile tutto ciò che vedremo nelle prossime lezioni: come si allena un modello di questo tipo (Lezione 4) e come, alla fine, genera davvero una risposta parola per parola (Lezione 6).

---

> **Prova tu — Disegna le frecce dell'attenzione**
>
> Ecco la frase di apertura della lezione: *"Marco ha detto a Luca che gli avrebbe prestato il libro."*
>
> 1. Scrivi la frase su un foglio, una parola per casella. Concentrati sulla parola "gli". Disegna delle frecce da "gli" verso le altre parole della frase, spesse in proporzione a quanto pensi che "gli" debba "ascoltarle" per essere interpretata correttamente. (Ricorda la regola del mascheramento causale: "gli" può guardare solo sé stessa e le parole *precedenti* — non "libro", che viene dopo.)
> 2. Ora prova con una seconda "testa" di attenzione: invece di chiederti "a chi si riferisce gli", chiediti "qual è l'azione principale della frase" e ridisegna le frecce da "gli" pensando a questa domanda diversa. Le frecce più spesse cambiano?
> 3. Prova con questa frase più ambigua, senza una risposta ovvia: *"Il professore ha restituito il compito allo studente perché era sbagliato."* A cosa si riferisce "era sbagliato" — al compito o al professore che l'ha corretto? Disegna le frecce secondo la tua interpretazione più naturale, poi confrontati con il ragionamento in Appendice A.
>
> Non esiste un'unica risposta "corretta" al 100% per il punto 3 — è proprio per questo che è un buon esempio: anche i modelli reali, di fronte a frasi ambigue, a volte "sbagliano" a distribuire l'attenzione, esattamente come farebbe un lettore umano distratto.

---

*Continua con la [Lezione 04 — Come si allena un gigante]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-04-come-si-allena-un-gigante.md %}.html)*
