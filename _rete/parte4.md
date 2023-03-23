---
title: 'Progetto Chat: il server'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

Ora che abbiamo fatto tutto questo percorso proviamo a costruire qualcosa: una chat che funziona su console.

## Cominciamo con il server

Il server deve essere in grado di accettare nuove connessione dai vari client. A questo proposito dobbiamo inventare un meccanismo per poter identificare in maniera univoca gli utenti. Potremmo procedere mostrandi gli indirizzi IP ma di solito si preferisce procedere con uno username. Al di la di questo il lavoro del server sarà ricevere un messaggio da un utente e distribuirlo a tutti gli altri utenti.

Iniziamo con i giusti import e impostando alcune variabili:

{% highlight python %}
import socket
import select

HEADER_LENGTH = 10

IP = "127.0.0.1"
PORT = 1234
{% endhighlight %}

La novità rispetto a prima è che andiamo ad importare la libreria **select**. Questa libreria è utile per monitorare molte connessioni simultanee. Potremmo semplicemente utilizzare un loop che itera su di una lista di connessioni ma utilizzare select sarà molto più efficiente anche perché lavora a livello di sistema operativo e non di interprete python.

Inisiamo impostando il socket:

{% highlight python %}
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
{% endhighlight %}

Ora settiamo l'opzione che ci permette di evitare di ricevere il messaggio "Address already in use" che riceviamo spesso quando lavoriamo in questo ambiente.

{% highlight python %}
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
{% endhighlight %}

Questo imposta il socket in modo che l'indirizzo possa essere riutilizzato.

Ora rimaniamo in ascolto:

{% highlight python %}
server_socket.bind((IP, PORT))
server_socket.listen()
{% endhighlight %}

Ora andiamo a creare una lista di socket che **select** gestirà per noi e creiamo anche un dizionario che andrà a contenere tutti i client (intesi come utenti che si sono collegati):

{% highlight python %}
sockets_list = [server_socket]
clients = {}
{% endhighlight %}

Teniamoci qualche informazione per il debug:

{% highlight python %}
print(f'Listening for connections on {IP}:{PORT}...')
{% endhighlight %}

Ora, il lavoro principale del server consiste nel ricevere messaggi e poi ridistribuirli tra i client. Facciamo una funzione per ricevere i messaggi:

{% highlight python %}
def receive_message(client_socket):
    """Handles message receiving"""
    try:
        pass

    except:

        # Something went wrong like empty message or client exited abruptly.
        return False
{% endhighlight %}

### 1 leggiamo l'header

{% highlight python %}
def receive_message(client_socket):
    """Handles message receiving"""
    try:
        message_header = client_socket.recv(HEADER_LENGTH)

    except:

        # Something went wrong like empty message or client exited abruptly.
        return False
{% endhighlight %}

Se il client chiude la connessione per bene chiamando socket.close() noi riceveremo un messaggio di lunghezza nulla. Gestiamo la cosa in questo modo:

{% highlight python %}
if not len(message_header):
    return False
{% endhighlight %}

Ora convertiamo il contenuto dell'header in una variabile contenente un numero intero.

{% highlight python %}
message_length = int(message_header.decode('utf-8').strip())
{% endhighlight %}

Alla fine restituiamo un dizionario con una sezione che contiene l'header ed una sezione che contiene il messaggio ricevuto.

{% highlight python %}
        return {'header': message_header, 'data': client_socket.recv(message_length)}
{% endhighlight %}

Il codice completo:

{% highlight python %}
def receive_message(client_socket):
    try:
        message_header = client_socket.recv(HEADER_LENGTH)

        if not len(message_header):
            return False

        message_length = int(message_header.decode('utf-8').strip())

        return {'header': message_header, 'data': client_socket.recv(message_length)}

    except:
        return False
{% endhighlight %}

Ora ci occorre un ciclo infinito che per sempre riceva messaggi e diffonda i messaggi. All'interno di questo ciclo utilizzeremo **select.select**:

{% highlight python %}
read_sockets, write_sockets, exception_sockets = select.select(sockets_list, [], sockets_list)
{% endhighlight %}

