---
id: 429
title: 'Interfacce grafiche'
date: '2020-02-19T21:33:05+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/02/19/131-revision-v1/'
permalink: '/?p=429'
---

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">from tkinter import *
 
window = Tk()
window.title("La mia prima applicazione")
window.geometry('350x200')
window.configure(background="light green") 
 
lbl = Label(window, text="Ciao!!!")
lbl.grid(column=0, row=0)
 
txt = Entry(window,width=10)
txt.grid(column=1, row=0)
 
def cliccato():
    lbl.configure(text=txt.get())
 
btn = Button(window, text="Cliccami", command=cliccato)
btn.grid(column=2, row=0)
 
window.mainloop()
```

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/finestra.png)</figure>Questo programma genera una finestra avente le dimensioni di 350px per 200px con un titolo (La mia prima applicazione) e un colore di sfondo verde.

Sulla finestra sono posizionati tre **widget**: una **Label**, una **Entry box** e un **Button**.

Se si clicca il bottone, il contenuto della Entry box finisce nella label.

E’ possibile cambiare i colori del bottone o della label con le proprietà: fg, bg. L’acronimo bg sta per background (sfondo) e fg sta per foreground (in primo piano). Ad esempio se voglio rendere lo sfondo del bottone nero e il testo rosso, posso scrivere:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">btn = Button(window, text="Cliccami", fg='black', bg='red', command=cliccato)

```

</div>#### Il sistema a griglia

Per sapere come posizionare gli elementi sulla finestra di fa uso si un sistema a **griglia**. In pratica si posizionano i **widget** in una griglia bidimensionale divisa in righe (row) e colonne (column). All’interno di una cella è possibile inserire un widget. Per posizionare un widget nella griglia si fa uso della sintassi:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">nomewidget.grid(column=0, row=0)

```

</div>#### Il Frame

Il frame è un pannello che può contenere altri widget, si usa per creare porzioni di interfacce in una finestra.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">frame = Frame(window) # inizializzazione
frame['padx'] = 5  #distanza oriz. del contenuto dai bordi
frame['pady'] = 10 #distanza vert. del contenuto dai bordi
frame['borderwidth'] = 2  # spessore del bordo
frame['relief'] = 'sunken' # stile bordo ("flat" "raised", "sunken", "solid", "ridge", or "groove".)
```

</div>#### Image:

Si può utilizzare una label per mostrare una immagine, questo può essere utile all’interno di una interfaccia grafica:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">image = PhotoImage(file='myimage.gif') 
label['image'] = image 
```

</div>Possiamo fare la stessa cosa per mostrare una immagine all’interno di un bottone.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">image = PhotoImage(file='myimage.gif') 
button['image'] = image 
```

</div>#### Checkbutton:

Il checkbutton è una casella di “spunta” che permette all’utente di selezionare o attivare una opzione oppure di disattivarla. Il checkbutton ha le funzionalità di un bottone, dunque dispone della proprietà command cui viene associata una funzione (azione) da attivare quando viene cliccato. Oltre a questo viene associato ad una variabile di cui imposta il valore quando viene attivato o disattivato cliccandoci con il puntatore del mouse.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">measureSystem = StringVar()
check = Checkbutton(window, text='Use Metric', command=metricChanged, variable=measureSystem, onvalue='metric', offvalue='imperial') 
```

</div>#### Bottone radio:

Il bottone radio permette all’utente di scegliere all’inteno di un insieme di opzioni mutualmente esclusive. E visualizzato come un pallino che può essere vuoto oppure pieno. Prima di essere definito bisogna inizlalizzare la variabile che andrà a contenere il valore selezionato dall’utente.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">phone = StringVar()
home = Radiobutton(window, text='Home', variable=phone, value='home') 
office = Radiobutton(window, text='Office', variable=phone, value='office')
cell = Radiobutton(window, text='Mobile', variable=phone, value='cell') 
```

</div>