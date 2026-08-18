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
