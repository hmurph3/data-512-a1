# data-512-a1
construct, analyze, and publish a dataset of monthly traffic on English Wikipedia from January 1 2008 through September 30 2018.

The goal of this analysis is to create a visualization of the English Wikipedia Traffic from January 1 2008 through Settember 30 2018 using mobile traffic, destop traffic and all traffic (mobile and desktop combined). 

The data used in this analysis is from the Wikimedia Foundation under CC BY-SA and GFDL. For more information, please see https://www.mediawiki.org/wiki/REST_API#Terms_and_conditions.

I used two API endpoints to gather the data.

1.  The **Legacy Pagecounts API** which accesses the desktop and mobile trafic from December 2007 to July 2016.
 * See documentation for the API at https://wikitech.wikimedia.org/wiki/Analytics/AQS/Legacy_Pagecounts
 * See information for the endpoint at https://wikimedia.org/api/rest_v1/#!/Pagecounts_data_(legacy)/get_metrics_legacy_pagecounts_aggregate_project_access_site_granularity_start_end
 * the information pulled from the quires include: project, access-site, granularity, timestamp and the count of views.
2.  The **Pageviews API** whihc accesses the desktop, mobile web and mobile app traffic froom July 2015 to lthe previous month. For my analysis, this was September 2018.
* See documentation for the API at https://wikitech.wikimedia.org/wiki/Analytics/AQS/Pageviews
 * See information for the endpoint at https://wikimedia.org/api/rest_v1/#!/Pageviews_data/get_metrics_pageviews_aggregate_project_access_agent_granularity_start_end.
 *the information pulled from the quires include: project, access-site, granularity, timestamp, agnet, and the number of views.
 
 
 There are **8 files** in this repository. 
 
 1-5. **The raw data from the API quires**:
 * _pagecounts_desktop-site_200712-201607.json_ -  query of the Legacy desktop site views from Dec 2007 to July 2016, aggrigated by month.
 * _pagecounts_mobile-site_200712-201607.json_ -  query of the Legacy mobile site views from Dec 2007 to July 2016, aggrigated by month.
 * _pageviews_desktop_201507-201809.json_ - query of Pageviews on the desktop site from July 2015 to Sept 2018, aggrigated by month.
 * _pageviews_mobile_web_201507-201809.json_ - query of Pageviews on the mobile website from July 2015 to Sept 2018, aggrigated by month.
 * _pageviews_mobile_app_201507-201809.json_ - query of Pageviews on the mobile app from July 2015 to Sept 2018, aggrigated by month.
 * **Please Note:** the newer dataset to track site traffic (**Pageviews API**) is able to differentiat between actual traffic and spider/crawler traffic. The *pageviews* queries have filtered out the spider/crawler traffic, since I am only interensted in tracking actual views. 
 6. **hcds-a1-data-curation.ipynb**: This is a commented Jupyter Notebook of my data quieries, data transformation, analysis, and visual creation. resources and reference sites I used solve issues I was having are docmented in code. 
 7. **en-wikipedia_traffic_200712-201809.csv**: a .csv of the required data to create the end plot.
 The columns in this file are (not in order):
 * Year - Year of the page views
 * Month - Month of the page views
 * pagecount_all_views - the number of all views for the Legacy API
 * pagecount_desktop_views - the number of desktop views for the Legacy API
 * pagecount_mobile_views - the number of mobile views for the LEgacy API 
 * pageview_all_views - the number of all views for the Pageviews API
 * pageview_desktop_views - the number of desktop views for the Pageviews API
 * pageview_mobile_views - the number of mobile app and web views for the Pageviews API
 
  I followed the following steps to create this file (steps can be seen in **hcds-a1-data-curation.ipynb**):
  
 a. Query the APIs to collect 5 datasets for the various forms of page views. I filtered by only user views (no spider/crawler traffic) and by the type of views (desktop vs mobile).
 
 b. Combine the mobile_app and mobile_web views from the Pageviews query. Since the Legacy data has no differentiation between the two, I needed to combine the app and web views to be comparable to the Legacy data.
 
 c. Seperate the *timestamp* into its year (YYYY) and month (MM), and remove the values for day (DD) and hour (HH). These values are not necessary since i quired the data at the month level, and they defalut to the first day of the month and midnight. This is necessary to plot by month later.
 
 d. Created **en-wikipedia_traffic_200712-201809.csv** by merging the datasets and removing all columns un-needed for the plotting (ex: agent, project, granularity).
 
 e. Saved the dataframe to a .csv.
 
8. **en-wikipedia_traffic_200712-201809.png** - an image output of the plot created by my analysis. The steps to create this can be seen in **hcds-a1-data-curation.ipynb**.
 
