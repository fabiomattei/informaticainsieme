---
title: 'Le form HTML'
date: '2026-10-09T10:00:00+02:00'
author: Fabio Mattei
layout: page
---

Finora abbiamo usato l'HTML per **mostrare** contenuti: titoli, paragrafi, elenchi, immagini. Le **form** (in italiano *moduli*) servono invece a fare il contrario: permettono all'utente di **inserire dati** e di inviarli al server.

Ogni volta che facciamo un login, scriviamo nella barra di ricerca di un sito, compiliamo un ordine online o lasciamo un commento, stiamo usando una form.

#### Il tag form

Una form è racchiusa tra i tag &lt;form&gt; e &lt;/form&gt;. Al suo interno troviamo i **campi** da compilare e almeno un **pulsante** per inviare i dati.

{% highlight html %}
<form action="registrazione.php" method="post">
  <label for="nome">Nome</label>
  <input type="text" id="nome" name="nome">

  <button type="submit">Invia</button>
</form>
{% endhighlight %}

Il tag **form** ha due attributi fondamentali:

* **action**: l'indirizzo (la pagina) a cui verranno inviati i dati quando l'utente preme il pulsante di invio. Se lo omettiamo, i dati vengono inviati alla pagina stessa.
* **method**: il modo in cui i dati vengono inviati. Può valere **get** oppure **post**.

#### GET o POST?

Con il metodo **GET** i dati vengono aggiunti in fondo all'indirizzo della pagina, dopo un punto interrogativo. Se cerchiamo "pizza" in una form di ricerca, il browser aprirà un indirizzo come questo:

```
https://www.esempio.it/cerca.php?parola=pizza
```

Con il metodo **POST** i dati viaggiano invece "dentro" la richiesta HTTP e non compaiono nella barra degli indirizzi.

Una regola pratica per scegliere:

