---
id: 562
title: 'La codifica del testo'
date: '2020-10-06T23:56:27+02:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/10/06/560-revision-v1/'
permalink: '/?p=562'
---

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/ascii.gif)</figure>La tabella ASCII (acronimo di American Standard Code for Information Interchange, Codice Standard Americano per lo Scambio di Informazioni) è un codice per la codifica di caratteri inventato per creare una corrispondenza tra numeri e caratteri digitabili da tastiera.

Gli elaboratori elettronici, possono soltanto lavorare con i simboli 1 e 0, per tanto c’era la necessità di associare ad ogni carattere una sequenza di 1 e di 0.

**Bob Bemer**, nel **1961**, risolse questo problema inventando la tabella ASCII.

La tabella ASCII associa a ciascun simbolo un numero che può essere facilmente convertito in numero binario.

### Esempio

Codifichiamo la parola CIAO. Ad ogni lettera associamo il corrispondente numero nella tabella ASCII.

<figure class="wp-block-table">| C | I | A | O |
|---|---|---|---|
| 67 | 73 | 65 | 79 |

</figure>Notiamo che le lettere maiuscole e le lettere minuscole sono codificate in modo diverso perché per l’elaboratore sono simboli diversi e indipendenti.

<figure class="wp-block-table">| C | i | a | o |
|---|---|---|---|
| 67 | 105 | 97 | 111 |

</figure>