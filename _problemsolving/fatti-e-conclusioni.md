---
title: 'Fatti e conclusioni'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

I problemi di tipo **"fatti e conclusioni"** presentano un insieme di **entità** (per esempio persone, oggetti, luoghi) ognuna delle quali possiede diversi **attributi** con **valori discreti** (per esempio nome, città, sport praticato, tempo di allenamento).

Nel testo del problema vengono elencati alcuni **fatti**: informazioni parziali che collegano tra loro i valori degli attributi. Il compito è **ragionare per esclusione** sui fatti per ricostruire, in modo univoco, l'associazione completa tra tutte le entità e i loro attributi.

## Metodo risolutivo

Il modo più efficace per risolvere questi problemi è costruire una **tabella** con una riga per ogni entità e una colonna per ogni attributo, e riempirla progressivamente:

1. Si esamina un fatto alla volta.
2. Se il fatto assegna direttamente un valore, lo si scrive subito in tabella.
3. Se il fatto esclude una possibilità (per esempio "non è quello che..."), si segna comunque l'informazione, perché unita ad altri fatti permetterà spesso di dedurre un valore per differenza.
4. Si ripete il procedimento finché la tabella non è completa: ad ogni riga e ad ogni colonna deve corrispondere **un solo** valore per attributo, mai ripetuto.

## Esempio 1

Tre amici, **Dario**, **Elena** e **Fabio**, vivono in tre città diverse (**Torino**, **Bari**, **Pisa**), praticano tre sport diversi (**scherma**, **atletica**, **canottaggio**) e si allenano un numero diverso di ore a settimana (**3**, **6**, **9**).

Dai seguenti fatti, determinare città, sport e ore di allenamento di ciascun amico:

1. Chi pratica scherma si allena 9 ore a settimana.
2. Fabio pratica canottaggio e vive a Pisa.
3. Dario si allena 3 ore in più di chi vive a Bari.

#### Soluzione

| NOME | CITTÀ | SPORT | ORE |
|---|---|---|---|
| Dario | Torino | scherma | 9 |
| Elena | Bari | atletica | 6 |
| Fabio | Pisa | canottaggio | 3 |

#### Come si arriva alla soluzione

Costruiamo la tabella vuota con le tre entità:

| NOME | CITTÀ | SPORT | ORE |
|---|---|---|---|
| Dario | | | |
| Elena | | | |
| Fabio | | | |

**Fatto 2** ci dà direttamente due informazioni su Fabio: sport = canottaggio, città = Pisa.

| NOME | CITTÀ | SPORT | ORE |
|---|---|---|---|
| Dario | | | |
| Elena | | | |
| Fabio | Pisa | canottaggio | |

