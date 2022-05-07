---
id: 720
title: Ordinamento
date: '2021-09-30T10:58:13+02:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=720'
---

L’algoritmo Bubble Sort

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def bubbleSort(arr):
    n = len(arr)
    # Si scandisce la lista elemento per elemento
    for i in range(n):
        # Gli ultimi i elementi sono gia' in rodne
        for j in range(0, n-i-1):
            # Se gli elementi da 0 ad i-1 non sono i ordine si invertono
            if arr[j] > arr[j+1] :
                arr[j], arr[j+1] = arr[j+1], arr[j]
 
# Esempio di chiamata
arr = [63, 38, 22, 12, 24, 12, 99]
 
bubbleSort(arr)
 
print ("Lista ordinata:")
for i in range(len(arr)):
    print ("%d" %arr[i]),
```

</div>