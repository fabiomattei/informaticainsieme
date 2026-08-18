---
title: 'Robot con memoria'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

Un'altra variante del [robot classico]({{ site.baseurl }}{% link _problemsolving/movimento-robot.md %}.html) è capace di **memorizzare** una sotto-sequenza di comandi e **richiamarla** più volte durante l'esecuzione, in punti diversi della lista di comandi.

## I comandi s e c

Oltre a **o**, **a** e **f**, questo robot possiede due nuovi comandi:

* **comando s**: è seguito da un **numero identificativo**, poi da una sequenza di comandi (il **corpo**), racchiusa tra i simboli `<` e `>`. Esempio: `s3<a,f,o,f>`, dove **3** è il numero identificativo e `a,f,o,f` è il corpo. **Attenzione**: eseguire il comando s **non sposta** il robot: si limita a memorizzare il corpo, associandolo al numero identificativo.
* **comando c**: è seguito da un numero identificativo, per esempio `c3`. Quando il robot lo esegue, se in precedenza ha già eseguito un comando **s** con lo **stesso** numero identificativo, allora esegue tutti i comandi del corpo memorizzato; altrimenti (se quel numero non è ancora stato definito) non fa nulla.

## Come risolvere questi problemi

Conviene esaminare la lista di comandi **da sinistra verso destra**, tenendo traccia di quali sotto-sequenze sono state definite (con **s**) fino a quel momento. Ogni volta che si incontra un comando **c** con un identificativo già definito, lo si sostituisce con il corpo corrispondente: si ottiene così una lista di soli comandi o, a, f, equivalente a quella di partenza, che si esegue come un robot classico.

## Esempio

Un robot parte dallo stato `[2,2,E]` ed esegue la lista di comandi:

`[f, s1<f,o,f>, a, c1, f, c1]`

Determinare lo stato finale del robot.

#### Soluzione

Analizziamo la lista comando per comando:

1. **f**: si esegue, il robot avanza.
2. **s1<f,o,f>**: memorizza il corpo `f,o,f` con identificativo 1. Non sposta il robot.
3. **a**: si esegue, il robot ruota.
4. **c1**: è già stato definito s1, quindi si esegue il corpo `f,o,f`.
5. **f**: si esegue, il robot avanza.
6. **c1**: si esegue di nuovo il corpo `f,o,f`.

La lista degli spostamenti **effettivi** del robot (equivalente per un robot classico) è quindi:

`[f, a, f,o,f, f, f,o,f]`

Eseguiamola a partire dallo stato `[2,2,E]`:

| Stato di partenza | Comando | Stato di arrivo |
|---|---|---|
| [2,2,E] | f | [3,2,E] |
| [3,2,E] | a | [3,2,N] |
| [3,2,N] | f | [3,3,N] |
| [3,3,N] | o | [3,3,E] |
| [3,3,E] | f | [4,3,E] |
| [4,3,E] | f | [5,3,E] |
| [5,3,E] | f | [6,3,E] |
| [6,3,E] | o | [6,3,S] |
| [6,3,S] | f | [6,2,S] |

Lo stato finale del robot è **[6,2,S]**.

## Esercizio proposto

Un robot parte dallo stato `[10,6,N]` ed esegue la lista di comandi:

`[s2<a,f,f>, f, c2, o, c2, f]`

Determina lo stato finale del robot, ricordando che il comando s2 non sposta il robot, ma memorizza il suo corpo per i successivi comandi c2.
