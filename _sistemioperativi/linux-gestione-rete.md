---
title: 'Gestione della rete in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Anche la configurazione di rete in Linux si gestisce comunemente da terminale, con strumenti dedicati.

### Vedere lo stato della rete

{% highlight shell %}
ip addr        # mostra le interfacce di rete e i relativi indirizzi IP
ip link        # mostra lo stato (attiva/disattiva) delle interfacce
{% endhighlight %}

`ip` è lo strumento moderno; su sistemi più datati si trova ancora il comando equivalente `ifconfig`.

### Indirizzo IP: automatico o manuale

Come su Windows, l'indirizzo IP può essere ottenuto automaticamente da un server **DHCP** o assegnato manualmente. Sulle distribuzioni desktop questo si configura solitamente tramite l'interfaccia grafica delle impostazioni di rete (**NetworkManager**); da terminale è possibile assegnare un indirizzo temporaneo con:

{% highlight shell %}
sudo ip addr add 192.168.1.50/24 dev eth0
{% endhighlight %}

### Verificare la connettività

{% highlight shell %}
ping google.com          # verifica che un host sia raggiungibile
traceroute google.com    # mostra il percorso dei pacchetti verso un host
{% endhighlight %}

### Nomi host e DNS

La risoluzione dei nomi (da `google.com` all'indirizzo IP corrispondente) è gestita dai server **DNS**, configurati in genere nel file `/etc/resolv.conf` o tramite NetworkManager.

### Connessioni attive e porte in ascolto

{% highlight shell %}
ss -tulwn      # elenca le porte in ascolto e le connessioni attive
{% endhighlight %}

Utile per verificare quali servizi sono raggiungibili dall'esterno sulla macchina.

### Firewall: iptables e ufw

Linux gestisce il filtraggio del traffico tramite **iptables** (il motore di firewall del kernel) o, su molte distribuzioni desktop come Ubuntu, tramite **ufw** (Uncomplicated Firewall), un'interfaccia semplificata:

{% highlight shell %}
sudo ufw enable              # attiva il firewall
sudo ufw allow 22            # consente il traffico sulla porta 22 (SSH)
sudo ufw status               # mostra lo stato e le regole attive
{% endhighlight %}

### Connessione remota con SSH

**SSH** (Secure Shell) permette di collegarsi al terminale di un altro computer attraverso la rete, in modo cifrato:

{% highlight shell %}
ssh utente@192.168.1.10
{% endhighlight %}

Una volta connessi, i comandi digitati vengono eseguiti sul computer remoto come se ci si trovasse davanti alla sua tastiera. È lo strumento standard per amministrare server Linux che non hanno (o non necessitano di) un'interfaccia grafica.

Per evitare di digitare la password a ogni connessione, si può configurare l'accesso tramite una coppia di **chiavi crittografiche**:

{% highlight shell %}
ssh-keygen                        # genera una coppia di chiavi (privata e pubblica)
ssh-copy-id utente@192.168.1.10   # copia la chiave pubblica sul computer remoto
{% endhighlight %}

Da quel momento, la connessione SSH verso quel computer non richiederà più la password: l'autenticazione avviene tramite la chiave.

### Copiare file su un computer remoto

`scp` (Secure Copy) usa lo stesso protocollo di SSH per copiare file tra due computer sulla rete:

{% highlight shell %}
scp appunti.txt utente@192.168.1.10:/home/utente/     # copia un file verso il computer remoto
scp utente@192.168.1.10:/home/utente/appunti.txt .     # copia un file dal computer remoto
{% endhighlight %}

Per sincronizzare intere cartelle in modo più efficiente (copiando solo le differenze), si usa invece `rsync`:

{% highlight shell %}
rsync -avz progetti/ utente@192.168.1.10:/home/utente/progetti/
{% endhighlight %}

### Scaricare file da Internet: wget e curl

Due strumenti da riga di comando permettono di scaricare risorse dal web senza passare da un browser:

{% highlight shell %}
wget https://esempio.it/file.zip           # scarica un file e lo salva sul disco
curl -O https://esempio.it/file.zip         # equivalente con curl
curl https://api.esempio.it/dati            # mostra a schermo il contenuto (utile con le API)
{% endhighlight %}

`wget` è pensato principalmente per scaricare file; `curl` è più versatile e supporta molti protocolli e opzioni, tanto da essere lo strumento standard per testare le API web da terminale:

{% highlight shell %}
curl -X POST -d "nome=Marco" https://api.esempio.it/utenti
{% endhighlight %}

### La tabella di routing

Quando il computer deve inviare un pacchetto a un indirizzo IP, consulta la propria **tabella di routing** per decidere attraverso quale interfaccia e verso quale gateway instradarlo:

{% highlight shell %}
ip route          # mostra la tabella di routing
{% endhighlight %}

La riga contrassegnata come `default` indica il **gateway predefinito** (di solito il router di casa o dell'ufficio): è l'indirizzo a cui vengono inviati tutti i pacchetti diretti a reti non elencate esplicitamente nella tabella.

### DNS in dettaglio

Oltre alla configurazione in `/etc/resolv.conf`, è possibile interrogare direttamente un server DNS per verificare a quale indirizzo IP corrisponde un nome, utile in fase di diagnosi:

{% highlight shell %}
nslookup google.com
dig google.com
{% endhighlight %}

`dig` fornisce un output più dettagliato ed è lo strumento preferito da chi amministra server DNS; `nslookup` è più semplice e immediato per un controllo rapido.

### NetworkManager da riga di comando

Sulle distribuzioni desktop che usano NetworkManager, è possibile gestire le connessioni anche da terminale con `nmcli`, senza passare dall'interfaccia grafica:

{% highlight shell %}
nmcli device status              # mostra lo stato delle interfacce di rete
nmcli connection show             # elenca le connessioni configurate
nmcli device wifi connect "NomeRete" password "password123"   # si collega a una rete Wi-Fi
{% endhighlight %}

Utile in particolare su server senza interfaccia grafica o durante l'installazione di un sistema da riga di comando.

### Named e porte: cosa sono davvero

Ogni servizio di rete (un server web, SSH, un database) resta in ascolto su una **porta**, un numero da 0 a 65535 che identifica lo specifico servizio su quella macchina, in aggiunta all'indirizzo IP che identifica la macchina stessa. Alcune porte sono standard per convenzione: la 22 per SSH, la 80 per il web non cifrato, la 443 per il web cifrato (HTTPS). Il comando `ss -tulwn`, già visto, mostra proprio su quali porte la macchina sta ascoltando connessioni in entrata.
