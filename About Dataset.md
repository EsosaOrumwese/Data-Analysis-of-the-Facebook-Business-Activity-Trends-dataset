# Facebook Business Activity Trends for Disaster Recovery

## About Dataset
### Introduction
Each year disasters, such as hurricanes, floods, earthquakes, cause disruptions in local
economies with the recovery from such disasters most times being very slow. Studies have
shown that even business affected by such disasters tend to lag behind when compared to
businesses in unaffected regions. Small and medium-sized business are especially vunerable
to the impacts of the disasters with poor countries and small-island nations still
experiencing the effect long after crisis is over. [1]

Ideally, in under for us to understand this issue, we would have approached this problem,
we would have survey the business owners in the affect regions. However this approach tends
to be expensive and time-consuming to execute. Other methods to measure recovery like remote
sensing through satelitte imagery supplimented by counting the number of open businesses, 
measuring foot traffic, etc., might yield fast results but tend to miss out some valuable
information [1]

This Business Activities Trends dataset was collected by implementing a method proposed by 
Eyre, De Luca and Simini[2]. It is one which is based on the frequency with which businesses
in a disaster-affected region make posts on their Facebook business pages. They validated 
their method through comparisons to surveys, mobile-phone data, satelitte imagery, etc., 
showing that it does give a reasonable estimate of time to recovery following three major 
natural disasters from the last decade (which ones boss?)[1][2] --This can also be stated
as "these authors propose and validate a methodology for using business social-media activity
to measure business downtime and recovery following natural disasters."

### Aim of Dataset
The aim of this dataset is to provide humanitarian organizations and researchers with data
about business recovery from disruptive events all over the world. It was collected by
implementing the methodology of Eyre, De Luca and Simini. Their approach involved aggregating 
posting behaviour of Facebook business pages, which are instances of Facebook's Pages product 
that are owned and operated by businesses. They hypothesize that posting to Facebook business 
pages will be affected by disruptive events (such as natural disasters) and that this change 
can be leveraged as a proxy measurement of the state of business sectors

### Methodology
This dataset was generated through the following steps:
  i. 	Identify crises, bounding boxes, and administrative polygons.
  ii. 	Generate a sample of business pages for each crisis.
  iii. 	Collect the raw data used to measure business activity.
  iv. 	Identify a pre-crisis baseline for each crisis.
  v. 	Calculate metrics from the raw data to measure business activity trends relative
	to the baseline period.
  vi. 	Share metrics with approved Data for Good partners.

I) Crises, Bounding Boxes, and Administrative Polygons
Real-world events such as natural disasters are represents in the Data for Good backend as
"crises". A crisis is specified by two things. Firstly a date on which the event occurred and
secondly by a bounding box which encapsulates the part of Earth's surface that was affected
by the event. The bounding box is used to identify a set of administrative polygons that are
used in the map calculation. Administrative polygons describe geographical units such as
counties, states, provinces, countries. 

For this dataset, polygons from the Database of Global Administrative Areas (GADM) were used.
This COVID-19 dataset isn't bounded by a bounding box since it is global and therefore was
released at 3 different polygon levels: county-level (GADM 2), state/region/province-level (GADM 1) and country-level (GADM 0).

IV) Baseline Period
A baseline period of the immediate 365 days before the crisis event data was establish for
this dataset. The baseline period is used to represent "normal" daily business activity of 
each page in the abscence of a crisis. These datasets use March 1, 2020 as the crisis date.

V) Metric Calculation
We measure business activity from the raw posts using activity quantile which is a metric 
calculated daily for each specific cell. A cell is a polygon-business vertical combination 
and all data are produced on a cell-level. 

  The activity quantile is calculated by taking <the daily post count of each page> and
  comparing it to its <distribution of daily post counts in the baseline period> to compute
  an individual page level activity "mid-quantile". 

  We then aggregate all the <individual page activity mid-quantiles> in a cell and do some
  transformations to form a <cell-specific daily activity quantile>.

  The resulting activity quantile can be interpreted as an aggregate measure of cell business
  activity relative to the baseline period. The activity quantile ranges from 0 to 1 with
  values near 0.5 indicating normal pre-crisis like behaviour. Significant and prolonged
  deviations from 0.5 indicate anomalous activity levels.

  Note: Although the metric is not strictly a quantile, we give it a quantile interpretation
  since it compares the daily activity of the cell to the distribution of daily baseline 
  activity of the cell under an assumed normal, non-crisis situation.

## Additional Datasets
### COVID-19 Government Response Policies and Business Activity Trends
The Oxford COVID-19 Government Response Tracker dataset[3] will be used together with the
BAT dataset. It tracks government coronavirus policies around the world at the country and
regional levels daily. The dataset contains various indicators on containment and closure
policies, economic policies, health system policies, vaccination policies, and other
miscellaneous policies. Our focus will be on the containment and closure policies[1] as it
relates to the whole country.

The Oxford policy tracker codes policies regarding closing of workplaces as an ordinal scale
with the following levels:
   0: No measures.
   1: Recommend closing (or work from home) or all business open with alterations resulting
      in significant differences compared to non-COVID-19 operation.
   2: Require closing (or work from home) for some sectors or categories of workers.
   3: Require closing (or work from home) for all non-essential workplaces (e.g. grocery
      stores, doctors??? <confirm this>)

To gauge the impact of workplace closing policies on business activity, 
 * we extract every policy change from level 0 to any non-zero at the country level between 
   01/03/2020 and 20/09/2021 globally and 
 * follow the trends of business activity for all business verticals within each country (at
   the GADM 0 level) for 60 days after the closing restriction policy.

