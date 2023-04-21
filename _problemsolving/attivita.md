---
title: 'Pianificazione delle attività'
date: '2023-04-18T15:21:11+01:00'
author: Fabio Mattei
layout: page
---

Supponiamo di voler costruire una casa e supponiamo che per costruire la nostra casa abbiamo bisono dell'aiuto di tanti diversi professionisti: il muratore, l'idraulico, il serramentista ecc. Le attività che queste professionisti devono realizzare sono interdipenti; ad esempio non posso certo montare le finestre o fare un impianto idraulico quindi ci sono attività che vanno svolte prima di altre; d'altro canto è anche vero che elettricista e serramentista possono lavorare insieme dato che le loro attività non si disturbano, questi possono quindi lavorare in parallelo.

| |Attività|  Persone   | Settimane |
|-|-|-|-|
| A1 |Fondazioni|  3 |  4  |
| A2 |Muratura| 6  |   3 |
| A3 | Imp idraulico| 2  | 3   |
| A4 |Imp elettrico|3   |  4  |
| A5 |Porte| 2  |  2  |
| A6 |Finiture| 3  | 3   |

### Formalizziamo la precedenza delle attività

|                            | precedente | successiva    |
|----------------------------|------------|---------------|
| [A1, A2]                   | Fondazioni | Muratura      |
| [A2, A3]                   | Muratura   | Imp idraulico |
| [A2, A4]                   | Muratura   | Imp elettrico |
| [A2, A5]                   | Muratura   | Porte         |
| [A3, A6] [A4, A6] [A5, A6] | Porte      | Finiture      |


### Trovare la durata in settimane del progetto.

### Trovare il numero massimo di persone che lavora contemporaneamente al progetto

## Diagramma di Pert



## Diagramma di Gantt

