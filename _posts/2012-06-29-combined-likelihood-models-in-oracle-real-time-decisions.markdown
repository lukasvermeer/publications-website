---
layout: post
title: "Combined Likelihood Models in Oracle Real-Time Decisions"
date: 2012-06-29
tags: ["implementation","Java","likelihoods","models","Oracle","RTD","rtd"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/06/29/combined-likelihood-models-in-oracle-real-time-decisions/
---

<span style="color:#bbb;">[ [Crossposting](https://blogs.oracle.com/rtd/en/entry/combined_likelihood_models) from the [Oracle Real-Time Decisions Blog](http://blogs.oracle.com/rtd/). ]</span>

In a series of posts on this blog we have already described a [flexible approach to recording events](http://lukasvermeer.wordpress.com/2012/01/10/recording-choices/), a [technique to create analytical models for reporting](http://lukasvermeer.wordpress.com/2012/01/24/analytical-models-in-oracle-real-time-decisions/), a [method that uses the same principles to generate extremely powerful facet based predictions](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/) and a [waterfall strategy that can be used to blend multiple (possibly facet based) models for increased accuracy](http://lukasvermeer.wordpress.com/2012/04/16/waterfall-predictions-in-oracle-real-time-decisions/).

This latest, and also last, addition to this sequence of increasing modeling complexity will illustrate an advanced approach to amalgamate models, taking us to a whole new level of predictive modeling and analytical insights; combination models predicting likelihoods using multiple child models.

The method described here is far from trivial. We therefore would not recommend you apply these techniques in an initial implementation of Oracle Real-Time Decisions. In most cases, basic RTD models or the approaches [described](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/) [before](http://lukasvermeer.wordpress.com/2012/04/16/waterfall-predictions-in-oracle-real-time-decisions/) will provide more than enough predictive accuracy and analytical insight. The following is intended as an example of how more advanced models could be constructed if implementation results warrant the increased implementation and design effort. [Keep implemented statistics simple](http://en.wikipedia.org/wiki/KISS_principle)!

**Combined Likelihood Models**

Because [facet based predictions](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/) are based on metadata attributes of the choices selected, it is possible to generate such predictions for more than one attribute of a choice. We can predict the likelihood of acceptance for a particular product based on the product category (e.g. 'toys'), as well as based on the color of the product (e.g. 'pink').

Of course, these two predictions may be completely different (the customer may well prefer toys, but dislike pink products) and we will have to somehow combine these two separate predictions to determine an overall likelihood of acceptance for the choice.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-offermetadata.jpg %} "Offer Metadata")

Perhaps the simplest way to combine multiple predicted likelihoods into one is to calculate the average (or perhaps maximum or minimum) likelihood. However, this would completely forgo the fact that some facets may have a far more pronounced effect on the overall likelihood than others (e.g. customers may consider the product category more important than its color).

We could opt for calculating some sort of weighted average, but this would require us to specify up front the relative importance of the different facets involved. This approach would also be unresponsive to changing consumer behavior in these preferences (e.g. product price bracket may become more important to consumers as a result of economic shifts).

Preferably, we would want Oracle Real-Time Decisions to learn, act upon and tell us about, the correlations between the different facet models and the overall likelihood of acceptance. This additional level of predictive modeling, where a single supermodel (no pun intended) combines the output of several (facet based) models into a single prediction, is what we call a combined likelihood model.

**Facet Based Scores**

As an example, we have implemented three different [facet based models (as described earlier)](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/) in a simple RTD inline service. These models will allow us to generate predictions for likelihood of acceptance for each product based on three different metadata fields: _Category_, _Price Bracket_ and _Product Color_. We will use an _Analytical Scores_ entity to store these different scores so we can easily pass them between different functions.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-analyticalscores.jpg %} "Analytical Scores")

A simple function, creatively named _Compute Analytical Scores_, will compute for each choice the different facet scores and return an _Analytical Scores_ entity that is stored on the choice itself. For each score, a choice attribute referring to this entity is also added to be returned to the client to facilitate testing.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-offerscores.jpg %} "Offer Scores")

**One Offer To Predict Them All**

In order to combine the different facet based predictions into one single likelihood for each product, we will need a supermodel which can predict the likelihood of acceptance, based on the outcomes of the facet models. This model will not need to consider any of the attributes of the session, because they are already represented in the outcomes of the underlying facet models.

For the same reason, the supermodel will not need to learn separately for each product, because the specific combination of facets for this product are also already represented in the output of the underlying models. In other words, instead of learning how session attributes influence acceptance of a particular product, we will learn how the outcomes of facet based models for a particular product influence acceptance at a higher level.

We will therefore be using a single _All Offers_ choice to represent all offers in our combined likelihood predictions. This choice has no attribute values configured, no scores and not a single eligibility rule; nor is it ever intended to be returned to a client. The _All Offers_ choice is to be used exclusively by the _Combined Likelihood Acceptance_ model to predict the likelihood of acceptance for **all choices**; based solely on the output of the facet based models defined earlier.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-combinedlikelihoodmodel.jpg %} "Combined Likelihood Model")

**The Switcheroo**

In Oracle Real-Time Decisions, models can only learn based on attributes stored on the session. Therefore, just before generating a combined prediction for a given choice, we will temporarily copy the facet based scores-stored on the choice earlier as an _Analytical Scores_ entity-to the session. The code for the _Predict Combined Likelihood Event_ function is outlined below.

