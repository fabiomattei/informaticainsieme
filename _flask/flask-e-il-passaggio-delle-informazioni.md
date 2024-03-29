---
title: 'Flask e il passaggio delle informazioni'
date: '2021-11-11T05:52:06+01:00'
author: Fabio Mattei
layout: post
guid: 'https://www.esercizidiinformatica.it/?p=806'
---

Quando sviluppiamo una applicazione abbiamo due modi di passare le infomazioni che coincidono con due diversi tipi di chiamate: **GET** e **POST**.

Abbiamo già visto la chiamata GET che consiste nel passare le informazioni attraverso la URL ora vediamo le chiamate POST.

Una chiamata **POST** si verifica quando mi trovo di fronte ad una form che ha al suo interni diversi campi e un bottone per spedire i dati:


{% highlight html %}
<html>
   <body>
      <form action = "http://localhost:5000/login" method = "post">
         <p>Inserisci le credenziali:</p>
         <p><input type = "text" name = "name" /></p>
         <p><input type = "password" name = "password" /></p>
         <p><input type = "submit" value = "submit" /></p>
      </form>   
   </body>
</html>
{% endhighlight %}

</div>Questa pagina contiene una form HTML che punta alla pagina route login del controller e utilizza il metodo POST come specificato alla riga 3.

Nelle riga 5 compare un input field, che corrisponde ad uno spazio nella pagina in cui l’utente può inserire dei dati.

Nella riga 6 compare un password field, il cui funzionamento è analogo all’input field, ma a differenza di questo non va a mostrare all’utente quanto digitato.

Alla fine compare un submit button, che permette di inviare i dati al server.

#### Il lato controller

{% highlight python %}
@app.route('/login', methods=['POST', 'GET'])
def login():
    if request.method == 'GET':
        error = None
        return render_template('login.html', error=error)
    else:   # significa request method = POST
        if request.form['username'] == 'gino' and request.form['password'] == 'gino':
            return render_template('paginasegreta.html')
        else:
            error = 'Invalid username/password'
            return render_template('login.html', error=error)
        
{% endhighlight %}

Proviamo a realizzare la logica che dal lato controller si occupa del log in.

Vediamo che dal lato controller viene specificato nella direttiva @app.route nella riga 1 che la funzione login() risponde alle chiamate GET e POST alla route /login.

Se il metodo richiesto è GET viene mostrata la pagina che ha al suo interno la form per il login.

Se il metodo richiesto è POST vuol dire che la form è stata precedentemente mostrata e l’utente ha mandato dei dati. I dati sono contenuti nel **dizionario request.form**. Le chiavi del dizionario corrispondono agli **attributi name** che abbiamo specificato nella definizione dell’html della form.

Se le informazioni inviate corrispondono agli username e password che il sistema conosce viene visualizzata paginasegreta.html in caso contrario viene mostrato un messaggio di errore e si torna alla pagina di login.

