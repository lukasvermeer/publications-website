---
layout: post
title: "Actionable Predictive Analytics with Oracle Data Mining"
date: 2012-10-03
tags: ["analytics","BI","Code","data mining","Database","Datamining","Oracle","RTD","SQL"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/10/03/actionable-predictive-analytics-with-oracle-data-mining/
---

[Oracle Data Mining](http://www.oracle.com/technetwork/database/options/advanced-analytics/odm/index.html) (ODM) provides powerful data mining functionality as native SQL functions within the Oracle Database. This [Oracle By Example Tutorial](http://apex.oracle.com/pls/apex/f?p=44785:24:5873518276883::NO:24:P24_CONTENT_ID,P24_PREV_PAGE:5271,2) gives a good overview of the GUI.

While being able to build predictive models on mountains of data without moving it out of the database is pretty cool in itself, I feel analysis without action is pretty much pointless. [Tom Davenport](http://www.tomdavenport.com) describes this common data mining conundrum in [Competing on Analytics](http://www.amazon.com/Competing-Analytics-New-Science-Winning/dp/1422103323).
> Many firms are able to segment their customers and determine which ones are most profitable or which are most likely to defect. However, they are reluctant to treat different customers differently-out of tradition or egalitarianism or whatever. With such compunctions, they will have a very difficult time becoming successful analytical competitors-yet it is surprising how often companies initiate analyses without ever acting on them. **The "action" stage of any analytical effort is, of course, the only one that ultimately counts.**
The OBE tutorial describes a scenario in which a business wants to identify customers who are most likely to purchase insurance. Through a set of simple steps, a (decision tree) classification model is built that can be used to predict whether a particular customer is likely to purchase based on historic data.

In a classical data mining approach, the predictions of this model would be written to some OUTPUT_TABLE where they would be available for subsequent processing. Growing staler every minute-and soon forgotten when its newer sibling OUTPUT_TABLE_NEW_FINAL_2 is inevitably created-our precious business intelligence slowly withers away in a disregarded section of the database until ultimately dropped by a careless DBA.

Output tables are where analytical insight goes to die.

If all we were interested in was building models, we'd be better off [glueing choo-choos](http://www.marklin.com). It is the new ways in which we can utilise these database resident models that makes this technology really interesting. With a few simple additional steps, this same model can be used in real-time to provide inline predictions based on up-to-date customer data; as well as for new customers.

All we need is a view<del> and a join</del>.

**Update (October 3rd, 2012)**: as Marcos points out in the comments, I was making things far too complicated. No need for a separate join; simply select the output columns you need and pass everything directly to the view.

![]({{site.baseurl}}{% link assets/2012-10-03-actionable-predictive-analytics-with-oracle-data-mining-odm_view.jpg %} "Oracle Data Mining Model Columns in a View")

<del>The join operations glues the original data and the prediction models together;</del> The view allows us to look at the harmonised results directly. When a customer record is selected from the view the source data for this record is passed to the model to generate the predicted values in real-time. When source data changes so does the prediction. When new source records are added they are automatically processed in the same way.

[sourcecode language="sql"]
-- Create a new customer.
INSERT INTO INSUR_CUST_LTV_SAMPLE (CUSTOMER_ID, LAST, FIRST) VALUES ('CU123', 'VERMEER', 'LUKAS');
1 rows inserted.
Elapsed: 00:00:00.003

-- Get prediction and probability for the new customer.
SELECT CUSTOMER_ID, insur_pred, insur_prob FROM insur_cust_ltv_prediction WHERE CUSTOMER_ID = 'CU123';
CUSTOMER_ID INSUR_PRED INSUR_PROB
----------- ---------- ----------
CU123       No         0.7262813
Elapsed: 00:00:00.004

-- Update customer data.
UPDATE INSUR_CUST_LTV_SAMPLE SET bank_funds = 500, checking_amount = 100 WHERE CUSTOMER_ID = 'CU123';
1 rows updated.
Elapsed: 00:00:00.003

-- Get prediction and probability for the updated customer.
SELECT CUSTOMER_ID, insur_pred, insur_prob FROM insur_cust_ltv_prediction WHERE CUSTOMER_ID = 'CU123';
CUSTOMER_ID INSUR_PRED INSUR_PROB
----------- ---------- ----------
CU123       Yes        0.6261398
Elapsed: 00:00:00.004
[/sourcecode]

Seamless. Any system that can read data from an Oracle database can now utilise Oracle Data Mining models. No need to move your data. No need to build new applications.

Applications reading data from the view need never know the difference between the original source data and machine generated predictions. [Oracle Business Intelligence Publisher](http://www.oracle.com/us/solutions/business-analytics/business-intelligence/publisher/overview/index.html) can easily display this data in forecasting reports; or use it to power pro-active alerts. In [Oracle Real-Time Decisions](http://www.oracle.com/us/solutions/business-analytics/business-intelligence/real-time-decisions/overview/index.html), rules can be built around the outcomes of these models; or predictions from multiple sources can be fed into [combined likelihood models](https://blogs.oracle.com/rtd/en/entry/combined_likelihood_models) for increased accuracy.

This is huge. Trust me. Stop over-analysing and start taking action. After all, that's the only step that ultimately counts.