---
layout: post
title: "Benford's Law Revised"
date: 2010-06-16
tags: ["Javascript","Mathematics","Statistics"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2010/06/16/benfords-law-revised/
---

After posting [my previous article](http://lukasvermeer.wordpress.com/2010/05/24/benfords-law/) I started wondering why my transactional data contained a disproportional amount of twos. _Something_ must be distorting the natural spread of first digits that Benford predicts. Later, while discussing this aberration with a friend, I finally figured it out.

**It's me. I distort the data. In fact, I distort the data every time I draw cash from an ATM.**

Like most people I have habits. One of them is that when I use an ATM I almost always withdraw twenty euros (I find cash a nuisance and prefer plastic, so I withdraw as little as I can manage). This, almost deterministic behavior, is the reason why there are a disproportionate amount of twos in the data. It seems [my previous conclusion](http://lukasvermeer.wordpress.com/2010/05/24/benfords-law/) applies to the data in more ways than one.
> Real-life data is obviously not random data, and when you think about it there are perfectly logical explanations for this result.
If I remove ATM transactions from the original dataset the graph looks different. Still not exactly matching Benford's prediction, but certainly a lot closer.

[caption id="attachment_142" align="aligncenter" width="374" caption="Tally of the first digits of bank transaction amounts excluding ATM transactions"][![Tally of the first digits of bank transaction amounts excluding ATM transactions]({{site.baseurl}}{% link assets/2010-06-16-benfords-law-revised-benford_fixed.jpg %} "Benford")](http://www.lukasvermeer.nl/projects/benford/)[/caption]

* * *

**Update (april 2nd 2011):** Code for this project is now available on [Github](https://github.com/lukasvermeer/benford).