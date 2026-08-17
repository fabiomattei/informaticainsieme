---
title: 'MySQL'
date: '2026-08-17T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

![Confronto tra l'architettura embedded di SQLite e quella client-server di MySQL](/images/database/mysql/mysql.svg){:class="aside-image"}

**MySQL** è uno dei sistemi di gestione di basi di dati relazionali (RDBMS) più diffusi al mondo. Nasce nel 1995, viene acquisito da Sun Microsystems nel 2008 e infine da Oracle nel 2010, che ne detiene tuttora i diritti pur mantenendolo open source. È il motore su cui si basano moltissimi siti e applicazioni web: WordPress, Facebook (nelle sue prime versioni), YouTube e migliaia di altri servizi lo hanno usato o lo usano tuttora. È anche una delle quattro lettere della celebre sigla **LAMP** (Linux, Apache, MySQL, PHP), lo stack che per anni ha fatto girare gran parte del web.

Proprio perché Oracle ne detiene il controllo, nel 2009 alcuni sviluppatori storici del progetto hanno creato **MariaDB**, un fork completamente compatibile con MySQL (stessi comandi, stesso formato dei dati) ma sviluppato in modo indipendente. Quasi tutto ciò che vedremo in questa pagina vale identico anche per MariaDB.

### SQLite vs MySQL: la differenza architetturale

Negli esempi di [SQLite e Python]({{ site.baseurl }}{% link _database/sqlite-e-python.md %}.html) abbiamo usato SQLite, un database **embedded**: la libreria che lo gestisce è inclusa direttamente nell'applicazione, e i dati vivono in un singolo file sul disco locale. MySQL funziona in modo completamente diverso: è un database **client-server**.

* Un **processo server** (`mysqld`) gira in background, in ascolto su una porta di rete (di solito la 3306), e resta attivo indipendentemente dalle applicazioni che lo usano.
* I **client** — un'applicazione web, uno strumento grafico come phpMyAdmin, il terminale, un programma Python — si collegano al server via rete (anche da un'altra macchina) per inviare query e ricevere risultati.
* Molti client possono lavorare **contemporaneamente** sugli stessi dati: è il server a occuparsi di sincronizzare gli accessi concorrenti, mettendo in coda o isolando le operazioni che potrebbero interferire tra loro.
* Un singolo server MySQL può ospitare **più database indipendenti** allo stesso tempo (ad esempio uno per ogni sito web ospitato su quel server).
* Il server mantiene in memoria una cache dei dati usati più di frequente (nel motore InnoDB si chiama *buffer pool*): le letture ripetute sono quindi molto più veloci della semplice lettura da disco.

Questo rende MySQL adatto a scenari che SQLite non è pensato per gestire: un sito web con migliaia di visitatori simultanei, un'applicazione aziendale con decine di dipendenti collegati nello stesso momento, un servizio che deve essere raggiungibile da più macchine sulla rete.

### Installazione e primi strumenti

MySQL si installa con il gestore di pacchetti del proprio sistema operativo:

{% highlight bash %}
# Linux (Debian/Ubuntu)
sudo apt install mysql-server

# macOS (con Homebrew)
brew install mysql
{% endhighlight %}

In alternativa, per lo sviluppo locale si possono usare bundle già pronti come **XAMPP** o **WAMP**, che includono anche Apache e PHP, oppure un container **Docker** (lo vedremo più avanti). Praticamente tutti i servizi di hosting web offrono invece MySQL già installato e pronto all'uso: in quel caso non serve installare nulla, basta conoscere host, utente, password e nome del database forniti dal servizio.

Dopo l'installazione su un sistema Linux, il server va avviato come servizio:

{% highlight bash %}
sudo systemctl start mysql      # avvia il server
sudo systemctl status mysql     # verifica che sia in esecuzione
sudo systemctl enable mysql     # lo avvia automaticamente all'accensione
{% endhighlight %}

Su Linux e macOS è buona norma eseguire subito, una sola volta, lo script guidato di messa in sicurezza:

{% highlight bash %}
sudo mysql_secure_installation
{% endhighlight %}

Lo script chiede di impostare una password robusta per l'utente `root`, rimuove gli utenti anonimi creati di default, disabilita l'accesso remoto di `root` e cancella il database di prova `test` — tutte cose che, lasciate ai valori predefiniti, sono altrettante porte aperte su un server esposto in rete.

Gli strumenti più comuni per lavorare con MySQL sono:

* il **client a riga di comando** `mysql`, incluso nell'installazione;
* **phpMyAdmin**, un'interfaccia grafica web molto diffusa sugli hosting condivisi;
* **MySQL Workbench**, uno strumento grafico desktop ufficiale, con editor di query e progettazione visuale dello schema.

Per collegarsi da riga di comando:

{% highlight bash %}
mysql -u root -p
{% endhighlight %}

Il flag `-u` specifica l'utente, `-p` chiede la password interattivamente. Da qui in poi si digitano comandi SQL esattamente come nel client di SQLite. Per uscire dal client si digita `exit` oppure `quit`.

### phpMyAdmin in breve

Chi preferisce non lavorare da riga di comando può usare **phpMyAdmin**, un'interfaccia grafica che gira nel browser e parla con il server MySQL sottostante: praticamente tutto ciò che si può fare digitando SQL si può fare anche cliccando.

* Il pannello di sinistra elenca tutti i database visibili con l'utente con cui ci si è collegati: cliccandone uno si apre l'elenco delle sue tabelle.
* La scheda **Struttura** di una tabella mostra colonne, tipi e chiavi, e permette di aggiungere o modificare colonne con un modulo, senza scrivere ALTER TABLE a mano.
* La scheda **Sfoglia** mostra i dati riga per riga, con pulsanti per modificarli o cancellarli singolarmente — utile per correggere al volo un dato senza scrivere un UPDATE.
* La scheda **SQL** apre un editor libero dove incollare ed eseguire query scritte a mano, con i risultati mostrati in una tabella HTML.
* La voce **Esporta** genera un backup nello stesso formato prodotto da `mysqldump`, direttamente dal browser, senza bisogno del terminale.

