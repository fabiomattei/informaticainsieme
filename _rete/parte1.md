---
title: 'Spedire e ricevere dati'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

La libreria socket, che utlizzeremo per fare servizi di rete, fa parte della libreria standard di python.

{% highlight python %}
import socket

# create the socket
# AF_INET == ipv4
# SOCK_STREAM == TCP
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
{% endhighlight %}

La variabile **s** è il nostro socket TCP/IP. Al momento della creazione di questa istanza dobbiamo specificare:

* AF_INET è il riferimento alla famiglia o dominio, significa ipv4 all'opposto c'è AF_INET6 che identifica ipv6;
* SOCK_STREAM identifica un socket TCP, il nostro tipo di socket; TCP sarà connection-oriented in opposto a connectionless.

Quindi cos'è un socket? Un socket è un punto di congiunzione in una comunicazione tra una applicazione e una rete.
Un socket è legato ad una porta e ad un host. In generale voi avrete sempre un programma per il client ed un programma per il server collegati attraverso socket.

Quando si va a creare il server bisogna collegare (bind) un socket ad una porta del server. 
Quando si va a creare il client, ci si va a collegare, sempre utilizzando un socket, a quel server nella porta che si è specificato in precedenza.

### Il server

{% highlight python %}
s.bind((socket.gethostname(), 1234))
{% endhighlight %}

Per dare la possibilità di accettare una connessione bisogna chiamare il metodo bind passando una tupla che contiene il nome dell'host e il numero della porta.

Fatto questo si resta in ascolto delle connessioni che verranno richieste. Possiamo gestire una sola connessione per volta per cui è bene creare una sorta di coda. Se la coda si riempie ulteriori connessioni verranno negate.
Predisponiamo una coda con 5 posti:

{% highlight python %}
s.listen(5)
{% endhighlight %}

Ed ora restiamo in ascolto:

{% highlight python %}
while True:
    clientsocket, address = s.accept()
    print(f"Connessione stabilita con: {address}.")
{% endhighlight %}

Il codice completo per il file server.py:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    clientsocket, address = s.accept()
    print(f"Connessione stabilita con: {address}.")
{% endhighlight %}

### Il client

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
{% endhighlight %}

Ora, dato che siamo nel client, ci andiamo a collegare alla porta lasciata aperta sul server:

{% highlight python %}
s.connect((socket.gethostname(), 1234))
{% endhighlight %}

Nel senso più tradizionale della logica client/server voi non avreste client e server sulla stessa macchina. Se aveste bisogno di uno scambio di dati tra due programmi "in locale" potreste otternelo in modo diverso. Quindi normalmente non vi andreste mai a collegare a **localhost** o **127.0.0.1** ma passereste una stringa contenete un indirizzo IP, ad esempio "175.123.78.96".

A questo punto il file client.py conterrà:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))
{% endhighlight %}

Benissimo, avviamo entrambi i programmi. Prima avviamo il server:

{% highlight shell %}
python3 server.py
{% endhighlight %}

E poi avviamo il client:

{% highlight shell %}
python3 client.py
{% endhighlight %}

Nel nostro server dovremmo vedere:

{% highlight shell %}
Connessione stabilita con ('192.168.86.34', 54276).
{% endhighlight %}

Il nostro client ad ogni modo termina non appena completato il suo lavoro.

### Inviamo qualche dato

Dunque ora abbiamo una connessione. Ma come facciamo a mandare dati avanti e indietro?

La nostra socket può mandare (send) e ricevere (recv) dati. Questi metodi lavorano su buffer. I Buffer sono portizioni di memoria di grandezza fissa. Vediamoli in azione. All'interno di server.py aggiungiamo:

{% highlight python %}
clientsocket.send(bytes("Ciao amico!!","utf-8"))
{% endhighlight %}

All'interno di un ciclo while il nostro codice diventa:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    clientsocket, address = s.accept()
    print(f"Connessione stabilita con: {address}.")
    clientsocket.send(bytes("Ciao amico!!","utf-8"))
{% endhighlight %}

Ora che abbiamo mandato un po' di dati, vogliamo essere in grado di riceverli dall'altra parte, quindi in client.py faremo:

{% highlight python %}
msg = s.recv(1024)
{% endhighlight %}

Questo significa che il nostro socket proverà a ricevere dati, in un buffer di 1024 byte per volta.

{% highlight python %}
print(msg.decode("utf-8"))
{% endhighlight %}

Il codice completo

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

