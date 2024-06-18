---
layout: post
title:  "Creiamo una news letter con python"
date:   2024-06-17 00:00:00
categories: python newsletter
excerpt: E' possibile mandare messaggi email ad un certo numero di destinatari interessati utilizzando python
author: Fabio
tags: python email
---

Le news letter sono la spina dorsale di internet. Esistono da sempre e permettono agli utenti di stare in contatto con le comunità a cui tengono di più.

E' importante stare in contatto con le persone che tengono ai nostri contenuti può quindi venire utile uno script che ci aiuta a mandare email a gruppi di persone.

Questo script utilizza il servizio di Gmail attraverso il server SMTP (che va attivato nell'apposita interfaccia interna di amministrazione) a mandare lo stesso messaggio a molti utenti. 

Attenzione: la distanta tra una news letter e lo SPAM è breve, utilizzate questo script per le persone che effettivamente sono interessati ai vostri contenuti.

{% highlight python %}
import smtplib
import ssl

# dettagli del server SMTP
smtp_server = 'smtp.gmail.com'
smtp_port = 465

# dettagli di origine e destinazione
mittente = 'Le News di Marco'
destinazioni = ['primo@gmail.com','secondo@gmail.com']     ## Lista dei destinatari

# Autenticazione
username = ''       ## Email di chi spedisce
password = ''       ## Password di chi spedisce


# Contenuti
oggetto = '🎉 Benvenuti nella mia news letter'
messaggio = '''\
Cari amici,

Vi aggiorno sugli accadimenti della settimana.

.............................................
.............................................

Cordialmente,
Marco
'''


# Crea un contesto SSL/TLS
context = ssl.create_default_context()

# Connect to the SMTP server using SSL/TLS
with smtplib.SMTP_SSL(smtp_server, smtp_port, context=context) as server:
    # Abilita il debug per poter vedere le risposte del server
    server.set_debuglevel(1)

    # Login al server SMTP
    server.login(username, password)

    # Creazione messaggio email
    message = f'From: {mittente}\r\nSubject: {oggetto}\r\nTo: {destinazioni}\r\n\r\n{messaggio}'
    message = message.encode()  # Converte il messaggio

    # Spedizione
    server.sendmail(from_address, to_address, message)
{% endhighlight %}
