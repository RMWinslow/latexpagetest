# Social Polarization in Response to Contagions

Abstract

Tests of Latex rendering:

$$\sum_i x^2$$

And Here's an inline example $$\sum_i x^2$$.


## Literature Background and Motivation

Michael Kremer's 1996 paper "Integrating behavioral choice into epidemiological models of AIDS"[^Kremer96]
models and compares steady states of endemic disease. In this model, people have preferences over the rate of sexual partners they have, and are also concerned about the chance they become infected.
Kremer shows that behavioral response can have either positive or negative feedback effects that augment or counteract the effectiveness of policy intervention. Furthermore, very high-prevalence sub-populations can exhibit a form a fatalism, where increased prevalence can sometimes increase desired sexual activity among high-activity groups.

Kremer's model studies the dynamics of HIV, a contagion which permanently infects a person, and which spreads across long enough time scales that the birth and death of new people play an important role in the dynamics of disease spread. 
For example, the question of whether people become fatalistic in this model ultimately comes down to the question of whether they are more likely to contract the virus or die from some other cause before that happens.

In contrast, respiratory pathogens like SARS-CoV or SARS-CoV-2 spread through populations very quickly. In standard compartmental models of these diseases, the stable equilibrium happens only at the end of the contagion's outbreak, after the population of susceptible people has fallen low enough for the contagion to die out. As such, the equilibrium model in Kremer 1996 cannot be directly applied to this kind of disease.

Another perspective on the spread of disease can be found in percolation theory. In "Spread of epidemic disease on networks" (2002)[^Newman02], Mark Newman conceptualizes a disease outbreak as a connected component in an infinite random social graph. 
Each vertex in this graph is a person, and their edges represent social connections along which a contagion can potentially spread. 
Give the distribution of degrees of this social graph, along with a measure of how transmissive the contagion is, Newman is able to derive exact solutions for the size of an outbreak, the chance that an outbreak turns into a full-blown epidemic, and the portion of the population that ultimately becomes infected at some point during the epidemic.

Even though this approach views the spread of disease from a timeless static perspective, a later paper (Meyers, Ancel, Pourbohloul, Newman, Skowronski, Brunham. 2005.) [^Newman05] compares the solutions of this percolation-based model to simulations of the spread of SARS. Even in complex social graphs, the static solutions made more accurate predictions about the spread of this simulated disease than a basic SIR model, which can be thought of as implicitly assumes that the social graph is fully mixed, such that everyone has the same chance of spreading the disease to everyone else.

A benefit of this percolation-approach is that it highlights the role of heterogeneity among people who spread the disease. In this model, it's not just the basic reproductive number $$R_0$$ (that is, the average number of new infections that a sick person at the beginning of an outbreak will cause) that determines the epidemiological outcomes of a disease outbreak, but also the variation between individuals in how many additional infections they cause.


In "Superspreading and the effect of individual variation on disease emergence"[^Schreiber], (Lloyd-Smith, Schreiber, Kopp, Getz. 2005.),  the authors demonstrate how the high variance and skewed distribution of the individual reproductive number has important implications in prediction and control of an epidemic disease. 
Higher variation makes disease outbreaks more likely to quickly die out, but also means that when they do spread, they spread a lot. 
The authors show that the spread of SARS-CoV was highly dispersed, meaning that a small portion of infected individuals are responsible for a large portion of new infections.
And research into the recent outbreak of SARS-CoV-2 suggests that this new contagion similar exhibits high dispersion in disease spread[^Endo20].







What follows is an adaptation of the behavioral equilibrium framework from Kremer's 1996 paper to the percolation-based model of disease spread.




## Model Setup

### The individual's problem.

Let $$\Psi$$ be the a priori chance that the contagion is transmitted to a person from a specific neighbor with whom they have social contact. $$\Psi$$ is the chance that a transmission event to this person ever occurs along this connection during the entire epidemic.
The person doesn't know the details of their neighbor's social activity, and so the parameter $$\Psi$$ is the same for all neighbors. 

If a person has precisely $$k\in\N$$ neighbors, then the chance that the person *doesn't* ever become infected is $$[1-\Psi]^k$$, and the chance that the person does become infected is $$1-[1-\Psi]^k$$.

It is a common to model an individual's spread of the disease to others as a Poisson process[^Schreiber].
To be consistent with this assumption, and to make the individual's problem continuous, each individual of type $$i$$ chooses a social activity level $$N_i\in\R_+$$, which is the mean of the Poisson distribution that determines the actual integer number of social connections the person has.

Given that a person has mean social connections $$N_i\in\R_+$$, their risk of becoming infected $$p(N_i)$$ is thus 

$$
p(N_i) \equiv \sum_{k=0}^{\infty}\left[\frac{N_{i}^{k}e^{-N_{i}}}{k!}\left(1-[1-\Psi]^{k}\right)\right]=1-e^{-\Psi N_{i}}
$$