msg = s.recv(1024)
print(msg.decode("utf-8"))
{% endhighlight %}

Ora lanciamo insieme server.py e client.py, il nostro server.py mostra:

{% highlight python %}
Connessione stabilita con ('192.168.86.34', 54276).
{% endhighlight %}

Mentre il nostro client mostra:

{% highlight python %}
Ciao amico!!
{% endhighlight %}

E poi il programma finisce.

### Il buffer 

Modifichiamo il buffer per fare in modo che la funzione recv in client.py riceva 8 byte alla volta.

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

msg = s.recv(8)
print(msg.decode("utf-8"))
{% endhighlight %}

Se ora lanciamo di nuovo il client vedremo:

{% highlight shell %}
Ciao ami
{% endhighlight %}

In effetti ricordiamo che ogni byte rappresenta un carattere. Ad ogni modo è meglio tornare a 1024.

Ci sarà un momento in cui non importa quanto faremo grande il buffer, questa memoria non basterà. A tal proposito è bene tenere presente che dovremo organizzare l'applicazione in modo che funzioni a pacchetti di dati. La grandezza del buffer varia da applicazione ad applicazione ma se organizziamo lo scambio dati in pacchetti la grandezza del buffer non sarà un problema.

A questo proposito organizziamo il programma client.py con un ciclo while che non termina mai.:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

while True:
    msg = s.recv(8)
    print(msg.decode("utf-8"))
{% endhighlight %}

Se lanciamo il programma adesso vedremo:

{% highlight shell %}
Ciao ami
co!!
{% endhighlight %}

Come poi notare stiamo ricevendo i dati "a pezzi".

Dovresti anche aver notato che il nostro client.py non chiude mai la connessione. La connessione rimane sempre aperta e la cosa è dovuta al nostro ciclo while. Possiamo usare il metodo *.close()* sulla istanza di un socket al fine di chiudere la comunicazione se lo desideriamo. Possiamo farlo sia sul server sia sul client, il primo che chiude la connessione interrompe la trasmissione di dati. Per questo è una buona idea essere preparati da entrambe le parte ad una connessione che si chiude, se per caso dovesse essere chiusa dall'altra capo.

### server.py

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    clientsocket, address = s.accept()
    print(f"Connessione stabilita con: {address}.")
    clientsocket.send(bytes("Ciao amico!!","utf-8"))
    clientsocket.close()
{% endhighlight %}

Se lanciamo questo programma però vedremo il nostro client.py che non tira fuori nulla, perchè i dati che sta ricevendo in effetti sono nulla, sono vuoti, 0 byte, ma stiamo continuiamo a dirgli di stampare ciò che riceve anche se non riceve nulla. Aggiustiamolo!

### client.py

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

full_msg = ''
while True:
    msg = s.recv(8)
    if len(msg) <= 0:
        break
    full_msg += msg.decode("utf-8")

print(full_msg)
{% endhighlight %}

In questo esempio stiamo utilizzando una variabile che fa da memoria buffer per accumulare al suo interno tutti i dati. Quando non si ricevono più dati interrompiamo il ciclo e restituiamo il messaggio. Tutto questo conclude il client.py, ora anche il client probabilmente vuole mantenere la connessione. Come faremo? Un altro ciclo while ci risolverà il problema.

### client.py

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))


while True:
    full_msg = ''
    while True:
        msg = s.recv(8)
        if len(msg) <= 0:
            break
        full_msg += msg.decode("utf-8")

    print(full_msg)
{% endhighlight %}

Certo dovremmo sincerarci che **full_msg** contenga qualche informazione prima di scriverlo:

### client.py

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))


while True:
    full_msg = ''
    while True:
        msg = s.recv(8)
        if len(msg) <= 0:
            break
        full_msg += msg.decode("utf-8")

    if len(full_msg) > 0:
        print(full_msg)
{% endhighlight %}

Tutto questo funziona ma ci sono delle questioni che restano aperte. Cosa succede se non chiudiamo la connessione dal server? Non riceveremo mai un messaggio. Perché?

TCP è uno *stream* di comunicazione, quindi come sappiamo se una connessione aperta sta scambiando informazioni in un certo momento? Generalmente abbiamo bisogno di un modo per notificare alla socket che stiamo per mandare un messaggio e quanto è grande questo messaggio. Ci sono molti modi per farlo. Un modo molto popolare consiste nel mandare una testata (header) che ha al suo interno queste informazioni. Potremmo anche utilizzare una sorta di footer ma questo potrebbe creare problemi.
