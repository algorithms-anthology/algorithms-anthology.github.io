---
layout: post
title: "A Cool Divide and Conquer Problem"
--- 

I found this at CSPrep from Tao Genna. At first glance, this looks like a typical constructive algorithm problem.: we have to provide the construction of a set from some information given about it. Here is the abridged problem statement.

You have a non-decreasing array $a_1, a_2, a_3, \dots, a_n$. You have created a _multiset_ of length $2^n$, which contains the sums of the elements in each possible subsequence of the array. You have lost the array (of course), but you know the multiset. Find all possible arrays which could have produced this multiset. Note that the elements of the multiset are listed in no particular order.

The constraints are $n \leq 18$, which means size of the multiset can be as long as $2^{18} \approx 2 \times 10^5$. If you want to try out your ideas, here's the problem [link](https://www.urionlinejudge.com.br/judge/es/problems/view/2913)

This problem is a nice combination of greedy, divide and conquer and recursive algorithm. As usual, let's solve a easier problem first. 

Consider this problem: Suppose you know a particular value $m$ is in the array. You want to partition the multiset in two equal halves such that one half consists of all sums which contain $m$, and other half consists of all sums which do **NOT** contain $m$. Let's call those two paritions $I$ and $J$ respectively, and let's call our multiset $S$. In other words, $S = I \cup J$. We can approach this greedily:

* If $m$ is positive, then notice that minimum value in the multiset does not include $m$. This implies, $\text{(minimum value + m)}$ must be in the multiset. We can insert the minimum value at $J$ and $\text{(minimum value + m)}$  at $I$, and remove both of them from $S$.
* If $m$ is negative, then notice that minimum value in the multiset must include $m$. This implies, $\text{(minimum value - m)}$ must be in the multiset. We can insert the minimum value at $I$ and $\text{(minimum value - m)}$  at $J$, and remove both of them from $S$.
* Continue the previous steps until $S$ becomes empty. 

If $S$ is sorted beforehand, this is solvable in linear (pseudo-linear is okay too) time.

Okay, so we've solved the easier problem. But the problem is how do we find a value which must be in the array. Here's comes extremal principle. For those who don't know what it is, extremal principle is working with extreme values in a problem (in our case, maximum or minimum values). Notice that, we've already applied this principle in our previous greedy algorithm as well. Working with minimum sums made things a lot easier. So our crucial observation is this:

At least one of second **(second minimum value - minimum value)** or **(minimum value - second minimum value)** will always be in the array. 

Why is this? Well, easy. Notice how the minimum value and the second minimum value of $S$ are formed. Minimum value must be the sum of all negative numbers from the array (if there is none, this is simply $0$). The second minimum will be either (minimum value + lowest positive number) or (minimum value - highest negative number). So, substracting the minimum value from the second minimum value or vice versa will give us either the lowest positive number or the highest negative number.

But which one to pick? Here we can employ divide and conquer. We will run the above algorithm on both options. From this, we will get two different partitions of half the size of our current multiset. Each partition is a multiset whose sums do not contain the number we have predicted. So we can solve our problem recursively on both partitions. If currently the size of the multiset is $N$, we will have two partitions of size $\frac{N}{2}$. If the complexity of our recursive algorithm is $T(N)$ for a multiset of size $N$, we can write 

$$T(N) = 2T\left (\frac{N}{2} \right ) + O(N)$$

Solving this yields $T(N) = O(N \log{N})$, where $N = 2^{n}$.


