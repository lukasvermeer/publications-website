---
layout: post
title: "Predicting Pi"
date: 2011-10-30
tags: ["Artificial Intelligence","Mathematics","Oracle","RTD","Statistics"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2011/10/30/predicting-pi/
---

A few weeks ago, I showed a colleague* [my little visual demo](http://www.xs4all.nl/~destack/projects/pi/) of [my favorite algorithm for estimating the number Pi](http://lukasvermeer.wordpress.com/2010/05/06/estimating-pi/). His immediate response was so obvious I am almost ashamed it did not occur to me before he mentioned it.

**"RTD could do that!"**

Turns out, he is of course right. Although [Oracle Real-Time Decisions](http://www.oracle.com/technetwork/middleware/real-time-decisions/index.html) was certainly not designed for the task, it does a pretty good job of _predicting_ Pi, given the right inputs and some mathemagic.

To reiterate, the idea behind the original algorithm is that we throw a bunch (well actually quite a lot) of darts at a square board (uniformly distributed, of course). We then count the number of darts that land in the largest circle we can fit in this square. The ratio between the darts in the circle and the total number of darts thrown, multiplied by four happens to approximate Pi for reasons explained [in an earlier post](http://lukasvermeer.wordpress.com/2010/05/06/estimating-pi/).

The key thing to understand when using RTD to implement this algorithm is that the ratio described above also represents the odds that a single dart will land in the circle. More easily put, the number of darts that land in the circle divided by the total number of darts thrown equals the chance of a dart landing in the circle. If we can predict the odds of hitting the circle we can predict Pi; and RTD is pretty good at making predictions.

**The Setup.**

I'll run through the RTD setup briefly. If you're interested in a more detailed explanation how I did this, feel free to drop me a line.

RTD can only predict the likelihoods related to choices, so we'll need a couple of those. For this experiment, we'll pretend we have a choice of landing the dart inside or outside the circle. We'll then diligently record whichever of those two happened to be the case each time we throw a dart so that RTD will learn to predict how likely either outcome is.

<span style="color:#bbb;">[ You can click on the screenshots below for a closer look. ]</span>

[caption id="attachment_684" align="aligncenter" width="720" caption="The "In Circle" choice"][![Two choices modeled in RTD representing a dart landing inside or outside the circle.](http://lukasvermeer.files.wordpress.com/2011/10/choice.png "The "In Circle" choice")]({{site.baseurl}}{% link assets/2011-10-30-predicting-pi-choice.png %})[/caption]

As you can see, I've already included the likelihood as a choice attribute to be returned to the calling application. This attribute will be populated by a model aptly named _Pi_.

[caption id="attachment_685" align="aligncenter" width="720" caption="The "Pi" model"][![RTD model configuration screen for the "Pi" model.](http://lukasvermeer.files.wordpress.com/2011/10/model.png "The "Pi" model")]({{site.baseurl}}{% link assets/2011-10-30-predicting-pi-model.png %})[/caption]

The _Pi_ model could hardly be simpler. It will predict the likelihood of mutually exclusive choices (hence the checkbox at the bottom of the configuration) from the choice group _Choices_.

Note that _Pi_ is configured as a simple so-called _Choice Model_, not the more common _Choice Event Model_. The latter, more complex model is more frequently used in practice, because it allows RTD to predict multi-step likelihoods (customer clicked an add, then put the product in his or her shopping basket, then proceeded to checkout, then completed the payment) for positive as well as negative events (e.g. the customer actively declines the offer).

We'll use a client application similar to the one used in the earlier Pi experiments to simulate the throwing of darts. This application will also show the predicted value of Pi. For this client to tell RTD about darts thrown and RTD to respond with the predicted value we will use a single advisor. The RTD server will generate a web service based on this configuration.

[caption id="attachment_691" align="aligncenter" width="720" caption="The "Get Prediction" advisor request"][![RTD advisor configuration screen with one boolean input parameter.](http://lukasvermeer.files.wordpress.com/2011/10/request.png "The "Get Prediction" advisor request")]({{site.baseurl}}{% link assets/2011-10-30-predicting-pi-request.png %})[/caption]

Each request to this advisor will tell RTD about a single dart thrown. The _Dart Was In Circle_ boolean input parameter seems pretty self-explanatory. To process this input and feed the model we will need some simple logic.

[caption id="attachment_692" align="aligncenter" width="720" caption="Advisor logic"][![Advisor logic to process the boolean input](http://lukasvermeer.files.wordpress.com/2011/10/logic.png "Advisor logic")]({{site.baseurl}}{% link assets/2011-10-30-predicting-pi-logic.png %})[/caption]

This is all that is needed to allow RTD to build a model that can predict the likelihoods for the darts landing inside or outside the circle.

<span style="color:#bbb;">[ Of course we also need so performance goal and decisions to allow the advisor request to return the two choices created, but we're not really interested in that here. As long as the request returns the _In Circle_ choice and its _Likelihood_ attribute we're happy. I'll leave this part of the configuration as an exercise for the reader. ]</span>

**The Result.**

It takes RTD a while (about a thousand darts or so) to come up with a prediction at all. But when it does, it is pretty much spot on.

The prediction RTD makes seems to be less sensitive to fluctuations resulting from the randomized input. At times, RTD's guess was even closer to the actual number Pi than the value calculated mathematical function I'd used in previous experiments (and of course, I couldn't resist taking a screenshot when it was).

[caption id="attachment_693" align="aligncenter" width="720" caption="Results"][![A screenshot of the RTD logs and the client application](http://lukasvermeer.files.wordpress.com/2011/10/estimatepi.png "Results")]({{site.baseurl}}{% link assets/2011-10-30-predicting-pi-estimatepi.png %})[/caption]

RTD not only good for [986% ROI](http://www.oracle.com/us/corporate/analystreports/infrastructure/forrester-tei-rtd-432543.pdf), but also for predicting Pi.

And this cake is [no lie](http://knowyourmeme.com/memes/the-cake-is-a-lie--5).

<span style="color:#bbb;">[* I forget who exactly suggested this. Either Alan, Simon or Tim. ]</span>