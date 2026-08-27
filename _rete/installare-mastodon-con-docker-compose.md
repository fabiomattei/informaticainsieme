---
title: 'Installare una istanza Mastodon con Docker Compose'
date: '2026-08-27T09:30:00+02:00'
author: Fabio Mattei
layout: page
---

Nell'articolo su [il Fediverso](/rete/il-fediverso) abbiamo visto che ogni account vive su un server (istanza) gestito in modo indipendente. Vediamo ora, in pratica, cosa serve per tirarne su una propria usando Docker Compose, che è il metodo ufficialmente supportato più rapido rispetto a un'installazione manuale di tutti i componenti (Ruby, Node, Postgres, Redis...) sul sistema operativo.

## Requisiti

Prima di iniziare servono alcune cose che non riguardano Docker ma l'infrastruttura attorno:

* un **server Linux** (VPS o dedicato) con almeno 2 GB di RAM, meglio 4 GB se prevedi più utenti;
* un **dominio** puntato con un record DNS di tipo A/AAAA all'IP del server (es. `mastodon.tuodominio.it`);
* **Docker** e **Docker Compose** installati sul server;
* un **server SMTP** (account su un servizio come SendGrid, Mailgun, Amazon SES, oppure un tuo server di posta) per l'invio delle email di conferma registrazione;
* facoltativo ma consigliato: uno spazio di **object storage S3-compatibile** (es. Backblaze B2, Wasabi, MinIO self-hosted) per i media, così da non riempire il disco del server con immagini e video della federazione.

## 1. Clonare il repository ufficiale

{% highlight shell %}
git clone https://github.com/mastodon/mastodon.git
cd mastodon
{% endhighlight %}

Nel repository trovi già un file `docker-compose.yml` che definisce tutti i servizi necessari.

## 2. I servizi definiti nel docker-compose.yml

Il file compose descrive, a grandi linee, questi container:

{% highlight yaml %}
services:
  db:            # PostgreSQL, il database principale
  redis:         # Redis, cache e code dei job
  web:           # Rails, il cuore dell'applicazione (Puma come app server)
  streaming:     # Node.js, l'API di streaming per gli aggiornamenti in tempo reale
  sidekiq:       # Worker in background per invio ActivityPub, notifiche, media processing
{% endhighlight %}

Ognuno di questi corrisponde esattamente ai componenti dello stack di cui abbiamo parlato: il container `web` è l'app Ruby on Rails, `streaming` è il processo Node separato, `sidekiq` elabora i job asincroni appoggiandosi a Redis, e `db` è Postgres che conserva tutto lo stato persistente.

## 3. Generare il file di configurazione

Mastodon fornisce un wizard interattivo che genera il file `.env.production` con tutte le variabili d'ambiente necessarie (chiavi segrete, credenziali del database, configurazione SMTP, dominio):

{% highlight shell %}
docker compose run --rm web bundle exec rake mastodon:setup
{% endhighlight %}

Il wizard chiede, tra le altre cose:

* il dominio dell'istanza (**attenzione**: non è modificabile in un secondo momento senza reinstallare, perché il dominio fa parte dell'identità federata degli account, come visto nell'articolo sul Fediverso);
* se usare un servizio S3-compatibile per i media;
* i parametri del server SMTP;
* se creare subito un account amministratore.

Il file generato va tenuto al sicuro: contiene chiavi segrete (`SECRET_KEY_BASE`, `OTP_SECRET`, le chiavi VAPID per le notifiche push) che non devono mai finire in un repository pubblico.

## 4. Preparare il database

Prima del primo avvio bisogna creare lo schema del database:

{% highlight shell %}
docker compose run --rm web bundle exec rails db:migrate
{% endhighlight %}

## 5. Avviare i servizi

{% highlight shell %}
docker compose up -d
{% endhighlight %}

A questo punto tutti i container (`db`, `redis`, `web`, `streaming`, `sidekiq`) partono in background. Puoi controllare lo stato con:

{% highlight shell %}
docker compose ps
docker compose logs -f web
{% endhighlight %}

## 6. Il reverse proxy

Il container `web` espone Mastodon su una porta interna (di solito 3000), ma non va esposto direttamente su Internet: serve un reverse proxy davanti, tipicamente **Nginx**, che si occupa di:

* servire gli asset statici in modo efficiente;
* gestire il certificato **TLS** (Let's Encrypt tramite `certbot`, indispensabile: Mastodon richiede HTTPS);
* inoltrare le richieste normali al container `web` e quelle di streaming (WebSocket) al container `streaming`.

Il repository di Mastodon fornisce un file di esempio (`dist/nginx.conf`) da adattare al proprio dominio e da copiare in `/etc/nginx/sites-available/`.

## 7. Creare l'account amministratore

Se non l'hai già fatto durante il wizard:

{% highlight shell %}
docker compose run --rm web bin/tootctl accounts create \
  admin_username \
  --email admin@tuodominio.it \
  --confirmed \
  --role Owner
{% endhighlight %}

## Manutenzione ordinaria

Una volta online, l'istanza richiede attenzione continua:

* **aggiornamenti**: `git pull` della nuova versione, seguito da `docker compose build` e dalle migrazioni del database, con una certa regolarità per le patch di sicurezza;
* **backup**: dump periodico di Postgres (`pg_dump`) e backup dei media se non usi storage S3 esterno;
* **pulizia media**: `tootctl media remove` per liberare spazio rimuovendo la cache dei media remoti scaricati dalla federazione, che altrimenti cresce senza limiti;
* **moderazione**: gestione di segnalazioni, blocchi di istanze problematiche (defederazione), revisione delle nuove registrazioni.

## In sintesi

| Comando | Cosa fa |
|---|---|
| `docker compose run --rm web bundle exec rake mastodon:setup` | Wizard di configurazione iniziale |
| `docker compose run --rm web bundle exec rails db:migrate` | Crea/aggiorna lo schema del database |
| `docker compose up -d` | Avvia tutti i servizi in background |
| `docker compose logs -f web` | Segue i log dell'applicazione Rails |
| `docker compose run --rm web bin/tootctl accounts create ...` | Crea un account (es. amministratore) da riga di comando |

Docker Compose non elimina la complessità intrinseca di gestire più servizi (Postgres, Redis, Rails, Node, Sidekiq, Nginx), ma la rende gestibile con pochi comandi invece di dover installare e configurare manualmente ogni componente sul sistema operativo host.
