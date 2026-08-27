---
title: 'Lezione 13, Il dato che non torna'
date: '2026-08-25T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Molti dati simili raggruppati, e un dato isolato e diverso, cerchiato come anomalia](/images/ia/come-pensano-le-macchine-che-imparano-lezione-13-il-dato-che-non-torna/come-pensano-le-macchine-che-imparano-lezione-13-il-dato-che-non-torna.svg){:class="aside-image"}

### 13.1 L'ispettore che nota il pezzo sbagliato

Immagina un ispettore di qualità in una fabbrica, che passa le giornate a guardare scorrere migliaia di pezzi identici su un nastro trasportatore. Dopo settimane a osservare sempre lo stesso pezzo, uguale a se stesso decine di migliaia di volte, l'ispettore sviluppa un istinto quasi automatico: non deve elencare a memoria tutti i modi in cui un pezzo può essere difettoso, gli basta notare, a colpo d'occhio, quando *qualcosa non torna* rispetto a tutto ciò che ha visto finora. Un bordo leggermente storto, una sfumatura di colore diversa, un pezzo che semplicemente "non sembra uno degli altri".

Questa lezione, l'ultima del libro, affronta esattamente questo compito, riconoscere ciò che si discosta da tutto il resto, con una famiglia di tecniche chiamata **rilevamento di anomalie** (in inglese *anomaly detection*): individuare automaticamente frodi bancarie, guasti industriali, comportamenti sospetti su una rete informatica, battiti cardiaci irregolari, ogni caso in cui interessa scovare "il dato che non torna" in mezzo a moltissimi dati che, invece, tornano perfettamente.

### 13.2 Perché è un problema diverso dalla classificazione

Verrebbe naturale pensare al rilevamento di anomalie come a una semplice classificazione, "normale" contro "anomalo", risolvibile con gli stessi strumenti della Lezione 3 o della Lezione 9. C'è però un ostacolo pratico che rende il problema sostanzialmente diverso: la classificazione supervisionata, per funzionare bene, ha bisogno di **molti esempi etichettati di entrambe le categorie**. Ma le anomalie, quasi per definizione, sono rare: una banca ha milioni di transazioni legittime e, si spera, pochissime frodi da cui imparare; un impianto industriale funziona correttamente quasi sempre, e i guasti veri, per fortuna, sono rari. Peggio ancora, un'anomalia nuova, mai vista prima, una frode con un meccanismo inedito, un guasto mai capitato, non somiglia necessariamente a nessuna delle poche anomalie passate su cui un modello supervisionato avrebbe potuto allenarsi.

La strategia più efficace, quindi, è spesso diversa: invece di imparare direttamente "come è fatta un'anomalia" (che potrebbe essere quasi ogni cosa), si impara bene **come è fatto il comportamento normale**, e si segnala come sospetto tutto ciò che si discosta abbastanza da quel comportamento consueto, indipendentemente dal fatto che quel tipo specifico di scostamento sia già stato visto prima o meno.

### 13.3 Un primo modo: la distanza dal proprio vicinato

Un modo diretto di misurare quanto un punto "non torna" riprende un'idea già incontrata due volte in questo libro: la distanza nello spazio delle caratteristiche della Lezione 2, e i centri di gruppo del clustering della Lezione 7. Se la maggior parte degli esempi normali forma gruppi fitti e compatti, un punto che si trova molto lontano da qualunque centro di gruppo, o i cui vicini più prossimi sono tutti insolitamente distanti (a differenza di un punto "normale", che di solito ha altri punti molto simili proprio lì vicino), è un buon candidato anomalia. Più un punto è isolato rispetto alla densità tipica dei suoi dintorni, più alto è il suo "punteggio di anomalia".

### 13.4 Un secondo modo: quanto è facile isolarlo

