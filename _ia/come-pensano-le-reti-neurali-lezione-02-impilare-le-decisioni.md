---
title: 'Lezione 02 — Impilare le decisioni: la rete a più piani'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 2.1 Un solo controllore non basta

La Lezione 1 ha lasciato un problema in sospeso: le quattro palline ai quattro angoli del quadrato, rosse in diagonale e blu in diagonale, che nessuna linea retta riesce a separare. Un solo interruttore, per quanto ben allenato, non ce la fa — è un limite geometrico, non una questione di allenamento migliore. Ma Minsky e Papert stessi, nel bel mezzo di un libro che elencava limiti, avevano lasciato cadere un suggerimento: e se, invece di un solo controllore, ne mettessimo più di uno in fila?

### 2.2 L'idea: più controllori, poi uno che li riassume

Immagina di sostituire il singolo interruttore-ombrello con due controllori intermedi, ciascuno che guarda gli stessi indizi ma con una propria regola, seguiti da un terzo controllore finale che guarda *le decisioni dei primi due* — non più gli indizi originali — per decidere l'output finale. Ogni controllore intermedio può tracciare la propria linea retta di separazione; il controllore finale, combinando due decisioni già prese, produce un confine complessivo che può piegarsi, spezzarsi, incurvarsi — non più vincolato a essere una singola linea dritta.

Vediamolo concretamente sul problema delle quattro palline. Chiamiamo i due indizi X e Y, ciascuno acceso o spento (1 o 0): pallina rossa in (0,0) e (1,1), pallina blu in (0,1) e (1,0) — esattamente lo schema XOR della Lezione 1. Costruiamo due controllori intermedi:

- **Controllore A**: si accende se *almeno uno* dei due indizi è acceso (una regola "OR").
- **Controllore B**: si accende se *non sono entrambi* accesi insieme (una regola "non-AND", cioè NAND).

E un controllore finale che guarda A e B:

- **Controllore finale**: dice "rosso" (output 1) se *sia A sia B* sono accesi insieme (una regola "AND").

Verifica tu stesso, punto per punto, che questa combinazione produce esattamente lo schema che cercavamo — lo faremo insieme nel "Prova tu" di questa lezione. Il punto essenziale: nessuno dei tre controllori, da solo, potrebbe risolvere l'XOR — ciascuno traccia solo una linea dritta — ma la loro combinazione a due piani sì.

### 2.3 Il piano intermedio ha un nome

Questa struttura a due piani ha un nome preciso: i due controllori di mezzo (A e B) formano uno **strato nascosto** — "nascosto" perché non osserviamo direttamente le loro decisioni, sono un passaggio intermedio che la rete usa internamente prima di arrivare all'output finale. Una rete costruita così — uno o più strati nascosti fra l'input e l'output, in cui ogni strato guarda solo l'output dello strato precedente — si chiama **rete feedforward multistrato** (in inglese, storicamente, *multi-layer perceptron*, anche se i controllori moderni usano le manopole sfumate della Lezione 1, non più l'interruttore a scatto netto del percettrone originale). "Feedforward" perché l'informazione scorre in una sola direzione, dall'input verso l'output, senza mai tornare indietro — una proprietà su cui torneremo nella Lezione 4, quando incontreremo un'architettura che invece un pezzo di informazione se lo tiene e lo riusa.

### 2.4 Quanto in profondità, quanto in larghezza

Due numeri descrivono un edificio come questo: quanti **piani** (strati) ha, e quanto è **largo** ogni piano (quanti controllori contiene). Una rete con un solo piano è semplicemente il singolo interruttore della Lezione 1; una rete "profonda" — da cui il termine "*deep learning*" — ne ha tipicamente più di due o tre. Non c'è una soglia precisa oltre cui una rete "diventa" profonda: è più una questione di convenzione storica, per contrasto con i modelli a un solo piano che l'hanno preceduta, che una linea netta da tracciare.

### 2.5 Perché aggiungere piani è spesso più efficiente che allargarli

Il risultato matematico citato alla fine della Lezione 1 garantisce che un solo piano intermedio, se abbastanza largo, basta in teoria per rappresentare quasi qualunque relazione fra input e output. In pratica, però, aggiungere piani è spesso più conveniente che allargare solo il primo: ogni piano aggiuntivo può riusare ciò che il piano precedente ha già "capito", componendo elementi semplici in elementi via via più complessi — invece di dover ricostruire tutto da zero con un solo strato enorme. Vedremo questa idea diventare molto più concreta e visiva nella Lezione 3, con reti che imparano a riconoscere prima bordi, poi forme, poi oggetti interi, un piano alla volta.

### 2.6 L'altra faccia della medaglia: quanto è grande non significa quanto è bravo

Più piani e più controllori per piano significano più impostazioni regolabili — la **capacità** della rete, cioè quanto complicate sono le relazioni che può, in linea di principio, rappresentare. Ma una capacità molto più grande di quanto il problema richieda ha un rischio concreto: la rete può arrivare a *memorizzare* le particolarità specifiche degli esempi su cui si è allenata, invece di imparare la regola generale sottostante — un fenomeno chiamato **overfitting**, che la Lezione 7 tratterà con i suoi segnali di riconoscimento e i suoi rimedi. Per ora basta tenere a mente che scegliere quanto grande costruire una rete non è mai una questione di "più piani sono sempre meglio": è un compromesso fra potere rappresentativo e capacità di funzionare bene anche su casi mai visti prima.

---

> **Prova tu — Risolvi l'XOR con due controllori**
>
> Verifica a mano che la combinazione della Sezione 2.2 funzioni davvero. Per ognuna delle quattro combinazioni di X e Y, calcola: **A** (acceso se almeno uno tra X, Y è acceso), **B** (acceso se non sono entrambi accesi), e il **controllore finale** (acceso se sia A sia B sono accesi). Confronta il risultato con la tabella delle palline della Lezione 1: rosso ("finale" acceso) quando X e Y sono *diversi*, blu ("finale" spento) quando sono *uguali*.
>
> | X | Y | A (OR) | B (NAND) | Finale (A AND B) | Che colore dovrebbe essere? |
> |---|---|---|---|---|---|
> | 0 | 0 | ? | ? | ? | blu |
> | 0 | 1 | ? | ? | ? | rosso |
> | 1 | 0 | ? | ? | ? | rosso |
> | 1 | 1 | ? | ? | ? | blu |
>
> Riempi le tre colonne con i "?" per tutte e quattro le righe. Il controllore finale riproduce esattamente la colonna dei colori attesi? Se una riga non torna, rileggi la definizione di A o B nella Sezione 2.2 — è un buon modo per sentire, con le tue mani, perché serva *proprio* questa coppia di regole intermedie e non un'altra.

---

*Continua con la [Lezione 03 — Uno stencil che scorre sull'immagine]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-03-uno-stencil-che-scorre-sullimmagine.md %}.html)*
