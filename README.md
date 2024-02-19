# Discovering Business Recovery Strategies and the Role of Governmental Policies through Analysis of Facebook Business Activity Trends

## Introduction

The COVID-19 pandemic left an indelible mark on the world. It reshaped economies, challenged
businesses and required unprecedented responses. To look at the impact of the COVID-19 pandemic
on businesses worldwide, I will be making use of two datasets; the Facebook Business Activity
Trends dataset (FBAT) and the Oxford COVID-19 Government Response Tracker dataset
(OxCGRT). The former captures the change in business activity globally during the pandemic
and its aftermath while the latter captures the response of governments through policies released
globally during the pandemic. The aim of this analysis is to uncover the strategies that aided in
the recovery of some businesses and to also investigate the pivotal role that governmental policies
play in crises like the COVID-19 pandemic.
To achieve this, the following objectives will be pursued: understanding the key metrics in the
FBAT dataset, assessing the quality of data captured in the FBAT dataset, analyse the timeseries
data for specific countries ( 5-6 countries which are chosen to b e as r epresentative as of the global
situation as possible), and identify the effects of policies on business trends in those chosen countries
through the integration of the OxCGRT dataset.

## Dataset Overview
To analyze the impact of the pandemic on businesses, we made use of the Facebook Business
Activity Trends (FBAT) dataset in synergy with the Oxford COVID-19 Government Response
Tracker dataset (OxCGRT).
### Facebook Business Activity Trends dataset
This is a dataset curated by Data for Good at Meta. It looks captures the change of that daily
posting patterns of business pages around the world on Facebook with regards to their activity
before the pandemic began. The data captures a pandemic period which starts from the crisis
data, March 1st 2020, till November 29th 2022. For the COVID-19 pandemic, it captures a period
of 365 days before the crisis date as its baseline period, this means that only business which had
active Facebook Business pages 365 days prior to the crisis date we’re included during the data
collation. The data is collated on a ’cell’ basis i.e., it looks at a particular business sector (business
vertical) in a particular country.

In this dataset, business activity is quantified using two metrics; activity quantile and activity
percentage. Activity quantile captures the change in the daily posting behaviour of businesses on
Facebook during the pandemic as compared to their precrisis behaviour. While activity quantile
measures the daily performance of business relative to the baseline period, activity percentage
focuses on its weekly performance. Activity percentage bases its comparison on the basis of the
business’ normal weekly activity. Anomalous behavior as compared to the baseline period can be
seen when the activity quantile significantly deviates from 0.5 or the activity percentage significantly
deviates from 100.
### Oxford COVID-19 Government Response Tracker dataset
The Oxford COVID-19 Government Response Tracker (OxCGRT) dataset was captured by the
Blavatnik School of Government at Oxford University. It is a comprehensive compilation of government
responses from to the COVID-19 pandemic, comprising 24 policy indicators grouped into
four categories: containment and closure policies (C), economic policies (E), health system policies
(H), and vaccination policies (V) and it spans from 2020 to 2022. It captures data on a national
jurisdiction level and also on subnational jurisdiction levels. To simplify the complex dataset,
four indices condense the information into single numerical scores, allowing for a comparative assessment
of government responses. These indices include the overall government response index,
containment and health index, stringency index, and economic support index, each scored between
0 to 100. However, they do not evaluate the effectiveness of the policies implemented but only help
in the comparison between nations of policies implemented. The data is split amongst different
csv files depending on the use case.

## Approach
This analysis was done on Python 3.10.9 using jupyter notebook. The FBAT dataset was gotten
of the [Facebook Data for Good website](https://dataforgood.facebook.com/dfg/tools/business-activity-trends) and it came in 1004 csv files, one for each day starting
from March 1st 2020 up until November 29th 2022. These files were read into Python and
combined into one csv file using the `pandas` and the `glob` library. The combined dataset contained
a total of 2,396,549 observations and 12 variables. For the FBAT dataset, only the country
names (`gadm0_name`) were included, the entire cells in the subregions variables (`gadm1_name`
and `gadm2_name`) were empty. There are 12 business verticals recorded in this dataset; Business
& Utility Services, Grocery & Convenience Stores, Home Services, Lifestyle Services, Local Events,
Manufacturing, Professional Services, Public Good, Restaurants, Retail, Travel and ’All’ (which
captures all businesses including the ones not included in the previous categories).


Following the advice of the authors, for this analysis, the activity quantile was used instead
of the activity percentage due its robustness to outliers (businesses in a particular business vertical
which post a lot) and its numerical stability. Due to the limited scope of this project, analysis was
performed on United Kingdom, Brazil, Nigeria, New Zealand, Kyrgyzstan and Sweden. The first
5 countries were chosen from different continents to be as representative of the global situation as
much as possible, while Sweden was chosen due to its relaxed approach with minimal restrictions.

Working with the OxCGRT dataset, we focused only on the containment and closure policies
which were issued in our chosen countries for everyone i.e., both vaccinated and unvaccinated.
This subset of the data was captured for 8 kinds of containment and closure polices (School
closing,Workplace closing, Cancel public events, Restrictions on gatherings, Close public transport,
Stay at home requirements, Restrictions on internal movement and International travel controls)
with ordinal values ranging from 0 to 2 (3 or 4), each representing the strictness of the policies
implemented. For each of the 8 kinds of containment and closure policies, there were associated
variables which contained notes which captured the news around that policy that was changed
which came in handy during analysis. Although the timeline of this dataset spanned past the date
of the FBAT dataset, this analysis was limited to only the period which the FBAT dataset covers.

## Summary
In this research we were able to uncover the countries and business verticals which felt the brunt
of the pandemic and also those which displayed remarkable resilence. Using the metric activity
quantile coupled with data from Oxford COVID-19 Government Response Tracker, we were able to
gain insights into the policies surrounding some trends in the country. Although business activity
could be explained with more than just the policy tracker dataset, I believe that this preliminary
insights would be helpful whilst deeper analysis is yet to be done.