**Fatto 3** dice che Dario si allena 3 ore più di chi vive a Bari. Se Dario stesso vivesse a Bari, dovrebbe allenarsi 3 ore più di se stesso, il che è impossibile. Quindi **Dario non vive a Bari**: per esclusione Dario vive a Torino, ed **Elena vive a Bari** (l'unica città rimasta libera, visto che Fabio vive a Pisa).

| NOME | CITTÀ | SPORT | ORE |
|---|---|---|---|
| Dario | Torino | | |
| Elena | Bari | | |
| Fabio | Pisa | canottaggio | |

Sempre dal fatto 3, ore(Dario) = ore(Elena) + 3. Le uniche coppie possibili tra i valori 3, 6, 9 che rispettano questa relazione sono (6,3) oppure (9,6).

**Fatto 1** dice che chi pratica scherma si allena 9 ore. Lo scherma è praticato da Dario o da Elena (Fabio pratica canottaggio). Se fosse la coppia (Dario=6, Elena=3), nessuno dei due avrebbe 9 ore, e quindi nessuno dei due potrebbe praticare scherma: ma lo scherma deve essere praticato da uno dei due! Questo esclude la coppia (6,3): resta valida solo **Dario=9, Elena=6** (e Fabio, per differenza, ha le 3 ore rimanenti).

Poiché chi pratica scherma ha 9 ore (fatto 1) e Dario ha 9 ore, **Dario pratica scherma**. Per esclusione, **Elena pratica atletica**.

| NOME | CITTÀ | SPORT | ORE |
|---|---|---|---|
| Dario | Torino | scherma | 9 |
| Elena | Bari | atletica | 6 |
| Fabio | Pisa | canottaggio | 3 |

La tabella è ora completa e rispetta tutti e tre i fatti.

## Esempio 2

Quattro colleghi, **Anna**, **Bruno**, **Carla** e **Dino**, hanno scritto ciascuno un libro di genere diverso (**giallo**, **fantasy**, **storico**, **saggio**), pubblicato in anni diversi (**2018**, **2020**, **2021**, **2023**).

1. Il libro fantasy è stato pubblicato nel 2020.
2. Anna ha pubblicato il suo libro due anni dopo Bruno.
3. Carla ha scritto un saggio e non è l'autrice del libro del 2023.
4. Il libro giallo è stato pubblicato prima del libro storico.

#### Soluzione

| NOME | GENERE | ANNO |
|---|---|---|
| Bruno | giallo | 2018 |
| Dino | fantasy | 2020 |
| Carla | saggio | 2021 |
| Anna | storico | 2023 |

Prova a verificare tu stesso, fatto per fatto, che questa è l'unica soluzione compatibile: comincia dal fatto 1 (assegna subito un anno a un genere), poi usa il fatto 3 per escludere il 2023 per Carla, infine il fatto 2 e il fatto 4 per fissare tutte le altre caselle.

## Esercizio proposto

Tre negozi (**A**, **B**, **C**) vendono ciascuno un solo prodotto diverso (**pane**, **frutta**, **fiori**) e sono aperti un numero diverso di ore al giorno (**8**, **10**, **12**).

1. Il negozio di fiori è aperto 8 ore al giorno.
2. Il negozio A vende frutta e resta aperto più a lungo del negozio B.
3. Il negozio C non vende pane.

Determina prodotto e orario di apertura di ciascun negozio.

## Esercizi dalle gare OPS

I problemi seguenti sono tratti (e adattati) dalle gare a squadre delle Olimpiadi di Problem Solving (OPS) 2026, categoria Secondaria di secondo grado. Prova a risolverli da solo prima di aprire la soluzione.

### Gara 1: il lancio dei dadi

Tre amici, **A**, **B** e **C**, hanno vinto, giocando in squadra, tre premi, che indichiamo con **P1**, **P2**, **P3**. I premi sono indivisibili e decidono di prenderne uno a testa. Per decidere chi deve scegliere per primo (per secondo e per terzo), decidono di affidarsi alla sorte, mediante il lancio di un dado. In particolare, ogni amico lancia il dado due volte in sequenza e chi ottiene il punteggio totale maggiore sceglie per primo, chi ottiene il secondo punteggio totale sceglie per secondo, eccetera. Al primo lancio escono i numeri 1, 3, 6. Al secondo lancio: 2, 4, 5 (i numeri sono elencati in ordine casuale rispetto a chi ha tirato il dado).

Si conoscono inoltre i seguenti fatti:

1. Chi ha ottenuto il numero maggiore nel primo lancio ha scelto il premio P3.
2. C ha totalizzato complessivamente 7.
3. Chi ha ottenuto il numero 1 nel primo lancio, poi ha ottenuto 5 nel secondo.
4. Chi ha scelto il premio P1 non ha ottenuto il numero 3 nel primo lancio.
5. A non ha ottenuto il numero 1 nel primo lancio.

Rispondi alle seguenti domande:

1. Quale premio ha scelto chi ha ottenuto il numero 6 nel primo lancio?
2. Chi ha scelto il premio P2?
3. Quale premio ha scelto A?

<details markdown="1">
<summary>Soluzione</summary>

1. **P3**
2. **C**
3. **P3**

Dal fatto 3, chi ha fatto 1 nel primo lancio ha totalizzato 1+5=6. Dal fatto 2, C ha totalizzato 7, quindi C non è quel giocatore: C ha fatto 3 nel primo lancio e 4 nel secondo (3+4=7). Restano i punteggi 1 e 6 per gli altri due lanci del primo turno, abbinati rispettivamente a 5 e 2 nel secondo. Dal fatto 5, A non ha fatto 1 nel primo lancio, quindi A ha fatto 6 (e quindi B ha fatto 1, poi 5, totale 6). Il fatto 1 dice che chi ha fatto il numero maggiore nel primo lancio (6, cioè A) ha scelto P3. Il fatto 4 esclude che C (che ha fatto 3) abbia scelto P1: C ha quindi scelto P2, e a B resta P1.

| | 1° lancio | 2° lancio | Premio |
|---|---|---|---|
| A | 6 | 2 | P3 |
| B | 1 | 5 | P1 |
| C | 3 | 4 | P2 |

</details>

### Gara 2: i doni di Giulio

Giulio, sistemando casa, ha trovato tre doni che gli fecero tre amici artisti, **Anna**, **Bruno** e **Cloe**, in occasione di una visita che gli fecero tempo addietro. I doni sono un quadro, un disco e una statua. Giulio non ricorda chi fosse un pittore, chi uno scultore, chi un musicista; ricorda che gli amici abitavano in città diverse (**Assisi**, **Bolzano**, **Taranto**) e che è andato a trovarli in mesi diversi dell'anno, ma non ricorda esattamente quando. Ha trovato però dei bigliettini di quel tempo:

1. Nota di aprile: "La primavera è arrivata in Trentino!" *(Bolzano si trova in Trentino-Alto Adige)*
2. Messaggio ricevuto in gennaio da Bruno: "Ciao Giulio, ti aspetto domani!"
3. Messaggio da Anna senza data: "Vedrai come ti piacerà l'Umbria!" *(Assisi si trova in Umbria)*
4. Messaggio di Cloe senza data: "Una canzone per te"
5. Nota senza data: "Il quadro raffigura uno scorcio della Basilica di S. Francesco di Assisi"
6. Nota senza data: "Bruno abita in Puglia" *(Taranto si trova in Puglia)*

Rispondi alle seguenti domande:

1. Dove abitava Cloe?
2. Chi regalò a Giulio una statua?
3. Quale città visitò Giulio in gennaio?

<details markdown="1">
<summary>Soluzione</summary>

1. **Bolzano**
2. **Bruno**
3. **Taranto**

Dal fatto 6, Bruno abita in Puglia, quindi a Taranto. Dal fatto 2, Giulio ha visitato Bruno (Taranto) in gennaio. Dal fatto 3, Anna è legata all'Umbria, quindi ad Assisi; per esclusione Cloe abita a Bolzano, coerente col fatto 1 (nota di aprile sul Trentino). Dal fatto 4, Cloe (musicista) ha regalato il disco. Dal fatto 5, il quadro raffigura Assisi: è quindi il dono di Anna, che vive lì. Per esclusione, la statua è il dono di Bruno.

| | città | dono | mese |
|---|---|---|---|
| Anna | Assisi | quadro | — |
| Bruno | Taranto | statua | gennaio |
| Cloe | Bolzano | disco | aprile |

</details>

### Gara 3: i cercatori d'oro

**Anna**, **Beatrice** e **Carlo** sono tre amici cercatori d'oro. Ciascuno ha cercato l'oro in una posizione diversa, lungo tre torrenti diversi del Piemonte: **Orco**, **Elvo**, **Cervo**. Il torrente Orco è il più a sud dei tre, il torrente Cervo il più a nord. Portano con sé tre sacchetti, con le pepite trovate. Ecco alcuni fatti raccolti dai loro racconti:

1. Anna dice di essere stata quella più a sud tra i tre.
2. Le pepite trovate sono, in numero diverso, 15, 20 e 25. Carlo è stato il più fortunato, trovandone il numero maggiore.
3. Nel torrente Orco sono state trovate più pepite che nel torrente Elvo.
4. I tre sacchetti di pepite pesano, in totale, 80 g, 90 g e 100 g (uno per amico). Il sacchetto con 20 pepite ha un peso medio di 4 g a pepita.
5. Il sacchetto con più pepite non è quello più pesante.
6. Il sacchetto più pesante non è quello di Anna.
7. Chi ha trovato meno pepite non si trovava nella posizione più a nord.

Rispondi alle seguenti domande:

1. Quanto pesa il sacchetto di Carlo? (in grammi)
2. Quante pepite ha trovato Anna?
3. Quanti grammi d'oro sono stati trovati nel torrente Elvo?

<details markdown="1">
<summary>Soluzione</summary>

1. **90 g**
2. **20 pepite**
3. **100 g**

Dal fatto 4, il sacchetto con 20 pepite pesa 80 g (20 × 4). Dal fatto 5, il sacchetto con più pepite (25) non è il più pesante (100 g), quindi pesa 90 g; di conseguenza il sacchetto con 15 pepite pesa 100 g. Dal fatto 2, Carlo ha trovato 25 pepite, quindi il suo sacchetto pesa **90 g**. Dal fatto 6, il sacchetto più pesante (100 g, 15 pepite) non è di Anna: ad Anna restano quindi **20 pepite** (80 g), e a Beatrice 15 pepite (100 g). Dal fatto 1, Anna è nella posizione più a sud, cioè nel torrente Orco. Dal fatto 3, l'Orco (20 pepite) ha più pepite dell'Elvo: Beatrice (15) è compatibile con l'Elvo, mentre Carlo (25, troppe) va nel Cervo, la posizione più a nord. Il fatto 7 conferma tutto: chi ha trovato meno pepite (Beatrice, 15) non è nella posizione più a nord (che infatti è di Carlo). Il torrente Elvo, di Beatrice, ha quindi prodotto **100 g** d'oro.

</details>

### Gara 4 (Finale): il fornaio e i suoi cugini

Salvatore è un fornaio che deve far visita ai suoi tre cugini, **Anna**, **Beatrice** e **Carlo**, che abitano in tre città diverse (**Milano**, **Firenze**, **Napoli**), portando a ciascuno una certa quantità di pane e di biscotti. Questo uno stralcio della loro chat (il nome del cugino che parla non è riportato, sostituito dal simbolo `[?]`):

1. Salvatore: "Cugini, finalmente verrò a farvi visita e vi porterò un po' del mio buon pane…"
2. `[?]`: "E biscotti!"
3. Salvatore: "Anna, quanto pane ti porto?"
4. `[?]`: "Direi 4 kg"
5. `[?]`: "Io invece direi la metà."
6. Salvatore: "Ok tu 2 kg… e scommetto che mi porti a vedere gli Uffizi…" *(gli Uffizi si trovano a Firenze)*
7. `[?]`: "Esatto! Ma solo se mi porti pure un po' di biscotti, direi la stessa quantità del pane…"
8. `[?]`: "A Napoli invece vorremmo il doppio di biscotti, cioè 4 kg."
9. Salvatore: "Lo immaginavo, Carlo, sei goloso. Tu però mi porti a visitare gli Scavi di Pompei…" *(gli Scavi di Pompei si trovano vicino a Napoli)*
10. `[?]`: "Sicuro! Ma portami pure un po' di pane… direi un quarto rispetto ai biscotti."
11. `[?]`: "Io invece ti porterò a visitare la Pinacoteca di Brera. Che dici Salvatore?" *(la Pinacoteca di Brera si trova a Milano)*
12. Salvatore: "E quanti biscotti ti porto?"
13. `[?]`: "Direi un chilo in meno rispetto al pane."

Rispondi alle seguenti domande:

1. In quale città abita Anna?
2. Quanti kg di biscotti Salvatore porterà a Beatrice?
3. Quanti kg di pane Salvatore porterà in Lombardia? *(Milano si trova in Lombardia)*

<details markdown="1">
<summary>Soluzione</summary>

1. **Milano**
2. **2 kg**
3. **4 kg**

Dalle battute 3-5, Anna riceve 4 kg di pane, mentre un altro cugino ne riceve la metà, cioè 2 kg. Salvatore associa questo secondo cugino agli Uffizi (battuta 6), cioè a Firenze: è **Beatrice**, che riceve 2 kg di pane e, per la battuta 7, altrettanti kg di biscotti (**2 kg**). La battuta 8 associa Napoli a 4 kg di biscotti; Salvatore la collega a Carlo (battuta 9, Scavi di Pompei): **Carlo** vive a Napoli, riceve 4 kg di biscotti e, per la battuta 10, un quarto di pane, cioè 1 kg. Resta **Anna**, collegata alla Pinacoteca di Brera (battuta 11), cioè a **Milano**; per la battuta 13 riceve 1 kg in meno di biscotti rispetto al pane (4 kg), cioè 3 kg di biscotti.

| | città | pane | biscotti |
|---|---|---|---|
| Anna | Milano | 4 kg | 3 kg |
| Beatrice | Firenze | 2 kg | 2 kg |
| Carlo | Napoli | 1 kg | 4 kg |

</details>
