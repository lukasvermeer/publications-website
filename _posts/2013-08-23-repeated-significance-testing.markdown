---
layout: post
title: "Simulating Repeated Significance Testing"
date: 2013-08-23
tags: ["data science","Data Science","Javascript","repeated significance testing","Statistics","statistics"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2013/08/23/repeated-significance-testing/
---

My colleague Mats has an excellent piece on the topic of repeated significance testing [on his blog](http://www.einarsen.no/is-your-ab-testing-effort-just-chasing-statistical-ghosts/).
> To demonstrate how much [repeated significance testing] matters, I've ran a simulation of how much impact you should expect repeat testing errors to have on your success rate.
The simulation simply runs a series of A/A conversion experiments (e.g. there is no difference in conversion between two variants being compared) and shows how many experiments ended with a significant difference, as well as how many were ever significant _somewhere along the course of the experiment_. To correct for wild swings at the start of the experiment (when only a few visitors have been simulated) a cutoff point (minimum sample size) is defined before which no significance testing is performed.

Although the post includes a link to the [Perl code](https://gist.github.com/anonymous/2945361) used for the simulation, I figured that for many people downloading and tweaking a script would be too much of a hassle, so I've ported the simulation to a simple web-based implementation.

[![Repeated Significance Testing Simulation Screenshot]({{site.baseurl}}{% link assets/2013-08-23-repeated-significance-testing-screen-shot-2013-08-23-at-3-44-01-pm.png %})](http://www.lukasvermeer.nl/projects/significance/)

You can tweak the variables and run your own simulation [in your browser here](http://www.lukasvermeer.nl/projects/significance/), or fork the code yourself [on Github](https://github.com/lukasvermeer/significance).