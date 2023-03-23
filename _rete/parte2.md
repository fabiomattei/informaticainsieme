---
title: 'Buffering and streaming data'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

Nella lezione precedente abbiamo imparato come mandare e ricevere dati attraverso un socket e abbiamo sollevato il problema di cosa succede quando il nostro messaggio è più grande della dimensione del nostro buffer. Ora risolveremo questo problema.

Come detto in precedenza ci sono delle strate per gestire la cosa a livello logico. La soluzione più gettonata consiste nel cominciare tutti i messaggi con un header che contiene la lunghezza del messaggio che sta per arrivare. La sfida successiva consiste nel normalizzare questo header in qualche modo. Potremmo considerare di usare una serie di caratteri per dividerlo dal resto del messaggio ma poi si corre il rischio che gli utenti non interpretino bene oppure modifichino questa informazione di proposito. Si può risolvere la cosa con un header di lunghezza fissa, sempre uguale, dove i primi n byte contengono i dati dell'header cioè la lunghezza del messaggio che sta per arrivare.

A questo punto ci resta da definire una lunghezza massima di messaggio vera e propria, ad esempio 1000000000 byte. Nessuno dovrebbe aver bisogno di mandare messaggi così lunghi quindi per il momento possiamo definire questa quantità come la quantità massima supportata. Questo numero viene spedito come insieme di caratteri ed utilizza 10 byte. In python per scrivere questo numero utilizziamo la formattazione di stringhe:

{% highlight shell %}
# Allineiamo il testo con una lunghezza specifica
>>> '{:<30}'.format('left aligned')
'left aligned                  '
>>> '{:>30}'.format('right aligned')
'                 right aligned'
>>> '{:^30}'.format('centered')
'           centered           '
>>> '{:*^30}'.format('centered')  # usa '*' come carattere di riempimento
'***********centered***********'
{% endhighlight %}

In questo caso possiamo vedere vari esempi in cui vengono generate stringhe della lunghezza di 30 caratteri. Di solito questa funzionalità viene utilizzata per fare delle interfacce utente carine ad ogni modo la possiamo usare per i nostri scopi.

{% highlight python %}
f'{len("your message here!"):<10}'
{% endhighlight %}

Nel caso in alto il comando produrrà una stringa di 10 caratteri.

{% highlight shell %}
>>> f'{len("your message here!"):<10}'
'18        '
{% endhighlight %}

Quello che facciamo ora è utilizzare questa tecnica a inizio messaggio in modo da indicarne la lunghezza con i primi 10 caratteri.

### server.py

{% highlight python %}
import socket

HEADERSIZE = 10

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1241))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")

    msg = "Welcome to the server!"
    msg = f"{len(msg):<{HEADERSIZE}}"+msg

    clientsocket.send(bytes(msg,"utf-8"))
{% endhighlight %}

Come possiamo vedere ora il nostro messaggio contiene un header che informa il client della lunghezza del messaggio.

### client.py

{% highlight python %}
import socket

HEADERSIZE = 10

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1241))

while True:
    full_msg = ''
    new_msg = True
    while True:
        msg = s.recv(16)
        if new_msg:
            print("new msg len:",msg[:HEADERSIZE])
            msglen = int(msg[:HEADERSIZE])
            new_msg = False

        print(f"full message length: {msglen}")

        full_msg += msg.decode("utf-8")

        print(len(full_msg))


        if len(full_msg)-HEADERSIZE == msglen:
            print("full msg recvd")
            print(full_msg[HEADERSIZE:])
            new_msg = True
{% endhighlight %}

Il client è un pochino più intricato. Abbiamo incrementato il buffer a 16 byte perché 8 non sarebbero stati abbastanza per includere l'header. Iniziamo in uno stato in cui i primi dati che riceviamo sono l'inizio di un messaggio.



Dato che il messaggio è un nuovo messaggio, prima di tutto dobbiamo parsare l'header che sappiamo già avere una lunghezza predefinita di 10 caratteri. Fatto questo andiamo a parsare il messaggio vero e proprio. Da qui continuiamo a costruire il nostro messaggio fino a quando var raggiunge la grandezza di msglen sommato ad HEADERSIZE. Quando questo avviene stampiamo il messaggio completo.

Andare da questo a una specie di API di è abbastanza semplice. Facciamo un esempio in cui il server manda in stream qualcosa di semplice come l'ora corrente.

A queto fine aggiungiamo alla fine dello script:

{% highlight python %}
    while True:
        time.sleep(3)
        msg = f"The time is {time.time()}"
        msg = f"{len(msg):<{HEADERSIZE}}"+msg

        print(msg)

        clientsocket.send(bytes(msg,"utf-8"))
{% endhighlight %}

server.py completo:

{% highlight python %}
import socket
import time


HEADERSIZE = 10

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((socket.gethostname(), 1243))
s.listen(5)

while True:
    # now our endpoint knows about the OTHER endpoint.
    clientsocket, address = s.accept()
    print(f"Connection from {address} has been established.")

    msg = "Welcome to the server!"
    msg = f"{len(msg):<{HEADERSIZE}}"+msg

    clientsocket.send(bytes(msg,"utf-8"))

    while True:
        time.sleep(3)
        msg = f"The time is {time.time()}"
        msg = f"{len(msg):<{HEADERSIZE}}"+msg

        print(msg)

        clientsocket.send(bytes(msg,"utf-8"))
{% endhighlight %}

Ora non cambia nulla per il nostro client, eccetto la preparazione che ci occorre fare per accettare un messaggio successivo al primo; dobbiamo resettare la variabile **full_msg**:

{% highlight python %}
import socket

HEADERSIZE = 10

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((socket.gethostname(), 1243))

while True:
    full_msg = ''
    new_msg = True
    while True:
        msg = s.recv(16)
        if new_msg:
            print("new msg len:",msg[:HEADERSIZE])
            msglen = int(msg[:HEADERSIZE])
            new_msg = False

        print(f"full message length: {msglen}")

        full_msg += msg.decode("utf-8")

        print(len(full_msg))


        if len(full_msg)-HEADERSIZE == msglen:
            print("full msg recvd")
            print(full_msg[HEADERSIZE:])
            new_msg = True
            full_msg = ""
{% endhighlight %}

Ora mandiamo il tutto in esecuzione e sul server dovremmo vedere:

{% highlight shell %}
28        The time is 1552068299.01783
30        The time is 1552068302.0181189
30        The time is 1552068305.0206459
29        The time is 1552068308.021842
29        The time is 1552068311.024837
29        The time is 1552068314.025016
28        The time is 1552068317.02619
29        The time is 1552068320.026504
29        The time is 1552068323.031633
30        The time is 1552068326.0359411
29        The time is 1552068329.039903
29        The time is 1552068332.040124
30        The time is 1552068335.0402749
27        The time is 1552068338.0437
29        The time is 1552068341.043971
{% endhighlight %}

mentre sul client:

{% highlight shell %}
new msg len: b'27        '
full message length: 27
16
full message length: 27
32
full message length: 27
37
full msg recvd
The time is 1552068338.0437
new msg len: b'29        '
full message length: 29
16
full message length: 29
32
full message length: 29
39
full msg recvd
The time is 1552068341.043971
{% endhighlight %}
