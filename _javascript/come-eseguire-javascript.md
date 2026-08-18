---
title: 'Come eseguire JavaScript'
date: '2026-08-18T09:00:00+01:00'
author: Fabio Mattei
layout: page
---

JavaScript è nato nel 1995, creato da **Brendan Eich** in soli dieci giorni per il browser Netscape. A differenza di PHP, che è pensato principalmente per il server, e a differenza di Python, che è un linguaggio general-purpose, JavaScript nasce con uno scopo molto specifico: rendere interattive le pagine web. Ricordiamo che le pagine web si basano sul linguaggio HTML, il javascript rende interattive gli elementi definiti attraverso l'HTML e viene eseguito dal **browser** dell'utente (il "client", come visto nella sezione dedicata alle [applicazioni web]({{ site.baseurl }}{% link _web/applicazione-web-crud-progettazione.md %}.html#client-e-server)).

Con in tempo, JavaScript si è evoluto, è stato adottato da una grande comunità di sviluppatori, ed è "evaso" dal browser. Oggi, grazie a **Node.js**, è possibile avere codice javascript che gira da linea di comando oppure che si comporta da vero e propio linguaggio server, esattamente come PHP, Python o Ruby. In questo corso useremo entrambi i modi, ma inizieremo dal più semplice: il browser, che non richiede alcuna installazione.

## Modo 1: la console del browser (nessuna installazione richiesta)

Ogni browser moderno (Chrome, Firefox, Edge...) include un **interprete JavaScript** pronto all'uso, accessibile tramite gli **strumenti per sviluppatori**.

1. Apri il browser e premi **F12** (oppure tasto destro → "Ispeziona")
2. Seleziona la scheda **Console**
3. Scrivi una riga di JavaScript e premi Invio

{% highlight javascript %}
console.log("JavaScript funziona!");
{% endhighlight %}

Se sulla console compare la scritta **JavaScript funziona!**, l'interprete è attivo e pronto. Questa modalità di scrittura del codice è perfetta per fare rapidi esperimenti, ma noi vogliamo essere in grado di salvare le istruzioni all'interno di un file per poterci lavorare nel tempo. Per questo motivo andiamo a posizionare il codice javascript nel suo luogo naturale: all'interno di una pagina HTML.

## Modo 2: dentro una pagina HTML, con il tag `<script>`

JavaScript può essere inserito al'interno di in una pagina HTML utilizzando il tag `<script>`, come visto in [HTML e CSS]({{ site.baseurl }}{% link _web/html.md %}.html). 

Crea un file chiamato `prova.html` contenente il seguente codice:

{% highlight html %}
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Prova JavaScript</title>
</head>
<body>
    <script>
        console.log("JavaScript funziona dentro la pagina!");
    </script>
</body>
</html>
{% endhighlight %}

Apri il file con il browser e guarda la Console (F12): vedrai il messaggio stampato lì, **non** sulla pagina. `console.log()` scrive nella console degli strumenti per sviluppatori, non nel corpo visibile della pagina: per quello serve manipolare il **DOM**, argomento che vedremo in [Il DOM]({{ site.baseurl }}{% link _javascript/il-dom.md %}.html).

## Modo 3: Node.js, da riga di comando

Per questo corso, esattamente come abbiamo fatto per PHP e Python, vogliamo poter scrivere un file ed eseguirlo da terminale con un solo comando, senza passare per un browser. Per farlo installiamo **Node.js**, un interprete JavaScript pensato per girare fuori dal browser (lo stesso motore V8 di Chrome, impacchettato per la riga di comando).

### Windows e macOS

Scarica l'installer dal sito ufficiale: <https://nodejs.org/> (scegli la versione **LTS**, cioè quella a supporto più lungo e stabile) e segui la procedura guidata.

### macOS (alternativa con Homebrew)

{% highlight shell %}
brew install node
{% endhighlight %}

### Linux

{% highlight shell %}
sudo apt install nodejs    # Debian / Ubuntu
sudo dnf install nodejs    # Fedora
{% endhighlight %}

### Verificare l'installazione

{% highlight shell %}
node -v
{% endhighlight %}

Se tutto è andato a buon fine il comando stampa la versione di Node.js installata.

Creiamo un file chiamato `prova.js` contenente:

{% highlight javascript %}
console.log("Node.js funziona!");
{% endhighlight %}

Da riga di comando, posizionandoci nella cartella del file, digitiamo:

{% highlight shell %}
node prova.js
{% endhighlight %}

Se sulla console compare la scritta **Node.js funziona!** l'installazione è andata a buon fine e siamo pronti per iniziare. Da questo punto in avanti, salvo quando parleremo esplicitamente del DOM e della pagina HTML, useremo Node.js da riga di comando: è il modo più simile a quanto già fatto con PHP e Python.
