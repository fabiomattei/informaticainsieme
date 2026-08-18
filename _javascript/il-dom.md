---
title: 'Il DOM: JavaScript nel browser'
date: '2026-08-18T10:20:00+01:00'
author: Fabio Mattei
layout: page
---

Fino ad ora abbiamo eseguito JavaScript con Node.js, da riga di comando, esattamente come facciamo con PHP e Python: è il modo più semplice per imparare il linguaggio, ma **non** è il motivo per cui JavaScript è stato creato. JavaScript nasce per vivere **dentro il browser** (il "client", come visto in [Client e server]({{ site.baseurl }}{% link _web/applicazione-web-crud-progettazione.md %}.html#client-e-server)) e per rendere interattiva una pagina HTML già caricata: aggiungere elementi, cambiarne il contenuto, reagire ai click dell'utente, senza dover ricaricare la pagina.

Lo strumento che rende possibile tutto questo si chiama **DOM**, acronimo di **Document Object Model**: è la rappresentazione, sotto forma di oggetti manipolabili da JavaScript, di ogni singolo elemento HTML presente nella pagina.

## Il documento come albero di oggetti

Quando il browser carica una pagina HTML come quella vista in [HTML e CSS]({{ site.baseurl }}{% link _web/html.md %}.html), non si limita a disegnarla: costruisce anche una struttura ad **albero**, dove ogni tag diventa un oggetto JavaScript. Questo albero è accessibile tramite la variabile globale `document`, già pronta all'uso dentro ogni pagina HTML, senza bisogno di alcuna dichiarazione.

Creiamo un file `pagina.html`:

{% highlight html %}
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Il DOM</title>
</head>
<body>
    <h1 id="titolo">Ciao</h1>
    <p class="descrizione">Un paragrafo di prova.</p>
    <button id="bottone">Clicca qui</button>

    <script>
        console.log(document.title);   // "Il DOM"
    </script>
</body>
</html>
{% endhighlight %}

Apri il file nel browser e guarda la Console (F12, come già visto in [Come eseguire JavaScript]({{ site.baseurl }}{% link _javascript/come-eseguire-javascript.md %}.html)).

## Selezionare un elemento

Per manipolare un elemento della pagina, prima dobbiamo **selezionarlo**. Il metodo più usato è `document.querySelector()`, che accetta un **selettore CSS**, esattamente la stessa sintassi vista in [HTML e CSS]({{ site.baseurl }}{% link _web/html.md %}.html) per lo stile.

{% highlight javascript %}
const titolo = document.querySelector('#titolo');       // seleziona per id (#)
const descrizione = document.querySelector('.descrizione'); // seleziona per classe (.)
const bottone = document.querySelector('button');        // seleziona per tag
{% endhighlight %}

`querySelector()` restituisce il **primo** elemento che corrisponde al selettore. Se serve selezionare **tutti** gli elementi che corrispondono, si usa `document.querySelectorAll()`, che restituisce una collezione su cui si può iterare con `for...of` (come visto in [Il ciclo for]({{ site.baseurl }}{% link _javascript/il-ciclo-for.md %}.html)):

{% highlight javascript %}
const paragrafi = document.querySelectorAll('p');
for (const p of paragrafi) {
    console.log(p.textContent);
}
{% endhighlight %}

## Leggere e modificare il contenuto

Una volta selezionato un elemento, la sua proprietà `textContent` permette di leggerne o modificarne il testo:

{% highlight javascript %}
const titolo = document.querySelector('#titolo');
console.log(titolo.textContent);   // "Ciao"
titolo.textContent = "Ciao a tutti!";   // modifica il testo mostrato nella pagina
{% endhighlight %}

A differenza di `console.log()`, che scrive nella console degli strumenti per sviluppatori, assegnare un valore a `textContent` modifica **visibilmente** la pagina che l'utente ha davanti: è la prima vera differenza pratica tra scrivere uno script Node.js e scrivere JavaScript per il browser.

Esiste anche `innerHTML`, che permette di inserire vero e proprio codice HTML (non solo testo semplice) dentro un elemento:

{% highlight javascript %}
const descrizione = document.querySelector('.descrizione');
descrizione.innerHTML = "Testo <strong>in grassetto</strong>";
{% endhighlight %}

## Reagire agli eventi

La parte più importante dell'interattività di una pagina è la capacità di **reagire** alle azioni dell'utente: un click, la pressione di un tasto, l'invio di un form. Si fa tramite `addEventListener()`, a cui si passa il nome dell'evento e una funzione da eseguire quando l'evento si verifica.

{% highlight javascript %}
const bottone = document.querySelector('#bottone');

bottone.addEventListener('click', function () {
    console.log("Hai cliccato il bottone!");
});

// la stessa cosa, con una arrow function (vista in Funzioni)
bottone.addEventListener('click', () => {
    document.querySelector('#titolo').textContent = "Hai cliccato!";
});
{% endhighlight %}

La funzione passata come secondo argomento non viene eseguita subito: viene **registrata** e verrà chiamata dal browser ogni volta che l'utente clicca il bottone. Questo stile di programmazione, in cui si definiscono funzioni che verranno eseguite in futuro in risposta a un evento, si chiama programmazione **guidata dagli eventi** (*event-driven*), ed è molto diverso dal flusso lineare, riga dopo riga, dei programmi Node.js che abbiamo scritto finora.

## Un esempio completo

{% highlight html %}
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Contatore</title>
</head>
<body>
    <p>Hai cliccato <span id="contatore">0</span> volte</p>
    <button id="incrementa">+1</button>

    <script>
        let conteggio = 0;
        const span = document.querySelector('#contatore');
        const bottone = document.querySelector('#incrementa');

        bottone.addEventListener('click', () => {
            conteggio = conteggio + 1;
            span.textContent = conteggio;
        });
    </script>
</body>
</html>
{% endhighlight %}

Ad ogni click, la funzione registrata con `addEventListener` viene eseguita: incrementa la variabile `conteggio` (una variabile globale del nostro script, letta e modificata liberamente dalla funzione, come visto in [Funzioni]({{ site.baseurl }}{% link _javascript/funzioni.md %}.html)) e aggiorna il testo dello `<span>` con il nuovo valore. Nota che **non serve ricaricare la pagina**: è esattamente questo il problema che JavaScript, nato nel browser, risolve.

## Client-side vs server-side, ancora una volta

Vale la pena ribadire la differenza vista nella pagina sulle [applicazioni web CRUD in PHP]({{ site.baseurl }}{% link _web/applicazione-web-crud-progettazione.md %}.html#client-e-server): il codice di questa pagina gira **interamente nel browser** dell'utente (client-side), non ha bisogno di alcun server per funzionare (puoi aprire il file `.html` direttamente con doppio click), e non può accedere a un database MySQL o al filesystem del server. Per quello serve un linguaggio server-side come PHP, che gira sul server e comunica con il browser tramite richieste HTTP GET e POST, come visto in quella stessa pagina. Le due tecnologie sono complementari, non alternative: in una vera applicazione web moderna, PHP prepara e serve i dati, mentre JavaScript li rende interattivi una volta arrivati nel browser dell'utente.

## Esercizi

#### Esercizio 1:
Crea una pagina HTML con un paragrafo e un bottone. Al click del bottone, il testo del paragrafo deve cambiare in "Hai cliccato!".

#### Esercizio 2:
Crea una pagina HTML con un campo di input (`<input type="text">`) e un bottone. Al click del bottone, leggi il valore inserito dall'utente con `document.querySelector('input').value` e mostralo dentro un `<p>` della pagina.

#### Esercizio 3:
Realizza il contatore visto nell'esempio completo, aggiungendo anche un secondo bottone "-1" che decrementa il contatore (senza farlo scendere sotto lo 0).

#### Esercizio 4:
Crea una pagina con tre `<div>` colorati diversamente tramite CSS (vedi [HTML e CSS]({{ site.baseurl }}{% link _web/html.md %}.html)) e un bottone "Nascondi tutti" che, al click, imposta `element.style.display = 'none'` su ciascuno dei tre.

#### Esercizio 5:
Crea una piccola lista della spesa: un `<input>` di testo, un bottone "Aggiungi" e una lista `<ul>` vuota. Al click del bottone, crea un nuovo `<li>` con `document.createElement('li')`, impostane il testo con `textContent`, e aggiungilo alla lista con `lista.appendChild(nuovoElemento)`.
