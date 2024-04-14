---
title: 'Il box model'
date: '2021-12-11T06:03:23+01:00'
author: Fabio Mattei
layout: page
---

Ciascun elemento in una pagina web occupa un certo spazio.
Il box-model in CSS ci aiuta a descrivere in modo appropriato come desideriamo che quello spazio sia occupato.
All'interno del box.model distinguiamo: 

- content: contenuto del box, che sta al centro, delimitato dalle proprietà css width e height
- padding: distanza tra contenuto e bordo, delimitato dalla proprietà css padding ma anche da padding-top, padding-bottom, padding-left, padding-right
- border: bordo che circonda il box 
- margin: distanza tra un box e quelli che lo circondano, delimitato dalla proprietà css margin ma anche da margin-top, margin-bottom, margin-left, margin-right

### Il bordo

Il bordo merita una trattazione specifica dato che è un elemento ricco di proprietà.

Un bordo può avere diversi stili di linea che si inseriscono nella proprietà border-style:

- dotted: punteggiato
- dashed: tratteggiato
- solid: continuo
- double: a doppia linea
- groove: effetto 3D con luce da sinistra in alto
- ridge: effetto 3D con luce da destra in basso
- inset: diverso effetto 3D con luce da sinistra in alto
- outset: diverso effetto 3D con luce da destra in basso
- none: nessun bordo
- hidden: bordo nascosto

Il colore si attribusce al bordo attraverso la proprietà border-color

/* <color> values */
border-color: red;

/* top and bottom | left and right */
border-color: red #f015ca;

Spessore
Si attribuisce attraverso la proprità border-width

Si può anche riassumere il tutto attraverso la proprietà border:

border: 1px solid red;
