---
title: 'Comandi di rete per la shell windows'
date: '2023-02-25T15:56:20+01:00'
author: Fabio Mattei
layout: page
---

## Il comando ping

{% highlight shell %}
ping www.google.com
{% endhighlight %}

Il comando ping è uno dei più usati, serve a capire se c’è una connessione possibile tra il computer che si sta utilizzando ed un computer remoto. Vegono spediti 4 pacchetti e il sistema fa un _echo_ (risponde esattamente ciò che gli è stato inviato)
Il formato è ping <hostname> o ping <indirizzo ip>

## Il comando tracert

{% highlight shell %}
tracert www.google.com
{% endhighlight %}

Come abbiamo detto lo scopo del Traceroute è seguire il percorso di un pacchetto di dati dalla sua sorgente fino a destinazione. Per farlo dovrà registrare ogni singolo "salto" del pacchetto.

## Il comando ipconfig
Mostra le informazioni che riguardano la macchina locale: indirizzo ip, server DNS, Gateway, subnet mask.

{% highlight shell %}
Ipconfig /all      mostra informazioni più dettagliate
{% endhighlight %}

## Il comando hostname
Mostra l’hostname della macchina locale (molto più veloce che non navigare nell’interfaccia grafica)

## Il comando getmac
Mostra gli indirizzi mac delle interfacce di rete

## Il comando arp

{% highlight shell %}
arp -a
{% endhighlight %}

Il protocollo ARP (Address Resolution Protocol) permette di conoscere l'indirizzo fisico di una scheda di rete (MAC Address) corrispondente ad un dato indirizzo IP, ed è per questo che si chiama Protocollo di risoluzione d'indirizzo. 
Ogni nodo (computer) di una rete mantiene una tabella (Arp Table) in cui viene indicata la corrispondenza fra MAC address e IP address degli altri dispositivi presenti nella rete, in modo da poter effettuare velocemente la conversione.
E' interessante scoprire la ARP table memorizzata sul nostro PC. Per fare questo su un computer con sistema operativo Windows è sufficiente digitare il comando "arp -a" per ottenere una lista dei MAC address e degli IP address che il PC conosce:
 
Una tipica Arp Table potrebbe essere fatta in questo modo:

{% highlight shell %}
C:\Users\User1> arp –a 
Interface: 192.168.1.3 on Interface 0x1000002 
Internet Address     Physical Address         Type 
192.168.1.2            00-50-56-be-f6-db    dynamic 
192.168.1.11          0c-d9-96-e8-8a-40    dynamic
192.168.1.12          0c-d9-96-d2-40-40    dynamic 
192.168.1.255        ff-ff-ff-ff-ff-ff            static 
{% endhighlight %}

La prima riga mostra l'indirizzo IP della scheda di rete del computer. Seguono gli indirizzi IP dei PC con cui il computer è venuto precedentemente in comunicazione e accanto a ciascuno vediamo l'indirizzo MAC corrispondente.
Il tipo (Type) dynamic indica che la corrispondente voce nella Arp Table viene cancellata dinamicamente dopo un certo periodo di non utilizzo. Viceversa le righe marcate come static vengono conservate permanentemente.
Si noti l'indirizzo ff-ff-ff-ff-ff-ff che corrisponde all'indirizzo IP di broadcast (pacchetto da inviare a tutti i nodi della rete).



