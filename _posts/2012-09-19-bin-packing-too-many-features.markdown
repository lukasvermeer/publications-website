---
layout: post
title: "Bin Packing Too Many Features"
date: 2012-09-19
tags: ["Code","excel","lp","Mathematics","solver"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/09/19/bin-packing-too-many-features/
---

[![Packed Motorbike]({{site.baseurl}}{% link assets/2012-09-19-bin-packing-too-many-features-2795354345_78c50a4af2_n.jpg?w=199 %} "Packed")](http://www.flickr.com/photos/lukasvermeer/2795354345/)My [girlfriend](http://lisannekrens.nl/) has been struggling with an interesting little problem lately. She was asked to determine the optimal distribution of medicine boxes and bottles over a set of adaptable cabinets; under volume as well as weight constraints. Not an easy task for a computer scientist; much less for a hospital pharmacist in training.

After describing the problem to me last night I (unhelpfully) mumbled that "this sounds like a [variable sized bin packing problem](http://en.wikipedia.org/wiki/Bin_packing_problem) to me, you can't solve the kind of thing in Excel, you probably need an LP solver".

Apparently I was wrong. It already seemed obvious to me that Excel suffers from a severe case of feature bloat, but [this](http://www.codeproject.com/Articles/257027/Excel-Solver-and-Liner-Programming) is just absurd.