phpMyAdmin è comodissimo per l'esplorazione rapida e per chi sta imparando, ma va usato con attenzione: essendo raggiungibile via browser, se esposto pubblicamente su internet senza protezioni aggiuntive diventa un facile bersaglio. Sugli hosting condivisi è quasi sempre protetto da un login separato o raggiungibile solo dalla rete interna.

### Esplorare il server: SHOW e DESCRIBE

Prima ancora di creare qualcosa, è utile sapere come guardarsi intorno. MySQL mette a disposizione una famiglia di comandi SHOW per ispezionare cosa esiste già sul server:

{% highlight sql %}
SHOW DATABASES;              -- elenca tutti i database sul server
SHOW TABLES;                 -- elenca le tabelle del database selezionato con USE
DESCRIBE alunni;              -- mostra le colonne della tabella alunni, con tipi e vincoli
SHOW CREATE TABLE alunni;     -- mostra il comando CREATE TABLE esatto usato per crearla
{% endhighlight %}

`DESCRIBE` (abbreviabile in `DESC`) è probabilmente il comando che si userà più spesso in assoluto: torna comodo ogni volta che ci si dimentica come si chiamava esattamente una colonna, o che tipo aveva.

### Creiamo un database

A differenza di SQLite, dove un file *è* il database, in MySQL bisogna prima creare esplicitamente un database all'interno del server, e poi selezionarlo con USE prima di usarlo:

{% highlight sql %}
CREATE DATABASE scuola
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

USE scuola;
{% endhighlight %}

Da questo momento, tutti i comandi CREATE TABLE, INSERT, SELECT eseguiti nella stessa sessione operano sul database `scuola`. Le clausole CHARACTER SET e COLLATE non sono obbligatorie, ma è buona pratica specificarle sempre: `utf8mb4` è la codifica che supporta correttamente accenti, ideogrammi ed emoji (il vecchio alias `utf8` di MySQL, per ragioni storiche, supporta solo un sottoinsieme di Unicode e può troncare silenziosamente alcuni caratteri).

Per eliminare un intero database, con tutte le tabelle e i dati che contiene:

{% highlight sql %}
DROP DATABASE scuola;
{% endhighlight %}

Vale lo stesso avvertimento visto per DROP TABLE in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html): è un'operazione irreversibile.

### I tipi di dato

Abbiamo visto in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html) che SQLite semplifica tutto a soli cinque tipi generici. MySQL, come la maggior parte dei database client-server, ne offre molti di più, più precisi:

| Tipo | Uso |
|---|---|
| TINYINT, SMALLINT, INT, BIGINT | numeri interi di dimensione crescente (TINYINT: -128..127, INT: circa ±2 miliardi) |
| DECIMAL(m,d) | numero decimale **esatto**, con m cifre totali e d dopo la virgola (ideale per gli importi in euro) |
| FLOAT, DOUBLE | numeri decimali **approssimati** a virgola mobile, più veloci ma non adatti a valori monetari |
| CHAR(n) | testo di lunghezza **fissa** n, riempito con spazi se più corto |
| VARCHAR(n) | testo di lunghezza **variabile**, fino a n caratteri |
| TEXT | testo lungo, senza limite pratico di lunghezza |
| DATE | una data (AAAA-MM-GG) |
| DATETIME | data e ora, senza legame con il fuso orario |
| TIMESTAMP | data e ora, convertita e memorizzata in UTC: utile per registrare *quando* è avvenuto un evento in modo confrontabile tra fusi orari diversi |
| BOOLEAN | vero/falso (in realtà un alias di TINYINT(1)) |
| ENUM('a','b','c') | testo vincolato a un insieme fisso di valori possibili |
| JSON | un documento JSON, con funzioni dedicate per leggerne e modificarne i campi |

Questa precisione ha un costo: bisogna decidere in anticipo, ad esempio, quanto può essere lungo al massimo un nome (VARCHAR(50)? VARCHAR(100)?), una scelta che con SQLite semplicemente non ci si pone. In compenso questi vincoli aiutano a individuare subito, al momento dell'inserimento, dati che non hanno senso: un ENUM impedisce di scrivere "Marschio" invece di "Maschio", cosa che una semplice colonna TEXT lascerebbe passare senza accorgersi dell'errore.

Un piccolo esempio di ENUM:

{% highlight sql %}
CREATE TABLE prodotti (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    stato ENUM('disponibile', 'esaurito', 'in arrivo') DEFAULT 'disponibile'
);
{% endhighlight %}

### Creiamo una tabella

{% highlight sql %}
CREATE TABLE alunni (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    cognome VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE
);
{% endhighlight %}

La sintassi è quasi identica a quella vista con SQLite, con una piccola ma frequente insidia: l'attributo si chiama **AUTO_INCREMENT** (con l'underscore), non AUTOINCREMENT come in SQLite. È l'errore di battitura più comune per chi passa da un motore all'altro.

I comandi INSERT, SELECT, UPDATE, DELETE, le clausole WHERE, ORDER BY, GROUP BY, HAVING e le JOIN funzionano esattamente come descritto in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html): fanno parte dello standard SQL, non di uno specifico database.

### Modificare una tabella già esistente: ALTER TABLE

Le tabelle raramente restano ferme come sono state pensate il primo giorno. Il comando ALTER TABLE permette di modificarne la struttura senza doverla ricreare da zero e senza perdere i dati già presenti:

{% highlight sql %}
-- aggiungere una colonna
ALTER TABLE alunni ADD COLUMN data_nascita DATE;

-- modificare il tipo o i vincoli di una colonna esistente
ALTER TABLE alunni MODIFY COLUMN cognome VARCHAR(80) NOT NULL;

-- rinominare una colonna (cambiandone anche il tipo)
ALTER TABLE alunni CHANGE COLUMN email indirizzo_email VARCHAR(150);

-- eliminare una colonna
ALTER TABLE alunni DROP COLUMN data_nascita;

-- rinominare l'intera tabella
ALTER TABLE alunni RENAME TO studenti;
{% endhighlight %}

