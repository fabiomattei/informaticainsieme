---
title: 'Lezione 04 — Come si allena un gigante'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 4.1 Il gioco del testo bucherellato

Immagina un esercizio scolastico enorme: ti danno milioni di pagine di testo — libri, articoli, siti web, forum — ma con parole a caso cancellate e sostituite da uno spazio vuoto. Il tuo compito, per ogni spazio vuoto, è indovinare quale parola mancava. Sbagli, ti viene detto qual era la parola giusta, correggi leggermente il tuo modo di ragionare, e passi al buco successivo. Ripeti questo per miliardi di buchi.

Questo, in sostanza, è il **pre-training**: la fase in cui un LLM viene "allenato" da zero. Non è tanto diverso, concettualmente, da come impari tu stesso il significato di una parola nuova leggendo tanti libri: nessuno ti dà mai una definizione perfetta di "sarcasmo", ma dopo averlo visto usato in cento contesti diversi, inizi a riconoscerlo. Il modello fa lo stesso, milioni di volte più in fretta e su una quantità di testo che nessun umano potrebbe mai leggere in una vita.

### 4.2 Cosa cambia davvero, dentro, quando "impara"

Dentro un modello ci sono miliardi di piccole "manopole" numeriche (i tecnici le chiamano **parametri** o **pesi**) — sono loro a determinare, tra le altre cose, dove va a finire ogni parola sulla mappa dei significati della Lezione 2 e a chi presta attenzione ogni parola nel meccanismo della Lezione 3. All'inizio, prima di qualunque addestramento, queste manopole sono impostate più o meno a caso: il modello, a quel punto, produce solo rumore senza senso.

Ogni volta che il modello sbaglia a indovinare una parola cancellata, un procedimento matematico (che qui non serve dettagliare) calcola *in che direzione* girare ciascuna delle miliardi di manopole per sbagliare un pochino di meno la prossima volta. Un pochino, non tutto in un colpo: un aggiustamento troppo brusco rischierebbe di "disimparare" cose già imparate bene. Ripetuto miliardi di volte, su miliardi di parole di testo diverso, questo lento girare di manopole è tutto quello che serve perché, gradualmente, dal rumore iniziale emerga un modello capace di scrivere frasi sensate.

### 4.3 Quanto testo serve, quanto "cervello" serve

Una domanda naturale: se leggere più testo aiuta, perché non allenare il modello più grande possibile sul testo più grande possibile, punto e basta? Il problema è che le due cose vanno tenute in equilibrio, un po' come una ricetta di cucina: troppa farina e poco lievito, o troppo lievito e poca farina, e il dolce non lievita bene comunque. Un modello enorme (tantissime manopole) allenato su troppo poco testo tende a **memorizzare** invece di generalizzare — un po' come uno studente che impara a memoria le risposte di un esercizio specifico invece di capire il metodo, e poi va in crisi appena cambia un dettaglio dell'esercizio. Un modello piccolo (poche manopole), invece, per quanto testo gli dai da leggere, ha semplicemente troppo poca "capacità" per catturare tutta la ricchezza del linguaggio: è come chiedere a un'agendina tascabile di contenere un'enciclopedia.

I ricercatori hanno scoperto, studiando empiricamente centinaia di modelli di dimensioni diverse allenati su quantità diverse di testo, che esiste più o meno un **rapporto giusto** tra quanto è grande il modello e quanto testo conviene dargli da leggere per usare al meglio sia l'uno sia l'altro — chiamano queste osservazioni **leggi di scala**. Non è una legge fisica immutabile, è più simile a una regola pratica solida: raddoppiare solo la dimensione del modello, senza dargli anche più testo da leggere, spreca gran parte del potenziale in più; e viceversa.

### 4.4 Rendimenti decrescenti: perché non basta "più tempo"

C'è un ultimo ingrediente che tocca ogni forma di apprendimento, umano o artificiale: i **rendimenti decrescenti**. La prima volta che studi un argomento nuovo, ogni minuto di studio in più ti fa capire moltissimo. La decima volta che ripassi lo stesso argomento, ormai saputo bene, ogni minuto in più aggiunge sempre meno. La curva del miglioramento non è una retta che sale dritta all'infinito: sale ripida all'inizio e via via si appiattisce.

Lo stesso succede a un LLM durante il pre-training: le prime ore di addestramento (in termini relativi) portano miglioramenti enormi — passa dal produrre rumore a produrre frasi grammaticalmente corrette. Continuando ad allenarlo, i miglioramenti ci sono ancora, ma sempre più piccoli e sempre più costosi da ottenere: serve moltissimo testo in più, e moltissima potenza di calcolo in più, per spremere l'ultimo, sottile miglioramento. Questo è anche uno dei motivi per cui allenare i modelli più grandi in circolazione costa cifre enormi: non perché il primo passo sia costoso, ma perché rincorrere gli ultimi miglioramenti sulla curva ormai piatta lo è.

---

> **Prova tu — L'esperimento dei rendimenti decrescenti**
>
> Ti serve solo carta, penna e un orologio (o il timer del telefono).
>
> 1. Scrivi questa sequenza di 12 simboli senza significato, in un ordine qualunque ma fisso: △ ○ □ ☆ ◇ ▽ ✕ ◎ ▲ ● ■ ✚
> 2. Guardala per **10 secondi**, copri il foglio, e scrivi a memoria quanti simboli (nell'ordine giusto) ricordi. Segna il numero: chiamalo *R1*.
> 3. Guarda di nuovo la sequenza per altri 10 secondi (quindi l'hai vista **3 volte in totale**, contando eventuali ripassi), copri, riscrivi a memoria. Segna *R3*.
> 4. Ripeti altre tre volte (**6 volte in totale**), poi riscrivi a memoria un'ultima volta. Segna *R6*.
> 5. Disegna su un foglio una curva con in orizzontale il numero di letture (1, 3, 6) e in verticale i simboli corretti ricordati (R1, R3, R6).
>
> La domanda del gioco: il salto da R1 a R3 è più grande, uguale, o più piccolo del salto da R3 a R6? Nella stragrande maggioranza delle persone che provano questo esercizio, il primo salto è nettamente più grande del secondo — la stessa identica forma di curva che si osserva allenando un LLM sempre più a lungo. Confronta la tua curva con la discussione in Appendice A.

---

*Continua con la [Lezione 05 — Insegnargli le buone maniere]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-05-insegnargli-le-buone-maniere.md %}.html)*
