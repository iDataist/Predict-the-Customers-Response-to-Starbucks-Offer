# Predict Customers' Response to Starbucks Offer

## Project Overview

Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.

As a simplification, there are no explicit products to track. Only the amounts of each transaction or offer are recorded.
There are three types of offers that can be sent: buy-one-get-one (BOGO), discount, and informational. In a BOGO offer, a user needs to spend a certain amount to get a reward equal to that threshold amount. In a discount, a user gains a reward equal to a fraction of the amount spent. In an informational offer, there is no reward, but neither is there a requisite amount that the user is expected to spend. Offers can be delivered via multiple channels. Every offer has a validity period before the offer expires. As an example, a BOGO offer might be valid for only 5 days. Even informational offers have a validity period even though these ads are merely providing information about a product; for example, if an informational offer has 7 days of validity, the assumption is that the customer is feeling the influence of the offer for 7 days after receiving the advertisement.

The transactional data shows user purchases made on the app including the timestamp of purchase and the amount of money spent on a purchase. This transactional data also has a record for each offer that a user receives as well as a record for when a user actually views the offer. There are also records for when a user completes an offer.

Someone using the app might make a purchase through the app without having received an offer or seen an offer. For example, A user could receive a discount offer buy 10 dollars get 2 off on Monday. The offer is valid for 10 days from receipt. If the customer accumulates at least 10 dollars in purchases during the validity period, the customer completes the offer. However, there are a few things to watch out for in this data set. Customers do not opt into the offers that they receive; in other words, a user can receive an offer, never actually view the offer, and still complete the offer. For example, a user might receive the "buy 10 dollars get 2 dollars off offer", but the user never opens the offer during the 10 day validity period. The customer spends 15 dollars during those ten days. There will be an offer completion record in the data set; however, the customer was not influenced by the offer because the customer never viewed the offer.

## Problem Statement
The goal of this project is to combine transaction, demographic and offer data to predict which demographic groups respond to a particular kind of  offer. I plan to implement a supervised learning to achieve the goal.

## Metrics
I plan to use running time, accuracy score and f1 score to measure the performance of the supervised learners.

## Document Description
The raw data of this project is stored in the [data folder](https://github.com/iDataist/Predict-the-Customer-s-Response-to-Starbucks-Offer/tree/master/data), with the data dictionary shown below. The analysis, methodology, results and conclusion are documented in [Starbucks.ipynb](https://github.com/iDataist/Predict-the-Customer-s-Response-to-Starbucks-Offer/blob/master/Starbucks.ipynb). 

The data is contained in three files:

* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

Here is the schema and explanation of each variable in the files:

**portfolio.json**
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

**profile.json**
* age (int) - age of the customer
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income

**transcript.json**
* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

## Results
In this project, I first explored, visualized and combined the data. I generated a function choose_type which allows users to choose the offer types (bogo, discount or informational). Then I built several supervised learning models to predict whether to extend that offer type to a customer based on the demographic and transaction data. I used accuracy and f1 scores to evaluate the performance. I also considered the performance of the naive evaluator, which predicted that we should give offers to everyone. I chose Random Forest as the learner because it is a good balance between performance and running time. Lastly, I used grid search to tune the model. The optimized model has the accuracy of 0.83 and the f1 score of 0.85.

To summarize, I built a model that predicts customers' response to an offer, utilizing Random Forest Classifiers. Specifically, the user of this model could choose an offer type, and identify the customers who are the ideal targets of the offer.

Future imporovement could include recommendations on the communication channels and reward amounts. Currently, the model only provides predictions on the customers who are more likely to respond to an offer type. The next step would be to identify the ideal channels to communicate the offers. In addition, regression models could be implemented to identify the optimal reward amounts.
