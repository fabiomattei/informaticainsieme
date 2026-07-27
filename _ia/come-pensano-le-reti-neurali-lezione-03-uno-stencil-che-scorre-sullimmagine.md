---
title: 'Lezione 03 — Uno stencil che scorre sull''immagine'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 3.1 Il problema: un'immagine non è un elenco di numeri qualsiasi

La rete a più piani della Lezione 2 tratta il proprio input come un elenco piatto di numeri, senza distinzioni. Va benissimo per due indizi come "quanto è nuvoloso" e "quanto è umido" — ma cosa succede se l'input è un'immagine, diciamo un quadrato di 256 per 256 puntini di colore (pixel)? Una rete come quella della Lezione 2 la vedrebbe come un unico elenco di oltre 65mila numeri in fila, senza alcuna nozione che il puntino in una certa posizione sia spazialmente vicino al puntino appena accanto. Questo crea due problemi seri.

Primo: collegare ogni controllore a *tutti* i 65mila puntini richiederebbe milioni di connessioni regolabili già per un solo piano — un costo enorme, e un rischio altissimo di "imparare a memoria" invece che a capire (il problema anticipato nella Sezione 2.6). Secondo, più sottile: una rete così allenata a riconoscere un gatto in alto a sinistra di una foto non avrebbe alcuna garanzia di riconoscere lo stesso identico gatto se comparisse in basso a destra, perché ogni puntino avrebbe un'importanza propria e indipendente da tutte le altre. Manca quella che si chiama **invarianza traslazionale**: la capacità di riconoscere un motivo locale ovunque appaia nell'immagine, senza doverlo re-imparare posizione per posizione.

### 3.2 L'idea: uno stencil riusabile

Le **reti convoluzionali** risolvono entrambi i problemi con un'unica idea, presa in prestito da qualcosa che forse hai già usato con carta e forbici: uno stencil, o un timbro di gomma. Invece di guardare l'intera immagine in un colpo solo, un piccolo stencil — diciamo, grande quanto un francobollo — osserva solo una piccola porzione dell'immagine alla volta, e lo stesso identico stencil viene fatto scorrere su ogni porzione della griglia, una alla volta. Questo doppio vincolo — guardare solo un pezzetto per volta, e riusare sempre lo stesso stencil — riduce drasticamente il numero di impostazioni da regolare (uno stencil piccolo ha pochissimi numeri, indipendentemente da quanto è grande l'immagine intera) e garantisce l'invarianza traslazionale quasi per costruzione: lo stesso stencil che riconosce un bordo verticale in un angolo dell'immagine riconosce lo stesso identico bordo, con la stessa sensibilità, ovunque compaia.

### 3.3 Come funziona lo stencil in pratica

Immagina un pezzetto di immagine in bianco e nero, rappresentato come una griglia di numeri (0 per il nero, 1 per il bianco), e uno stencil altrettanto piccolo — una griglia di numeri anch'essa, pensata per "accendersi" quando incontra un certo motivo, per esempio un bordo verticale netto fra chiaro e scuro. L'operazione è semplice da descrivere passo passo: appoggia lo stencil in un angolo dell'immagine, moltiplica ogni numero dello stencil per il numero dell'immagine che gli sta esattamente sotto, somma tutti quei prodotti in un unico numero, e scrivi quel numero in una nuova griglia — la **mappa delle caratteristiche** — nella posizione corrispondente. Poi sposta lo stencil di una casella e ripeti, fino ad aver coperto l'intera immagine.

Il numero che risulta a ogni posizione misura quanto fortemente il motivo cercato dallo stencil è presente proprio lì: alto se il motivo c'è, basso o nullo altrimenti. Uno strato di questo tipo, in pratica, non usa un solo stencil ma decine o centinaia in parallelo, ciascuno alla ricerca di un motivo diverso — un bordo orizzontale, uno diagonale, una macchia di un certo colore — producendo altrettante mappe delle caratteristiche, ognuna seguita, come nella Lezione 1, da una manopola sfumata che decide quanto "accendere" quella risposta.

### 3.4 Riassumere senza perdere l'essenziale

Dopo che uno o più stencil hanno prodotto le loro mappe delle caratteristiche, è comune applicare un passaggio di "riassunto": si prendono piccole regioni della mappa (tipicamente blocchi di 2×2 caselle) e si conserva solo il valore più alto di ciascun blocco, scartando gli altri tre — un'operazione chiamata **max pooling**. L'effetto è duplice: la mappa diventa più piccola e più veloce da elaborare nei passaggi successivi, e la rete diventa un po' più tollerante a piccoli spostamenti (se il motivo cercato si sposta di un solo puntino dentro quel blocco di 2×2, il massimo del blocco spesso resta lo stesso).

### 3.5 Dai bordi agli oggetti: una gerarchia di motivi

Impilando più coppie stencil-e-riassunto, una rete convoluzionale costruisce tipicamente una scala di rappresentazioni sempre più astratte, un piano alla volta — proprio l'idea di "riusare ciò che il piano precedente ha già capito" anticipata nella Sezione 2.5, qui resa particolarmente concreta e visiva. I primi piani imparano stencil che rispondono a motivi elementari: bordi orizzontali, verticali, diagonali, transizioni di colore. I piani intermedi combinano quei motivi elementari in forme più composite: angoli, texture, curve semplici. I piani più profondi combinano quelle forme in parti riconoscibili di un oggetto — un occhio, una ruota — fino ad arrivare, negli ultimi piani (tipicamente piani densi come quelli della Lezione 2), a una decisione sull'immagine intera: "questo è un gatto".

---

> **Prova tu — Fai scorrere lo stencil**
>
> Ecco una mini-immagine 4×4 (1 = chiaro, 0 = scuro) e uno stencil 3×3 pensato per rilevare un bordo verticale (dove il "chiaro" finisce e comincia lo "scuro" andando da sinistra a destra):
>
> ```
> Immagine:        Stencil:
> 1 1 0 0          1  0  -1
> 1 1 0 0          1  0  -1
> 1 1 0 0          1  0  -1
> 1 1 0 0
> ```
>
> Lo stencil può appoggiarsi in quattro posizioni diverse sull'immagine (il suo angolo in alto a sinistra può stare in riga 1 o 2, colonna 1 o 2). Per ciascuna posizione: moltiplica ogni numero dello stencil per il numero dell'immagine esattamente sotto, e somma i nove prodotti.
>
> 1. Calcola il risultato per lo stencil appoggiato in alto a sinistra (righe 1-3, colonne 1-3 dell'immagine).
> 2. Calcola il risultato per lo stencil spostato di una colonna a destra (righe 1-3, colonne 2-4).
> 3. Senza rifare tutti i calcoli, indovina (e poi verifica) i risultati per le due posizioni rimanenti (righe 2-4). Suggerimento: l'immagine è identica riga dopo riga — cosa significa questo per il risultato dello stencil, che guarda solo colonne diverse a seconda della posizione orizzontale?
> 4. Che numero ti aspetteresti se l'immagine, invece di avere il bordo netto fra colonna 2 e colonna 3, fosse tutta uniformemente chiara (tutti 1)? Prova a ricalcolare per la posizione del punto 1 e confronta.

---

*Continua con la [Lezione 04 — Il filo della memoria]({{ site.baseurl }}{% link _ia/come-pensano-le-reti-neurali-lezione-04-il-filo-della-memoria.md %}.html)*
