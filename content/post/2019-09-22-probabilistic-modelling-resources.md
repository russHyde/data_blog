---
title: Probabilistic modelling resources
author: Russ Hyde
date: '2019-09-22'
slug: probabilistic-modelling-resources
categories: []
tags:
  - bayes
lastmod: '2019-09-22T11:27:03+01:00'
layout: post
type: post
highlight: no
---

**Probabilistic Programming**

I've been doing a bit of probabilistic modelling in [STAN](https://mc-stan.org)
recently and have used [JAGS](mcmc-jags/sourceforge.net) for a long time.
Probabilistic modelling (as embodied in probabilistic graphical models - PGMs
- and Bayesian statistics and implemented in probabilistic programming
languages and libraries like STAN) is a way to model some phenomenon that
incorporates various sources of randomness, and the dependence between
components of that model. The models used tend to be more sparse and more
informative than would be generated by a neural-network based model for the
same phenomenon (IMO). However, even for modestly sized models or models that
encode hierarchical dependencies between variables, you have to use
approximation techniques (Markov-Chain Monte Carlo - MCMC, or variational
Bayes) during model fitting.

Here are some resources for getting started with MCMC models (and their
variants).

Tools

- [JAGS](https://mcmc-jags/sourceforge.net)

    - JAGS was the first MCMC tool I used. At the time it provided a
    cross-platform BUGS-like syntax. You can call it from R (see 
    [this blogpost](https://r-bloggers.com/getting-started-with-jags-rjags-and-bayesian-modelling/)
    for details of R-specific tools for working with JAGS).

- [STAN](https://mc-stan.org)

    - if you've got no missing data, and all your variables are
    continuous, you should start with STAN. It uses Hamiltonian MCMC
    which, although individual steps may be slower than an equivalent
    JAGS step, STAN typically works out faster than JAGS because you
    typically require fewer samples during your runs.

    - There are some case studies, user's guides etc at the website and
    there are good tools for integrating with R and python.
    
    - STAN models are compiled to C++ before running, so there is a bit
    of overhead before your sampling steps kick off.

- [PyMC3](https://docs.pymc.io)

    - this is a pythonic tool for doing Bayesian modelling. It is independent
    of JAGS / BUGS / STAN. I have relatively little experience with PyMC3, but
    it does have some good resources: the tutorials section is brilliant and
    the book by Osvaldo Martin provides a good introduction.

- There are dozens of other tools for probabilistic programming

Tutorials

- The [PyMC3 tutorials](https://docs.pymc.io/nb_tutorials/index.html) are really good.

- I haven't found any really good JAGS / STAN tutorials.

Courses

- Matthew Heiner and Herbie Lee have a course on MCMC and techniques for
Bayesian statistics / probabilistic modelling at Coursera, which I can highly recommend. You'll need to be able to manipulate probability distributions first though.:

    - [Bayesian Statistics: Techniques and Models](
    https://coursera.org/learn/mcmc-bayesian-statistics)

- Daphne Koller does a three-part course on Probabilistic graphical models
  through Coursera (it used Octave / Matlab when I did it, which I didn't care
  for) but, at the time (~ 2013?), I found the courses quite eye-opening.

    - [Probabilistic Graphical Models Specialization](
    https://coursera.org/specializations/probabilistic-graphical-models)

Books

- Statistical Rethinking - Richard McElreath

    - This book is remarkably well written. It's tied to the R ecosystem but it
    should be possible to translate the models over to other languages (indeed
    others have converted the exercises into Python, Julia and other R DSLs:
    https://xcelab.net/rm/statistical-rethinking). The later chapters use STAN
    and cover hierarchical models, generalised linear models etc.

    - Note that his lecture course is available on youtube

- Bayesian Analysis with Python - Osvaldo Martin

    - Although I'd recommend working through the PyMC3 tutorials instead, this
    book was pretty good. You may be able to get it for free through the Packt
    book of the day thing [packt](https://packtpub.com/free-learning)

- There's a range of machine learning books that cover probabilistic modelling.
  Three with a strong Bayesian / PGM bias are the Bishop, Barber and Koller 
  books. 

Blogs & Other notes

- [Darren Wilkinson](darrenjw.wordpress.com) at Newcastle Uni has a blog that
covers MCMC and also some other stuff I'm into at the moment: functional
programming and scala. The [Rainier](https://github.com/stripe/rainier/)
package combines all three of these threads and this blog has included a few
posts on rainer recently.