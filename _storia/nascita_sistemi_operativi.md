---
title: 'Nascita dei sistema operativo UNIX'
date: '2026-03-27'
author: Fabio Mattei
layout: page
---


![IBM System 360](/images/storia/nascitaunix/ibm-system-360.webp){:class="aside-image"}

Molti pensano che il sistema operativo UNIX sia il frutto di enormi investimenti e di uno sforzo di decine di ingegneri
sotto il controllo di un attento design architettonico. Pensano che sia il frutto di illuminati visionari
che sapevano esattamente cosa stavano costruendo.

Nulla è più lontano dalla realtà!

Nel 1966 **IBM** commercializzò il **System 360**. Questa macchina ebbe un enorme successo. 
Era dotato di un sistema operativo **monolitico** per cui ogni componente era 
strettamente integrato con ogni altro componente.

L'IBM System 360 era un incubo da programmare. Il software veniva scritto in 
linguaggio macchina ed era strettamente legato alla macchina. Un programma
scritto per l'IBM System 360 poteva girare soltanto sull'IBM System 360!
Se si voleva far girare quel software su di un sistema diverso, bisognava
riscrivere tutto da capo.

Il sistema operativo e l'hardware era una sola cosa, erano inseparabili!

Durante gli anni '60, il MIT, la General Electrics e la BELL laboratoires misero a lavorare
un team di informatici al fine di creare il MULTIX.
Era un progetto molto ambizioso: un sistema operativo multi utente multi processo. 
Sarebbe stato fatto girare su di un Mainframe e avrebbe servito allo stesso momento molti utenti
attraverso un sistema di **time sharing**. Lo sforzo fu enorme ma non portò a nulla.
La complessità era troppo elevata. Nel 1969 il progetto fu dichiarato morto.

![DEC PDP 7](/images/storia/nascitaunix/DEC-PDP-7.jpg)

![Space Travel](/images/storia/nascitaunix/space-travel.jpg){:class="aside-image"}

**Ken Thompson** era una delle brillanti menti che lavorava al progetto. Tutto di un tratto
si trovò senza progetto. Aveva a sua disposizione un vecchio DEC PDP 7, impolverato
in un angolo. La macchina era in attesa di essere portata via ma era ancora in
quell'ufficio.

La macchina disponeva di **8KByte** di memoria. Al giorno d'oggi una emoji occupa molta più memoria.

Thompson voleva scrivere un piccolo video-gioco: Space Travel. Il gioco avrebbe dovuto simulare il viaggio nel 
sistema solare di una piccola astronave. Fu quel gioco che scatenò una serie di eventi che portò alla creazione
del sistema operativo UNIX.

Per conservare il gioco Thomposon aveva bisogno di un **File System**, per avere un File system aveva
bisogno di un **Kernel**, per interagire con il kernel aveva bisogno di una **shell di comandi**.

Un componente aveva bisogno dell'altro. In circa tre settimane scrisse uno scheletro di sistema operativo.
Era estate, la moglie e i figli erano in vacanza, in laboratorio c'era soltanto lui. Più tardi
descrisse questo momento di creazione in questo modo: una settimana per il kernel, una per il file system
e una per la shell.

Poco dopo si unì a questo progetto **Dennis Ritchie**.

Il PDP 7 aveva soltanto 8 KByte di memoria e questo vincolo stringente si dimostrò
l'elemento vincente del progetto. Questo vincolo era così stringente che obbligò 
i Thompson e Ritchie ad **andare all'essenziale**. 

Thompson più tardi dichiarò: "The constraint was clarifying when you cannot afford waste you stop producing it"

Quel vincolo così stringente fece in modo che UNIX fosse abbastanza piccolo da essere 
completamente scritto da due persone nell'arco di un tempo molto piccolo. 


![Ken Thompson e Dennis Ritchie](/images/storia/nascitaunix/Ken_Thompson_and_Dennis_Ritchie_1973.jpg){:class="aside-image"}

## Un sistema universale

All'epoca ogni software era strettamente legato all'hardware su cui era stato scritto.
Lo UNIX era stato scritto in ASSEMBLY per il PDP 7. Lo UNIX poteva funziona soltanto su quella macchina.

In quel periodo Dennis Ritchie stava lavorando ad un altro progetto: **il linguaggio C**.

Thompson aveva in precedenza scritto il linguaggio B il cui nome era dovuto al BCPL da cui aveva preso ispirazione. 
Il BCPL non era il primo linguaggio ad alto livello ad essere scritto. Ritchie migliorò il linguaggio B e creò
il lingaggio C.

Il linguaggio C permetteva di avere pieno controllo sulla memoria del computer inoltre poteva essere compilato
su diverse architetture più facilmente del linguaggio Assembly. 

**Nel 1973 Thompson e Ritchie scrissero la versione 4 di UNIX in linguaggio C.** 
In questo modo lo UNIX poteva essere ricompilato su diverse macchine e diverse architetture e 
divenne il primo sistema operativo a non essere
legato ad una singola macchina, divenne il primo sistema operativo **portable**. 
Lo stesso valeva per il software che veniva scritto per
UNIX, anche questo poteva essere portato da una architettura all'altra.

**Avevano creato un sistema universale.**

Si diffuse dal PDP 7 al VAX e in molte altre architetture. 

Le idee alla base di UNIX rappresentano i pilastri su cui sono scritti tutti
i sistemi operativi moderni. I sistemi che vengono usati nei computer, 
nei telefonini, nei tablet, nei robot che puliscono a terra, negli orologi smart, 
nelle automobili... in ogni dispositivo dotato di memoria e processore.