Su una tabella con milioni di righe alcune di queste operazioni possono richiedere tempo e bloccare temporaneamente gli accessi: è uno dei motivi per cui, quando possibile, conviene pensare bene alla struttura delle tabelle fin dalla progettazione, come visto in [Le forme normali]({{ site.baseurl }}{% link _database/forme-normali.md %}.html).

### Indici

Una SELECT con una clausola WHERE, senza aiuto, costringe MySQL a scorrere ogni singola riga della tabella per verificare se soddisfa la condizione: su una tabella di poche righe non se ne accorge nessuno, ma su milioni di righe la differenza è tra una query istantanea e una che impiega diversi secondi.

Un **indice** è una struttura dati aggiuntiva, mantenuta automaticamente dal database, che permette di trovare le righe che soddisfano una condizione senza doverle scorrere tutte — un po' come l'indice analitico di un libro, che evita di sfogliarlo pagina per pagina per trovare un argomento.

{% highlight sql %}
CREATE INDEX idx_cognome ON alunni (cognome);
{% endhighlight %}

Da questo momento, una query come `SELECT * FROM alunni WHERE cognome = 'Rossi'` userà l'indice invece di scorrere l'intera tabella. Una colonna dichiarata PRIMARY KEY o UNIQUE riceve automaticamente un indice, perché il database deve comunque poter verificare rapidamente l'unicità dei valori.

Gli indici non sono gratuiti: occupano spazio e rallentano leggermente le operazioni di INSERT, UPDATE e DELETE, perché ogni modifica ai dati deve aggiornare anche l'indice. La regola pratica è: **indicizzare le colonne usate spesso nelle WHERE, nelle JOIN o negli ORDER BY**, non tutte le colonne indiscriminatamente.

Per vedere se e come una query sta effettivamente usando gli indici disponibili si usa EXPLAIN davanti alla query:

{% highlight sql %}
EXPLAIN SELECT * FROM alunni WHERE cognome = 'Rossi';
{% endhighlight %}

Il risultato mostra, tra le altre cose, se MySQL sta usando l'indice `idx_cognome` oppure se sta eseguendo una scansione completa della tabella (`type: ALL`, il caso da evitare su tabelle grandi).

### Funzioni utili di MySQL

Oltre alle funzioni aggregate viste in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html), MySQL mette a disposizione centinaia di funzioni predefinite per manipolare stringhe, date e numeri direttamente nella query. Alcune tra le più usate nella pratica quotidiana:

{% highlight sql %}
SELECT CONCAT(nome, ' ', cognome) AS nome_completo FROM alunni;
SELECT UPPER(cognome), LOWER(nome) FROM alunni;
SELECT NOW();                                    -- data e ora correnti
SELECT DATE_FORMAT(NOW(), '%d/%m/%Y');            -- 17/08/2026
SELECT IFNULL(telefono, 'non disponibile') FROM alunni;
{% endhighlight %}

`IFNULL(colonna, valore_alternativo)` è particolarmente utile: restituisce `valore_alternativo` al posto di NULL, evitando che un dato mancante si propaghi come NULL fino al risultato finale mostrato all'utente.

Una funzione che merita una menzione a parte è **GROUP_CONCAT**, perché estende GROUP BY in un modo che SQLite non offre in forma altrettanto diretta: invece di restituire un solo valore aggregato per gruppo, concatena in un'unica stringa tutti i valori del gruppo.

{% highlight sql %}
SELECT alunni.cognome, GROUP_CONCAT(voti.materia SEPARATOR ', ') AS materie
FROM alunni
JOIN voti ON alunni.id = voti.id_alunno
GROUP BY alunni.id;
{% endhighlight %}

| cognome | materie |
|---|---|
| Rossi | Matematica, Storia, Italiano |
| Bianchi | Matematica, Inglese |

Senza GROUP_CONCAT, ottenere questo stesso risultato richiederebbe di elaborare i dati riga per riga lato applicazione dopo averli letti con una normale JOIN.

### Ricerca full-text

Un filtro con `WHERE titolo LIKE '%parola%'`, visto in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html), funziona ma non può usare un indice: MySQL è costretto a scorrere ogni riga e ogni carattere. Per ricerche testuali frequenti su colonne di testo lungo (titoli, descrizioni, articoli) esiste un tipo di indice dedicato, l'**indice FULLTEXT**, pensato per cercare parole indipendentemente dalla loro posizione nel testo:

{% highlight sql %}
ALTER TABLE film ADD FULLTEXT (titolo, trama);

SELECT titolo FROM film
WHERE MATCH(titolo, trama) AGAINST('cowboy deserto');
{% endhighlight %}

A differenza di LIKE, MATCH...AGAINST usa l'indice per trovare rapidamente le righe rilevanti, ordina i risultati per rilevanza (quante volte e quanto "centralmente" compaiono le parole cercate) e ignora automaticamente parole troppo comuni per essere utili come filtro (dette *stopword*, ad esempio "il", "di", "che").

### JSON in pratica

Dalla versione 5.7, MySQL offre un tipo di colonna JSON nativo, utile quando alcuni dati hanno una struttura variabile che non conviene forzare in colonne fisse — ad esempio le caratteristiche tecniche di prodotti molto diversi tra loro:

{% highlight sql %}
CREATE TABLE prodotti (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    caratteristiche JSON
);

INSERT INTO prodotti (nome, caratteristiche) VALUES
('Laptop', '{"ram_gb": 16, "colore": "grigio"}'),
('Mouse', '{"colore": "nero", "wireless": true}');
{% endhighlight %}

L'operatore `->>` estrae un campo dal documento JSON come testo, e può essere usato anche in una WHERE:

{% highlight sql %}
SELECT nome, caratteristiche->>'$.colore' AS colore FROM prodotti;
SELECT nome FROM prodotti WHERE caratteristiche->>'$.ram_gb' > 8;
{% endhighlight %}