We only consider days in which the daily policy matches the original policy change, so if a
country changes its policy again after 30 days, only the first 30 days are considered. The
resulting dataset contains 200 different policy changes across 142 different countries.

## Codebook [4]
GADM ID (gadm_id): The GADM ID of the polygon

GADM name (gadm_name): The GADM name of the polygon

GADM level (gadm_level): The GADM level of the polygon

GADM level 0 name (gadm0_name): The name of the polygon at GADM level 0 (i.e., country name).
Equivalent to GADM name if the GADM level is 0.

GADM level 1 name (gadm1_name): The name of the polygon at GADM level 1 (i.e., US state
name). Equivalent to GADM name if GADM level is 1.

GADM level 2 name (gadm2_name): The name of the polygon at the GADM 2 level (i.e., US county
name). Equivalent to GADM name if GADM level is 2.

Country (country): 2-character (ISO alpha-2) country code

Business vertical (business_vertical): The business vertical of the aggregation. Business 
verticals are defined internally within Facebook from categories selected by the Page admins. 
We use business verticals as a proxy for local economic sectors. Included as a business 
vertical is the “All” category, which includes but is not limited to all the other business 
verticals.

Activity quantile (activity_quantile): The level of activity as a quantile relative to the 
baseline period. This is equivalent to the 7-day average of what the University of Bristol 
researchers call the aggregated probability integral transform metric (see this article in 
Nature Communications). It’s calculated by first computing the approximate quantiles (the 
midquantiles in the article) of each Page’s daily activity relative to their baseline 
activity. The quantiles are summed and the sum is then shifted, rescaled and variance-
adjusted to follow a standard normal distribution. The adjusted sum is then probability 
transformed through a standard normal cumulative distribution function to get a value between 
0 and 1. We then average this value over the last 7 days to smooth out daily fluctuations. We 
give this metric a quantile interpretation since it compares the daily activity to the 
distribution of daily activity within the baseline period, where a value around 0.5 is 
considered normal activity. This is a one-vote-per-Page metric that gives equal weight to all 
businesses and is not heavily influenced by businesses that post a lot. We advise 
preferencing this metric, especially if robustness to outliers and numerical stability are 
important concerns.

Activity percentage (activity_percentage): The 7-day rolling sum of total activity (i.e., 
total posts) as a percent of the average weekly baseline average. The weekly baseline average 
is calculated as the average of the 7-day sum of total activity every Monday within the 
baseline period. For each day during the crisis, we divide the 7-day rolling sum of total 
posts by this weekly baseline average and multiply by 100. A value around 100 is considered 
to be normal activity. This metric is the most easily interpretable but tends to be heavily 
influenced by businesses that post a lot, which may give misleading results. This metric is 
also numerically less stable when the number of posts is relatively low. We advise using this 
metric if interpretability is the most important criteria.

Crisis start (crisis_ds): The crisis start date in YYYY-MM-DD format. The baseline period 
used is the 365 days prior to this date.

Date (ds) - The date of the activity provided in YYYY-MM-DD format defined by Pacific Time

### Business verticals
We derived the business verticals by aggregating categories as defined by the admins on the 
business Pages.

All: Refers to all businesses in the polygon. This includes but is not limited to all the 
following categories.

Grocery and convenience stores: Retailers that sell everyday consumable goods including food 
(typically unprepared foods and ingredients) and a limited range of household goods (like 
toilet paper). These can include grocery stores, convenience stores, pharmacies and general 
stores.

Retail: Retail other than grocery and convenience stores such as auto dealers, home goods 
stores, personal goods stores and general merchandise/big-box stores like Walmart

Restaurants: Businesses that sell prepared food and beverages for on-premise or off-premise 
dining

Local events: Events, activities and businesses that sell real-life experiences, such as 
amusement parks, bowling alleys, concert venues and social clubs

Professional services: Services driven by demand from an individual event such as a legal 
need or health issue that require high customization. Providers usually have an advanced 
degree or certification and are considered experts and “knowledge workers.” Examples include 
CPAs, lawyers, medical professionals, architects.

Business and utility services: Business services offering business-to-business services like 
construction, office cleaning, advertising and marketing companies and business software 
solutions. Utility services offer commodity services like electric, phone, internet, water 
and energy.

Home services: Services driven by demand from an individual event at home such as plumbing or 
electrical work. Examples include home repairs, photographers, cleaning, mechanics, plumbers, 
electricians, landscapers, interior decorators.

Lifestyle services: Specific to beauty, care and fitness services. These businesses offer 
standardized services that are part of a customer's regular routines. Examples include gyms, 
salons, barbers, and nonmedical and noneducational supervision, like childcare nurseries and 
pet care.

Travel: Businesses that provide or sell transportation or accommodation services, such as 
airlines, hotels, car rentals and tour operators

Manufacturing: Businesses that manufacture durable goods (like furniture and cars) or 
consumable goods (like food and personal goods) and have no or limited business-to-customer 
sales

Public good: Includes government agencies, nonprofits and religious organizations

## References
[1] Business Activity Trends Methodology paper (https://dataforgood.facebook.com/dfg/resources/business-activity-trends-methodology-paper)

[2] Robert Eyre, Flavia De Luca, and Filippo Simini. Social media usage reveals recovery of small businesses after natural hazard events. Nature communications, 11(1):1–10, 2020.

[3] Oxford COVID-19 Government Response Tracker (https://github.com/OxCGRT/covid-policy-dataset/tree/main)

[4] https://data.humdata.org/dataset/facebook-business-activity-trends-during-covid19?fbclid=IwAR3HMIOal-xZ9zleb8QsW_Kpt29oNg769KJNNmB_TjjU4yWh8l8x8maT-gw#





