[sourcecode language="java"]
// set session attribute to contain facet based scores.
// (this is the only input for the combined model)
session().setAnalyticalScores(choice.getAnalyticalScores);

// predict likelihood of acceptance for All Offers choice.
CombinedLikelihoodChoice c = CombinedLikelihood.getChoice("AllOffers");
Double la = CombinedLikelihoodAcceptance.getChoiceEventLikelihoods(c, "Accepted");

// clear session attribute of facet based scores.
session().setAnalyticalScores(null);

// return likelihood.
return la;
[/sourcecode]

This sleight of hand will allow the _Combined Likelihood Acceptance_ model to predict the likelihood of acceptance for the _All Offers_ choice using these choice specific scores. After the prediction is made, we will clear the _Analytical Scores_ session attribute to ensure it does not pollute any of the other (facet) models.

To guarantee our combined likelihood model will learn based on the facet based scores-and is not distracted by the other session attributes-we will configure the model to exclude any other inputs, save for the instance of the _Analytical Scores_ session attribute, on the model attributes tab.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-combinedlikelihoodmodelattributes.jpg %} "Combined Likelihood Model Attributes")

**Recording Events**

In order for the combined likelihood model to learn correctly, we must ensure that the _Analytical Scores_ session attribute is set correctly at the moment RTD records any events related to a particular choice. We apply essentially the same switching technique as before in a _Record Combined Likelihood Event_ function.

[sourcecode language="java"]
// set session attribute to contain facet based scores
// (this is the only input for the combined model).
session().setAnalyticalScores(choice.getAnalyticalScores);

// record input event against All Offers choice.
CombinedLikelihood.getChoice("AllOffers").recordEvent(event);

// force learn at this moment using the Internal Dock entry point.
Application.getPredictor().learn(InternalLearn.modelArray,
                                 session(),
                                 session(),
                                 Application.currentTimeMillis());

// clear session attribute of facet based scores.
session().setAnalyticalScores(null);
[/sourcecode]

In this example, _Internal Learn_ is a special informant configured as the learn location for the combined likelihood model. The informant itself has no particular configuration and does nothing in itself; it is used only to force the model to learn at the exact instant we have set the _Analytical Scores_ session attribute to the correct values.

**Reporting Results**

After running a few thousand (artificially skewed) simulated sessions on our ILS, the Decision Center reporting shows some interesting results. In this case, these results reflect perfectly the bias we ourselves had introduced in our tests. In practice, we would obviously use a wider range of customer attributes and expect to see some more unexpected outcomes.

The facetted model for categories has clearly picked up on the that fact our simulated youngsters have little interest in purchasing the one red-hot vehicle our ILS had on offer.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-driverscars.jpg %} "Drivers Cars")

Also, it would seem that customer age is an excellent predictor for the acceptance of pink products.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-driverspink.jpg %} "Drivers Pink")

Looking at the key drivers for the _All Offers_ choice we can see the relative importance of the different facets to the prediction of overall likelihood.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-combineddriversacceptance.jpg %} "Combined Drivers Acceptance")

The comparative importance of the category facet for overall prediction might, in part, be explained by the clear preference of younger customers for toys over other product types; as evident from the report on the predictiveness of customer age for offer category acceptance.

![]({{site.baseurl}}{% link assets/2012-06-29-combined-likelihood-models-in-oracle-real-time-decisions-agepredictingcategories.jpg %} "Age Predicting Categories")

**Conclusion**

Oracle Real-Time Decisions' flexible decisioning framework allows for the construction of exceptionally elaborate prediction models that facilitate powerful targeting, but nonetheless provide insightful reporting. Although few customers will have a direct need for such a sophisticated solution architecture, it is encouraging to see that this lies within the realm of the possible with RTD; and this with limited configuration and customization required.

There are obviously numerous other ways in which the predictive and reporting capabilities of Oracle Real-Time Decisions can be expanded upon to tailor to individual customers needs. We will not be able to elaborate on them all on this blog; and finding the right approach for any given problem is often more difficult than implementing the solution. Nevertheless, we hope that these last few posts have given you enough of an understanding of the power of the RTD framework and its models; so that you can take some of these ideas and improve upon your own strategy.

As always, if you have any questions about the above-or any Oracle Real-Time Decisions design challenges you might face-please do not hesitate to contact us; via the comments below, social media or directly at Oracle. We are completely multi-channel and would be more than glad to help. :-)

* * *

**Update (19th September, 2012)**:_ Please note that the above is intended only as an example_. The implementation details shown here have been simplified so as to keep the application comprehensive enough for a single blog post. Most notably, an unhighlighted shortcut has been taken with regards to the way feedback events are recorded.

When implementing combined likelihood models in production systems, care must be taken to assure that the analytical scores used when recording feedback for a choice are _exactly identical_ to the scores used when this particular choice was recommended. In real world applications, this will require that all scores for recommended choices are stored separately on the session for later retrieval, rather than recalculating the scores when feedback occurs (which is what the example above will do).

The method described here is far from trivial. We therefore would not recommend you apply these techniques in an initial implementation of Oracle Real-Time Decisions and that you enlist the help of experienced RTD resources to ensure the resulting implementation is correct.

&nbsp;