Questa chiamata è una interfaccia per il sistema operativo che chiama direttamente le routine di più basso livello. Accetta tre liste:

* rlist: è composta da oggetti in attesa che possono essere pronti per la lettura
* wlist: è composta da oggetti in attesa che possono essere pronti per la scrittura
* xlist: è composta da oggetti in attesa che riportano condizioni eccezionali (per lo più errori)

Gli oggetti nelle liste possono essere socket oppure file handler. Ciò che viene restituito sono sottoliste composte da elementi che sono **ready**.

* read_sockets: è composta da oggetti pronti per la lettura
* write_sockets: è composta da oggetti pronti per la scrittura
* exception_sockets: è composta da oggetti che riportano condizioni eccezionali (per lo più errori)

Ora possiamo iterare sulla lista read_sockets che contiene le sockets che devono essere lette.

{% highlight python %}
for notified_socket in read_sockets:
    if notified_socket == server_socket:
{% endhighlight %}

Se la socket che è pronta per essere letta, è la server_socket vuol dire che c'è un nuovo client che si sta connettendo e che dobbiamo gestire.

{% highlight python %}
for notified_socket in read_sockets:
    if notified_socket == server_socket:
        client_socket, client_address = server_socket.accept()
        user = receive_message(client_socket)
        if user is False:
            continue
{% endhighlight %}

Chiamiamo il metodo **server_socket() ** che restituisce la socket che si sta collegando e l'indirizzo del client. Quello che facciamo è conservare il nome utente (la prima cosa che il client invia). Se per qualche ragione il nome utente non arriva semplicemente andiamo avanti.
La cosa successiva da fare è aggiungere il nuovo **client_socket** alla nostra lista **sockets_list**.

{% highlight python %}
sockets_list.append(client_socket)
{% endhighlight %}

Fatto questo vogliamo salvare il nome utente. Lo utilizzeremo come valore del dizionario che ha la socket come chiave:

{% highlight python %}
clients[client_socket] = user
print('Accepted new connection from {}:{}, username: {}'.format(*client_address, user['data'].decode('utf-8')))
{% endhighlight %}

Se non si tratta del server socket allora si tratta di un messaggio da propagare. 

{% highlight python %}
else:
    message = receive_message(notified_socket)
{% endhighlight %}

Prima di leggere il messaggio ci assicuriamo che il messaggio esista perché se il client si disconnettesse il messaggio sarebbe vuoto:

{% highlight python %}
if message is False:
    print('Closed connection from: {}'.format(clients[notified_socket]['data'].decode('utf-8')))
    sockets_list.remove(notified_socket)
    del clients[notified_socket]
    continue
{% endhighlight %}

Ora, assumendo che non si sia disconnesso possiamo ricevere l'informazione in questo modo:

{% highlight python %}
user = clients[notified_socket]

print(f'Received message from {user["data"].decode("utf-8")}: {message["data"].decode("utf-8")}')
{% endhighlight %}

Ora diffondiamo a tutti i client connessi il messaggio ricevuto:

{% highlight python %}
# Iterate over connected clients and broadcast message
for client_socket in clients:

    # But don't sent it to sender
    if client_socket != notified_socket:

        # Send user and message (both with their headers)
        # We are reusing here message header sent by sender, and saved username header send by user when he connected
        client_socket.send(user['header'] + user['data'] + message['header'] + message['data'])
{% endhighlight %}

In fine possiamo gestire le eccezzioni e gli errori dei socket con:

{% highlight python %}
# It's not really necessary to have this, but will handle some socket exceptions just in case
for notified_socket in exception_sockets:

    # Remove from list for socket.socket()
    sockets_list.remove(notified_socket)

    # Remove from our list of users
    del clients[notified_socket]
{% endhighlight %}

### Codice completo per il server.py

{% highlight python %}
import socket
import select

HEADER_LENGTH = 10

IP = "127.0.0.1"
PORT = 1234