Il JSON va usato con misura: è comodo per dati davvero variabili, ma un uso eccessivo — infilarci dati che in realtà sono sempre gli stessi campi per ogni riga — è quasi sempre un segnale che quei campi avrebbero dovuto essere normali colonne, magari con le tecniche di normalizzazione viste in [Le forme normali]({{ site.baseurl }}{% link _database/forme-normali.md %}.html).

### Funzioni di finestra e CTE (MySQL 8+)

Le **funzioni di finestra** (window function) assomigliano alle funzioni aggregate viste con GROUP BY, ma con una differenza fondamentale: invece di comprimere un gruppo di righe in una sola riga di risultato, calcolano un valore aggregato **mantenendo tutte le righe originali visibili**.

{% highlight sql %}
SELECT nome, cognome, materia, voto,
       RANK() OVER (PARTITION BY materia ORDER BY voto DESC) AS posizione
FROM voti
JOIN alunni ON voti.id_alunno = alunni.id;
{% endhighlight %}

Questa query mostra ogni singolo voto, ma aggiunge una colonna `posizione` che classifica ogni alunno rispetto agli altri **nella stessa materia** (grazie a PARTITION BY), senza raggruppare né perdere il dettaglio riga per riga come farebbe un GROUP BY.

Le **CTE** (Common Table Expression, introdotte con la clausola WITH) permettono di dare un nome a una sotto-query e riutilizzarla come se fosse una tabella temporanea, rendendo più leggibili le query complesse:

{% highlight sql %}
WITH media_per_materia AS (
    SELECT materia, AVG(voto) AS media
    FROM voti
    GROUP BY materia
)
SELECT * FROM media_per_materia WHERE media < 6;
{% endhighlight %}

Senza la CTE, la stessa query richiederebbe di ripetere l'intera sotto-query ovunque servisse, oppure di annidarla in modo meno leggibile dentro la FROM.

### Chiavi esterne e InnoDB

MySQL supporta diversi **motori di archiviazione** (storage engine), ciascuno con caratteristiche diverse:

| Motore | Transazioni | Chiavi esterne | Note |
|---|---|---|---|
| InnoDB | sì | sì | il motore predefinito dalla versione 5.5 in poi, adatto quasi sempre |
| MyISAM | no | no | più vecchio, più veloce in sola lettura ma senza le garanzie di InnoDB |
| MEMORY | no | no | tabelle temporanee tenute interamente in RAM, dati persi al riavvio del server |

Nella pratica quotidiana, salvo esigenze molto specifiche, si usa sempre **InnoDB**. A differenza di SQLite — dove il controllo delle chiavi esterne va abilitato esplicitamente con `PRAGMA foreign_keys = ON;` — con InnoDB i vincoli FOREIGN KEY sono attivi di default:

{% highlight sql %}
CREATE TABLE voti (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_alunno INT NOT NULL,
    materia VARCHAR(50),
    voto DECIMAL(3,1),
    FOREIGN KEY (id_alunno) REFERENCES alunni(id)
        ON DELETE CASCADE
) ENGINE=InnoDB;
{% endhighlight %}

La clausola ON DELETE CASCADE stabilisce cosa succede ai voti quando l'alunno a cui si riferiscono viene cancellato. Le opzioni principali sono:

* **CASCADE** — cancella automaticamente anche tutte le righe collegate (qui: se un alunno viene cancellato, i suoi voti vengono cancellati con lui);
* **SET NULL** — imposta a NULL la chiave esterna delle righe collegate, invece di cancellarle (richiede che la colonna ammetta NULL);
* **RESTRICT** (il comportamento predefinito se non si specifica nulla) — impedisce la cancellazione della riga referenziata finché esistono righe collegate, sollevando un errore.

La scelta giusta dipende dal significato dei dati: cancellare in cascata i voti di uno studente cancellato ha senso, ma cancellare in cascata tutti gli ordini di un cliente cancellato, in un negozio, probabilmente no — lì avrebbe più senso RESTRICT, o mantenere lo storico impostando il cliente a NULL.

Per una spiegazione generale di come le chiavi esterne realizzano le relazioni tra tabelle, vedi [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html).

### Transazioni

Una **transazione** è un gruppo di operazioni che devono essere eseguite tutte insieme, oppure per niente: se una di esse fallisce a metà, tutte le altre vengono annullate, lasciando il database esattamente come si trovava prima di iniziare. È la garanzia di **atomicità** dell'acronimo ACID già incontrato parlando di SQLite in [SQLite e Python]({{ site.baseurl }}{% link _database/sqlite-e-python.md %}.html).

L'esempio classico è un bonifico bancario: prelevare da un conto e versare su un altro sono due operazioni distinte, ma devono avvenire entrambe o nessuna delle due — altrimenti, in caso di guasto a metà strada, i soldi rischiano di sparire o di raddoppiarsi.

{% highlight sql %}
START TRANSACTION;

UPDATE conti SET saldo = saldo - 100 WHERE id = 1;
UPDATE conti SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
{% endhighlight %}

Se qualcosa va storto prima del COMMIT (ad esempio il secondo UPDATE fallisce perché il conto 2 non esiste più), si usa ROLLBACK per annullare tutto quanto fatto dall'inizio della transazione:

{% highlight sql %}
START TRANSACTION;

UPDATE conti SET saldo = saldo - 100 WHERE id = 1;
-- qualcosa va storto...
ROLLBACK;   -- il prelievo da id = 1 viene annullato, come se non fosse mai avvenuto
{% endhighlight %}

Solo i motori transazionali come InnoDB supportano START TRANSACTION, COMMIT e ROLLBACK: è un altro dei motivi per cui InnoDB è la scelta predefinita.

### Utenti e permessi

Questa è una differenza sostanziale rispetto a SQLite, che non ha alcun concetto di utente: essendo pensato per più persone e applicazioni che condividono lo stesso server, MySQL ha un sistema di **utenti e permessi**. Usare sempre l'utente amministratore `root` per ogni applicazione è una cattiva pratica: se le credenziali dell'applicazione trapelano, chi le ottiene ha accesso a tutto il server.

