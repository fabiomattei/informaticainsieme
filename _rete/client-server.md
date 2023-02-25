---
title: 'Client server'
date: '2023-02-25T15:55:20+01:00'
author: Fabio Mattei
layout: page
---

In questa lezione dedicata ai socket faremo comunicare una coppia Server-Client TCP da noi creata.

Utilizzeremo i moduli socket per la connessione, subprocess per l'elaborazione delle richieste e sys in caso di errore per uscire dal sistema. Il client potrà effettuare richieste al server, che risponderà dopo aver processato la richiesta effettuata tramite il modulo subprocess.

Scriveremo due script, uno chiamato server.py e uno chiamato client.py, responsabili rispettivamente della parte server e della parte client. Il questa prima parte ci dedicheremo alla scrittura dello script per il client.

## Il client TCP

I passi da seguire per la creazione di un client TCP col modulo socket sono i seguenti:

{% highlight shell %}
# Creazione CLIENT SOCKET:
--------------------------------
# 1 - Creazione socket                      # socket.socket()
# 2 - Connessione al Server                 # connect(indirizzo)
# 3 - Invio di una Richiesta al Server      # send()
# 4 - Ricezione della Risposta dal Server   # recv()
{% endhighlight %}

Tra le funzioni generiche di socket utilizzate nello script abbiamo:

{% highlight shell %}
# Funzioni Generiche:
--------------------------------
# s.recv()                => Riceve messaggi TCP
# s.send()                => Trasmette messaggi TCP
# s.close()               => Chiude il Socket
# socket.gethostname()    => Restituisce l'hostname della macchina su cui sta girando l'Interprete di Python
# socket.gethostbyname()  => Restituisce l'IP associato al nome passato
{% endhighlight %}

Mentre la funzione specifica per il lato client è la funzione connect():

{% highlight shell %}
# Funzioni Client Socket:
--------------------------------
# s.connect()    => Inizia la connessione col Server. Richiede una tupla contenente IP e Porta del Servizio
{% endhighlight %}

Di seguito trovate il codice utilizzato nello script, vi invito a guardare il video per commenti e spiegazioni al riguardo!

{% highlight python %}
import socket
import sys

def invia_comandi(s):
    while True:
        comando = input("-> ")
        if comando == "ESC":
            print("Sto chiudendo la connessione col Server.")
            s.close()
            sys.exit()
        else:
            s.send(comando.encode())
            data = s.recv(4096)
            print(str(data, "utf-8"))

def conn_sub_server(indirizzo_server):
    try:
        s = socket.socket()             # creazione socket client
        s.connect(indirizzo_server)     # connessione al server
        print(f"Connessessione al Server: { indirizzo_server } effettuata.")
    except socket.error as errore:
        print(f"Qualcosa è andato storto, sto uscendo... \n{errore}")
        sys.exit()
    invia_comandi(s)

if __name__ == '__main__':
    conn_sub_server(("192.168.1.6", 20000))
{% endhighlight %}


## Il server TCP

Le funzioni specifiche per il lato server sono:

{% highlight shell %}
# Creazione SERVER SOCKET:
--------------------------------
# 1 - Creazione socket                                                              # socket.socket()
# 2 - Collegamento del socket all'indirizzo della macchina e alla Porta Designata   # bind()
# 3 - Messa in ascolto in attesa della connessione del Client                       # listen()
# 4 - Accettazione del Client                                                       # accept()
# 5 - Ricezione Richiesta dal Client                                                # recv()
# 4 - Elaborazione di una Risposta                                                  # subprocess()
# 5 - Invio Risposta al Client                                                      # send()
{% endhighlight %}

Di seguito trovate il codice utilizzato nello script server.py

{% highlight python %}
import socket
import subprocess


def ricevi_comandi(conn):
    while True:
        richiesta = conn.recv(4096)
        risposta = subprocess.run(richiesta.decode(), shell=True, stdout = subprocess.PIPE, stderr = subprocess.PIPE)
        data = risposta.stdout + risposta.stderr
        conn.sendall(data)


def sub_server(indirizzo, backlog=1):
    try:
        s = socket.socket()
        s.bind(indirizzo)
        s.listen(backlog)
        print("Server Inizializzato. In ascolto...")
    except socket.error as errore:
        print(f"Qualcosa è andato storto... \n{errore}")
        print(f"Sto tentando di reinizializzare il server...")
        sub_server(indirizzo, backlog=1)
    conn, indirizzo_client = s.accept() #conn = socket_client
    print(f"Connessione Server - Client Stabilita: {indirizzo_client}")
    ricevi_comandi(conn)


if __name__ == '__main__':
    sub_server(("", 20000))
{% endhighlight %}