# Create a socket
# socket.AF_INET - address family, IPv4, some otehr possible are AF_INET6, AF_BLUETOOTH, AF_UNIX
# socket.SOCK_STREAM - TCP, conection-based, socket.SOCK_DGRAM - UDP, connectionless, datagrams, socket.SOCK_RAW - raw IP packets
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# SO_ - socket option
# SOL_ - socket option level
# Sets REUSEADDR (as a socket option) to 1 on socket
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# Bind, so server informs operating system that it's going to use given IP and port
# For a server using 0.0.0.0 means to listen on all available interfaces, useful to connect locally to 127.0.0.1 and remotely to LAN interface IP
server_socket.bind((IP, PORT))

# This makes server listen to new connections
server_socket.listen()

# List of sockets for select.select()
sockets_list = [server_socket]

# List of connected clients - socket as a key, user header and name as data
clients = {}

print(f'Listening for connections on {IP}:{PORT}...')

# Handles message receiving
def receive_message(client_socket):

    try:

        # Receive our "header" containing message length, it's size is defined and constant
        message_header = client_socket.recv(HEADER_LENGTH)

        # If we received no data, client gracefully closed a connection, for example using socket.close() or socket.shutdown(socket.SHUT_RDWR)
        if not len(message_header):
            return False

        # Convert header to int value
        message_length = int(message_header.decode('utf-8').strip())

        # Return an object of message header and message data
        return {'header': message_header, 'data': client_socket.recv(message_length)}

    except:

        # If we are here, client closed connection violently, for example by pressing ctrl+c on his script
        # or just lost his connection
        # socket.close() also invokes socket.shutdown(socket.SHUT_RDWR) what sends information about closing the socket (shutdown read/write)
        # and that's also a cause when we receive an empty message
        return False

while True:

    # Calls Unix select() system call or Windows select() WinSock call with three parameters:
    #   - rlist - sockets to be monitored for incoming data
    #   - wlist - sockets for data to be send to (checks if for example buffers are not full and socket is ready to send some data)
    #   - xlist - sockets to be monitored for exceptions (we want to monitor all sockets for errors, so we can use rlist)
    # Returns lists:
    #   - reading - sockets we received some data on (that way we don't have to check sockets manually)
    #   - writing - sockets ready for data to be send thru them
    #   - errors  - sockets with some exceptions
    # This is a blocking call, code execution will "wait" here and "get" notified in case any action should be taken
    read_sockets, _, exception_sockets = select.select(sockets_list, [], sockets_list)


    # Iterate over notified sockets
    for notified_socket in read_sockets:

        # If notified socket is a server socket - new connection, accept it
        if notified_socket == server_socket:

            # Accept new connection
            # That gives us new socket - client socket, connected to this given client only, it's unique for that client
            # The other returned object is ip/port set
            client_socket, client_address = server_socket.accept()

            # Client should send his name right away, receive it
            user = receive_message(client_socket)

            # If False - client disconnected before he sent his name
            if user is False:
                continue

            # Add accepted socket to select.select() list
            sockets_list.append(client_socket)

            # Also save username and username header
            clients[client_socket] = user

            print('Accepted new connection from {}:{}, username: {}'.format(*client_address, user['data'].decode('utf-8')))

        # Else existing socket is sending a message
        else:

            # Receive message
            message = receive_message(notified_socket)

            # If False, client disconnected, cleanup
            if message is False:
                print('Closed connection from: {}'.format(clients[notified_socket]['data'].decode('utf-8')))

                # Remove from list for socket.socket()
                sockets_list.remove(notified_socket)

                # Remove from our list of users
                del clients[notified_socket]

                continue

            # Get user by notified socket, so we will know who sent the message
            user = clients[notified_socket]

            print(f'Received message from {user["data"].decode("utf-8")}: {message["data"].decode("utf-8")}')

            # Iterate over connected clients and broadcast message
            for client_socket in clients:

                # But don't sent it to sender
                if client_socket != notified_socket:

                    # Send user and message (both with their headers)
                    # We are reusing here message header sent by sender, and saved username header send by user when he connected
                    client_socket.send(user['header'] + user['data'] + message['header'] + message['data'])

    # It's not really necessary to have this, but will handle some socket exceptions just in case
    for notified_socket in exception_sockets:

        # Remove from list for socket.socket()
        sockets_list.remove(notified_socket)

        # Remove from our list of users
        del clients[notified_socket]
		
{% endhighlight %}