Similarly to the individual's problem in Kremer 1996[^Kremer96], an individual of type $$i$$ gains some utility from social activity $$u_i(N_i)$$, and disutility equal to the chance that they become sick.
A person of type $$i$$ solves the problem:

$$\max_{N_i\geq0} \left[u_i(N_i)-p(N_i)\right]$$


TODO: Conditions required of U

TODO: Description of the marginal risk and fatalism.






### Spread of Contagion.

Individuals are nodes in an infinite random graph.
The relative population of each type $i$ is $$A_i$$, such that $$\sum_i A_i = 1$$.
And the degree distribution within type $$i$$ is Poisson with mean $$N_i$$.

Following Newman's[^Newman02] example, the spread of disease is modelled as a branching process on the network of social connections between people. 

The probability generating function (PGF) for the degree distribution on individuals is $$G_0(x)\equiv \sum_{k=0}^\infty p_k x^k$$, where $$p_k$$ is the probability that a person has exactly $$k$$ neighbors.
So 

$$G_0(x) = \sum_{k=0}^\infty \left[ \sum_i \left[ A_i \frac{N_{i}^{k}e^{-N_{i}}}{k!}\right] x^k \right]
= \sum_i \left[A_ie^{(x-1)N_i}\right]$$

*Excess Degree* is the number of additional connections a person has besides the one along which the contagion spread *to them*. 
The PGF for the excess degree of a person at the end of a random edge is 

$$G_1(x) \equiv \sum_{k=0}^\infty \frac{(k+1)p_{k+1}x^k}{\sum_k p_k} = \frac{G_0'(x)}{G_0'(1)}
=\frac{\sum_i \left[A_i N_i e^{(x-1)N_i}\right]}{\sum_i  A_i N_i }$$

The parameter $$T$$ is the a priori chance that the contagion can transmit along a given connection if one of the two people on that edge gets sick. $$1-T$$ can be thought of as the iid chance that a social connection is closed off or removed from the graph of infection spread.

If a person has $$k$$ neighbors, then the probability that this person has exactly $$m$$ "open" connections is $$\binom{k}{m} T^m (1-T)^k$$. The PGF for transmissible degree is thus

$$G_0(x;T) \equiv \sum_{k=0}^\infty \left[\sum_{m=0}^k \left[\binom{k}{m} T^m (1-T)^k\right] x^k \right]
=G_0\left(1-(1-x)T\right)
= \sum_i \left[A_ie^{(x-1)TN_i}\right]$$

Likewise, the PGF the excess transmissible degree of a random neighbor is 

$$
G_1(x;T)\equiv G_1(1-(1-x)T) 
= \frac{\sum_i \left[A_i N_i e^{(x-1)TN_i}\right]}{\sum_i  A_i N_i }
$$

If a contagion is spontaneously introduced to an individual, then it will spread along all open connections starting from this patient zero. 
The ultimate size of the outbreak depends on the size of the component of the social graph which is connected to patient zero even after nodes have been randomly removed.
The size of the outbreak can be finite, or it can be infinite, such that the probability is non-zero  that any specific person is in this connected outbreak component. 
This later outcome means that the outbreak becomes an epidemic, while the former means that the contagion goes extinct before epidemic occurs.

For any given $$\left\{A_i,N_i \right\}_i$$, there is a critical transmissibility threshold $$T_c$$ such that epidemic occurs with zero probability whenever $$T \leq T_c$$:

