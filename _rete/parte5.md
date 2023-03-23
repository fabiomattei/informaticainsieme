---
title: 'Progetto Chat: il client'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

Andiamo ora a costruire il client del nostro sistema di chat.

{% highlight python %}
import socket
import select
import errno

HEADER_LENGTH = 10

IP = "127.0.0.1"
PORT = 1234
my_username = input("Username: ")

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

client_socket.connect((IP, PORT))
{% endhighlight %}

L'inizio è analogo al server, ora però settiamo la socket in modo che non si blocchi mai.

{% highlight python %}
client_socket.setblocking(False)
{% endhighlight %}

Se ricordate il server si aspetta che il primo messaggio comunichi il nome utente di chi si sta collegando:

{% highlight python %}
username = my_username.encode('utf-8')
username_header = f"{len(username):<{HEADER_LENGTH}}".encode('utf-8')
client_socket.send(username_header + username)
{% endhighlight %}

Fatto questo siamo pronti per il ciclo principale del client il quale sarà pronto ad accettare i messaggi del client. Per ora ci accontenteremo della funzione input() di python anche se sarebbe il caso di costruire una qualche interfaccia dato che input andrà a bloccare il programma restando in attesa dell'utente, il che significa che dovremo mandare un messaggio per vedere gli aggiornamenti. Ci sono metodi specifici del sistema operativo che potremmo utilizzare.

{% highlight python %}
while True:
    message = input(f'{my_username} > ')
{% endhighlight %}

Si tratta semplicemente di un input ma ci mettiamo dentro il nome utente in modo da farlo sembrare più professionale. Controlliamo, prima di inviare un messaggio, che questo contenga una qualche informazione, dato che sarebbe inutile mandare un messaggio vuoto.

{% highlight python %}
    if message:
        # Encode message to bytes, prepare header and convert to bytes, like for username above, then send
        message = message.encode('utf-8')
        message_header = f"{len(message):<{HEADER_LENGTH}}".encode('utf-8')
        client_socket.send(message_header + message)
{% endhighlight %}

Ora che abbiamo mandato messaggio vogliamo riceverne a nostra volta. A questo proposito facciamo un ciclo infinito che tenta di ricevere messaggi finché possibile. Quando non ce ne sono più si solleverà un errore. Andremo a gestire gli altri errori ma se l'errore che arriva è la fine dei messaggi in code andiamo semplicemente ad uscire dal ciclo infito.

Al fine di mostrare i messaggi abbiamo bisodno dei nome utente e del corpo del messaggio inviato. Ogni porzione avrà un header separato. Andiamo a gestire il tutto.

{% highlight python %}
        while True:

            username_header = client_socket.recv(HEADER_LENGTH)

            # If we received no data, server gracefully closed a connection, for example using socket.close() or socket.shutdown(socket.SHUT_RDWR)
            if not len(username_header):
                print('Connection closed by the server')
                sys.exit()
{% endhighlight %}

Ora raccogliamo il nome utente:

{% highlight python %}
            username_length = int(username_header.decode('utf-8').strip())
            username = client_socket.recv(username_length).decode('utf-8')
{% endhighlight %}

Usando la stessa logica raccogliamo il messaggio:

{% highlight python %}
            message_header = client_socket.recv(HEADER_LENGTH)
            message_length = int(message_header.decode('utf-8').strip())
            message = client_socket.recv(message_length).decode('utf-8')
{% endhighlight %}

Ora mostriamo l'output.

{% highlight python %}
            print(f'{username} > {message}')
{% endhighlight %}

Aspettiamo che si arrivi all'errore che ci indica la fine dei messaggi ricevuti. Ci aspettiamo questo errore e lo gestiamo con un try/except:

