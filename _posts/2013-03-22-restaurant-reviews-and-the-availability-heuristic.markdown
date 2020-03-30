---
layout: post
title: "Restaurant Reviews and the Availability Heuristic"
date: 2013-03-22
tags: ["availability heuristic","Psychology","R","Statistics","statistics"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2013/03/22/restaurant-reviews-and-the-availability-heuristic/
---

You could say fine dining is a bit of a hobby of mine; and as I've [mentioned before](http://lukasvermeer.wordpress.com/2012/01/02/from-restaurant-advice-to-recommending-associates/), I've composed quite a few [restaurant reviews](https://plus.google.com/106518479241050228896/reviews) over the years. I enjoy writing about food almost as much as I love eating it.

Whilst fantasising about fancy food with a colleague the other day, we wondered whether there is any relation between the lengthiness of my reviews and the associated score. In some strange way it made intuitive sense to me that I would devote more words to describe why a particular restaurant did not live up to my expectations.

Thinking about this, the first negative review that came to my mind was one I wrote for ["The Good View" in Chiang Mai, Thailand](https://plus.google.com/106395365263529906125/about).
> If service were any slower than it already is, cobwebs would certainly overrun the place. When food and drinks eventually do arrive they're hardly worth the wait.> 
> 
> Fruit juice contained more sugar than a Banglamphu brothel and cocktails had less alcohol in them than a Buddhist monk. The mixed Northern specialties appetizer revealed itself to be three kinds of sausage and some raw chillies; very special indeed.> 
> 
> The spicy papaya salad probably tasted alright, but I was unable to tell because my taste buds were destroyed on the first bite. (Yes, I see the irony in complaining a spicy papaya salad was too spicy, but in my mind there's a difference between spicy food and napalm.)> 
> 
> Also, the view is terribly overrated.
Conversely, the first positive review that popped into my brain was this rather terse piece for ["Opium" in Utrecht, the Netherlands](https://plus.google.com/116496292311938777959/about).
> Om nom nom.
Judging by this tiny sample there might indeed be something to the hypothesis that review length and review score are negatively correlated. To confirm my hunch, I decided to load my reviews into [R](http://www.r-project.org/) for a proper statistical analysis.

[sourcecode language="R"]
> cor.test(nn_reviews$char_count, nn_reviews$score)

Pearson's product-moment correlation
data: nn_reviews$char_count and nn_reviews$score
t = 0.2246, df = 121, p-value = 0.8227
alternative hypothesis: true correlation is not equal to 0
95 percent confidence interval:
-0.1571892 0.1967366

sample estimates:
   cor
0.02041319
[/sourcecode]

To my surprise, the analysis shows there is practically no relation between length and score. Contrary to what the two reviews above seem to suggest I do not require more letters to describe an unpleasant dining experience as opposed to a pleasant one.

A simple plot of the two variables gives some insight into a possible cause for my misconception.

[caption id="attachment_1616" align="aligncenter" width="650"]![Review scores vs review length]({{site.baseurl}}{% link assets/2013-03-22-restaurant-reviews-and-the-availability-heuristic-review_scores_vs_length.jpeg %}) Review scores vs review length[/caption]

The outlier in the bottom right happens to represent my review for the Good View. All my other reviews are much shorter in length and seem to be quite evenly distributed over the different scores.

My misjudgement is an excellent example of the [availability heuristic](http://en.wikipedia.org/wiki/Availability_heuristic). The pair of examples that presented themselves to me upon initial reflection were not representative of the complete set, but that did not stop me from drawing overarching, and incorrect, conclusions based on a sample of two.

This is why I use statistics, because I am a fallible human being; just like everyone else