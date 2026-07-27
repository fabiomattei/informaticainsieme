---
title: 'Appendice A — Soluzioni ai giochi'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

Le soluzioni qui sotto sono organizzate per lezione. Per molti giochi non esiste un'unica risposta "corretta" — il valore dell'esercizio sta nel ragionamento, non nel risultato. Leggile solo dopo aver provato tu stesso: sbirciare prima toglie gran parte del divertimento.

### Soluzione — Lezione 1: il modello a bigrammi improvvisato

- **Dopo "Il":** le quattro frasi si dividono esattamente a metà — "cane" compare 2 volte (frasi 1 e 2), "gatto" compare 2 volte (frasi 3 e 4). Un vero modello a bigrammi, di fronte a questo pareggio perfetto, non avrebbe alcun modo di scegliere con sicurezza: dovrebbe tirare a sorte, 50 e 50.
- **Dopo "cane":** stessa storia in piccolo — "corre" (frase 1) e "dorme" (frase 2) sono ugualmente probabili. Guardando solo "cane", non c'è modo di sapere se il cane sta correndo o dormendo: serve sapere *quale* frase stiamo completando, un'informazione che il modello a bigrammi ha già dimenticato.
- **Dopo "dorme":** qui c'è una sorpresa. "dorme" è seguito da "sul" **in entrambe** le frasi 2 e 3 — quindi, curiosamente, il bigramma "dorme → sul" è perfettamente prevedibile al 100%! Il problema arriva un passo dopo: dopo "sul", le frasi divergono di nuovo tra "divano" (cane) e "tappeto" (gatto), 50 e 50. Il punto da portarti a casa: anche quando un pezzetto di previsione sembra facile, l'ambiguità vera si nasconde spesso un passo più in là — ed è recuperabile solo sapendo *l'intero contesto* (di chi stiamo parlando), non le ultime una o due parole. Questo è esattamente il limite che l'attenzione, vista dalla Lezione 3 in poi, è nata per superare.

### Soluzione — Lezione 2: il cruciverba dei vettori

- **Regina:** partendo da "re" = (8, 6) e applicando lo stesso spostamento visto tra "uomo" (2,6) e "donna" (2,2) — cioè stessa coordinata orizzontale, coordinata verticale −4 — si ottiene **regina = (8, 2)**.
- **Principessa:** partendo da "principe" = (7, 7) e applicando lo stesso spostamento (−4 in verticale, orizzontale invariato), si ottiene **principessa = (7, 3)** — coerente con lo spostamento usato sopra.
- **Bonus:** guardando solo le coordinate orizzontali — uomo (2), donna (2), ragazzo (1), ragazza (1) da un lato; re (8), principe (7) dall'altro — si nota che l'asse orizzontale non separa affatto per genere (uomo e donna hanno lo stesso valore!): separa invece **persone comuni** (valori bassi, 1-2) da **persone di sangue reale** (valori alti, 7-8). Il genere, in questa mappa giocattolo, è catturato invece dall'asse verticale (valori più alti per uomo/ragazzo/re/principe, più bassi per le controparti femminili). Una mappa reale ha centinaia di assi come questi, quasi mai etichettabili con un singolo concetto pulito come "genere" o "regalità" — ma il principio di fondo, direzioni diverse per concetti diversi, è esattamente questo.

### Soluzione — Lezione 3: le frecce dell'attenzione