{% highlight python %}
    try:

        while True:

            username_header = client_socket.recv(HEADER_LENGTH)

            if not len(username_header):
                print('Connection closed by the server')
                sys.exit()

            username_length = int(username_header.decode('utf-8').strip())
            username = client_socket.recv(username_length).decode('utf-8')

            message_header = client_socket.recv(HEADER_LENGTH)
            message_length = int(message_header.decode('utf-8').strip())
            message = client_socket.recv(message_length).decode('utf-8')

            print(f'{username} > {message}')

    except IOError as e:
        # This is normal on non blocking connections - when there are no incoming data, error is going to be raised
        # Some operating systems will indicate that using AGAIN, and some using WOULDBLOCK error code
        # We are going to check for both - if one of them - that's expected, means no incoming data, continue as normal
        # If we got different error code - something happened
        if e.errno != errno.EAGAIN and e.errno != errno.EWOULDBLOCK:
            print('Reading error: {}'.format(str(e)))
            sys.exit()

        # We just did not receive anything
        continue

    except Exception as e:
        # Any other exception - something happened, exit
        print('Reading error: '.format(str(e)))
        sys.exit()
{% endhighlight %}

Ecco il codice completo:

{% highlight python %}
import socket
import select
import errno

HEADER_LENGTH = 10

IP = "127.0.0.1"
PORT = 1234
my_username = input("Username: ")

# Create a socket
# socket.AF_INET - address family, IPv4, some otehr possible are AF_INET6, AF_BLUETOOTH, AF_UNIX
# socket.SOCK_STREAM - TCP, conection-based, socket.SOCK_DGRAM - UDP, connectionless, datagrams, socket.SOCK_RAW - raw IP packets
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to a given ip and port
client_socket.connect((IP, PORT))

# Set connection to non-blocking state, so .recv() call won;t block, just return some exception we'll handle
client_socket.setblocking(False)

# Prepare username and header and send them
# We need to encode username to bytes, then count number of bytes and prepare header of fixed size, that we encode to bytes as well
username = my_username.encode('utf-8')
username_header = f"{len(username):<{HEADER_LENGTH}}".encode('utf-8')
client_socket.send(username_header + username)

while True:

    # Wait for user to input a message
    message = input(f'{my_username} > ')

    # If message is not empty - send it
    if message:

        # Encode message to bytes, prepare header and convert to bytes, like for username above, then send
        message = message.encode('utf-8')
        message_header = f"{len(message):<{HEADER_LENGTH}}".encode('utf-8')
        client_socket.send(message_header + message)

    try:
        # Now we want to loop over received messages (there might be more than one) and print them
        while True:

            # Receive our "header" containing username length, it's size is defined and constant
            username_header = client_socket.recv(HEADER_LENGTH)

            # If we received no data, server gracefully closed a connection, for example using socket.close() or socket.shutdown(socket.SHUT_RDWR)
            if not len(username_header):
                print('Connection closed by the server')
                sys.exit()

            # Convert header to int value
            username_length = int(username_header.decode('utf-8').strip())

            # Receive and decode username
            username = client_socket.recv(username_length).decode('utf-8')

            # Now do the same for message (as we received username, we received whole message, there's no need to check if it has any length)
            message_header = client_socket.recv(HEADER_LENGTH)
            message_length = int(message_header.decode('utf-8').strip())
            message = client_socket.recv(message_length).decode('utf-8')

            # Print message
            print(f'{username} > {message}')

    except IOError as e:
        # This is normal on non blocking connections - when there are no incoming data error is going to be raised
        # Some operating systems will indicate that using AGAIN, and some using WOULDBLOCK error code
        # We are going to check for both - if one of them - that's expected, means no incoming data, continue as normal
        # If we got different error code - something happened
        if e.errno != errno.EAGAIN and e.errno != errno.EWOULDBLOCK:
            print('Reading error: {}'.format(str(e)))
            sys.exit()

        # We just did not receive anything
        continue

    except Exception as e:
        # Any other exception - something happened, exit
        print('Reading error: '.format(str(e)))
        sys.exit()
{% endhighlight %}
