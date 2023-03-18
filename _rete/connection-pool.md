---
title: 'Connection pool'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

# Socket Chatroom server - Creating chat application with sockets in Python

We've made it through the basics of working with sockets, and now we're ready to try to actually build something with them, so, in this sockets with Python tutorial, we're going to build a console-based chat app.

To begin, we'll build the chat_server.py. What will the functions of our server be?

First, the server needs to accept new connections from clients. From here, we need to come up with some way to identify our unique users. We could display users by IP address, but most people tend to rather come up with some sort of username, so our server will first allow clients to connect and choose a username. Beyond this, the server will collect incoming messages and then distribute them to the rest of the connected clients.

So, we will start with the imports and some starting values:

{% highlight python %}
import socket
import select

HEADER_LENGTH = 10

IP = "127.0.0.1"
PORT = 1234
{% endhighlight %}

Nothing new here except for the select import. What's this? The select module gives us OS-level monitoring operations for things, including for sockets. It is especially useful in cases where we're attempting to monitor many connections simultaneously. While you could use a for loop to just iterate over all of the sockets, using select is going to be far more efficient and will scale much better, mainly since it's going to work on your OS layer, rather than all the way through Python. For how to use it, we'll talk about it more when we get to that point!

Initially setup our socket:

{% highlight python %}
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
{% endhighlight %}

Next, we can set the following to overcome the "Address already in use" that we hit often while building our programs:

{% highlight python %}
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
{% endhighlight %}

This modifies the socket to allow us to reuse the address.

Next, we bind and listen:

{% highlight python %}
server_socket.bind((IP, PORT))

server_socket.listen()
{% endhighlight %}

Next, we'll create a list of sockets for select to keep track of, as well as begin our clients dict:

{% highlight python %}
sockets_list = [server_socket]

clients = {}
{% endhighlight %}

Some debugging info:

{% highlight python %}
print(f'Listening for connections on {IP}:{PORT}...')
{% endhighlight %}

Now, this server's main job is to receive messages, and then disperse them to the connected clients. For receiving messages, we're going to make a function:

## Handles message receiving

{% highlight python %}
def receive_message(client_socket):
    try:
        pass

    except:

        # Something went wrong like empty message or client exited abruptly.
        return False
{% endhighlight %}

Step 1 for receiving a message is to read the header:

## Handles message receiving
{% highlight python %}
def receive_message(client_socket):
    try:
        message_header = client_socket.recv(HEADER_LENGTH)

    except:

        # Something went wrong like empty message or client exited abruptly.
        return False
{% endhighlight %}

If a client closes a connection gracefully, then a socket.close() will be issued and there will be no header. We can handle for that with:

{% highlight python %}
        if not len(message_header):
            return False
{% endhighlight %}

Then, we can convert our header to a length:

{% highlight python %}
        message_length = int(message_header.decode('utf-8').strip())
{% endhighlight %}

Finally, we can return some meaningful data:

{% highlight python %}
        return {'header': message_header, 'data': client_socket.recv(message_length)}
{% endhighlight %}

Full code now:

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

Now, what we wish to do is, in a continuous loop, receive messages for all of our client sockets, then send all of the messages out to all of the client sockets. To begin, we will use a while loop and then we will make use of select.select:

{% highlight python %}
    read_sockets, _, exception_sockets = select.select(sockets_list, [], sockets_list)
{% endhighlight %}

This isn't totally straightforward, but it's fairly simple. We're purely using select.select here for the aforementioned OS-level i/o for our sockets. What this function takes as parameters is rlist, wlist, and xlist...which are read list, write list, and error list respectively. The return of this function is that same 3 elements where the returns are "subsets" of the input lists where the subset is a list of those sockets that are ready.

Now, from here, we're going to iterate over the read_sockets list. These are sockets that have data to be read.

{% highlight python %}
    for notified_socket in read_sockets:
        if notified_socket == server_socket:
{% endhighlight %}

If the notified socket is our server socket, then this means we just got a new connection, which we want to handle for.

{% highlight python %}
    for notified_socket in read_sockets:
        if notified_socket == server_socket:
            client_socket, client_address = server_socket.accept()
            user = receive_message(client_socket)
            if user is False:
                continue
{% endhighlight %}

So with client_socket, client_address = server_socket.accept() we get that unique client socket and their address. We then store their chosen username to the one they picked (this should be the very first thing the client will send). If, for some reason, that doesn't happen (such as client closed before sending a name), then we will just move along.

Next, we want to append this new client_socket to our sockets_list

{% highlight python %}
            sockets_list.append(client_socket)
{% endhighlight %}

After this, we'd like to save this client's username, which we'll save as the value to the key that is the socket object:

{% highlight python %}
            clients[client_socket] = user
            print('Accepted new connection from {}:{}, username: {}'.format(*client_address, user['data'].decode('utf-8')))
{% endhighlight %}

If the notified socket is not a server socket, then this means instead we've got a message to read:

{% highlight python %}
        else:
            message = receive_message(notified_socket)
{% endhighlight %}

Before we attempt to read the message, let's make sure one exists. If the client disconnects, then the message would be empty:

{% highlight python %}
            if message is False:
                print('Closed connection from: {}'.format(clients[notified_socket]['data'].decode('utf-8')))
                sockets_list.remove(notified_socket)
                del clients[notified_socket]

                continue
{% endhighlight %}

Now, assuming it wasn't a disconnect, we can the information like so:

{% highlight python %}
            user = clients[notified_socket]

            print(f'Received message from {user["data"].decode("utf-8")}: {message["data"].decode("utf-8")}')
{% endhighlight %}

Next, we'd like to basically broadcast this out to all of our connected clients:

{% highlight python %}
            # Iterate over connected clients and broadcast message
            for client_socket in clients:

                # But don't sent it to sender
                if client_socket != notified_socket:

                    # Send user and message (both with their headers)
                    # We are reusing here message header sent by sender, and saved username header send by user when he connected
                    client_socket.send(user['header'] + user['data'] + message['header'] + message['data'])
{% endhighlight %}

Finally, we can handle for the exception/error sockets with:

{% highlight python %}
    # It's not really necessary to have this, but will handle some socket exceptions just in case
    for notified_socket in exception_sockets:

        # Remove from list for socket.socket()
        sockets_list.remove(notified_socket)

        # Remove from our list of users
        del clients[notified_socket]
{% endhighlight %}

Full noted code for chat server:

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
