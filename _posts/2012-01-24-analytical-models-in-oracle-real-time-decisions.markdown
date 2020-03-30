---
layout: post
title: "Analytical Models in Oracle Real-Time Decisions"
date: 2012-01-24
tags: ["analytics","choice","dynamic","events","implementation","Java","Oracle","RTD"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/01/24/analytical-models-in-oracle-real-time-decisions/
---

<span style="color:#bbb;">[ [Crossposting](http://blogs.oracle.com/rtd/en/entry/analytical_models) from the [Oracle Real-Time Decisions Blog](http://blogs.oracle.com/rtd/). ]</span>

As explained in a [previous post](http://lukasvermeer.wordpress.com/2012/01/10/recording-choices/), we can record events against unsourced dynamic choices created on-the-fly using the getPrototype method. Choices instantiated in this fashion, and the events recorded against them, will be visible in decision center reports.

This enables us to create extensive reporting based on arbitrary input from different sources without the need to specify all the possible choice values upfront. Creating so-called analytical models can be very useful for analysis.

**Recording Client Input**

Consider the following example which shows how this approach can be used to create an analytical model based on informant input. In our ILS, Oracle Real-Time Decisions will be used to find and report on correlations between a regular session attribute and arbitrary codes passed through an informant.

**Choice Group Setup**

A choice group Reason is used to store codes passed through the informant. During initialization, the choice group will attempt to grab choices from the ReasonEntityArray, but the array is a dummy entity that will always return nothing, because we've not defined a value for it.

![Reason choice group configuration, dynamic choices tab.]({{site.baseurl}}{% link assets/2012-01-24-analytical-models-in-oracle-real-time-decisions-reasondynamicchoice.jpg %})

![Reason choice group configuration, group attributes tab.]({{site.baseurl}}{% link assets/2012-01-24-analytical-models-in-oracle-real-time-decisions-reasonemptyattribute.jpg %})

**Informant Setup**

When invoked, a RecordReason informant will record an event for the ReasonCode input parameter. The logic for this informant is pretty straightforward.

[sourcecode language="java"]
// create a new choice based on the request attribute (a string that describes the reason)
ReasonChoice c = new ReasonChoice(Reason.getPrototype());
// set properties of the choice (SDOId should be of the form "{ChoiceGroupId}${ChoiceLabel}")
c.setSDOId("Reason" + "$" + request.getReasonCode());
// record choice in model (catching an exception just in case)
try { c.recordChoice(); } catch (Exception e) { logTrace("Exception: " + e); }
[/sourcecode]

**Model Setup**

In order to actually find and report on correlations, we will need to define at least one event model on our choice group. For this example, we'll keep things as simple as possible.

![Reasons choice event model configuration.]({{site.baseurl}}{% link assets/2012-01-24-analytical-models-in-oracle-real-time-decisions-reasonsmodel.jpg %})

**Reports**

The reports in decision center will show the reason codes sent to the informant as if they were dynamic choices and calculate statistics and correlations against session attributes.

<span style="color:#bbb;">(In this example, an Oracle Real-Time Decisions Load Balancer script was used to send four different codes to the ILS with a severe bias towards certain age groups.)</span>

![Decision center report for Reason choice, analysis tab.]({{site.baseurl}}{% link assets/2012-01-24-analytical-models-in-oracle-real-time-decisions-reasonsreport.jpg %})

This approach enables us to generate detailed reporting and analysis of more than just regular choices in the familiar decision center environment. In this example we were using informant input, but this technique can also be applied using the attributes of other choices to gain additional insight into the correlations between session attributes and choice attributes like product group or category (rather than individual choices).

This method can also be used in conjunction with predictive models. We will explore this possibility and its applications in future posts.