Un'idea più recente, alla base dell'algoritmo **Isolation Forest** ("foresta di isolamento", un parente stretto della random forest della Lezione 8), capovolge la prospettiva in un modo elegante: invece di misurare quanto un punto è lontano dagli altri, misura **quanto è facile separarlo da tutti gli altri** con poche domande casuali, dello stesso tipo di quelle poste da un albero decisionale nella Lezione 3, ma scelte qui a caso invece che per massimizzare la purezza.

L'intuizione è questa: un punto sepolto in mezzo a un gruppo fitto di punti simili richiede molte domande, molti tagli successivi, prima di restare isolato da solo in un gruppo tutto suo, proprio perché è circondato da vicini quasi indistinguibili da lui. Un punto anomalo, isolato e diverso da tutto il resto, viene invece separato dagli altri con pochissime domande, spesso una sola, perché non ha bisogno di essere districato da un affollamento di punti simili a lui.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 160" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="iso-title iso-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="iso-title">Un taglio isola subito l'anomalia, molti tagli servono nel gruppo fitto</title>
  <desc id="iso-desc">Una linea numerica con un gruppo fitto di punti a sinistra e un punto isolato a destra; un solo taglio separa subito il punto isolato, mentre servirebbero più tagli per isolare un punto dentro il gruppo fitto.</desc>

  <line x1="30" y1="90" x2="490" y2="90" stroke="#828282" stroke-width="1.5" />
  <g fill="#2a7ae2"><circle cx="70" cy="90" r="6" /><circle cx="85" cy="90" r="6" /><circle cx="100" cy="90" r="6" /><circle cx="115" cy="90" r="6" /><circle cx="130" cy="90" r="6" /></g>
  <circle cx="440" cy="90" r="6" fill="#f66a0a" />

  <path d="M 280,60 L 280,120" stroke="#3aa655" stroke-width="2" stroke-dasharray="4 3" />
  <text x="280" y="45" fill="#3aa655" font-size="11" text-anchor="middle">1 taglio isola l'anomalia</text>

  <path d="M 78,55 L 78,125" stroke="#c85506" stroke-width="1.5" stroke-dasharray="2 2" />
  <path d="M 108,55 L 108,125" stroke="#c85506" stroke-width="1.5" stroke-dasharray="2 2" />
  <text x="100" y="140" fill="#c85506" font-size="11" text-anchor="middle">più tagli per isolare un punto nel gruppo fitto</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Isolation Forest misura proprio questo: quanti tagli casuali servono, in media, per isolare ogni punto.</figcaption>
</figure>

Ripetendo questo esperimento molte volte, con tagli scelti a caso in modo leggermente diverso ogni volta, e mediando i risultati su una "foresta" di questi tentativi, esattamente come la random forest della Lezione 8 mediava le previsioni di molti alberi, si ottiene un punteggio di anomalia affidabile per ogni punto, senza aver mai avuto bisogno di un solo esempio etichettato "anomalo" in partenza.

### 13.5 Dove si usa

Il rilevamento di anomalie è oggi uno degli usi più concreti e diffusi del machine learning fuori dai riflettori: le banche lo usano per segnalare transazioni sospette in tempo reale, i sistemi di sicurezza informatica per notare traffico di rete anomalo, gli impianti industriali per prevedere guasti prima che accadano davvero, monitorando continuamente sensori di temperatura, vibrazione, pressione. In quasi tutti questi casi il tratto comune è lo stesso incontrato in questa lezione: pochissimi esempi (per fortuna) della categoria che davvero interessa scovare, e la necessità di imparare cosa sia "normale" per poter riconoscere, per differenza, ciò che non lo è.

### 13.6 Il limite condiviso, e dove va questo libro da qui

