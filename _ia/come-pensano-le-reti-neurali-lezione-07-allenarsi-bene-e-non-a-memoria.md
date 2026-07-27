---
title: 'Lezione 07 — Allenarsi bene (e non a memoria)'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 7.1 Non basta sapere la direzione, conta anche il passo

La Lezione 6 ha mostrato come far risalire la colpa lungo la catena e capire *in che direzione* correggere ogni peso. Ma sapere la direzione giusta non basta: bisogna anche decidere *quanto* muoversi in quella direzione a ogni correzione, e con quale strategia. Questa sezione e le due successive raccontano tre accorgimenti pratici che rendono l'allenamento molto più efficace di una correzione "ingenua" — un piccolo passo alla volta, sempre della stessa dimensione, senza mai guardare la propria storia recente.

### 7.2 Momentum: uno skateboard che prende velocità in discesa

Immagina di correggerti sempre da capo, senza mai ricordare in che direzione ti sei mosso l'ultima volta: se il tuo errore ti ha spinto ripetutamente nella stessa direzione negli ultimi dieci aggiustamenti, ignorare quella storia è uno spreco — è ragionevole aspettarsi che continuerà a spingerti da quella parte anche stavolta. Il **momentum** introduce esattamente questa memoria: invece di correggersi in modo indipendente ogni volta, la rete accumula una specie di "velocità" nella direzione in cui si è mossa consistentemente di recente, un po' come uno skateboard che rotola su un pendio — più il pendio continua nella stessa direzione, più prende velocità, invece di dover ripartire da fermo a ogni piccolo tratto. Se invece la direzione oscilla avanti e indietro (una valle stretta, in cui la correzione rimbalza da un lato all'altro), la storia recente tende a cancellare quelle oscillazioni contrastanti invece di amplificarle, smorzando il rimbalzo.

### 7.3 Adam: un ritmo diverso per ogni impostazione

Il momentum accelera tutta la rete allo stesso modo. Un raffinamento ulteriore, chiamato **Adam**, osserva che pesi diversi meritano ritmi di correzione diversi: alcuni pesi ricevono storicamente segnali di errore grandi e ballerini, altri segnali piccoli e stabili. Adam tiene traccia, per ciascun peso separatamente, di quanto "agitato" sia stato il suo segnale di correzione nel tempo, e rallenta automaticamente i pesi che hanno ricevuto segnali storicamente grandi (per non farli oscillare troppo), mentre lascia più libertà a quelli con segnali storicamente piccoli. È come un allenatore che non applica lo stesso programma a tutti gli atleti della squadra, ma calibra l'intensità individualmente su ciascuno in base a come ha risposto agli allenamenti recenti. Questa capacità di adattarsi peso per peso rende Adam robusto anche quando la rete ha impostazioni di scale molto diverse fra loro, ed è oggi la scelta predefinita per allenare quasi ogni rete di grandi dimensioni.

### 7.4 Da dove si parte conta

Un dettaglio facile da sottovalutare: prima ancora di cominciare a correggersi, i pesi iniziali di una rete non possono essere scelti a caso in un modo qualunque. Impostarli tutti esattamente a zero è un errore grave e sottile: ogni controllore dello stesso piano riceverebbe esattamente lo stesso segnale di correzione e resterebbe identico a tutti gli altri per l'intero allenamento — come una squadra di gemelli identici che, ricevendo sempre le stesse istruzioni, non si differenziano mai l'uno dall'altro, vanificando il vantaggio di averne più di uno. La soluzione è partire da piccoli valori casuali — abbastanza diversi da rompere questa simmetria, ma non così grandi da mandare la rete in tilt fin dal primo passo (un rischio concettualmente imparentato con il bigliettino che si sporca della Lezione 4). Esistono schemi di inizializzazione calibrati con cura — pensati per far sì che i segnali non si gonfino né si sgonfino sistematicamente attraversando molti piani — ma il principio di fondo resta questo: piccola casualità iniziale, mai zero assoluto.

### 7.5 Partire piano, poi rallentare verso la fine

Anche la dimensione del passo di correzione conta, e tipicamente non resta costante per tutto l'allenamento. Uno schema comune fa crescere gradualmente il passo all'inizio (partendo da correzioni molto piccole, quando la rete è ancora vicina alla sua inizializzazione casuale e ogni segnale di errore è particolarmente rumoroso) e poi lo fa ridurre progressivamente verso la fine, con correzioni via via più piccole e precise — un po' come parcheggiare un'auto: ci si avvicina velocemente allo spazio libero, ma gli ultimi centimetri richiedono aggiustamenti piccoli e delicati, non un'unica sterzata brusca.

### 7.6 L'obiettivo vero non è il compito di ieri

Tutti gli accorgimenti visti finora aiutano a ridurre l'errore sugli esempi già visti durante l'allenamento. Ma minimizzare quell'errore non è, di per sé, l'obiettivo finale — è un po' come uno studente che si esercita solo sulle domande di un vecchio compito in classe già corretto: sapere tutte le risposte di *quel* compito specifico non garantisce di saper rispondere a un compito nuovo, con domande simili ma non identiche. L'obiettivo reale è la **generalizzazione**: funzionare bene su casi mai visti prima. Per misurarla, si tiene volutamente da parte un insieme di esempi — l'**insieme di validazione** — mai usato per correggere i pesi, su cui si controlla periodicamente come va la rete durante l'allenamento, proprio come un compito a sorpresa che lo studente non ha potuto preparare in anticipo.

Il segnale diagnostico classico è un confronto fra due curve nel tempo: l'errore sugli esempi di allenamento scende sempre, in modo quasi monotono — lo studente impara sempre meglio le domande che ha già visto. L'errore sull'insieme di validazione, invece, scende per un po' insieme al primo, ma a un certo punto smette di migliorare e comincia a **risalire** — segno che la rete ha smesso di imparare la regola generale e ha iniziato a memorizzare le stranezze specifiche degli esempi di allenamento, un fenomeno chiamato **overfitting**. Il caso opposto, **underfitting**, è uno studente che non ha capito nemmeno il compito già visto: entrambe le curve restano alte, senza che nessuna delle due scenda davvero.

### 7.7 Studiare capendo, non a memoria

Diverse tecniche pratiche aiutano a evitare l'overfitting, ciascuna con una propria logica intuitiva. Una prima famiglia penalizza esplicitamente i pesi troppo grandi, aggiungendo all'errore una specie di "multa" proporzionale alla loro grandezza (chiamata **regolarizzazione**): pesi grandi tendono a produrre risposte che cambiano bruscamente per piccole variazioni dell'input — utile per adattarsi al rumore specifico di singoli esempi, ma dannoso per catturare la tendenza generale — mentre pesi più contenuti producono risposte più "morbide", meno capaci di rincorrere il rumore.

Una seconda tecnica, chiamata **dropout**, è particolarmente ingegnosa: durante ogni passo di allenamento, alcuni controllori vengono temporaneamente spenti a caso, come se una parte della squadra fosse assente a ogni sessione di allenamento. L'effetto è che nessun controllore può fare eccessivo affidamento sulla presenza costante di altri specifici compagni per correggere i propri errori sistematici — ognuno è costretto a essere utile anche da solo, un po' come uno studio di gruppo in cui, a turno, qualcuno manca: alla lunga, tutti imparano a cavarsela anche senza gli altri.

Infine, due tecniche più dirette: l'**early stopping** monitora la curva di validazione della Sezione 7.6 e ferma l'allenamento — conservando i pesi del punto migliore osservato — non appena quella curva inizia sistematicamente a risalire, invece di proseguire fino a un numero di sessioni prestabilito a tavolino. La **data augmentation** attacca il problema da un'altra direzione, aumentando artificialmente la varietà degli esempi di allenamento con piccole trasformazioni che non ne cambiano il significato (una foto leggermente ruotata resta la foto dello stesso gatto) — rendendo più difficile per la rete memorizzare esempi specifici, perché, in un certo senso, non vede mai esattamente lo stesso identico esempio due volte.

---

> **Prova tu — Chi ha capito e chi ha mandato a memoria**
>
> Due studenti si preparano per lo stesso compito in classe di matematica usando lo stesso libro di esercizi svolti.
>
> - **Studente Rossi** rifà gli esercizi del libro finché non li sa risolvere a menadito, uno per uno.
> - **Studente Bianchi** si esercita su metà degli esercizi del libro, e usa l'altra metà (mai guardata prima) solo per controllare, di tanto in tanto, se sta davvero capendo il metodo.
>
> Il giorno del compito in classe, le domande sono nuove — nessuna identica a quelle del libro, ma dello stesso tipo.
>
> 1. Chi dei due, secondo te, sta seguendo una procedura più simile a "tenere un insieme di validazione" (Sezione 7.6)? Perché?
> 2. Se Rossi, interrogato sugli esercizi *del libro*, li risolve perfettamente, ma sul compito in classe (domande nuove) va male — a quale delle due curve della Sezione 7.6 assomiglia il suo andamento: quella di allenamento, quella di validazione, o entrambe?
> 3. Immagina che Bianchi, controllando periodicamente il proprio andamento sulla metà di esercizi mai vista, noti che da un certo punto in poi il suo punteggio su quella metà smette di migliorare e comincia a peggiorare, pur continuando a migliorare su quelli già visti. Quale delle tecniche della Sezione 7.7 starebbe imitando, se decidesse di fermare lì lo studio invece di continuare a ripassare?

---

*Continua con la [Lezione 08 — Perché serviva un'idea completamente nuova]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-08-perche-serviva-unidea-nuova.md %}.html)*
