---
layout: post
title: "Facet Based Predictions in Oracle Real-Time Decisions"
date: 2012-02-17
tags: ["facet","implementation","Java","Oracle","predictive accuracy","RTD","rtd"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/02/17/facet-based-predictions-in-oracle-real-time-decisions/
---

<span style="color:#bbb;">[ [Crossposting](https://blogs.oracle.com/rtd/en/entry/facet_based_predictions) from the [Oracle Real-Time Decisions Blog](http://blogs.oracle.com/rtd/). ]</span>

The analytical models method detailed in [a previous post](http://lukasvermeer.wordpress.com/2012/01/24/analytical-models-in-oracle-real-time-decisions/) are not only extremely valuable for reporting, but can also be used to predict likelihoods for things other than regular choices. We can for instance generate predictions based on statistics for an attribute of a choice, rather than the choice itself. We use the term _facet based prediction_ to describe this advanced form of generating predictions.

This novel approach to modeling can be applied to significantly improve predictive accuracy and model quality. It can also facilitate the rapid transfer of existing learnings to newly created choices based on their facet values. These capabilities can be of use to practically all implementations, but they are of utmost importance in cases where the number of choices is very high or individual choices have short shelf life. In these instances, there might simply not be enough time or data to be able to predict likelihoods for individual choices. We could predict likelihoods for certain facets of our choices; as long as their cardinality remains relatively low.

Consider the following example in which we recommend products based on the acceptance of other products in the same category. In our ILS, Oracle Real-Time Decisions will be used to recommend a single product based on a single performance goal: _Likelihood_.

**Choice Groups Setup**

Products that may be recommended are stored in a choice group _Products_ (we will use static choices, but this approach could be implemented for dynamic choices also). Product choices have an attribute _Category_ which will contain a category name. We will use a second and separate dynamic choice group _Categories_ to record acceptance of the different product categories.

![Choice Group Setup]({{site.baseurl}}{% link assets/2012-02-17-facet-based-predictions-in-oracle-real-time-decisions-categorychoicegroups.jpg %} "CategoryChoiceGroups")

Note that we never intend to return any choices from the _Categories_ choice group to a client. It is configured using a dummy source and will not contain any actual choices. This group is only used within the ILS for predicting likelihoods. Statistics for this group may however be viewed in decision center reports.

**Recording Events**

Similar to the [example for analytical models](http://blogs.oracle.com/rtd/en/entry/analytical_models), we will record events against a dynamically generated choice representing a facet value rather than against the actual choice. In this example, both the actual choice and the event to record will be passed through a request represented as Strings.

[sourcecode language="java"]
// create a new choice to represent the category facet
CategoriesChoice c = new CategoriesChoice(Categories.getPrototype());
// set properties of the choice (SDOId should be of the form "{ChoiceGroupId}${ChoiceLabel}")
c.setSDOId("Category" + "$" + Products.getChoice(request.getChoice()).getCategory());
// record event in model (catching an exception just in case)
try { c.recordEvent(request.getEvent()); } catch (Exception e) { logError("Exception: " + e); }
[/sourcecode]

**Model Setup**
Our model setup is practically identical to before, but this time we'll enable "_Use for prediction_".

![Model Setup]({{site.baseurl}}{% link assets/2012-02-17-facet-based-predictions-in-oracle-real-time-decisions-categoriesmodel.jpg %} "CategoriesModel") 

**Predicting Likelihoods**
<div>A function _PredictLikelihood_ will be used to predict likelihoods for our products. The function takes a _Products_ choice and an _Event (String)_ as parameters and returns a _Double_ value representing the predicted likelihood.</div>
[sourcecode language="java"]
// get instance of the model used for predicting Category Events
CategoryEvents m = CategoryEvents.getInstance();
// return the likelihood based on the generated SDOId and the "Accepted" event
return m.getChoiceEventLikelihood("Categories$"+product.getCategory(), event );
[/sourcecode]

![Prediction Function]({{site.baseurl}}{% link assets/2012-02-17-facet-based-predictions-in-oracle-real-time-decisions-categoryfunction.jpg %} "CategoryFunction")

**Choice Group Scores Setup**

On the scores tab for the _Products_ choice group we configure the _Likelihood_ performance goal to be populated by the_PredictLikelihood_ function using parameters _this_ and "Accepted". The keyword _this_ refers to the particular choice being scored and will ensure each choice is scored according to its category facet.

![Scoring Setup]({{site.baseurl}}{% link assets/2012-02-17-facet-based-predictions-in-oracle-real-time-decisions-categoriesscoring.jpg %} "CategoriesScoring")

That is all that is required to score choices against a facet. We can now create decisions and advisors that use these predictions to recommend products based on their categories.

In this example, we have predicted likelihoods based on a single product facet. As a result, products in the same category will be scored the same. In practical implementations this will rarely be an issue, because there will presumably be multiple performance goals. Also, likelihoods may be mixed with product specific attributes like price or cost; resulting in score differentiation between products regardless of equality in likelihoods.

In a later post, we will discuss how we can expand on this to include multiple product facets in our likelihood prediction.