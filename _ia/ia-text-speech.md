---
title: 'Text to speech e Speech to Text'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

## La sintesi vocale (text to speech)

La sintesi vocale (in inglese speech synthesis) è la tecnica per la riproduzione artificiale della voce umana. Un sistema usato per questo scopo è detto sintetizzatore vocale e può essere realizzato tramite software o hardware. I sistemi di sintesi vocale sono noti anche come sistemi text-to-speech (TTS) (in italiano: da testo a voce) per la loro possibilità di convertire il testo in parlato.

Un sistema di sintesi vocali è normalmente organizzato in due parti: una parte si occupa della conversione del testo in simboli fonetici, mentre la parte back-end interpreta i simboli fonetici e li **legge**, trasformandoli così in voce artificiale. 

Le trascrizioni fonetiche sono convenzionalmente scritte tra parentesi quadre nei nostri vocabolari ed indicano in modo preciso come una parola deve essere letta: [ˈskambjo], [ˈʃɔkko], [veˈdeːre].

### La sintesi vocale per il grande pubblico

In una presentazione del 1984, Steve Jobs per intrdurre il MAC al grande pubblico diede un esempio di sintesi vocale.
Il sisetma in realtà usava un piccolo trucco, ma per l'epoca era qualcosa di molto avanzato.


{::options parse_block_html="true" /}
<iframe width="560" height="315" src="https://www.youtube.com/embed/2B-XwPjn9YY?si=5eoqi45-HU9iHTFN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
{::options parse_block_html="false" /}


### L'annunciatore degli orari in stazione

Esempi di sintesi vocale negli anni si trovano nei posti più disparati.
Ad esempio nelle stazioni dei treni, dagli anni '90, ci sono sistemi che fanno sintesi vocale anche se più che di sintesi vocale si tratta di giustapposizione di registrazione di parole.

### Siri, Alexa, Ok Google

Gli assistenti virtuali al giorno di oggi hanno sistemi di sintesi vocale molto avanzati. Esempi sono Siri, Alexa e Ok Google che riescono
a parlarci in modo **quasi** naturale.

Tuttavia dato che i sistemi informatici non capiscono **il contesto** di ciò che stanno leggendo, ascoltare questi sistemi troppo a lungo
risuta essere faticoso. Ad esempio ascoltare un libro letto da Alexa risulta essere molto faticoso per l'utente.

{::options parse_block_html="true" /}
<iframe width="560" height="315" src="https://www.youtube.com/embed/pZhSiCRCuxw?si=MN2l4TwodELdenwW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
{::options parse_block_html="false" /}

## Speech to text

Lo Speech to text è il processo di conversione delle parole pronunciate in una trascrizione testuale. 
In genere un sistema speech to text combina la tecnologia di riconoscimento vocale basata sull'AI, nota anche come **riconoscimento vocale automatico**, con la **trascrizione**. 

Un programma per computer capta l'audio sotto forma di vibrazioni delle onde sonore attraverso un microfono e utilizza algoritmi linguistici per convertire l'input audio in caratteri, parole e frasi digitali.

Il software di speech to text contiene diversi componenti. Eccone alcune:

- Input vocale: un microfono cattura le parole pronunciate
- Estrazione delle caratteristiche: il computer identifica toni e schemi distintivi nel discorso
- Decodificatore: l'algoritmo abbina le caratteristiche del parlato ai caratteri e alle parole attraverso un modello linguistico.
- Output: viene formattato il testo finale con la punteggiatura e le maiuscole corrette in modo che sia leggibile dall'uomo

Esistono tre metodi principali per eseguire il riconoscimento vocale: sincrono, asincrono e in streaming.

- Il riconoscimento sincrono avviene quando c'è una conversione immediata dell'audio in testo. Questo metodo è in grado di elaborare solo file audio di durata inferiore a un minuto. Viene utilizzato nei sottotitoli in diretta per le trasmissioni televisive.
- Il riconoscimento in streaming avviene quando l'audio in streaming viene elaborato in tempo reale, quindi testi frammentati potrebbero apparire mentre l'utente sta ancora parlando.
- Il riconoscimento asincrono avviene quando file audio preregistrati di grandi dimensioni vengono inviati per la trascrizione. Potrebbe essere messo in coda per essere elaborato e completato in un secondo momento.


## La sfida più grande  

Quando si hanno a disposizione sistemi text to speech e speech to text si può pensare di fare assistenti vocali che utlizzando queste tecnologie e, utilizzando l'AI generativa, si può implementare un vero assistente virtuale con cui si può interagire parlando.

Si possono creare assistenti in grado di aiutare i clienti in una telefonata o di interagire con app vocali.

## Non vedenti e accessibilità

Questo sistema rappresentano una grande occasione per permettere a persone che disabilità visive di poter leggere, oppure di avere guide virtuali che le assistono durante le azioni di tutti i giorni.


