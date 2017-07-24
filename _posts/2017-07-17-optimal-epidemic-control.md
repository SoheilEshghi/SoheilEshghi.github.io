---
layout: post
title: "Epidemics - to act early or to act late?"
date: 2017-07-17
---
As many of you may know, my PhD was on the optimal control of epidemics, so I will start close to home.

Among other things, I (along with my colleagues at Penn) broadly generalized earlier work to show that even with more fine-grained information about social network structure and resource availability, optimal epidemic immunization/treatment policies share a general structure: any action is taken as soon as possible, and no resources are kept in reserve.

This month, I saw a paper titled "[Time-optimal control strategies in SIR epidemic models](https://arxiv.org/abs/1706.04447
)" by Luca Bolzoni, Elena Bonacini, Cinzia Soresina, and Maria Groppi, on an e-mail from arXiv. Naturally, I was intrigued. Then I read the abstract:

"[The] optimal [epidemic eradication] strategy is to delay the control action some amount of time and then apply the control at the maximum rate for the remainder of the outbreak. This result is in contrast with previous findings on the unconstrained problems of minimizing the total infectious burden over an outbreak, where the optimal strategy is to use the maximal [cost-less] control for the entire epidemic."

They had my full attention.

### A little background

[Compartmental models in epidemiology](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology) broadly classify individuals according to their state with respect to the epidemic. In the SIR model, individuals are either susceptible (S) to the epidemic, already infected (I), or have recovered (R), meaning they cannot become infected again. The process of recovery can happen through immunization of susceptible individuals as well as treatment and recovery of the infected.

Given some assumptions on how individuals move around and the size of the population, the fraction of individuals in each of these states has been shown (by Thomas Kurtz, 1970 onwards and others) to converge (speaking loosely, pathwise a.s.) to the solution of a set of deterministic ordinary differential equations (ODEs). Working with the ODEs to devise optimal control policies in lieu of the underlying stochastic system has been standard. Another recent arXiv paper ("[Optimal control of Markov jump processes: asymptotic analysis, algorithms and applications to the modelling of chemical reaction systems](https://arxiv.org/abs/1512.00216)", Wei Zhang, Carsten Hartmann, Max von Kleist) seems to prove that this working assumption has been more-or-less correct, but I digress!

### The setting

In the paper, Bolzoni *et al.* derive an optimal control-based framework for policies to minimize the time to eradication of a disease. They define eradication to be the time by which the collection of infected individuals in the population falls below an $$\epsilon$$ percentage of the population.

Using interesting, but somewhat standard analysis, they derive a result that goes against most other similar models: the optimal epidemic control policies advise acting *late*, rather than early.

### Goals goals goals

Typically, the goal of epidemic control policies is to limit the burden of disease at minimum cost of action. In the homogenous population case, we can formulate a cumulative cost to be minimized by a policy:

$$\text{Burden}=\int_0^T \big(f(I(t)) + g(u(t)) \big)\,dt,$$

where $$u$$ is the relevant control action. For a more general setting with multiple sub-populations, see [our paper in the Transactions on Networking](http://ieeexplore.ieee.org/abstract/document/6966800/?reload=true). For a broader overview, see this excellent [article in Control Systems Magazine](http://www.georgejpappas.org/papers/07393962.pdf) by some of my former colleagues at Penn.

On the other hand, the goal of the policies in the paper under consideration is

$$\text{Time to eradication (ToE)}=\int_0^{T_\epsilon} 1\,dt,$$

where $$T_\epsilon$$ is the first time the fraction of infectives falls below $$\epsilon$$.

This seems like a perfectly reasonable goal for an epidemic control policy, so why are the resulting recommendations so starkly in contrast with disease burden-based cases?

### Epidemics fast and slow

I will now peak under the hood of the dynamical system to give a glimpse of why this incongruity is not unexpected. The arguments lean towards explanation rather than rigor, but you can easily interpolate the reasoning!

In a generic single-population SIR epidemic model, infectives create other infectives through contact, or they are healed/quarantined at rates related to their population. Thus, the generic differential equation showing infective dynamics can be written as $$\dot{I} = M(S(t)) I$$ in which $$M(\cdot)$$ is (implicitly) some bounded function of time. So one can use the approximation $$I(t+\delta)\approx I(t) e^{-M(S(t)) (t+\delta)}$$ locally.

Now we draw upon two other facts:
1. $$M(S)$$ is an increasing function of $$S$$, as the more susceptibles there are, the more chance the infection has of propagating.

2. The Recovered state is terminal in SIR models: once infectives recover, individuals do not transition back to either of the other states. Thus, as time goes by and more and more individuals recover, the pool of individuals for the other two states diminishes.

So when the infective population is vanishingly small (e.g., around $$T_\epsilon$$), $$S(t)\ll S(0)$$ (we can also argue this directly) and so the rate of change of the infectives will be much slower than at the initial time by fact 1.

Using the above argument, we can broadly see that there are at least two regimes in such epidemic models and their generalizations: the explosive fast growth of infectives when susceptibles are abundant, and an anemic damped move towards terminal states/equilibria later on.

Given a uniformly bounded additive control action that affects $$M(\cdot)$$, applying the control to the early explosive growth phase (with its large $$M(\cdot)$$ terms) may have a tiny effect on the outcome, while applying the same action later on (given the smaller $$M(\cdot)$$ terms) would be much more effective, increasing the damping speed significantly.

Given the above, the results of the paper, which advocates for later rather than earlier action, are not as unexpected as they would have appeared.

However, there is a twist in the tail: these optimal policies, as stated, may suggest inordinately long periods of action affecting a tiny population (e.g., in the isolation policy) for small gains in the time to eradication of the disease. On the other hand, they would ignore doing anything about the disease at its peak, potentially leading to a huge disease burden.

### My take-away
So what should a policy-maker do, given **two perfectly correct mathematical models and analyses** that make **opposing recommendations**?

In my opinion, a policy-maker can do much worse than simply evaluating their own policy goals against the mathematical objective of the models whose recommendations they are considering. Curbing the tail-end of an epidemic that has ravaged a population with no regard to the cost of an intervention seems to me to be much less practically useful than considering minimizing the disease burden.

So while I admire the interesting mathematical result in the paper and the insight they provide into the dynamical system, I would **urge caution in actually implementing its policy suggestions**.

One interesting extension of the approach, however, might be to combine its goal with that of more conventional approaches. I suspect that the results that would be obtained from such a fusion would be much closer to the classical view which advocates early action.

### Let me know what you think - about the format and the content!

P.S. As I am new to academic blogging, it might take some time for me to get the tone right. My apologies in advance.

-----------
Bibliography:

"[Time-optimal control strategies in SIR epidemic models](https://arxiv.org/abs/1706.04447)", Luca Bolzoni, Elena Bonacini, Cinzia Soresina, and Maria Groppi

"[Optimal control of Markov jump processes: asymptotic analysis, algorithms and applications to the modelling of chemical reaction systems](https://arxiv.org/abs/1512.00216)", Wei Zhang, Carsten Hartmann, Max von Kleist

"[Optimal patching in clustered malware epidemics](http://ieeexplore.ieee.org/abstract/document/6966800/?reload=true)", Soheil Eshghi, M.H.R. (Arman) Khouzani, Saswati Sarkar, Santosh S. Venkatesh

"[Analysis and control of epidemics](http://www.georgejpappas.org/papers/07393962.pdf)", Cameron Nowzari, Victor M. Preciado, George J. Pappas

-----------
