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


In the more traditional sense of client and server, you wouldnt actually have the client and server on the same machine. If you wanted to have two programs talking to eachother locally, you could do this, but typically your client will more likely connect to some external server, using its public IP address, not socket.gethostname(). You will pass the string of the IP instead.

Full client.py code up to this point:

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

Okay, now we just run both. First, let's run our server:

python3 server.py

python3 client.py

On our server, we should see:

Connection from ('192.168.86.34', 54276) has been established.

Our client, however, just exits after that, because it has completed its job.

So we've made a connection, and that's cool, but we really want to send messages and/or data back and forth. How do we do that?

Our sockets can send and recv data. These methods of handling data deal in buffers. Buffers happen in chunks of data of some fixed size. Let's see that in action:

Inside server.py, let's add:

    clientsocket.send(bytes("Hey there!!!","utf-8"))

Into our while loop, so our full code for server.py becomes:

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1234))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")
    clientsocket.send(bytes("Hey there!!!","utf-8"))

So we've sent some data, now we want to receive it. So, in our client.py, we'll do:

msg = s.recv(1024)

This means our socket is going to attempt to receive data, in a buffer size of 1024 bytes at a time.

Then, let's just do something basic with the data we get, like print it out!

print(msg.decode("utf-8"))

Cool, our full client.py code is now:

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

msg = s.recv(1024)
print(msg.decode("utf-8"))

Now, run both server.py and then client.py. Our server.py shows:

Connection from ('192.168.86.34', 55300) has been established.

While our client.py now shows:

Hey there!!!

And it exits. Okay, so let's adjust that buffer slightly, changing the client.py recv to be in 8 bytes at a time.

client.py

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))

msg = s.recv(8)
print(msg.decode("utf-8"))

Now, re-run the client.py, and you will instead see something like:

Hey ther

Not lookin so hot! So you might realize that literally adds up to 8 characters, so each byte is a character. Why not... go back to 1024? or some massive number. Why work in buffers at all?

At some point, no matter what number you set, many applications that use sockets will eventually desire to send some amount of bytes far over the buffer size. Instead, we need to probably build our program from the ground up to actually accept the entirety of the messages in chunks of the buffer, even if there's usually only one chunk. We do this mainly for memory management. The calculations depending on application can vary, and you're free to play with the buffer size later. The only thing I can for sure promise is: you need to plan from the beginning to handle communications in chunks.

For our client, how might we do this? A while loop sounds like it could fit the bill. Data will come in as a stream, so really, handling for this is as simple as changing our client.py file to:

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1234))


while True:
    msg = s.recv(8)
    print(msg.decode("utf-8"))

So, at the moment, we will receive this data and print it in chunks. If we run client.py now, we see:

Hey ther
e!!!

You should also take note that our client.py no longer exits. This connection now is remaining open. This is due to our while loop. We can use .close() on a socket to close it if we wish. We can do this either on the server, or on the client...or both. It's probably a good idea to be prepared for either connection to drop or be closed for whatever reason. For example, we could close the connection after we send our message on the server:

server.py

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

If we run this, however, we will see our client.py then spams out a bunch of nothingness, because the data it's receiving, is, well, nothing. It's empty. 0 bytes, but we are still asking it to print out what it receives, even if that's nothing! We could fix that:

client.py

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

So, now we are buffering through the full message. When we reach the end, which we're noting by getting 0 bytes, we break, and then return the message. This then concludes client.py. Now, client probably wants to also maintain the connection. How might we do that? Another while loop could do the trick.

client.py

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

Of course, we probably should yet again make sure the full_msg has something of substance before we print it out:

client.py

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

This works, but we have issues. What happens when we stop closing the client socket on the server's side? We never actually get a message! Why's this?

TCP is a communication *stream*...so how do we actually know when a message is actually happening? Generally, we need sort of way to notify the receiving socket about the message and how big it will be. There are many ways that we can do this. One popular way is to use a sort of header that always leads our message. We could also use some sort of footer, but this could cause trouble should someone learn about our methods.

We will be working on this in the next tutorial.

