---
layout: post
title: "Epidemics - to act early or to act late?"
date: 2017-07-15
published: false
---

Bolzoni et al (Time-optimal control strategies in SIR epidemic models)'s pre-print raised an interesting question about the optimal control of epidemics, and the varying recommendations that one may make depending on the goals one sets.

In this paper, the authors derive an optimal control-based framework for policies to minimize the time to eradication of a disease. They define eradication to be the time by which the collection of infected agents in the population falls below an $\epsilon$ percentage of the population.

Using interesting, but standard analysis, they derive a result that goes against most other similar models: the optimal epidemic control policies advise acting *late*, rather than early (caveat emptor: my work has broadly shown the latter).

This, however, is not unexpected: in a generic single-population SIR epidemic model, the rate of change of infective individuals is a function of their population. Infectives create other infectives through contact, or they are healed/quarantined at rates related to their population. Thus, the generic differential equation showing infective dynamics can be written as:

$$\dot{I} = M I$$

in which $M$ is (implicitly) some bounded function of time. Furthermore, $M$ is an increasing function of $S$, as the more susceptibles there are, the more chance the infection has of propagating.

Now, in an SIR model, the Recovered state is terminal: once infectives become recovered, they do not transition back to either of the other states. Thus, as time goes by and more and more individuals recover, the pool of individuals for the other two states diminishes. Thus, for cases where the system is moving towards the disease free equilibrium (i.e., $\dot{I}<0$), one can expect that for large $t$, $M$ will also be a diminishing value.


-----------
"Time-optimal control strategies in SIR epidemic models", Luca Bolzoni, Elena Bonacini, Cinzia Soresina, and Maria Groppi, pre-print: https://arxiv.org/abs/1706.04447
