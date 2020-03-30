---
layout: post
title: "Waterfall Predictions in Oracle Real-Time Decisions"
date: 2012-04-16
tags: ["implementation","Java","Oracle","predictive accuracy","RTD","rtd","waterfall"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/04/16/waterfall-predictions-in-oracle-real-time-decisions/
---

<span style="color:#bbb;">[ [Crossposting](https://blogs.oracle.com/rtd/en/entry/waterfall_predictions) from the [Oracle Real-Time Decisions Blog](http://blogs.oracle.com/rtd/). ]</span>

Facet Based Predictions are a powerful method to increase predictive accuracy and facilitate rapid learning and knowledge transfer, but the simple approach described in [an earlier post](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/) comes at a price. By using a single facet rather than individual choices for prediction, we decrease the granularity of our predictions. Choices that share the same facet value will be treated as equals by our predictive model; and even when important distinctions could be made after sufficient feedback is collected our simple facet based model will never learn to exploit these differences.

In most cases, the advantages of facet based models will outweigh the drawback of a reduction in granularity. This is especially true in implementations where shelf-life is short and no individual choice is ever expected to gather enough responses to build a predictive model. However, sometimes we will want to combine the power of facet based prediction with the accuracy of models defined at the lowest level of granularity; for instance when some choices are expected to collect sufficient feedback while others are not.

The complete and open decision management framework architecture of Oracle Real-Time Decisions allows us to blend predictive models in several ways. In this post, we will describe how we can mix-and-match two models at different levels detail using an approach we call waterfall prediction.

**Waterfall Prediction**

In essence, the waterfall method described here will try to predict a likelihood at the lowest possible level of granularity. If the choice based model at this grade has not received enough feedback to be considered mature, we will resort to a facet based model.

This implementation will build on the example described in [our previous post about facet based prediction](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/).

**Product Model Setup**

In addition to the facet based category events model configured earlier we will need a choice based event model Product Events. This model will predict likelihoods of events (Accepted and Ordered) based on feedback for individual choices.

![Product Model Setup]({{site.baseurl}}{% link assets/2012-04-16-waterfall-predictions-in-oracle-real-time-decisions-productmodel.jpg %} "Product Model Setup")

**Recording Events**

Previously, we would record feedback events only for our facet based model. As we now have two models at different levels of granularity, we will alter our code slightly to ensure we record any event against both models.

[sourcecode language="java"]
// create a new choice to represent the product
ProductsChoice p = Products.getChoice(request.getChoice());
// create a new choice to represent the category attribute
CategoriesChoice c = new CategoriesChoice(Categories.getPrototype());

// set properties of the category choice
c.setSDOId("Categories$" + p.getCategory());

// record choice in models (catching an exception just in case)
try { p.recordEvent(request.getEvent()); } catch (Exception e) { logError(e); }
try { c.recordEvent(request.getEvent()); } catch (Exception e) { logError(e); }
[/sourcecode]

Note that we are recording the same event twice, but against two separate choices p and c representing the two different levels of granularity. The Oracle Real-Time Decisions framework will automatically ensure the relevant models are updated accordingly.

**Predicting Likelihoods**

In this implementation, a new function will be used to predict likelihoods for our products. Rather than just returning the likelihood at the category level like before, this function will first check whether the more granular model has received enough feedback (in this case 100 positive events) to be considered mature. If the product model is deemed sufficiently trained, the function will use this model instead of the more general facet base one.

[sourcecode language="java"]
// get instance of the model used for predicting Product Events
ProductEvents productmodel = ProductEvents.getInstance();
// get instance of the model used for predicting Category Events
CategoryEvents categorymodel = CategoryEvents.getInstance();

// check if model for Product Events is sufficiently trained
if (productmodel.getChoiceEventModelCount(ModelCount.POSITIVE_COUNT, product.getSDOId(), event) >= 100)
{
    // return the likelihood based on the Product Event model
    return productmodel.getChoiceEventLikelihood("Products$"+product.getSDOId(), event);
}
// else if the model is not sufficiently trained
else {
    // return the likelihood based on the Category Event model
    return categorymodel.getChoiceEventLikelihood("Categories$"+product.getCategory(), event);
}
[/sourcecode]

![Waterfall Function]({{site.baseurl}}{% link assets/2012-04-16-waterfall-predictions-in-oracle-real-time-decisions-waterfallfunction.jpg %} "Waterfall Function")

Proper decision design and continuous in-live testing are crucial here. Precisely how much feedback should be considered "enough feedback" can wildly differ between implementations and use-cases. Moreover, some implementation might also permit the use of the quality of the models as reported by RTD, rather than the number of positive events, to determine the cascade threshold. Decision Center is an indispensable tool in this process.

**Choice Group Scores Setup**

Similar to before, on the scores tab for the Products choice group we configure the Likelihood performance goal to be populated by the new WaterfallLikelihood function instead of the PredictLikelihood function.

![Waterfall Score]({{site.baseurl}}{% link assets/2012-04-16-waterfall-predictions-in-oracle-real-time-decisions-waterfallscore.jpg %} "Waterfall Score")

These simple changes to our previous example empower our new implementation to benefit from two models at varying levels of granularity; leveraging both the accuracy of choice based models and the advantages of [facet based prediction](http://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/).

**The Power of Waterfall Prediction**

Waterfall prediction is a compelling example of how Oracle Real-Time Decisions enables us to blend multiple real-time models for use in rapid decisioning. This advanced approach to modeling can easily be expanded to cover more than just two levels and more than one hierarchy to further improve the predictive prowess of an RTD implementation.

Cascading models allows businesses to express various forms of decision logic that are far more subtle than simple one-step prediction models. In implementations where convergence time is incompatible with business requirements - or there is a low tolerance for random behavior in the initial stages of deployment - this flexibility in designing models and decisions is crucial.

In a future post, we will discuss another approach to amalgamate models, taking us to a whole new level of predictive modeling and analytical insights; combination models predicting likelihoods using multiple child models.