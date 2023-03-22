---
title: 'Spedire e ricevere dati'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

La libreria socket fa parte della libreria standard di python.

{% highlight python %}
import socket

# create the socket
# AF_INET == ipv4
# SOCK_STREAM == TCP
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
{% endhighlight %}

La variabile s è il nostro socket TCP/IP. 
AF_INET è il riferimento alla famiglia o dominio, significa ipv4 all'opposto c'è AF_INET6 che identifica ipv6. 
SOCK_STREAM identifica un socket TCP, il nostro tipo di socket. 
TCP sarà connection-oriented in opposto a connectionless.

Quindi cos'è un socket? Un socket è un punto di congiunzione in una comunicazione tra una applicazione e una rete.
Un socket è legato ad una porta e ad un host. In generale voi avrete sempre un programma per il client ed un programma per il server collegati attraverso un socket.

Nel caso del server voi collegherete (bind) un socket ad una porta del server. 
Nel caso del client, collegherete un socket a quel server nella stessa porta che il server ha aperto.

Scriviamo un po' di codice per il lato server:

{% highlight python %}
s.bind((socket.gethostname(), 1234))
{% endhighlight %}

Per dare la possibilità di accettare una connessione bisogna chiamare il metodo bind passando una tupla che contiene il nome dell'host e il numero della porta.

Ora che abbiamo fatto questo possiamo restare in ascolto per le connessioni che verranno richieste. Noi possiamo gestire una sola connessione per volta per cui vogliamo per cui è bene creare una sorta di coda. Se la coda si riempie ulteriori connessioni verranno negate.

Facciamo una coda con 5 posti:

{% highlight python %}
s.listen(5)
{% endhighlight %}

Ed ora restiamo in ascolto:

{% highlight python %}
while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")
{% endhighlight %}

Il codice completo per il file server.py:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")
{% endhighlight %}

Ora abbiamo bisogno del codice per il client:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
{% endhighlight %}

Ora, dato che siamo nel client, piuttosto che lasciare aperta una porta (binding) ci andiamo a collegare.

{% highlight python %}
s.connect((socket.gethostname(), 1234))
{% endhighlight %}

Nel senso più tradizionale della logica client/server non client e server voi non avreste client e server sulla stessa macchina. Se aveste bisogno di uno scambio di dati tra due programmi "in locale" potreste otternelo in modo diverso. Quindi normalmente non vi andreste mai a collegare a **localhost** o **127.0.0.1** ma passereste una stringa contenete un indirizzo IP, ad esempio "175.123.78.96".

A questo punto il file client.py conterrà:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))
{% endhighlight %}

Benissimo, avviamo entrambi i programmi. Prima avviamo il server:

python3 server.py

E poi avviamo il client:

python3 client.py

Nel nostro server dovremmo vedere:

{% highlight shell %}
Connection from ('192.168.86.34', 54276) has been established.
{% endhighlight %}

Il nostro client ad ogni modo termina non appena completato il suo lavoro.

Dunque ora abbiamo una connessione. Ma come facciamo a mandare dati avanti e indietro?

La nostra socket può mandare (send) e ricevere (recv) dati. Questi metodi lavorano su buffer. I Buffer sono portizioni di memoria di grandezza fissa prefissata. Vediamoli in azione

All'interno di server.py aggiungiamo

{% highlight python %}
    clientsocket.send(bytes("Hey there!!!","utf-8"))
{% endhighlight %}

All'interno di un ciclo while il nostro codice diventa:

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")
    clientsocket.send(bytes("Hey there!!!","utf-8"))
{% endhighlight %}

Ora che abbiamo mandato un po' di dati, vogliamo essere in grado di riceverli dallìaltra parte, quindi in client.py faremo:

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
Connection from ('192.168.86.34', 55300) has been established.
{% endhighlight %}

Mentre il nostro client mostra:

{% highlight python %}
Hey there!!!
{% endhighlight %}

E poi il programma finisce.
Ora aggiustiamo un po' quel buffer per fare in modo che la funzione recv in client.py riceverà 8 byte alla volta.

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

msg = s.recv(8)
print(msg.decode("utf-8"))
{% endhighlight %}

Se ora lanciamo di nuovo il client vedremo:

{% highlight shell %}
Hey ther
{% endhighlight %}

Ora capiamo che ogni byte rappresenta un carattere. Ad ogni modo è meglio tornare a 1024.

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

So, at the moment, we will receive this data and print it in chunks. If we run client.py now, we see:

{% highlight shell %}
Hey ther
e!!!
{% endhighlight %}

Dovresti anche aver notate che il nostro client.py non esiste più. Questa connessione adesso rimane aperta e la cosa è dovuta al nostro ciclo while. Possiamo usare il metodo *.close()* sulla istanza di un socket al fine di chiudere la comunicazione se lo desideriamo. Possiamo farlo sia sul server sia sul client, il primo che chiude la connessione interrompe la trasmissione di dati. Per questo è una buona idea essere preparati da entrambe le parte ad una connessione che si chiude, se per caso dovesse essere chiusa dall'altra capo.

### server.py

{% highlight python %}
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")
    clientsocket.send(bytes("Hey there!!!","utf-8"))
    clientsocket.close()
{% endhighlight %}

If we run this, however, we will see our client.py then spams out a bunch of nothingness, because the data it's receiving, is, well, nothing. It's empty. 0 bytes, but we are still asking it to print out what it receives, even if that's nothing! We could fix that:

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

Tutto questo funziona ma ci sono delle questioni che restano aperte. Cosa succede se non chiudiamo la connessione dal server? Non non riceveremo mai un messaggio. Perché?

TCP è uno *stream* di comunicazione, quindi come sappiamo se una connessione aperta sta scambiando informazioni in un certo momento? Generalmente abbiamo bisogno di un modo per notificare alla socket che stiamo per mandare un messaggio e quanto è grande questo messaggio. Ci sono molti modi per farlo. Un modo molto popolare consiste nel mandare una testata (header) che ha al suo interno queste informazioni. Potremmo anche utilizzare una sorte di footer ma questo potrebbe creare problemi.
