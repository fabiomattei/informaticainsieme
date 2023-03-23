---
title: 'Sending and receiving Python Objects with sockets'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

Fino ad ora abbiamo scambiato messaggi di testo tra client e server. Andiamo a vedere cosa bisogna fare per scambiarsi oggetti attraverso la tecnica di serializzazione con la libreria pinkle.

In Python ogni cosa è un oggetto e ogni oggetto più essere serializzato utilizzando Pickle. Serializzare significa convertire le informazioni contenuti nelle proprietà di un oggetto in byte.

I byte saranno inviati su di un sockets. Questo perché ogni cosa può essere inviata attraverso socket.

Cominciamo a scaldarci, creiamo un dizionario e ceonvertiamolo in una stringa di byte utilizzando pinckle:

{% highlight shell %}
>>> import pickle
>>> d = {1:"hi", 2: "there"}
>>> msg = pickle.dumps(d)
>>> msg
b'\x80\x03}q\x00(K\x01X\x02\x00\x00\x00hiq\x01K\x02X\x05\x00\x00\x00thereq\x02u.'
{% endhighlight %}

Ora, questo costituisce il nostro messaggio che possiamo spedire. Una volta ricevuto andremo a fare:

{% highlight shell %}
>>> recd = pickle.loads(msg)
>>> recd
{1: 'hi', 2: 'there'}
{% endhighlight %}

Quindi nel nostro server dovremo scrivere:

{% highlight python %}
d = {1:"hi", 2: "there"}
msg = pickle.dumps(d)
msg = bytes(f"{len(msg):<{HEADERSIZE}}", 'utf-8')+msg
print(msg)
clientsocket.send(msg)
{% endhighlight %}

### server.py completo

{% highlight python %}
import socket
import time
import pickle


HEADERSIZE = 10

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1243))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")

    d = {1:"hi", 2: "there"}
    msg = pickle.dumps(d)
    msg = bytes(f"{len(msg):<{HEADERSIZE}}", 'utf-8')+msg
    print(msg)
    clientsocket.send(msg)
{% endhighlight %}

Anche in questo caso stiamo spedendo un header che riposta il conteggio dei byte che costituiscono il messaggio. Ora il nostro client deve essere in grado di gestire la cosa. Dunque cambiamo la stringa di inzializzazione:

{% highlight python %}
full_msg = b''
{% endhighlight %}

Quindi procediamo come sempre:

{% highlight python %}
full_msg += msg
{% endhighlight %}

Quando il messaggio è completo utilizziamo **pickel.loads()** per riconvertire da byte a dizionario:

{% highlight python %}
if len(full_msg)-HEADERSIZE == msglen:
     print("full msg recvd")
     print(full_msg[HEADERSIZE:])
     print(pickle.loads(full_msg[HEADERSIZE:]))

     new_msg = True
     full_msg = b""
{% endhighlight %}

### client.py completo

{% highlight python %}
import socket
import pickle

HEADERSIZE = 10

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1243))

while True:
    full_msg = b''
    new_msg = True
    while True:
        msg = s.recv(16)
        if new_msg:
            print("new msg len:",msg[:HEADERSIZE])
            msglen = int(msg[:HEADERSIZE])
            new_msg = False

        print(f"full message length: {msglen}")

        full_msg += msg

        print(len(full_msg))

        if len(full_msg)-HEADERSIZE == msglen:
            print("full msg recvd")
            print(full_msg[HEADERSIZE:])
            print(pickle.loads(full_msg[HEADERSIZE:]))
            new_msg = True
            full_msg = b""
{% endhighlight %}

Mandando in esecuzione ottengo:

{% highlight shell %}
$ python3 server.py
Connection from ('192.168.86.25', 50373) has been established.
b'33        \x80\x03}q\x00(K\x01X\x02\x00\x00\x00hiq\x01K\x02X\x05\x00\x00\x00thereq\x02u.'
{% endhighlight %}

E per il client:

{% highlight shell %}
$ python3 client.py
new msg len: b'33        '
full message length: 33
16
full message length: 33
32
full message length: 33
43
full msg recvd
b'\x80\x03}q\x00(K\x01X\x02\x00\x00\x00hiq\x01K\x02X\x05\x00\x00\x00thereq\x02u.'
{1: 'hi', 2: 'there'}
{% endhighlight %}
