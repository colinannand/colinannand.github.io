---
title: Compare Viz to Visitech.ai
date: 2025-01-15
always_allow_html: true
image: /images/Annual-Average.png
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../projects/_posts") })
description: Prompted from an interesting conversation.
layout: post
categories:
  - R Markdown
  - Jekyll    
    
---

## Quickly Visualizing Large Scale Open Data

- I want to make a quick comparison between a sizable data sets, and
  visualization that is simple an intuitive.
- I’ll be using R for this, and comparing to
  visitech.ai\[<https://www.visitech.ai/>\]

### 1. Reading in Data on weather.

    ## [1] 96317    29

First Initial Plot ![](/images/initial%20plot-1.png)<!-- -->

Converting to Farenheit manually

![](/images/data%20conversion-1.png)<!-- -->

### And a quick comparison to the image created by Visitech.ai

![](/images/Annual-Average.png)

### 1. World Development Indicators Data

I chose this one because it’s used commonly, has a wide variety of
factors, and is further available in a R Dataset:

1.  World Bank Group
    1.  The World Development Indicators (WDI) is the primary World Bank
        collection of development indicators, compiled from
        officially-recognized international sources. It presents the
        most current and accurate global development data available, and
        includes national, regional and global estimates.
    2.  <https://datacatalog.worldbank.org/home>
    3.  <https://datatopics.worldbank.org/world-development-indicators/>
    4.  Direct CSV URL:
        <https://databank.worldbank.org/data/download/WDI_CSV.zip>
    5.  Size ~270MB
    6.  Version published on HuggingFace.ai
        - <https://huggingface.co/datasets/datonic/world_development_indicators>

![](/images/Word%20Dev%20Data-1.png)<!-- -->

### 2. Census Data

    1. There is a large variety of Census data out there, and I think one of the easiest to work with is the ACS (American Community Survey) which is released in 1 and 5 year batches (these are estimates of populations with varying characteristics). 
    2. As an example, I chose the Selected Economic Characteristics (various job sectors and pay, populations numbers and percentages). 
    3. URL Link:
        1. https://data.census.gov/table/ACSDP1Y2023.DP03?g=010XX00US$0400000 
    4. API Direct Link:
        1. https://api.census.gov/data/2023/acs/acs1/profile?get=group(DP03)&ucgid=pseudo(0100000US$0400000)

![](/images/plotting%20code-1.png)<!-- -->

### 3. Example of Facebook Data

4.  Facebook Metrics
    1.  Brand specific advertisement metrics, across a variety of pages.
    2.  URL Link:
        <https://archive.ics.uci.edu/dataset/368/facebook+metrics>
    3.  Format: CSV
    4.  Size: ~20 KB

<!-- -->

    ## Rows: 500
    ## Columns: 19
    ## $ Page_total_likes                                                    <int> 13…
    ## $ Type                                                                <chr> "P…
    ## $ Category                                                            <int> 2,…
    ## $ Post_Month                                                          <int> 12…
    ## $ Post_Weekday                                                        <int> 4,…
    ## $ Post_Hour                                                           <int> 3,…
    ## $ Paid                                                                <int> 0,…
    ## $ Lifetime_Post_Total_Reach                                           <int> 27…
    ## $ Lifetime_Post_Total_Impressions                                     <int> 50…
    ## $ Lifetime_Engaged_Users                                              <int> 17…
    ## $ Lifetime_Post_Consumers                                             <int> 10…
    ## $ Lifetime_Post_Consumptions                                          <int> 15…
    ## $ Lifetime_Post_Impressions_by_people_who_have_liked_your_Page        <int> 30…
    ## $ Lifetime_Post_reach_by_people_who_like_your_Page                    <int> 16…
    ## $ Lifetime_People_who_have_liked_your_Page_and_engaged_with_your_post <int> 11…
    ## $ comment                                                             <int> 4,…
    ## $ like                                                                <int> 79…
    ## $ share                                                               <int> 17…
    ## $ Total_Interactions                                                  <int> 10…

![](/images/Facebook%20Data-1.png)<!-- -->