{% highlight sql %}
CREATE USER 'marco'@'localhost' IDENTIFIED BY 'password_sicura';
GRANT SELECT, INSERT, UPDATE ON scuola.* TO 'marco'@'localhost';
FLUSH PRIVILEGES;
{% endhighlight %}

Questo comando crea un utente `marco` che può connettersi solo da `localhost`, e gli concede solo i permessi di lettura, inserimento e modifica sul database `scuola` — non, ad esempio, il permesso di cancellare tabelle (DROP) o creare nuovi utenti. È il principio del **minimo privilegio**: ogni account ha solo i permessi strettamente necessari al suo scopo.

La parte `'marco'@'localhost'` non è solo un nome utente: in MySQL un account è sempre la combinazione di **nome utente + host di provenienza**. Lo stesso nome può esistere più volte con permessi diversi a seconda da dove si connette:

{% highlight sql %}
CREATE USER 'marco'@'localhost' IDENTIFIED BY 'password_locale';
CREATE USER 'marco'@'%' IDENTIFIED BY 'password_remota';
{% endhighlight %}

Il simbolo `%` è un carattere jolly che significa "da qualsiasi host": va usato con cautela, perché consente connessioni da qualunque indirizzo IP raggiunga il server. Per verificare quali permessi ha effettivamente un utente:

{% highlight sql %}
SHOW GRANTS FOR 'marco'@'localhost';
{% endhighlight %}

