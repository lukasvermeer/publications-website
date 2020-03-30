---
layout: post
title: "Putting the Business back in Business Intelligence"
date: 2011-04-16
tags: ["BI","Oracle","RTD"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2011/04/16/putting-the-business-back-in-business-intelligence/
---

> The reason why a large percentage of business intelligence (BI) applications fail is not due to technology. To a large degree these applications fail because of organizational, cultural, and infrastructure dysfunctions.
This observation from [an article by Larissa T. Moss](http://businessintelligence.com/article/120) seems to be common knowledge in the BI community. Too often have we built wonderful BI applications; only to discover that the business has changed its mind, want something else, or has no idea how to use the reports available. Our baby is [thrown out with the bathwater](http://en.wikipedia.org/wiki/Throw_out_the_baby_with_the_bath_water).

We sigh, shrug, consider the money spent on the project wasted and move on. [Unused report are unused](http://knowyourmeme.com/memes/x-y-is-x-redundant-adjectives-are-redundant), so no real harm done there. Obviously there is also some business opportunity lost, but it is difficult to estimate the real cost of a failed BI project.

**Not so with Oracle [Real-time Decisions](http://www.oracle.com/technetwork/middleware/real-time-decisions/overview/index.html) (RTD).**

BI reporting solutions support strategic decisions by providing reports and insight. Humans are then required to interpret the performance indicators provided and convert data and knowledge into actions to meet business goals. If the reports and insights are not aligned with business goals the human interpreters will simply not use them and come up with answers in other ways. Sure, these answers will probably not be perfect, but they will stil be directed towards meeting the actual business goals.

Conversely, RTD supports tactical decisions by combining business rules and predictive analytics to _automatically_ provide answers that optimize the defined business goals. Humans are required to monitor results, tweak rules and set priorities. If the business goals defined do not correlate to what the business actually wants, RTD will still (attempt to) optimize whatever it is that you are measuring.

The difference is that RTD will optimize _regardless_ of whether the defined goals match the reality of the business; and RTD is pretty damn good at optimizing the hell out of anything.

**Optimizing the right thing(s).**

As an example, imagine we implement RTD at a call centre to advise agents which products to sell to customers dialing in. Call centre operators care a lot about call handeling time (CHT); the average time it takes agents to handle a single call. We will (naively) use this as our only business goal; we want to minimize CHT. RTD will recommend a single product to sell and is informed of the results (total CHT for the call and whether the customer accepted the product) at the end of each call.

What do you think will happen?

The shortest calls are those where you sell nothing; the customers simply says "no" and hangs up. Thus more offers being rejected leads to lower CHT. Since RTD was told to minimize CHT it will quickly start recommending things it expects to be rejected. It will recommend things it knows have little chance of actually being sold.

[Mission accomplished!](http://images.google.com/images?q=mission+accomplished)

What do you say? You're saying you want to sell stuff? Well, too bad for you. That is not one of the business goals. All we said we cared about was CHT; and we're doing a pretty good job minimizing CHT once we stop selling stuff.

In BI reporting projects, lack of business involvement and ill-defined business goals might lead to unused reports. In RTD projects it could lead to disaster. We will waste more than just money (and RTD's built-in reporting will be able to show just how much is lost).

**We really, really, _really_ need the business involved.**