---
title: 'Dimensione esatta del gruppo'
date: '2023-04-18'
author: Fabio Mattei
layout: page
---

## Il problema

Bessie and her friends are excited to visit the Capitol Building, which houses the US Congress, the legislative arm of the US. The guided tour they are planning to take walks through the two chambers of Congress and promises to be a very educational experience.

There are two lines for groups waiting to have a guided tour. Each line has N groups, and the group sizes are a<sub>1</sub>,a<sub>2</sub>,…,a<sub>N</sub> and b<sub>1</sub>,b<sub>2</sub>,…,b<sub>N</sub>, respectively.

The guided tour is planned for a group of exactly K members.

Please help the tour organizers find two groups, one from each line, such that the total number of members is K.

#### Input Format

Three lines.

The first line contains two integers: N, K.

The next two lines contain N integers each, denoting the sizes of the different groups:
a<sub>1</sub>,a<sub>2</sub>,…,a<sub>N</sub> and b<sub>1</sub>,b<sub>2</sub>,…,b<sub>N</sub>.

It is given that a1<a2<a3< … <aN<104 and b1<b2<b3< … <bN<104, and that N and K are positive and smaller than 106.

#### Output Format

One line with two numbers, the sizes of the two selected groups. 

You can print these in any order.

#### Sample Input

4 20

4 7 12 15

6 8 14 17

#### Sample Output

12 8

Groups with 12 and 8 members are from different lines, and they add up to 20.

SCORING:
* In test cases 2-5, N is less than 1000.
* In test cases 6-10, there are no additional constraints.

#### Soluzione 

{% highlight python %}
for i in range(N):
	for j in range(N):
		somma = line1[i] + line2[j];
		if somma == K:
			print(line1[i], line2[j])
{% endhighlight %}

Questa soluzione è molto onerosa, in effetti calcola tutte le possibili soluzioni per determinare quella giusta. Per N = 4 fa 16 moltiplicazioni ma se N fosse un numero più grande quante moltiplicazioni farebbe?

#### Soluzione più leggera

{% highlight python %}
i = 0
j = N-1
somma = 0
while i < N and j >= 0 and somma != K:
	somma = line1[i] + line2[j];
	# Abbiamo tre possibilità
	if somma == K: # la somma è esatta
	    print(line1[i], line2[j])
	if somma > K: 
	    j = j - 1 # la somma è troppo grande
	if somma < K: 
	    i = i + 1 # la somma è troppo piccola
{% endhighlight %}

In questo modo riduciamo drasticamente la quantità di calcoli da fare


## Esercizio

You are given an array of n integers, and your task is to find two values (at distinct positions) whose sum is x.

#### Input

The first input line has two integers n and x: the array size and the target sum.

The second line has n integers a<sub>1</sub>,a<sub>2</sub>,…,a<sub>N</sub>: the array values.

#### Output

Print two integers: the positions of the values. If there are several solutions, you may print any of them. If there are no solutions, print IMPOSSIBLE.

#### Constraints: 

* 1≤n≤2 x 10 <sup>5</sup>
* 1≤x,a<sub>i</sub>≤10<sup>9</sup>

#### Example

Input:

4 8

2 7 5 1

Output:

2 4