- **Punto 1:** la lettura più naturale rende "gli" fortemente collegato a "Luca" (è lui il destinatario del prestito) e, con peso minore, a "Marco" e a "prestato" (chi fa l'azione, e quale azione). Nessuna freccia dovrebbe puntare a "libro", perché — mascheramento causale — "gli" viene prima di "libro" nella frase e non può "vederlo".
- **Punto 2:** cambiando domanda ("qual è l'azione principale"), ci si aspetta che le frecce più spesse si spostino verso i verbi "detto" e "prestato", riducendo il peso su "Luca" e "Marco" come singole entità. Questo è esattamente il senso della multi-head attention: teste diverse, stessa frase, pesi diversi.
- **Punto 3:** nella frase "il professore ha restituito il compito allo studente perché era sbagliato", la lettura più naturale per la maggior parte dei lettori italiani collega "era sbagliato" a **"il compito"** (ha senso restituire qualcosa perché è sbagliato) piuttosto che a "il professore". Non è però un caso impossibile per l'altra lettura — è proprio questa ambiguità residua, presente anche nei modelli reali, il motivo per cui frasi di questo tipo (chiamate frasi di Winograd) sono usate come test per misurare quanto bene un modello risolve i riferimenti ambigui.

### Soluzione — Lezione 4: l'esperimento dei rendimenti decrescenti

Non c'è un numero "giusto" da confrontare — dipende dalla persona e dalla sequenza. Quello che quasi tutti osservano, però, è la **forma** della curva: il salto da 1 a 3 letture (R1 → R3) è quasi sempre molto più grande del salto da 3 a 6 letture (R3 → R6), spesso di parecchio. Le prime ripetizioni "fissano" l'ordine generale e i simboli più memorabili; le ripetizioni successive faticano sempre di più a correggere gli ultimi due o tre simboli che continuano a confondersi. Questa è, in miniatura, esattamente la stessa curva a rendimenti decrescenti discussa per l'addestramento di un LLM: i primi passaggi di lettura del testo insegnano moltissimo, i passaggi successivi (ripetuti su testo già "visto" concettualmente) insegnano sempre meno per unità di sforzo.

### Soluzione — Lezione 5: fai l'annotatore per un giorno

- **Domanda 1 (moltiplicazione):** entrambe le risposte sono numericamente corrette (127 × 8 = 1016). La maggior parte dei team di annotazione, per domande puramente computazionali, preferisce risposte concise come A: la premessa di B ("Bella domanda!... possono essere complicate...") non aggiunge informazione utile. È un esempio diretto del rischio discusso nella Lezione 8: se gli annotatori premiassero sistematicamente la verbosità di B anche quando non serve, il modello imparerebbe ad "allungare il brodo" anche dove non aiuta.
- **Domanda 2 (scoperta dell'America):** qui la risposta A è generalmente preferita nelle linee guida moderne di annotazione, perché più accurata e onesta: menziona la presenza preesistente di popolazioni indigene e i contatti vichinghi precedenti, invece di presentare una semplificazione da scuola elementare come fatto assoluto e non discusso. B non è "falsa", ma è incompleta in un modo che può rinforzare un'idea storicamente imprecisa.
- **Domanda 3 (scassinare una serratura):** B è la risposta corretta da premiare, anche se la richiesta sembra legittima (persona chiusa fuori casa propria): il modello non ha modo di verificare se chi scrive stia davvero descrivendo la propria situazione, e le stesse istruzioni per scassinare funzionerebbero identiche per un uso illegittimo. B offre comunque un'alternativa costruttiva (fabbro, amministratore) invece di un rifiuto secco — un buon equilibrio tra sicurezza e utilità, il tema della Lezione 8.

### Soluzione — Lezione 6: l'urna a due temperature

- **Bassa temperatura** (esempio nel testo): su 100 palline, 85 "albero", 12 "tetto", 2 "ramo", 1 "frigorifero".
- **Alta temperatura**: su 100 palline, circa 25 per ciascuna delle quattro parole (25-25-25-25).
- **In quale urna è più probabile pescare "frigorifero"?** Nell'urna ad **alta temperatura** (25% contro 1%) — resta comunque una tra quattro possibilità, non garantita, ma enormemente più probabile che nell'urna a bassa temperatura.
- **In quale urna, pescando 5 volte di fila, ti aspetti quasi sempre la stessa parola?** Nell'urna a **bassa temperatura**: con "albero" all'85%, è molto probabile pescarlo 4 o 5 volte su 5. Nell'urna ad alta temperatura, con le proporzioni quasi identiche, ti aspetti invece una sequenza molto più varia — magari "albero, ramo, tetto, frigorifero, albero" — proprio il comportamento "più creativo ma meno prevedibile" descritto nella lezione.

### Soluzione — Lezione 7: il quiz-trappola

1. **"Qual è la capitale della Francia?"** — sospetta: è tra le domande di cultura generale più comuni al mondo, presente innumerevoli volte in qualsiasi raccolta di testo.
2. **Il problema del fruttivendolo di Cuneo** — genuina: la combinazione specifica di numeri (47 casse, 23 mele, 0,34 €, tre quarti, 22%) è così particolare che è estremamente improbabile trovarla già risolta altrove parola per parola, anche se il *metodo* per risolverla (moltiplicazioni e percentuali) è comunissimo.
3. **"Quanto fa 2+2?"** — sospetta, ovviamente: probabilmente la domanda matematica più ripetuta nella storia dei testi scritti.
4. **Il romanzo inventato "Il sentiero di vetro spezzato"** — genuina per definizione: titolo e autore sono stati inventati apposta per questo esercizio, quindi nessun modello addestrato prima della pubblicazione di questo libro può averli visti. Nota curiosa: se in futuro questo stesso libro finisse tra i testi di addestramento di un nuovo modello, questa domanda diventerebbe automaticamente "contaminata" per quel modello — un promemoria di quanto sia scivoloso, in pratica, garantire che un test resti sempre genuino nel tempo.

### Soluzione — Lezione 8: fai il fact-checker

1. **Torre Eiffel come struttura temporanea (1889)** — **vera**. Fu progettata per durare 20 anni e doveva essere smontata; fu salvata perché si scoprì utile come antenna radiotelegrafica.
2. **"I Promessi Sposi" pubblicato nel 1712"** — **inventata**. Alessandro Manzoni nacque nel 1785: una pubblicazione nel 1712 precederebbe la sua nascita di 73 anni, un segnale d'allarme evidente una volta controllato. La prima edizione vera è del 1827 (edizione definitiva 1840-42).
3. **Acqua che bolle a temperatura più bassa in montagna** — **vera**. La pressione atmosferica più bassa in quota abbassa il punto di ebollizione dell'acqua.
4. **Einstein bocciato in matematica** — **inventata**: è un mito molto diffuso (probabilmente un'allucinazione "umana" ancora prima che uno bot potesse inventarla). In realtà Einstein eccelleva in matematica fin da giovanissimo; il mito nasce forse da una confusione sui sistemi di voto svizzeri, dove una scala invertita rispetto a quella tedesca ha fatto sembrare bassi dei voti che in realtà erano alti.
5. **Python dal nome del serpente per "flessibilità"** — **inventata**. Il creatore, Guido van Rossum, scelse il nome in onore del gruppo comico britannico Monty Python (in particolare "Monty Python's Flying Circus"), non del serpente — un dettaglio spesso frainteso proprio perché il logo del linguaggio raffigura effettivamente due serpenti stilizzati.

Se ti sei fatto ingannare da qualcuna di queste — specialmente la 2, la 4 o la 5 — hai appena sperimentato di persona quanto sia facile scambiare "plausibile e scritto con sicurezza" per "vero", esattamente il meccanismo alla base delle allucinazioni discusse nella lezione.

### Soluzione — Lezione 9: quale strumento chiameresti?

1. **Capitale del Giappone** → (a) rispondere a memoria: fatto stabile, ben noto, non cambia nel tempo.
2. **Cambio dollaro-euro oggi** → (b) cercare informazioni aggiornate: il valore cambia di minuto in minuto, un modello allenato anche solo un mese fa avrebbe un numero ormai sbagliato.
3. **Moltiplicazione con decimali** → (c) usare una calcolatrice/interprete di codice: calcoli precisi su numeri scomodi sono un punto debole noto per un modello che "indovina" invece di eseguire aritmetica esatta.
4. **Elezioni recenti in un paese** → (b) cercare informazioni aggiornate: è un evento successivo, quasi per definizione, alla fine dell'addestramento del modello.
5. **Foto di una pianta che appassisce** → (d) guardare l'immagine: senza vederla, qualunque risposta sarebbe una congettura generica, non un'osservazione reale del problema specifico.

Il filo conduttore delle domande 2, 3, 4 e 5: rispondere "a memoria" sarebbe non tanto sbagliato quanto **strutturalmente impossibile da fare bene** — non per un limite di "intelligenza" del modello, ma perché l'informazione richiesta o non esisteva ancora al momento dell'addestramento, o richiede una precisione che il meccanismo di previsione della parola successiva non è progettato per garantire da solo.

### Soluzione — Lezione 10: le tue previsioni

Non essendoci ancora una risposta definitiva possibile, ecco solo un riferimento con cui confrontarti: al momento della stesura di questo libro, la view prevalente tra i ricercatori del settore era più o meno questa — il meccanismo di base ("indovinare il prossimo pezzo di testo", Lezione 1) resterà probabilmente il nucleo per ancora diversi anni, ma sempre più affiancato da un affinamento successivo pesante orientato al ragionamento esplicito (Lezione 7); le allucinazioni sono viste da molti come attenuabili ma difficilmente eliminabili del tutto, perché nascono dal meccanismo stesso e non da un singolo errore correggibile; e l'efficienza (Sezione 10.1-10.4) sta effettivamente rendendo sempre più comuni modelli capaci direttamente su dispositivo, in parallelo — non al posto — dei modelli più grandi ospitati nei data center. Rileggi le tue risposte tra qualche anno: è probabile che almeno una delle tre si sia rivelata sbagliata, e va benissimo così — è il modo in cui funziona la scienza che si muove in fretta.

---

*Continua con l'[Appendice B — Glossario]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-appendice-b-glossario.md %}.html)*
