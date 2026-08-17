---
title: 'Appendice A — Soluzioni ai giochi'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un elenco di spunte, una per ogni lezione con soluzione risolta](/images/ia/come-pensano-le-reti-neurali-appendice-a-soluzioni/come-pensano-le-reti-neurali-appendice-a-soluzioni.svg){:class="aside-image"}

Le soluzioni qui sotto sono organizzate per lezione. Per alcuni giochi non esiste un'unica risposta "corretta" — il valore dell'esercizio sta nel ragionamento, non nel risultato. Leggile solo dopo aver provato tu stesso: sbirciare prima toglie gran parte del divertimento.

### Soluzione — Lezione 1: correggi l'interruttore dell'ombrello

1. Punteggio di domani: 2×3 + 1×8 = 6+8 = 14. Essendo 14 minore di 15, l'interruttore dice **no ombrello**.
2. Domani pioverà davvero, quindi l'interruttore ha sbagliato (ha detto "no" quando la risposta giusta era "sì"). Regola di correzione: aumenta di 1 il peso di ogni indizio che oggi valeva più di 5. C = 3 non supera 5 (resta invariato), U = 8 sì (il suo peso passa da 1 a 2). Nuovi pesi: peso di C resta 2, peso di U diventa 2.
3. Punteggio ricalcolato: 2×3 + 2×8 = 6+16 = 22, maggiore o uguale a 15. Ora l'interruttore dice **sì ombrello** — la correzione ha funzionato, esattamente come nell'esempio del percettrone della Sezione 1.2: un solo aggiustamento, nella direzione giusta, ha corretto l'errore su questo caso specifico.

### Soluzione — Lezione 2: risolvi l'XOR con due controllori

| X | Y | A (OR) | B (NAND) | Finale (A AND B) | Atteso |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 1 | 0 | blu |
| 0 | 1 | 1 | 1 | 1 | rosso |
| 1 | 0 | 1 | 1 | 1 | rosso |
| 1 | 1 | 1 | 0 | 0 | blu |

Tutte e quattro le righe tornano: il controllore finale riproduce esattamente lo schema delle palline della Lezione 1 (rosso quando X e Y sono diversi, blu quando sono uguali), nonostante ciascuno dei tre controllori, preso da solo, sappia tracciare solo una linea dritta. È la combinazione a due piani a produrre il confine "piegato" che un solo controllore non può disegnare.

### Soluzione — Lezione 3: fai scorrere lo stencil

1. Posizione in alto a sinistra (righe 1-3, colonne 1-3): ogni riga dell'immagine sotto lo stencil è 1, 1, 0; moltiplicata per 1, 0, -1 dà 1+0-0=1 per riga, per un totale di 3 su tre righe.
2. Posizione spostata di una colonna (righe 1-3, colonne 2-4): ogni riga è 1, 0, 0; moltiplicata per 1, 0, -1 dà 1+0-0=1 per riga, totale 3 — identico al punto 1.
3. Poiché l'immagine è identica riga dopo riga, spostare lo stencil in verticale (righe 2-4 invece di 1-3) non cambia nulla: entrambe le posizioni rimanenti danno anch'esse 3. La mappa delle caratteristiche completa è quindi 3, 3, 3, 3 in tutte e quattro le posizioni — lo stencil rileva lo stesso bordo verticale ovunque appaia, esattamente l'invarianza traslazionale della Sezione 3.2.
4. Con un'immagine tutta chiara (tutti 1), ogni riga sotto lo stencil è 1, 1, 1; moltiplicata per 1, 0, -1 dà 1+0-1=0 per riga, totale 0. Nessun bordo verticale presente, nessuna risposta dallo stencil — il contrasto con il risultato di 3 del punto 1 mostra concretamente come lo stencil "si accenda" solo quando il motivo cercato è davvero lì.

### Soluzione — Lezione 4: il telefono senza fili

1. Non c'è una risposta netta, ma un pattern tipico: i sostantivi vividi e concreti ("gatto grigio", "Marta", "davanzale") tendono a sopravvivere meglio di dettagli grammaticali sottili (il "tutto" di "tutto il pomeriggio", l'esatta preposizione "sul" invece di "vicino al"). Non è un caso che assomigli al bigliettino che si sporca della Sezione 4.4: l'informazione più "densa di significato" tende a resistere più a lungo del dettaglio fine, che si dilava già dopo due o tre passaggi.
2. Ripetere parola per parola solo una parte del messaggio, lasciando che il resto si trasformi liberamente, imita esattamente il **gating** di LSTM/GRU (Sezione 4.5): decidere esplicitamente cosa tenere intatto e cosa lasciar rimescolare, invece di trattare l'intero messaggio allo stesso modo.
3. Con sessanta persone invece di sei, ti aspetteresti che il messaggio arrivi quasi irriconoscibile — il problema che peggiora drammaticamente è il **bigliettino che si sporca** (Sezione 4.4, il vanishing gradient nel tempo): più anelli ci sono nella catena, più l'informazione originale si diluisce a ogni passaggio. Il problema della sequenzialità (Sezione 4.6) resta invece costante nella sua natura — sessanta persone o sei, comunque nessuna può "saltare" il proprio turno — ma qui è il primo problema a manifestarsi più visibilmente.