* usiamo **GET** quando la form serve solo a **leggere** informazioni (una ricerca, un filtro): l'indirizzo risultante può essere salvato nei preferiti o condiviso;
* usiamo **POST** quando la form **modifica** qualcosa sul server (una registrazione, un ordine, l'inserimento di un record nel database) oppure quando contiene dati riservati come una password.

Attenzione: POST non significa "sicuro". I dati non sono visibili nella barra degli indirizzi, ma non sono cifrati. Per proteggerli serve una connessione **HTTPS**.

#### L'attributo name

Ogni campo che vogliamo inviare al server deve avere l'attributo **name**. È il nome con cui il server riconoscerà il dato ricevuto. Un campo senza `name` viene semplicemente ignorato al momento dell'invio.

{% highlight html %}
<input type="text" name="cognome">
{% endhighlight %}

Se l'utente scrive "Rossi" in questo campo, il server riceverà la coppia `cognome=Rossi`. In PHP, ad esempio, potremo leggerlo con `$_POST["cognome"]` (oppure `$_GET["cognome"]` se la form usa il metodo GET).

#### Le etichette: il tag label

Il tag **label** associa un testo descrittivo a un campo. Il collegamento avviene tramite l'attributo **for** della label, che deve avere lo stesso valore dell'attributo **id** del campo.

{% highlight html %}
<label for="email">Indirizzo email</label>
<input type="email" id="email" name="email">
{% endhighlight %}

Usare le label non è un dettaglio estetico:

* cliccando sull'etichetta, il cursore si posiziona automaticamente nel campo (molto comodo per checkbox e radio, che sono piccoli);
* i lettori di schermo usati dalle persone non vedenti leggono l'etichetta quando il campo riceve il focus, rendendo la form **accessibile**.

Notiamo la differenza tra **id** e **name**: `id` identifica l'elemento all'interno della pagina (serve alla label, al CSS, a JavaScript), mentre `name` è il nome con cui il dato viene inviato al server. Spesso hanno lo stesso valore, ma svolgono due compiti diversi.

#### Il tag input

Il tag **input** è il campo più utilizzato. Non ha un tag di chiusura e il suo comportamento cambia a seconda del valore dell'attributo **type**.

##### Testo e password

{% highlight html %}
<label for="utente">Nome utente</label>
<input type="text" id="utente" name="utente">

<label for="pwd">Password</label>
<input type="password" id="pwd" name="pwd">
{% endhighlight %}

Il tipo **password** funziona come il tipo text, ma nasconde i caratteri digitati sostituendoli con dei pallini.

##### Tipi specializzati

HTML5 ha introdotto molti tipi di input pensati per dati specifici. Il browser li visualizza in modo adatto (ad esempio un calendario per le date) e sugli smartphone mostra la tastiera più comoda (solo numeri, oppure la tastiera con la @ per le email).

{% highlight html %}
<input type="email" name="email">
<input type="number" name="eta" min="0" max="120">
<input type="date" name="nascita">
<input type="tel" name="telefono">
<input type="url" name="sito">
<input type="color" name="colore">
<input type="range" name="volume" min="0" max="10">
{% endhighlight %}

| Tipo | Utilizzo |
|------|----------|
| `email` | indirizzi email, controlla la presenza della @ |
| `number` | numeri, con eventuali limiti `min`, `max` e passo `step` |
| `date` | una data, scelta da un calendario |
| `tel` | numeri di telefono |
| `url` | indirizzi web |
| `color` | un colore, scelto da una tavolozza |
| `range` | un cursore per scegliere un valore in un intervallo |
| `file` | caricamento di un file dal proprio computer |
| `hidden` | un campo invisibile all'utente, ma inviato al server |

##### Checkbox

Le **checkbox** (caselle di spunta) permettono di scegliere **zero, una o più** opzioni.

{% highlight html %}
<p>Quali linguaggi conosci?</p>

<input type="checkbox" id="python" name="linguaggi[]" value="python">
<label for="python">Python</label>

<input type="checkbox" id="ruby" name="linguaggi[]" value="ruby" checked>
<label for="ruby">Ruby</label>

<input type="checkbox" id="cpp" name="linguaggi[]" value="cpp">
<label for="cpp">C++</label>
{% endhighlight %}

L'attributo **value** indica il valore inviato al server quando la casella è spuntata; se la casella non è spuntata non viene inviato nulla. L'attributo **checked** fa sì che la casella sia già spuntata al caricamento della pagina.

Le parentesi quadre in `name="linguaggi[]"` sono una convenzione di PHP: dicono al server di raccogliere tutti i valori selezionati in un array.

##### Radio button

I **radio button** permettono di scegliere **una sola** opzione tra diverse alternative. Tutti i radio dello stesso gruppo devono avere lo **stesso name**: è proprio questo che li lega tra loro, e selezionarne uno deseleziona automaticamente gli altri.

{% highlight html %}
<p>Livello di esperienza</p>

<input type="radio" id="base" name="livello" value="base" checked>
<label for="base">Base</label>

<input type="radio" id="intermedio" name="livello" value="intermedio">
<label for="intermedio">Intermedio</label>

<input type="radio" id="avanzato" name="livello" value="avanzato">
<label for="avanzato">Avanzato</label>
{% endhighlight %}

Il nome deriva dai pulsanti delle vecchie autoradio: premendo un tasto per scegliere una stazione, quello premuto in precedenza tornava su.

#### Il tag select

Il tag **select** crea un **menu a tendina**. Ogni voce è indicata da un tag **option**.

{% highlight html %}
<label for="regione">Regione</label>
<select id="regione" name="regione">
  <option value="">-- scegli --</option>
  <option value="lazio">Lazio</option>
  <option value="lombardia" selected>Lombardia</option>
  <option value="sicilia">Sicilia</option>
</select>
{% endhighlight %}

Come per le checkbox, al server arriva il contenuto dell'attributo **value** dell'opzione scelta, non il testo visualizzato. L'attributo **selected** indica l'opzione preselezionata.

Aggiungendo l'attributo **multiple** al tag select l'utente potrà scegliere più opzioni contemporaneamente (tenendo premuto Ctrl).

Il menu a tendina è preferibile ai radio button quando le opzioni sono molte: una lista di venti radio occuperebbe troppo spazio nella pagina.

#### Il tag textarea

Il tag **textarea** crea un campo di testo su **più righe**, adatto a messaggi, commenti e descrizioni. A differenza di input, ha un tag di chiusura: il testo eventualmente scritto tra apertura e chiusura è il contenuto iniziale del campo.

{% highlight html %}
<label for="messaggio">Messaggio</label>
<textarea id="messaggio" name="messaggio" rows="5" cols="40"></textarea>
{% endhighlight %}

Gli attributi **rows** e **cols** indicano rispettivamente il numero di righe e di colonne visibili.

#### I pulsanti

Il tag **button** crea un pulsante. Il suo comportamento dipende dall'attributo **type**:

* **submit**: invia la form (è il comportamento predefinito);
* **reset**: riporta tutti i campi al loro valore iniziale;
* **button**: non fa nulla da solo, viene usato insieme a JavaScript.

{% highlight html %}
<button type="submit">Invia</button>
<button type="reset">Cancella tutto</button>
{% endhighlight %}

Il pulsante reset è usato raramente: un clic per sbaglio cancella tutto il lavoro fatto dall'utente.

#### Raggruppare i campi: fieldset e legend

Nelle form più lunghe conviene raggruppare i campi che riguardano lo stesso argomento. Il tag **fieldset** disegna una cornice attorno al gruppo e il tag **legend** ne indica il titolo.

{% highlight html %}
<fieldset>
  <legend>Dati personali</legend>

  <label for="n">Nome</label>
  <input type="text" id="n" name="nome">

  <label for="c">Cognome</label>
  <input type="text" id="c" name="cognome">
</fieldset>
{% endhighlight %}

#### Attributi utili

Ci sono alcuni attributi che possiamo aggiungere ai campi per migliorare l'esperienza dell'utente:

| Attributo | Effetto |
|-----------|---------|
| `placeholder` | mostra un testo di suggerimento grigio che scompare quando si inizia a scrivere |
| `value` | imposta il valore iniziale del campo |
| `required` | il campo è obbligatorio, la form non viene inviata se è vuoto |
| `minlength` / `maxlength` | numero minimo e massimo di caratteri |
| `min` / `max` | valore minimo e massimo (numeri e date) |
| `pattern` | un'espressione regolare che il testo deve rispettare |
| `readonly` | il campo si vede ma non si può modificare (e viene inviato) |
| `disabled` | il campo è disattivato (e **non** viene inviato) |
| `autofocus` | il cursore si posiziona su questo campo al caricamento della pagina |

{% highlight html %}
<input type="text" name="cap" placeholder="es. 00100"
       required pattern="[0-9]{5}" maxlength="5">
{% endhighlight %}

Attenzione: il `placeholder` **non sostituisce la label**. Scompare appena si inizia a scrivere, e l'utente rischia di dimenticare cosa doveva inserire.

#### La validazione

Attributi come `required`, `type="email"`, `min`, `max` e `pattern` fanno sì che sia il **browser** a controllare i dati prima dell'invio, mostrando un messaggio di errore se qualcosa non va. Si parla di **validazione lato client**.

È comoda per l'utente, che scopre subito l'errore, ma **non è una protezione**: chiunque può disattivarla o inviare dati al server senza passare dalla form. Per questo i dati vanno **sempre ricontrollati anche sul server** (validazione lato server) prima di salvarli o usarli.

#### Un esempio completo

Mettiamo insieme quello che abbiamo visto in un modulo di iscrizione a un corso.

{% highlight html %}
<!doctype html>
<html lang="it">
<head>
  <meta charset="utf-8">
  <title>Iscrizione al corso</title>
</head>
<body>
  <h1>Iscrizione al corso di programmazione</h1>

  <form action="iscrizione.php" method="post">

    <fieldset>
      <legend>Dati personali</legend>

      <p>
        <label for="nome">Nome</label>
        <input type="text" id="nome" name="nome" required>
      </p>

      <p>
        <label for="email">Email</label>
        <input type="email" id="email" name="email"
               placeholder="nome@esempio.it" required>
      </p>

      <p>
        <label for="nascita">Data di nascita</label>
        <input type="date" id="nascita" name="nascita">
      </p>
    </fieldset>

    <fieldset>
      <legend>Il corso</legend>

      <p>
        <label for="corso">Corso</label>
        <select id="corso" name="corso" required>
          <option value="">-- scegli --</option>
          <option value="python">Python</option>
          <option value="ruby">Ruby</option>
          <option value="web">HTML e CSS</option>
        </select>
      </p>

      <p>Frequenza</p>
      <p>
        <input type="radio" id="presenza" name="frequenza" value="presenza" checked>
        <label for="presenza">In presenza</label>

        <input type="radio" id="online" name="frequenza" value="online">
        <label for="online">Online</label>
      </p>

      <p>
        <label for="note">Note</label><br>
        <textarea id="note" name="note" rows="4" cols="40"></textarea>
      </p>
    </fieldset>

    <p>
      <input type="checkbox" id="privacy" name="privacy" value="si" required>
      <label for="privacy">Accetto il trattamento dei dati personali</label>
    </p>

    <button type="submit">Iscriviti</button>
  </form>
</body>
</html>
{% endhighlight %}

Possiamo salvare questo codice in un file `.html` e aprirlo con il browser: provando a premere "Iscriviti" senza compilare i campi obbligatori vedremo la validazione all'opera. Naturalmente, finché non scriviamo la pagina `iscrizione.php`, l'invio vero e proprio non porterà da nessuna parte.

#### E poi?

Una form, da sola, raccoglie i dati ma non sa cosa farne: serve un programma lato server che li riceva, li controlli e li elabori. Per vedere le form all'opera in una applicazione completa possiamo leggere:

* [CRUD: creare autori e libri]({{ site.baseurl }}{% link _web/applicazione-web-crud-creare.md %}.html), dove una form invia i dati a una pagina PHP che li salva in un database MySQL;
* [Le form con Flask]({{ site.baseurl }}{% link _flask/flask-form.md %}.html), dove gli stessi concetti vengono applicati in Python.