Per togliere un permesso concesso in precedenza si usa REVOKE (l'esatto opposto di GRANT), e per eliminare un account:

{% highlight sql %}
REVOKE UPDATE ON scuola.* FROM 'marco'@'localhost';
DROP USER 'marco'@'localhost';
{% endhighlight %}

### Viste (VIEW)

Una **vista** è una query SELECT salvata con un nome, che si può interrogare come se fosse una tabella vera e propria. Non contiene dati propri: ogni volta che viene interrogata, MySQL riesegue la query che la definisce.

{% highlight sql %}
CREATE VIEW media_voti AS
SELECT alunni.nome, alunni.cognome, AVG(voti.voto) AS media
FROM alunni
JOIN voti ON alunni.id = voti.id_alunno
GROUP BY alunni.id;
{% endhighlight %}

Una volta creata, si interroga come una tabella qualunque:

{% highlight sql %}
SELECT * FROM media_voti WHERE media > 7;
{% endhighlight %}

Le viste sono utili per due motivi: nascondono la complessità di una JOIN o di un GROUP BY dietro un nome semplice da ricordare, e permettono di dare a un utente accesso solo a un sottoinsieme controllato di colonne (ad esempio una vista `alunni_pubblici` che esclude email e dati sensibili), senza concedergli l'accesso diretto alla tabella completa.

### Stored procedure e trigger

Una **stored procedure** è un blocco di istruzioni SQL, con un nome, salvato ed eseguito direttamente sul server. Va utile quando una stessa sequenza di operazioni viene ripetuta spesso, perché evita di riscriverla ogni volta lato applicazione:

{% highlight sql %}
DELIMITER //

CREATE PROCEDURE aggiungi_voto(IN alunno INT, IN voto_materia VARCHAR(50), IN voto_valore DECIMAL(3,1))
BEGIN
    INSERT INTO voti (id_alunno, materia, voto) VALUES (alunno, voto_materia, voto_valore);
END //

DELIMITER ;
{% endhighlight %}

Il comando DELIMITER cambia temporaneamente il carattere che segnala la fine di un'istruzione (di solito `;`), perché il corpo della procedura contiene già dei punto e virgola al suo interno e altrimenti il client interromperebbe la definizione a metà. Una volta creata, la procedura si richiama con CALL:

{% highlight sql %}
CALL aggiungi_voto(1, 'Matematica', 8.0);
{% endhighlight %}

Un **trigger** è simile, ma invece di essere richiamato esplicitamente si attiva automaticamente in risposta a un evento su una tabella (un INSERT, UPDATE o DELETE):

{% highlight sql %}
DELIMITER //

CREATE TRIGGER log_nuovo_voto
AFTER INSERT ON voti
FOR EACH ROW
BEGIN
    INSERT INTO log_modifiche (descrizione, quando)
    VALUES (CONCAT('Nuovo voto per alunno ', NEW.id_alunno), NOW());
END //

DELIMITER ;
{% endhighlight %}

`NEW` fa riferimento alla riga appena inserita: da questo momento, ogni INSERT nella tabella `voti` scrive automaticamente una riga di log, senza che l'applicazione debba occuparsene esplicitamente. Procedure e trigger sono strumenti potenti ma vanno usati con misura: spostano logica dall'applicazione al database, rendendola meno visibile a chi legge solo il codice dell'applicazione.

### Backup e ripristino

Un database che non viene mai salvato altrove è un database che, prima o poi, si perde: un disco che si guasta, un comando DROP eseguito per errore, un aggiornamento andato male. MySQL include `mysqldump`, uno strumento a riga di comando (da eseguire nel terminale del sistema operativo, non dentro al client `mysql`) che esporta un database in un file di testo contenente tutti i comandi SQL necessari a ricrearlo:

{% highlight bash %}
mysqldump -u root -p scuola > backup_scuola.sql
{% endhighlight %}

Per ripristinare il database da quel file, su un server nuovo o dopo un disastro:

{% highlight bash %}
mysql -u root -p scuola < backup_scuola.sql
{% endhighlight %}

In un contesto reale questo comando viene tipicamente eseguito automaticamente ogni notte da un `cron job`, con i backup conservati per un certo numero di giorni: un backup vecchio di un mese, quando serve davvero, è quasi sempre inutile quanto non averne nessuno.

### Replica e alta disponibilità

Il backup risponde alla domanda "come recupero i dati dopo un disastro?", ma richiede tempo per essere ripristinato e nel frattempo il servizio resta fermo. Sui progetti più grandi, un solo server MySQL è comunque un **singolo punto di guasto**: se si ferma per un guasto hardware o un aggiornamento, l'intero sito o applicazione si ferma con lui. La **replica** (replication) affronta questo problema diverso mantenendo una o più copie sincronizzate dello stesso database su server distinti, pronte a subentrare quasi immediatamente.

Nello schema più comune, chiamato *master-replica* (in passato *master-slave*), un server **primario** riceve tutte le scritture (INSERT, UPDATE, DELETE) e le propaga automaticamente a uno o più server **secondari**, che restano allineati in tempo quasi reale. I server secondari possono farsi carico delle sole letture (SELECT), alleggerendo il carico sul primario, e sono pronti a sostituirlo rapidamente in caso di guasto.

Impostare la replica richiede una configurazione dedicata sul file `my.cnf` di ciascun server (identificativi univoci, log binari abilitati) ed esula dagli scopi di questa introduzione, ma è utile sapere che il problema — "cosa succede se il server si guasta?" — ha una risposta consolidata, ed è uno dei motivi per cui, superata una certa scala, si sceglie MySQL invece di un database embedded come SQLite: SQLite, vivendo in un unico file locale, non offre nulla di equivalente.

### MySQL e Python

Il modo di lavorare da Python è molto simile a quanto visto con `sqlite3` in [SQLite e Python]({{ site.baseurl }}{% link _database/sqlite-e-python.md %}.html), con la libreria `mysql-connector-python` (si installa con `pip install mysql-connector-python`):

{% highlight python %}
import mysql.connector

db = mysql.connector.connect(
    host="localhost",
    user="marco",
    password="password_sicura",
    database="scuola"
)
cursor = db.cursor()

cursor.execute("SELECT nome, cognome FROM alunni")
for (nome, cognome) in cursor.fetchall():
    print(nome, cognome)

db.close()
{% endhighlight %}

Le differenze principali rispetto a `sqlite3`:

* `connect()` richiede host, utente, password e nome del database, non un semplice percorso di file: bisogna sapere *dove* si trova il server, non solo *quale file* aprire;
* nelle query parametrizzate il segnaposto è **%s**, non **?** come in SQLite — un dettaglio facile da dimenticare quando si passa da un motore all'altro:

{% highlight python %}
cursor.execute("SELECT * FROM alunni WHERE cognome = %s", (cognome,))
{% endhighlight %}

A parte queste differenze, l'uso del cursore — `execute()`, `fetchall()`, `fetchone()`, `commit()` — è concettualmente identico a quanto già visto con SQLite.

Come per SQLite, un errore in mezzo a più operazioni collegate lascia i dati a metà strada se non si gestisce l'errore: conviene racchiudere le operazioni di scrittura in un blocco try/except che esegue un rollback in caso di problemi:

{% highlight python %}
import mysql.connector

db = mysql.connector.connect(
    host="localhost", user="marco", password="password_sicura", database="scuola"
)
cursor = db.cursor()

try:
    cursor.execute("UPDATE alunni SET cognome = %s WHERE id = %s", ("Rossi", 1))
    cursor.execute("INSERT INTO voti (id_alunno, materia, voto) VALUES (%s, %s, %s)", (1, "Storia", 7.5))
    db.commit()
except mysql.connector.Error as errore:
    print(f"Qualcosa è andato storto, annullo tutto: {errore}")
    db.rollback()
finally:
    db.close()
{% endhighlight %}

Per leggere le righe come dizionari invece che come tuple posizionali (`riga['nome']` invece di `riga[0]`), analogamente a `sqlite3.Row` visto per SQLite, si passa `dictionary=True` al momento di creare il cursore:

{% highlight python %}
cursor = db.cursor(dictionary=True)
cursor.execute("SELECT nome, cognome FROM alunni")
for riga in cursor.fetchall():
    print(riga['nome'], riga['cognome'])
{% endhighlight %}

Infine, host, utente e password non andrebbero mai scritti direttamente nel codice sorgente (e tantomeno pubblicati su un repository condiviso): si tende a leggerli da variabili d'ambiente, così le credenziali reali restano fuori dal codice.

{% highlight python %}
import os
import mysql.connector

db = mysql.connector.connect(
    host=os.environ["DB_HOST"],
    user=os.environ["DB_USER"],
    password=os.environ["DB_PASSWORD"],
    database=os.environ["DB_NAME"],
)
{% endhighlight %}

### Sicurezza: l'SQL injection

Il motivo per cui le query vanno sempre parametrizzate con `%s` (o `?` in SQLite), invece di costruire la stringa SQL concatenando manualmente i valori, non è solo una questione di stile. Consideriamo questo codice, scritto **male**:

{% highlight python %}
# NON FARE COSÌ
cognome = input("Cerca per cognome: ")
query = f"SELECT * FROM alunni WHERE cognome = '{cognome}'"
cursor.execute(query)
{% endhighlight %}

Se un utente malintenzionato digita, invece di un normale cognome, la stringa `' OR '1'='1`, la query eseguita diventa:

{% highlight sql %}
SELECT * FROM alunni WHERE cognome = '' OR '1'='1'
{% endhighlight %}

La condizione `'1'='1'` è sempre vera, quindi la query restituisce **tutte** le righe della tabella, bypassando completamente il filtro previsto. Con input ancora più elaborati si possono arrivare a cancellare tabelle, leggere dati di altri utenti o, in certi casi, prendere il controllo dell'intero server: questa classe di attacchi si chiama **SQL injection**, ed è stata per anni una delle vulnerabilità più sfruttate sul web.

La query parametrizzata evita il problema alla radice, perché il driver invia al server il testo della query e i valori **separatamente**: il server sa sempre con certezza che il contenuto di `cognome` è un dato, mai un pezzo di comando SQL da eseguire, qualunque cosa contenga.

{% highlight python %}
# corretto
cognome = input("Cerca per cognome: ")
cursor.execute("SELECT * FROM alunni WHERE cognome = %s", (cognome,))
{% endhighlight %}

La regola pratica è semplice quanto tassativa: **mai** costruire una query concatenando input dell'utente dentro la stringa SQL, **sempre** passarlo come parametro separato.

### MySQL con Docker

Per lo sviluppo locale, senza installare nulla direttamente sul proprio sistema operativo, è comune avviare MySQL in un container Docker:

{% highlight bash %}
docker run --name mysql-scuola \
  -e MYSQL_ROOT_PASSWORD=password_sicura \
  -e MYSQL_DATABASE=scuola \
  -p 3306:3306 \
  -d mysql:8
{% endhighlight %}

Questo comando scarica (se non già presente) l'immagine ufficiale di MySQL versione 8, avvia un server con quella password per `root`, crea subito un database `scuola`, e rende la porta 3306 del container raggiungibile dalla porta 3306 della macchina locale. A quel punto ci si connette esattamente come a un'installazione normale (`mysql -h 127.0.0.1 -u root -p`). Per fermarlo e rimuoverlo:

{% highlight bash %}
docker stop mysql-scuola
docker rm mysql-scuola
{% endhighlight %}

Il vantaggio è che l'intero server, con tutti i suoi dati, vive isolato dentro al container: cancellarlo non lascia file sparsi sul sistema, e si può avere in parallelo una versione diversa di MySQL per ogni progetto senza che entrino in conflitto tra loro.

Per un progetto con più servizi (ad esempio MySQL insieme a un'applicazione web che vi si collega) conviene descrivere l'intero ambiente in un file `docker-compose.yml`, invece di ricordarsi a memoria il comando `docker run` con tutte le sue opzioni:

{% highlight yaml %}
services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: password_sicura
      MYSQL_DATABASE: scuola
    ports:
      - "3306:3306"
    volumes:
      - dati_mysql:/var/lib/mysql

volumes:
  dati_mysql:
{% endhighlight %}

Con `docker compose up -d` l'intero ambiente si avvia in un solo comando. La sezione `volumes` è importante: senza di essa, i dati vivrebbero solo dentro al container e andrebbero persi cancellandolo; con un volume dedicato (`dati_mysql`), i dati sopravvivono anche se il container viene ricreato da zero.

### Monitorare il server

Quando un'applicazione sembra "bloccata" su un'operazione al database, il primo posto dove guardare è l'elenco delle query in corso di esecuzione sul server:

{% highlight sql %}
SHOW PROCESSLIST;
{% endhighlight %}

Il risultato mostra ogni connessione attiva, da quanto tempo è aperta, cosa sta facendo e, se sta eseguendo una query, quale. È il modo più rapido per scoprire una query che gira da minuti bloccando le altre, magari perché ha dimenticato un WHERE (come visto per UPDATE e DELETE in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html)) o perché è in attesa di un lock tenuto da un'altra transazione mai chiusa con COMMIT.

Per un'analisi più sistematica, MySQL può registrare automaticamente in un file di log tutte le query più lente di una certa soglia:

{% highlight sql %}
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 1;   -- registra le query più lente di 1 secondo
{% endhighlight %}

Rileggere periodicamente questo log è spesso il modo più efficace per scoprire *quali* query meriterebbero un indice (visto sopra), invece di indicizzare colonne a caso sperando che aiuti.

### Errori comuni

Alcuni messaggi d'errore ricorrono così spesso, soprattutto nelle prime settimane di uso di MySQL, da meritare una traduzione a parte:

| Errore | Causa più probabile |
|---|---|
| `Access denied for user 'x'@'y'` | utente, password o host di provenienza non corrispondono a nessun account creato con CREATE USER / GRANT |
| `Unknown database 'x'` | il database non esiste ancora, oppure ci si è dimenticati di eseguire USE dopo essersi collegati |
| `Table 'x.y' doesn't exist` | nome della tabella sbagliato, oppure ci si è collegati al database sbagliato |
| `Duplicate entry 'x' for key 'y'` | si sta violando un vincolo UNIQUE o PRIMARY KEY inserendo un valore già presente |
| `Cannot add or update a child row: a foreign key constraint fails` | si sta inserendo in una colonna FOREIGN KEY un valore che non esiste nella tabella referenziata |
| `Lock wait timeout exceeded` | una transazione sta aspettando un lock tenuto da un'altra transazione mai chiusa con COMMIT o ROLLBACK |

Il denominatore comune di quasi tutti questi errori è che MySQL, a differenza di un semplice file di testo, **si rifiuta attivamente** di eseguire operazioni che violerebbero le regole dichiarate nello schema: è esattamente il tipo di garanzia di integrità descritta nell'introduzione a [Database]({{ site.baseurl }}{% link _database/database.md %}.html) come uno dei motivi per usare un DBMS al posto di un file.

### Da SQLite a MySQL: le differenze in sintesi

Chi ha imparato prima SQLite, come nel percorso di questo capitolo, incontra sempre le stesse piccole insidie passando a MySQL. Un riepilogo rapido:

| Aspetto | SQLite | MySQL |
|---|---|---|
| Architettura | embedded, un file | client-server, un processo in rete |
| Auto-incremento | AUTOINCREMENT | AUTO_INCREMENT |
| Segnaposto nelle query parametrizzate (Python) | `?` | `%s` |
| Chiavi esterne | disattivate di default (PRAGMA foreign_keys = ON) | attive di default con InnoDB |
| Utenti e permessi | non esistono | CREATE USER, GRANT, REVOKE |
| Tipi di dato | 5 tipi generici | decine di tipi precisi (VARCHAR(n), DECIMAL(m,d), ENUM, JSON...) |
| Connessione da Python | percorso di un file | host, utente, password, nome database |

Nessuna delle due colonne è "quella sbagliata": sono conseguenze dirette delle due architetture viste all'inizio di questa pagina, non scelte arbitrarie.

### Quando scegliere MySQL e quando SQLite

| Situazione | Motore consigliato |
|---|---|
| App mobile o desktop che usa dati solo localmente | SQLite |
| Prototipo, esercizio, piccolo progetto scolastico | SQLite |
| Sito web con molti visitatori simultanei | MySQL |
| Applicazione con più utenti che scrivono contemporaneamente | MySQL |
| Necessità di accedere ai dati da più macchine sulla rete | MySQL |
| Necessità di gestire permessi diversi per persone diverse | MySQL |

Non è una gerarchia di qualità: sono due strumenti pensati per problemi diversi. Usare MySQL per un piccolo esercizio locale è come installare un server aziendale per tenere la lista della spesa. Vale la pena sapere che MySQL non è l'unica alternativa client-server: **PostgreSQL** è un altro RDBMS open source molto diffuso, spesso preferito per applicazioni con schemi di dati complessi o che richiedono funzionalità avanzate (tipi di dato geometrici, ricerche full-text sofisticate); i concetti visti in questa pagina — client-server, utenti e permessi, transazioni, indici — si applicano, con sintassi leggermente diversa, anche a lui.

### Esercizi

**Esercizio 1 — Primi passi**

1. Collegati al server MySQL da riga di comando con `mysql -u root -p`.
2. Crea un database chiamato `biblioteca` con charset `utf8mb4` e selezionalo con USE.
3. Crea le tabelle `libri (id, titolo, autore)` e `prestiti (id, id_libro, data_prestito)`, con una FOREIGN KEY che collega `prestiti.id_libro` a `libri.id`. Usa ENGINE=InnoDB.
4. Inserisci 3 libri e 2 prestiti, poi visualizza con una JOIN il titolo di ogni libro insieme alla relativa data di prestito.
5. Usa SHOW TABLES e DESCRIBE per verificare la struttura delle tabelle appena create.

**Esercizio 2 — ALTER TABLE e indici**

1. Nel database `biblioteca`, aggiungi alla tabella `libri` una colonna `genere VARCHAR(30)`.
2. Rinomina la colonna `autore` in `nome_autore`.
3. Crea un indice sulla colonna `nome_autore`, motivando in una frase perché può essere utile.
4. Usa EXPLAIN su una SELECT che filtra per `nome_autore` e osserva se l'indice viene utilizzato.

**Esercizio 3 — Chiavi esterne e cancellazioni**

1. Ricrea la tabella `prestiti` in modo che, cancellando un libro, vengano cancellati automaticamente anche i suoi prestiti (ON DELETE CASCADE).
2. Motiva, in una frase, se ON DELETE CASCADE sia una scelta sensata anche per una tabella `multe` collegata ai prestiti in ritardo, oppure se sarebbe più corretto usare RESTRICT.

**Esercizio 4 — Transazioni**

1. Crea una tabella `conti (id, intestatario, saldo)` e inserisci due conti con saldo iniziale di 500€ ciascuno.
2. Scrivi una transazione che trasferisce 100€ dal primo conto al secondo, usando START TRANSACTION e COMMIT.
3. Scrivi, questa volta senza eseguirla fino in fondo, una transazione che si interrompe con ROLLBACK dopo il primo UPDATE: verifica che il saldo del primo conto sia rimasto invariato.

**Esercizio 5 — Utenti e permessi**

1. Crea un utente `lettore` che possa collegarsi solo da `localhost`.
2. Concedigli solo il permesso SELECT sul database `biblioteca`.
3. Crea un secondo utente `bibliotecario` con i permessi SELECT, INSERT, UPDATE, DELETE sullo stesso database.
4. Usa SHOW GRANTS per verificare i permessi di entrambi gli utenti.
5. Spiega, in una frase, perché non è una buona idea far usare a entrambi l'utente `root`.

**Esercizio 6 — Viste**

1. Crea una vista `prestiti_attivi` che mostri titolo del libro, autore e data di prestito solo per i prestiti non ancora restituiti.
2. Interroga la vista con una normale SELECT, come se fosse una tabella.

**Esercizio 7 — MySQL da Python**

1. Installa `mysql-connector-python` con pip.
2. Scrivi un programma Python che si collega al database `biblioteca` creato nell'Esercizio 1, leggendo host, utente, password dalle variabili d'ambiente, e stampa l'elenco di tutti i libri usando un cursore con `dictionary=True`.
3. Aggiungi una funzione che, dato un id di libro, inserisce un nuovo prestito con la data odierna (usa una query parametrizzata con `%s`, non concatenare le stringhe).
4. Racchiudi le operazioni di scrittura in un blocco try/except/finally che esegue un rollback e stampa un messaggio di errore leggibile in caso di problemi.

**Esercizio 8 — Riconoscere l'SQL injection**

Osserva questo frammento di codice:

{% highlight python %}
titolo = input("Titolo da cercare: ")
query = f"SELECT * FROM libri WHERE titolo = '{titolo}'"
cursor.execute(query)
{% endhighlight %}

1. Spiega perché questo codice è vulnerabile a SQL injection.
2. Scrivi un valore che, inserito dall'utente, farebbe restituire alla query tutti i libri della tabella indipendentemente dal titolo cercato.
3. Riscrivi il codice in modo sicuro, usando una query parametrizzata.

**Esercizio 9 — Funzioni di finestra e CTE**

Usa la tabella `voti` creata negli esercizi precedenti (o quella dell'esempio con `alunni` e `materia`).

1. Scrivi una query con RANK() OVER (PARTITION BY materia ORDER BY voto DESC) che mostri, per ogni voto, la posizione dello studente in classifica in quella materia.
2. Scrivi una CTE (WITH) che calcoli la media dei voti per materia, e usala in una SELECT che mostri solo le materie con media inferiore a 6.
3. Scrivi una query con GROUP_CONCAT che mostri, per ogni alunno, l'elenco delle materie in cui ha almeno un voto, separate da virgola.

**Esercizio 10 — Dati semi-strutturati con JSON**

1. Crea una tabella `eventi (id, nome, dettagli JSON)`.
2. Inserisci 3 eventi con dettagli diversi tra loro (ad esempio uno con `data` e `luogo`, un altro con `data`, `luogo` e `costo`).
3. Scrivi una query che estrae solo il campo `luogo` di ogni evento usando l'operatore `->>`.
4. Spiega, in due o tre frasi, perché non converrebbe usare JSON per memorizzare nome e cognome degli iscritti a ciascun evento, e come lo struttureresti invece usando le tecniche viste in [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html).
