---
title: 'Creating chat application with sockets in Python'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---


Welcome to part 5 of the sockets tutorial, in this tutorial we're going to build the client's code for our chatroom application.

To begin, we do a lot of the same similar setup that we've seen before:

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

Next, we're going to set the recv method to not block:

{% highlight python %}
client_socket.setblocking(False)
{% endhighlight %}

Now, if you recall, our server is expecting the very first message to be a username choice for the client:

{% highlight python %}
username = my_username.encode('utf-8')
username_header = f"{len(username):<{HEADER_LENGTH}}".encode('utf-8')
client_socket.send(username_header + username)
{% endhighlight %}

After this, we're ready for the main loop for the client, which will be there to accept new messages from the client. For now, we'll just use the input function from Python. Eventually, you would probably want to have some sort of GUI here, and input is going to block the rest of the program from running/updating messages, which means we will have to send a message to see the updates. There are some OS specific methods we can use to get around this, but I am going to not get into that here. If you want to, feel free to research them and implement them. You can add a sys check at the beginning to determine what OS your client is running with.

{% highlight python %}
while True:
    message = input(f'{my_username} > ')
{% endhighlight %}

It's really just an input, but we'll slap their username in there to give it a look and feel like a chat app. Next, before just sending the message, we should make sure there is one. People may also just hit enter to refresh with this app, but also people may just accidentally hit enter later or something. No reason to send an empty message.

{% highlight python %}
    if message:

        # Encode message to bytes, prepare header and convert to bytes, like for username above, then send
        message = message.encode('utf-8')
        message_header = f"{len(message):<{HEADER_LENGTH}}".encode('utf-8')
        client_socket.send(message_header + message)
{% endhighlight %}

So that's sending messages, now we actually want to receive them. This is slightly more tricky. What we will do is just do a while True loop that will infinitely attempt to receive any incoming messages. Once there are no more to receive, we will get an error. We'll handle for other expected errors, but if we get the specific error we're expecting to just be out of messages, then we'll break the loop cleanly and repeat.

To display messages, we need both the username and the message, each of which have separate headers and contents, so we need to grab each.

{% highlight python %}
        while True:

            username_header = client_socket.recv(HEADER_LENGTH)

            # If we received no data, server gracefully closed a connection, for example using socket.close() or socket.shutdown(socket.SHUT_RDWR)
            if not len(username_header):
                print('Connection closed by the server')
                sys.exit()
{% endhighlight %}

Now we actually get the username:

{% highlight python %}
            username_length = int(username_header.decode('utf-8').strip())
            username = client_socket.recv(username_length).decode('utf-8')
{% endhighlight %}

Using the same logic, let's grab the message too:

{% highlight python %}
            message_header = client_socket.recv(HEADER_LENGTH)
            message_length = int(message_header.decode('utf-8').strip())
            message = client_socket.recv(message_length).decode('utf-8')
{% endhighlight %}

Then output it to the screen:

{% highlight python %}
            print(f'{username} > {message}')
{% endhighlight %}

Now, eventually, we're going to hit an error because there wont be messages to receieve anymore. We expect this error, but we wouldn't want to assume for just this error. Thus, let's encase this in a try/except:

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

Great, that's it! Fully noted code up to this point:

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