$$T_c = \frac{1}{G_1'(1)} = \frac{\sum_i  A_i N_i }{\sum_i A_i N_i^2 }$$

And the familiar basic reproductive rate is $$R_0 = \frac{T}{T_c} = T \frac{E[N^2]}{E[N]}$$.

When $$T > T_c$$, let $$U$$ be the chance that the end of a random edge remains uninfected by an outbreak. (TODO: Mention how this Poisson setup makes this part slightly simpler.) $$U$$ is the solution $$\in(0,1)$$ to 

$$U = G_1(U;T) = \frac{\sum_i \left[A_i N_i e^{(U-1)TN_i}\right]}{\sum_i  A_i N_i }$$

The probability that an outbreak turns into an epidemic (and the portion of the population affected by that epidemic) is $$R_\infty \equiv 1-G_0(U;T) = 1- \sum_i \left[A_ie^{(U-1)N_i}\right]$$ 

So far, this follows along with the generic formulation from Newman 2002. But now to make this compatible with the individual's decision, rewrite the above two expressions in terms of $$\Psi$$. The chance that a neighbor transmits to a person is the chance that the neighbor gets sick times the chance that their connection is open. So $$\Psi=(1-U)T$$ and 


$$\Psi = T - T \frac{\sum_i \left[A_i N_i e^{-\Psi N_i}\right]}{\sum_i  A_i N_i } = T \frac{\sum_i \left[A_i N_i [1-e^{-\Psi N_i}]\right]}{\sum_i  A_i N_i }$$

$$R_\infty = 1-\sum_i \left[A_ie^{-\Psi N_i}\right] = \sum_i \left[A_i[1-e^{-\Psi N_i}]\right] $$ 







### Equilibrium


As in Kremer 1996 [^Kremer96], equilibrium will be defined by a behavioral condition and an epidemiological condition. 
The behavioral condition is that taking $$\Psi$$ as given, each individual chooses the level of social activity best for them. 
The epidemiological condition is that these decisions lead to an outbreak that generates $$\Psi$$.

To put it more explicitly:

Given exogenous $$T$$, $$\left\{A_i\right\}$$, an equilibrium in this model consists of $$\Psi, \left\{N_i\right\}$$ such that 

$$\Psi = T - T \frac{\sum_i \left[A_i N_i e^{-\Psi N_i}\right]}{\sum_i  A_i N_i }  \tag{ec}$$

$$N_i = \argmax_{N_i \geq 0} \left[ u_i(N_i) - 1+e^{-\Psi N_i}\right] \tag{bc}$$





## Facts about the model

Fatalism?

Proof of equilibrium

Phase diagram?


## example with exponential utility.







## Effects of intervention

### Decrease T
When does it actually hurt? Ever?

### Decrease cost of disease?
That will increase activity and so increase incidence. Can that decrease utility?


### Lock down people at random.










## Discussion

### Shortcomings of this approach

These percolation solutions work well in capturing the features of a static social graph, and my model supposes that each individual chooses an average level of social activity to engage in throughout the entire duration of the epidemic. 
However, in real life, people can change their behavior day to day in response to hearing news about the changing prevalence of the disease or the number of people dying. 
Models which take this into account, like Farboodi, Jarosch, Shimer 2021 [^Farboodi], predict that people engage in more activity when the risk from social activity is low, and less activity when the risk is high. 
This leads to the social risk stabilizing to some specific value, but through dynamic differences in behavior across time rather than static differences in behavior between different types of people.

To the extent that frequent behavioral changes are significant, it may be that this percolation-based perspective is more useful in understanding the initial stages of public response. Or more useful in understanding very rapidly spreading contagions than it is in thinking about persistent contagions.

Likewise, the interventions I look at are from a static perspective. My model doesn't allow interventions to dynamically change across time in reaction to changing disease statistics. This is a better fit for jurisdictions like those in the US which tended to maintain relatively constant lockdown stringency across time as opposed to European countries which had large changes in lockdown intensity [^lockdownPolitics]. 










## REFERENCES <!--using [Chicago Author-Date style](https://www.aeaweb.org/journals/policies/sample-references)-->



[^Newman05]: **Meyers, Lauren Ancel, Babak Pourbohloul, Mark EJ Newman, Danuta M. Skowronski, and Robert C. Brunham.** 2005. "Network theory and SARS: predicting outbreak diversity." *Journal of theoretical biology* 232, no. 1: 71-81.

[^Schreiber]: **Lloyd-Smith, James O., Sebastian J. Schreiber, P. Ekkehard Kopp, and Wayne M. Getz.** 2005. "Superspreading and the effect of individual variation on disease emergence." *Nature* 438, no. 7066: 355-359.

[^Newman02]: **Newman, Mark EJ.** 2002. "Spread of epidemic disease on networks." *Physical review* E 66, no. 1: 016128.

[^Kremer96]: **Kremer, Michael.** 1996. "Integrating behavioral choice into epidemiological models of AIDS." *The Quarterly Journal of Economics* 111, no. 2: 549-573.



---

<!--**Endo, Akira.** 2020. "Estimating the overdispersion in COVID-19 transmission using outbreak sizes outside China." *Wellcome open research* 5.-->

[^Endo20]: **Endo, A., Centre for the Mathematical Modelling of Infectious Diseases COVID-19 Working Group, Abbott, S., Kucharski, A. J., & Funk, S.** 2020. "Estimating the overdispersion in COVID-19 transmission using outbreak sizes outside China." *Wellcome open research*, 5, 67. https://doi.org/10.12688/wellcomeopenres.15842.3


[^Farboodi]: **Farboodi, Maryam, Gregor Jarosch, and Robert Shimer.** 2021. "Internal and external effects of social distancing in a pandemic." *Journal of Economic Theory*: 105293.


[^lockdownPolitics]: TODO: Track down the source of that article you read about US State lockdown policies being more stable across time than Euro Members'. (Driven by exogenous political propensities?) (.5 correlation in lockdown index between months US States vs .2 Euro States?) 



---

## Appendix Material?

- Show that in more basic Poisson setup, duration of contagion doesn't matter.