Le tredici lezioni di questo libro hanno percorso, un pezzo alla volta, gli strumenti fondamentali del machine learning "classico": imparare da esempi invece che da regole scritte a mano (Lezione 1), classificare guardando i vicini più simili (Lezione 2) o ponendo una sequenza di domande (Lezione 3), prevedere numeri con una retta (Lezione 4), riconoscere e correggere overfitting e underfitting (Lezione 5), misurare onestamente quanto un modello è davvero bravo (Lezione 6), trovare struttura nei dati senza etichette (Lezione 7), combinare più modelli per ottenere risultati più affidabili di qualunque singolo modello (Lezione 8), classificare sommando indizi deboli (Lezione 9), separare categorie con il confine più sicuro possibile (Lezione 10), semplificare tanti indizi in pochi senza perdere l'essenziale (Lezione 11), scoprire quali eventi tendono a presentarsi insieme (Lezione 12), e riconoscere ciò che non assomiglia a nulla di già visto (questa lezione).

C'è però un limite condiviso da tutti questi algoritmi, che vale la pena rendere esplicito proprio ora che il libro si chiude: ciascuno di essi lavora su **caratteristiche già scelte da un essere umano**, il suono, il colore del puntino, il peso, il prezzo, la parola presente o assente. Funzionano magnificamente quando qualcuno sa già quali indizi contano e sa misurarli. Ma cosa succede quando i dati non sono una tabella ordinata di numeri, bensì i milioni di pixel di una fotografia, o il flusso grezzo di parole di un testo, dati dove nessuno saprebbe elencare a mano "le caratteristiche giuste" da misurare? È esattamente a questo punto che il testimone passa alle reti neurali di *Come Pensano le Reti Neurali*: un modo di imparare *anche le caratteristiche stesse* direttamente dai dati grezzi, invece di riceverle già pronte, l'ultimo, decisivo passo che avvicina il machine learning ai chatbot di *Come Pensano le Macchine che Parlano*.

---

> **Prova tu, Trova il dato che non torna**
>
> Sei transazioni, in euro: 18, 20, 19, 22, 21, 95.
>
> 1. Guarda le distanze fra i valori vicini sulla linea dei numeri: quali valori hanno vicini molto vicini a loro? Quale valore, invece, ha tutti gli altri estremamente lontani?
> 2. Con un solo taglio a metà, per esempio a 50, quanti valori restano isolati da soli in un gruppo tutto loro? E quanti tagli, invece, servirebbero circa per isolare da solo il valore 19, sepolto in mezzo agli altri quattro valori simili (18, 20, 21, 22)?
> 3. Il ragionamento del punto 2 non ha mai avuto bisogno di sapere in anticipo "quali transazioni sono fraudolente". Perché questo è un vantaggio pratico importante, proprio nei casi in cui il rilevamento di anomalie viene usato davvero (frodi, guasti, intrusioni)?

---

## Esercizi

1. Spiega perché il rilevamento di anomalie è un problema diverso dalla classificazione supervisionata vista nelle lezioni precedenti, e quale ostacolo pratico rende difficile applicare direttamente gli stessi strumenti.
2. Pensa a un contesto reale, diverso da frodi bancarie, guasti industriali o sicurezza informatica, in cui riconoscere "il dato che non torna" potrebbe essere utile. Descrivi brevemente cosa considereresti "normale" in quel contesto.
3. Spiega, con parole tue, l'idea di Isolation Forest: perché un punto anomalo richiede in media pochi tagli casuali per essere isolato, mentre un punto in mezzo a un gruppo fitto ne richiede molti di più?
4. Confronta l'approccio basato sulla distanza (Sezione 13.3) con quello di Isolation Forest (Sezione 13.4): in che modo sono simili nell'idea di fondo, e in cosa differiscono nel modo di misurare quanto un punto è anomalo?
5. La Sezione 13.6 chiude il libro spiegando che tutti gli algoritmi visti lavorano su caratteristiche già scelte da un essere umano. Spiega con parole tue questo limite, e fai un esempio di un tipo di dato, oltre a immagini e testo, per cui sarebbe difficile stabilire a mano "le caratteristiche giuste".

---

*Continua con l'[Appendice A, Soluzioni ai giochi]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-appendice-a-soluzioni.md %}.html)*