### Soluzione — Lezione 5: due risposte "sicure al 70%"

1. Predizione A assegna 70% alla risposta corretta (cane): dalla tabella, **2 punti** di sorpresa.
2. Predizione B assegna solo 20% alla risposta corretta (cane, non coniglio, che è dove B ha messo per errore il suo 70%): dalla tabella, **8 punti** di sorpresa.
3. Sono molto diversi (2 contro 8, quattro volte tanto), anche se entrambe le predizioni "sembrano" ugualmente sicure di sé in generale. Il punto chiave: il test a crocette non guarda quanto sei sicuro in astratto, ma quanto sei sicuro **esattamente della risposta giusta** — essere sicurissimi ma sulla categoria sbagliata costa quasi quanto essere del tutto incerti, non meno.

### Soluzione — Lezione 6: fai risalire la colpa lungo tre anelli

1. Colpa al piano 2: -4 × 2 × 0,5 = -4.
2. Colpa al piano 1: -4 × 1,5 × 0,4 = -2,4.
3. I numeri di colpa sono -4 (piano 3), -4 (piano 2), -2,4 (piano 1): restano stabili per un anello e poi cominciano a restringersi. Con 30 anelli invece di 3, e moltiplicatori nello stesso ordine di grandezza (spesso sotto 1 in totale per anello), la colpa si restringerebbe progressivamente a ogni passaggio all'indietro, fino a diventare quasi impercettibile una volta arrivata ai primi piani della catena — esattamente il vanishing gradient incontrato, con un'altra metafora, nella Lezione 4.

### Soluzione — Lezione 7: chi ha capito e chi ha mandato a memoria

1. Bianchi, tenendo da parte metà degli esercizi mai usati per esercitarsi e controllando periodicamente il proprio andamento su quelli, sta facendo esattamente ciò che un **insieme di validazione** fa in una rete neurale: misurare le prestazioni su qualcosa che non ha influenzato l'allenamento.
2. Il caso di Rossi — perfetto sugli esercizi del libro, in difficoltà sul compito con domande nuove — è la firma classica dell'**overfitting**: l'errore "di allenamento" (gli esercizi già visti) è bassissimo, ma l'errore su casi nuovi (il compito vero) resta alto, il segno che ha memorizzato le risposte specifiche invece di imparare il metodo generale.
3. Fermare lo studio non appena il punteggio sulla metà mai vista comincia a peggiorare, invece di continuare a ripassare comunque, imita l'**early stopping** (Sezione 7.7): usare il segnale di validazione per decidere quando smettere, non un numero di sessioni di studio deciso in anticipo.

### Soluzione — Lezione 8: distanza e parallelismo

1. Il dettaglio deve sopravvivere per 290 − 15 = 275 pagine prima di diventare rilevante.
2. Sì, anche con LSTM/GRU il lettore deve comunque leggere le pagine in ordine, una alla volta: il diario più furbo della Sezione 4.5 migliora *cosa* sopravvive nella memoria, ma non rimuove l'obbligo di leggere sequenzialmente — è esattamente la distinzione fra i due problemi della Sezione 8.1.
3. Un lettore basato sulla sola attenzione può guardare direttamente la pagina 15 dalla pagina 290, in un solo passo, senza dover ripassare le 274 pagine di mezzo. La distanza — 15 pagine o 289 — non cambia la difficoltà di quel collegamento diretto: è un percorso di lunghezza costante, non proporzionale alla distanza (Sezione 8.3).
4. Con un lettore pagina-per-pagina, dieci lettori in parallelo non aiutano quasi per nulla: leggere la pagina 31 richiede comunque di sapere cosa diceva il bigliettino alla pagina 30, quindi il lavoro non si può davvero dividere in blocchi indipendenti. Con un lettore basato sulla sola attenzione, invece, non c'è alcuna dipendenza di questo tipo fra le pagine: dieci lettori (o hardware) possono davvero elaborare blocchi diversi del libro contemporaneamente, riducendo drasticamente il tempo totale necessario — il punto centrale della Sezione 8.5.

---

*Continua con l'[Appendice B — Glossario]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-appendice-b-glossario.md %}.html)*
