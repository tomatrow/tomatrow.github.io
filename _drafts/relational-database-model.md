---
title: Relational Database Model
layout: post
---

I found the explanation of the [Relational Model via Set Theory](https://en.wikipedia.org/wiki/Relational_model#Set-theoretic_formulation) enlightening, with the following adjustments. 

Let  $V$ := "atomic variables" and $N$ := "attribute names".
Def. Tuple: A partial function ![t:N \to V][01]
Def. Header: $H \subseteq A$ such that $|H| < \infty$ then is a header. 
Def. Projection: Let $A \subseteq N$ with $|A| < \infty$ and let $t$ be a Tuple. Then the projection is $t[A] = \{(a,v) \mid t(a) = v, a \in A \} = graph(t|_sub{})$. 
Def. Relation: A relation is $(H,B)$ where $H$ is a header and $B \subseteq \{t \st t:H \to V \text{ is a Tuple}\}$. 

Keys are identifiers for items. 
Superkeys are keys that are completely determined by some subset of the header. 
Candidate keys are like indivisible superkeys. 
Functional Dependencies are where one set of attributes, $Y$, are completely determined by another set of attributes, $X$. This can be written $X \to Y$. 



