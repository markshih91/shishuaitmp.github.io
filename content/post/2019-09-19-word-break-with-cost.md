
---
title: "Word break with cost"
subtitle: ""
summary: ""
authors: [Shuai Shi]
tags: [Graph Theory, Algorithm]
categories: []
date: 2020-01-21T18:51:40+08:00
lastmod: 2020-01-21T18:51:40+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

This problem is a generalization of the [word break](https://leetcode.com/problems/word-break/) problem on leetcode. Many algorithms you see online assumes that string in $W$ has constant length, checking the hash table takes $O(1)$ time, and obtain an $O(n^2)$ time algorithm. It is not as easy. Here we show an algorithm that considers the strings in $W$ have arbitrary length.

Consider the following graph $G=(V,E)$, where $V=\set{0,\ldots,n}$, and there is an edge from $i$ and $j$, if $s[i+1..j]=w\in W$, and the label of the edge $(i,j)$ is the string $w$, and the cost is $c(w)$. 
Let $z$ be number of substrings in $s$ matches some element in $W$. The graph has $z$ edges. Note $z=O(n\sqrt{L})$. Indeed, the sum of the length of the labels of all outgoing edges cannot be more than $L$, and the length of each label is different. Hence each vertex can have at most $O(\sqrt{L})$ outgoing edges. The graph is a DAG, so we can find the shortest path from $0$ to $n$ in linear time with respect to the number of edges. 
This shows if we can compute the graph in $O(z+L)$ time, then we solve the problem in $O(z+L)$ time.

We can build the Aho–Corasick automaton for $W$ in $O(L)$ time. It can be used to find all substrings of $s$ that matches something in $W$ by traversing the automaton once. The running time is the total number of substrings matched, which is $O(z)$. Hence building the graph takes $O(z+L)$ time.
$z$ is clearly no more than $nm$, where $m=|W|$. Also, it is also clear $z=O(n\sqrt{L})$. Indeed, there can be at most $O(\sqrt{L})$ edges start from $i$, since each edge has a label of different length, and sum of those length labels is no larger than $L$. 

If we only want to know if there exists a solution, then there is a $\tilde{O}(nL^{1/3}+L)$ time algorithm [@BringmannGL17]. The algorithm is close to optimal assuming the algorithm is combinatorial and the alphabet can be arbitrarily large. 

Can we obtain similar running time for the word break with cost problem? There are evidence against it. If the alphabet is unary, this problem is equivalent to the unbounded knapsack problem, which likely does not have an algorithm with running time $O((n+L)^{2-\e})$ for any $\e>0$ [@CyganMWW19] and $m$ can be as large as $\Omega(\sqrt{L})$. Of course, this does not mean there might not be a $O(nL^{1/3}+L)$ time algorithm, since the reduction involved in the paper might not hold when we require $m=\Omega(\sqrt{L})$. 
