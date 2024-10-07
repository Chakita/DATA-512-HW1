# Analysis of Wikipedia article traffic for rare diseases

This repository contains the code and artifacts for the analysis of Wikipedia article traffic for a list of rare diseases as specified by [National Organization for Rare Diseases (NORD)](https://rarediseases.org). The collected list can be found in the csv file on the repository associated with this notebook ( [Link](https://github.com/Chakita/data-512-homework_1/blob/master/rare-disease_cleaned.AUG.2024.csv) to csv file on repository).

The aim of this analysis is to visualize the desktop and mobile traffic trends for these rare diseases and try to unearth any patterns within the data.  

The data used in this work is licensed under the [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) license. The data gathered is subject to the [Wikimedia Foundation terms of use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use) which states that users can freely access and reuse the content on Wikimedia platforms, including articles and datasets, under free and open licenses. Any contributions made to Wikimedia platforms must be licensed under a free and open license, allowing the content to be freely shared and reused by others. Users are responsible for their edits and contributions and must adhere to laws, avoid copyright infringement, and respect the platform's policies.

To gather the trafiic data, we use the Wikimedia Analytics Pageviews API ([documentation](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html)).  The Pageviews API provides data pertaining to desktop, mobile web, and mobile app traffic data starting from July 2015 through the previous complete month. As of writing this analysis, this consists of 111 months of data (if available). Some articles created after 2015 will have lesser number of months in the data. The extracted data is stored in JSON files ordered using the article title as the key. 

The resulting JSON response contains the field ```items``` that contains the required metrics.

The ```items``` field contains the following sub-fields:
 1. project - is usually en.wikipedia since these are wikipedia articles.
 2. article - the title of the article or in this case the name of the rare disease
 3. granularity - in our case this is monthly since we are pulling monthly traffic data from the API
 4. timestamp - timestamp associated with the data in the format YYYYMMDD
 5. agent - in this case we only consider user pageview details so this field should be "user"
 6. views - the number of page views for the given article and timestamp.

 We drop a field called access since this field is misleading for mobile accesses and cumulative access. 
 Mobile views consists of 2 types - mobile app views and mobile webiste views. The API distinguishes between these 2 types and hence we query it separately for app and website accesses and sum up the views to find the total number of mobile views.

 We then sort the records using the article title as key and save them in the JSON format.

There are 3 JSON files containing data corresponding to desktop access, mobile access and cumulative (sum total of desktop and mobile) accesses. The JSON files are named in the following format: ```rare-disease_monthly_<access_type>_<startYYYYMM>-<endYYYYMM>.json ```
where access type can be one of mobile, desktop or cumulative.
The JSON files produced for the time period July 1, 2015 to Oct 1, 2024 can be found in the xip file ```JSON_data_files``` [here](https://github.com/Chakita/data-512-homework_1) on this repostiory 

The data is utilised to visualize:

1. <b> Maximum Average and Minimum Average </b> - Articles that have the highest average page requests and the lowest average page requests for desktop access and mobile access over the entire time series.

2. <b> Top 10 Peak Page Views </b> -  Top 10 article pages by largest (peak) page views over the entire time series by access type (top 10 for desktop access + top 10 for mobile access).

3. <b> Fewest Months of Data </b> -  The 10 articles with the fewest months of data for desktop access and the 10 articles with the fewest months of data for mobile access.

The visualizations can be found on this repository with the names ```fewest_months.png```, ```max_min_average.png```, ```top_10_peak.png```

The work in this notebook builds upon the code provided in this [notebook](https://drive.google.com/file/d/1fYTIX79t9jk-Jske8IwysV-rbRkD4_dc/view) by Dr. David McDonald that provides example usage for the PageViews API

# Conclusion

From the plots we can observe that the rare disease with most number of monthly accesses on desktop and mobile is "Black Death" while the disease with the least number of monthly accesses on desktop and mobile is "Filippi Syndrome".

It is interesting to note that "COVID-19 vaccine misinformation and hesitancy" is listed as a rare disease on the list and features on the plot of the articles with fewest months of data. This makes sense considering that the COVID-19 virus is relatively new and the peak can be explained by a sudden interest in the topic due to the widespread vaccination efforts to contol the pandemic.

It is also worth noting the peak of the term "pandemic" both for mobile and desktop access in the year 2020 when the COVID-19 pandemic hit. It seems that the impact of the COVID-19 pandemic prompted people to look into other fatal pandemics that were experienced in the past with "Black Death" being considered one of the most fatal pandemics, claiming the lives of around 50 million people. This cpould explain why the two pandemic related terms peaked in terms of page views during the year 2020.




