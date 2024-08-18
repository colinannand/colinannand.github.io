---
title: "R4_Debris_Removal_Formulas_Exploration"
author: "Colin T. Annand"
date: "2024-06-01"
always_allow_html: true
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
description: "Work from a FEMA short suspense analysis."
layout: post
categories:
  - R Markdown
  - Jekyll    
    
---

![](/../images/fema-logo-blue.svg)<!-- -->

# Debris Model Analysis

This report is split into multiple sections, which you can access with
the tabs below. There is a summary, then **Vegetative Debris** and
**Construction Debris**, and an *Appendix*. You can switch between the
tabs/analysis by clicking the tab headers below:
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## BLUF

We support the following formulas for Vegetative debris prediction and
Construction debris predictions:

### Vegetative

$$Cost = 10^{(log10( Cubic Yards )*0.73076 + 2.5827 )}$$

### Construction

$$Cost = 10^( log10 (Cubic Yards )*0.54892  + 3.5681)$$ The analysis and
explanation for each of these follows below. The multipliers(cost per
cubic yard) and the intercept (base costs for removal) are derived from
*linear regression*.

> > Use the table of contents to the left, and the analysis tabs to view
> > a given section.

### Debris Data

Below is data aggregated to *separate instances of debris removal* at
the **county** level. We briefly view the data below, and will use
**Gross_Cost** and **Quantity** (in Cubic Yards) to calculate ratios and
relationships like ‘*cost per cubic yard*’ across the counties and
projects in the dataset. We are including **all regions** in this
version of the analysis, to be able to fit more robust relationships
between cost and quantity.

- Data Range: 2016-2024
- Regions: (ALL)
- Number of Debris Removals: 14384

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:650px; ">

<table class="table table-striped" style="color: black; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Disaster_Number
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Incident_Type
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Declaration_Date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Region_Number
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
State
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Damage_CatA_Removal_Id
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Damage_Id
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Project_Id
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Id
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Name
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Event_Id
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
County
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
FIPS
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Gross_Cost
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Net_Cost
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Work_Category
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Location_Type
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Debris_Type
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Debris_Type_Other
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Quantity
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Quantity_Unit_Of_Measure
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Start_Date
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
End_Date
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42697
</td>
<td style="text-align:right;">
1305611
</td>
<td style="text-align:right;">
708608
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
4208286.2
</td>
<td style="text-align:right;">
4208286.2
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
48.6128
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
41281
</td>
<td style="text-align:right;">
1256239
</td>
<td style="text-align:right;">
687485
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
65804428.0
</td>
<td style="text-align:right;">
65804428.0
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
100000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42599
</td>
<td style="text-align:right;">
1305614
</td>
<td style="text-align:right;">
708612
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
2082260.3
</td>
<td style="text-align:right;">
2082260.3
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3266.6500
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-12-08
</td>
<td style="text-align:left;">
2023-01-24
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42607
</td>
<td style="text-align:right;">
1305614
</td>
<td style="text-align:right;">
708612
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
2082260.3
</td>
<td style="text-align:right;">
2082260.3
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
5999.5400
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42422
</td>
<td style="text-align:right;">
1265535
</td>
<td style="text-align:right;">
692446
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
41109052.2
</td>
<td style="text-align:right;">
41109052.2
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
87744.5500
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-12-08
</td>
<td style="text-align:left;">
2023-01-24
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
41403
</td>
<td style="text-align:right;">
1259472
</td>
<td style="text-align:right;">
689433
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
41768603.2
</td>
<td style="text-align:right;">
41768603.2
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1500000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42431
</td>
<td style="text-align:right;">
1265535
</td>
<td style="text-align:right;">
692446
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
41109052.2
</td>
<td style="text-align:right;">
41109052.2
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
140391.2800
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42452
</td>
<td style="text-align:right;">
1304851
</td>
<td style="text-align:right;">
708168
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
8306922.7
</td>
<td style="text-align:right;">
8306922.7
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
7790.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42445
</td>
<td style="text-align:right;">
1304851
</td>
<td style="text-align:right;">
708168
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
8306922.7
</td>
<td style="text-align:right;">
8306922.7
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3819.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-12-08
</td>
<td style="text-align:left;">
2023-02-09
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42405
</td>
<td style="text-align:right;">
1305219
</td>
<td style="text-align:right;">
708411
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
15657618.7
</td>
<td style="text-align:right;">
15657618.7
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
352620.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-12-08
</td>
<td style="text-align:left;">
2023-02-15
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42345
</td>
<td style="text-align:right;">
1305219
</td>
<td style="text-align:right;">
708411
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
15657618.7
</td>
<td style="text-align:right;">
15657618.7
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
286696.9400
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Florida
</td>
<td style="text-align:right;">
42686
</td>
<td style="text-align:right;">
1305611
</td>
<td style="text-align:right;">
708608
</td>
<td style="text-align:right;">
201362
</td>
<td style="text-align:left;">
Florida Division of Emergency Management
</td>
<td style="text-align:right;">
1732
</td>
<td style="text-align:left;">
Leon County
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:right;">
4208286.2
</td>
<td style="text-align:right;">
4208286.2
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
28336.7000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-12-08
</td>
<td style="text-align:left;">
2023-01-24
</td>
</tr>
<tr>
<td style="text-align:right;">
4578
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Utah
</td>
<td style="text-align:right;">
34246
</td>
<td style="text-align:right;">
439802
</td>
<td style="text-align:right;">
172555
</td>
<td style="text-align:right;">
53511
</td>
<td style="text-align:left;">
Utah (Utah Division of Emergency Management)
</td>
<td style="text-align:right;">
491
</td>
<td style="text-align:left;">
Salt Lake County
</td>
<td style="text-align:left;">
49035
</td>
<td style="text-align:right;">
422306.6
</td>
<td style="text-align:right;">
422306.6
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9554.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2020-09-08
</td>
<td style="text-align:left;">
2020-10-26
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41490
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
30363.1000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-07-15
</td>
<td style="text-align:left;">
2023-09-30
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41501
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2755.2000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-04-10
</td>
<td style="text-align:left;">
2023-06-30
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41481
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
5023.2000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-05-19
</td>
<td style="text-align:left;">
2023-06-23
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41503
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3651.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-04-25
</td>
<td style="text-align:left;">
2023-05-01
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41476
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2062.9000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-05-01
</td>
<td style="text-align:left;">
2023-06-22
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41504
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9049.8000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-05-08
</td>
<td style="text-align:left;">
2023-06-19
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41505
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2601.9000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-05-04
</td>
<td style="text-align:left;">
2023-05-07
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41508
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
910.9000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-04-26
</td>
<td style="text-align:left;">
2023-06-27
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41511
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2090.2000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-05-31
</td>
<td style="text-align:left;">
2023-06-22
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
42002
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
6331.3000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-05-07
</td>
<td style="text-align:left;">
2023-06-30
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41470
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
54753.8000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-06-06
</td>
<td style="text-align:left;">
2023-07-12
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:right;">
41473
</td>
<td style="text-align:right;">
1238782
</td>
<td style="text-align:right;">
679681
</td>
<td style="text-align:right;">
199625
</td>
<td style="text-align:left;">
Montana Disaster & Emergency Services
</td>
<td style="text-align:right;">
1709
</td>
<td style="text-align:left;">
Lewis and Clark County
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:right;">
4623743.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1121.1000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2023-07-10
</td>
<td style="text-align:left;">
2023-07-10
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Kentucky
</td>
<td style="text-align:right;">
44887
</td>
<td style="text-align:right;">
1248085
</td>
<td style="text-align:right;">
688836
</td>
<td style="text-align:right;">
200220
</td>
<td style="text-align:left;">
KY Division of Emergency Management
</td>
<td style="text-align:right;">
1718
</td>
<td style="text-align:left;">
Franklin County
</td>
<td style="text-align:left;">
21073
</td>
<td style="text-align:right;">
25248115.2
</td>
<td style="text-align:right;">
25248115.2
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public ROW
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
8072.9400
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-08-04
</td>
<td style="text-align:left;">
2023-02-01
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Kentucky
</td>
<td style="text-align:right;">
44664
</td>
<td style="text-align:right;">
1248084
</td>
<td style="text-align:right;">
688835
</td>
<td style="text-align:right;">
200220
</td>
<td style="text-align:left;">
KY Division of Emergency Management
</td>
<td style="text-align:right;">
1718
</td>
<td style="text-align:left;">
Franklin County
</td>
<td style="text-align:left;">
21073
</td>
<td style="text-align:right;">
20835364.4
</td>
<td style="text-align:right;">
20835364.4
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public Road
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1305964.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-08-08
</td>
<td style="text-align:left;">
2022-09-06
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Kentucky
</td>
<td style="text-align:right;">
44405
</td>
<td style="text-align:right;">
1248091
</td>
<td style="text-align:right;">
688844
</td>
<td style="text-align:right;">
200220
</td>
<td style="text-align:left;">
KY Division of Emergency Management
</td>
<td style="text-align:right;">
1718
</td>
<td style="text-align:left;">
Franklin County
</td>
<td style="text-align:left;">
21073
</td>
<td style="text-align:right;">
11305017.6
</td>
<td style="text-align:right;">
11305017.6
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Waterway
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
315639.3600
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2022-08-08
</td>
<td style="text-align:left;">
2022-09-06
</td>
</tr>
<tr>
<td style="text-align:right;">
4476
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:left;">
2020-03-05
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Tennessee
</td>
<td style="text-align:right;">
33378
</td>
<td style="text-align:right;">
377703
</td>
<td style="text-align:right;">
136382
</td>
<td style="text-align:right;">
21710
</td>
<td style="text-align:left;">
Tennessee Department of Military/TEMA
</td>
<td style="text-align:right;">
265
</td>
<td style="text-align:left;">
Davidson County
</td>
<td style="text-align:left;">
47037
</td>
<td style="text-align:right;">
614317.3
</td>
<td style="text-align:right;">
614317.3
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Public Road
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
</td>
<td style="text-align:right;">
3131.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2020-03-03
</td>
<td style="text-align:left;">
2020-03-29
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32324
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
4000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32323
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32325
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
7000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32322
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2800.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32321
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
7000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32326
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
22600.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32320
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1100.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32327
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
20400.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32318
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32317
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
8000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32316
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9500.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32315
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32314
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
20000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32328
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32313
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
17000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32330
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3500.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32312
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
25000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
33386
</td>
<td style="text-align:right;">
418197
</td>
<td style="text-align:right;">
161938
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
5056100.1
</td>
<td style="text-align:right;">
5056100.1
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1346068.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
2020-09-21
</td>
<td style="text-align:left;">
2020-12-21
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
33003
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
500.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32329
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9000.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
Severe Storm(s)
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Iowa
</td>
<td style="text-align:right;">
32311
</td>
<td style="text-align:right;">
405187
</td>
<td style="text-align:right;">
155915
</td>
<td style="text-align:right;">
48341
</td>
<td style="text-align:left;">
Iowa Department of Homeland Security and Emergency Management
</td>
<td style="text-align:right;">
451
</td>
<td style="text-align:left;">
Polk County
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:right;">
3443899.9
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
Other Public Property
</td>
<td style="text-align:left;">
Vegetative Debris
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1500.0000
</td>
<td style="text-align:left;">
Cubic Yard
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
NA
</td>
</tr>
</tbody>
</table>

</div>

## Veg-Debris

### The Vegetative Debris Data

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:650px; ">

<table class="table table-striped" style="color: black; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Disaster_num
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
FIPS
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Dec_Date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Num_Removals
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Num_Damages
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Num_Projects
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Gross_Cost
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Net_Cost
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Quantity
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
year
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Population
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Infrastructure_Value
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
RISK_SCORE
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
RESL_SCORE
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
disasterName
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
incidentType
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
region
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13021
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
59753.43
</td>
<td style="text-align:right;">
59753.43
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
157280
</td>
<td style="text-align:right;">
31029231200
</td>
<td style="text-align:right;">
76.42380
</td>
<td style="text-align:right;">
65.85
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13025
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
242878.98
</td>
<td style="text-align:right;">
242878.98
</td>
<td style="text-align:right;">
59825.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
17976
</td>
<td style="text-align:right;">
2506510997
</td>
<td style="text-align:right;">
57.46102
</td>
<td style="text-align:right;">
24.76
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13029
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
370
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1232186.75
</td>
<td style="text-align:right;">
1232186.75
</td>
<td style="text-align:right;">
156564.45
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
44426
</td>
<td style="text-align:right;">
6670531454
</td>
<td style="text-align:right;">
87.52784
</td>
<td style="text-align:right;">
78.64
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13031
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
2055807.22
</td>
<td style="text-align:right;">
1716018.52
</td>
<td style="text-align:right;">
176455.11
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
80942
</td>
<td style="text-align:right;">
14170658859
</td>
<td style="text-align:right;">
89.21413
</td>
<td style="text-align:right;">
47.96
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13039
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
525855.78
</td>
<td style="text-align:right;">
525855.78
</td>
<td style="text-align:right;">
6462364.82
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
54619
</td>
<td style="text-align:right;">
8633305371
</td>
<td style="text-align:right;">
85.30067
</td>
<td style="text-align:right;">
57.57
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13043
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
137094.82
</td>
<td style="text-align:right;">
137094.82
</td>
<td style="text-align:right;">
2038.40
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
10953
</td>
<td style="text-align:right;">
1814850368
</td>
<td style="text-align:right;">
40.18454
</td>
<td style="text-align:right;">
5.54
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13051
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
59
</td>
<td style="text-align:right;">
48
</td>
<td style="text-align:right;">
29
</td>
<td style="text-align:right;">
94891483.38
</td>
<td style="text-align:right;">
48112145.72
</td>
<td style="text-align:right;">
1941658.36
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
293520
</td>
<td style="text-align:right;">
56341879834
</td>
<td style="text-align:right;">
98.66370
</td>
<td style="text-align:right;">
90.77
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13103
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
483461.32
</td>
<td style="text-align:right;">
483461.32
</td>
<td style="text-align:right;">
18393.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
64648
</td>
<td style="text-align:right;">
9810531158
</td>
<td style="text-align:right;">
79.16004
</td>
<td style="text-align:right;">
62.13
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13109
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
311933.31
</td>
<td style="text-align:right;">
311933.31
</td>
<td style="text-align:right;">
49181.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
10729
</td>
<td style="text-align:right;">
2224696592
</td>
<td style="text-align:right;">
63.85619
</td>
<td style="text-align:right;">
29.09
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13121
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5540903.00
</td>
<td style="text-align:right;">
5540903.00
</td>
<td style="text-align:right;">
210105.62
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
1065539
</td>
<td style="text-align:right;">
231755056791
</td>
<td style="text-align:right;">
92.30035
</td>
<td style="text-align:right;">
55.28
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13127
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
30
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
17031721.60
</td>
<td style="text-align:right;">
11648106.88
</td>
<td style="text-align:right;">
675247.55
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
84424
</td>
<td style="text-align:right;">
16154990642
</td>
<td style="text-align:right;">
90.80496
</td>
<td style="text-align:right;">
83.99
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13165
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
21963.86
</td>
<td style="text-align:right;">
21963.86
</td>
<td style="text-align:right;">
207.44
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
8650
</td>
<td style="text-align:right;">
1504153897
</td>
<td style="text-align:right;">
28.57143
</td>
<td style="text-align:right;">
11.04
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13179
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2412210.84
</td>
<td style="text-align:right;">
2412210.84
</td>
<td style="text-align:right;">
315013.07
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
65134
</td>
<td style="text-align:right;">
12052551996
</td>
<td style="text-align:right;">
94.11390
</td>
<td style="text-align:right;">
50.48
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13183
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
212102.25
</td>
<td style="text-align:right;">
212102.25
</td>
<td style="text-align:right;">
7388.50
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
16069
</td>
<td style="text-align:right;">
1885031282
</td>
<td style="text-align:right;">
56.47471
</td>
<td style="text-align:right;">
6.91
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13191
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1789692.63
</td>
<td style="text-align:right;">
1789692.63
</td>
<td style="text-align:right;">
80688.62
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
10934
</td>
<td style="text-align:right;">
2539384376
</td>
<td style="text-align:right;">
79.28731
</td>
<td style="text-align:right;">
45.93
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13229
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
134919.75
</td>
<td style="text-align:right;">
134919.75
</td>
<td style="text-align:right;">
7012.72
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
19713
</td>
<td style="text-align:right;">
3511888999
</td>
<td style="text-align:right;">
70.15590
</td>
<td style="text-align:right;">
39.24
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13247
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
101
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
19160893.85
</td>
<td style="text-align:right;">
1980813.65
</td>
<td style="text-align:right;">
173142.70
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
93392
</td>
<td style="text-align:right;">
17184906899
</td>
<td style="text-align:right;">
63.88801
</td>
<td style="text-align:right;">
44.08
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13251
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
204294.87
</td>
<td style="text-align:right;">
204294.87
</td>
<td style="text-align:right;">
101575.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
13974
</td>
<td style="text-align:right;">
2746722077
</td>
<td style="text-align:right;">
67.99236
</td>
<td style="text-align:right;">
29.54
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13267
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
11
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1660602.41
</td>
<td style="text-align:right;">
916350.53
</td>
<td style="text-align:right;">
140426.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
22744
</td>
<td style="text-align:right;">
4622910204
</td>
<td style="text-align:right;">
83.32803
</td>
<td style="text-align:right;">
11.81
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13277
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
6479.00
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
41296
</td>
<td style="text-align:right;">
10042961707
</td>
<td style="text-align:right;">
73.84664
</td>
<td style="text-align:right;">
42.58
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
</tbody>
</table>

</div>

### Distribution of Costs per Cubic Yard

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ##     3.58    19.70    27.09   236.10    43.93 80868.80

Minimum, Central Tendency and Quartile Points. Median Cost is at
**\$27.09** dollars. The Mean is *skewed* by extremely high cost
projects. This is also why we log (exponentially) scale quantity and
cost in most of the analysis.

![](/images/unnamed-chunk-8-1.png)<!-- --> Histogram showing the
frequency of projects around certain cost per cubic yards. **Region 4**
looks very similar to the others.

#### Chart of Cost against Quantity

![](/images/unnamed-chunk-9-1.png)<!-- -->

Notes: - This is on log-log scales, so major tick lines increase by
powers of 10: - This allows us to again confirm that **Region 4 is
similar to other regions**. - We can start to see an approximately
linear relationship between cost and quantity. - There are some outliers
we need to remove: - Low values for Cost/Quantity (probably incorrect
info, or not entered info)

#### Chart with Regression and Line Relationships

![](/images/unnamed-chunk-10-1.png)<!-- -->

Quite the chart! First, R4 is not separated out (it would be too
confusing and we don’t need to anyway). - What I find interesting here,
is the evidence for a changing relationship between **Cost** and
**Quantity**. - If we use a multiplier like
$25 (1 CY of debris cost 25)$ we would have a single straight line, but
it wouldn’t fit well at the beginning or end of the chart.

- The line that fits on this chart (green line) is actually a log-log
  line assuming an equal scaling relationship (slope =1) between the
  variables. This is actually a scaling relationship that looks straight
  because we transformed *Cost* and *Quantity* with a log10 function. -I
  adjusted the intercept (“base cost”) of where the line starts to make
  it fit close to the scattered data cloud.
- Note: the Orange line will be used to filter out the extremely large
  debris removal amounts.

![](/images/unnamed-chunk-11-1.png)<!-- --> One more chart: - The blue
line is a regression relationship. - The long green line is one that I
fit “by eye”.

### Fitting a Regression Line

- The previous lines were fit “by eye”.
- We can use regression to find *slope* and *intercept* for the
  relationship.

``` r
veg_model <- 
veg_pred %>% 
  filter(Quantity > 10,
         Quantity < 1e07,
         Gross_Cost > 500,
         Gross_Cost < 3.1e09) %>% 
  lm(formula = log10(Gross_Cost) ~ log10(Quantity))

summary(veg_model)
```

    ## 
    ## Call:
    ## lm(formula = log10(Gross_Cost) ~ log10(Quantity), data = .)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.5159 -0.3614 -0.0335  0.3361  2.7097 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)      2.58276    0.04412   58.55   <2e-16 ***
    ## log10(Quantity)  0.73076    0.01148   63.64   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6577 on 2401 degrees of freedom
    ## Multiple R-squared:  0.6278, Adjusted R-squared:  0.6277 
    ## F-statistic:  4051 on 1 and 2401 DF,  p-value: < 2.2e-16

The model is significant, with a reasonable R^2 fit. We can then turn
this model into a formula using the slope intercept transformation of
the regression coefficients. Note: these are log(10) transformed, but
the function below puts a given quantity back to a dollar amount.

``` r
veg_reg_formula <- function(Quantity){
  10^(log10(Quantity)*.73076 + 2.58276)
}
```

#### Example

``` r
dollar(veg_reg_formula(1000))
```

    ## [1] "$59,571.70"

#### Formula Explained

- Slope intercept formula converted to log-log format is:
- $log10(Cost) = intercept + slope * log10(Quantity)$
- $log10(Cost) ~= 2.583 + .731 * log10(1000)$
- $59,571.70 = 2.583 + .731 * 3$

Comparing this to the \$25 per CY, where we have - $1000*25 - 25,000$

We can build a comparison of the model based formula v. other formulas
later. There is also a possibility of adding in further correlated
variables as predictors to the model formula, such as: - average
(historical) project cost per county - infrastructure value per county,
population per county.

#### Correlated Predictors (For Possible Future Iterations)

- Not many promising predictors at the moment…

``` r
#glimpse(veg_pred.m)

cor.test(veg_pred.m$Gross_Cost, veg_pred.m$Quantity, method = "spearman")
```

    ## 
    ##  Spearman's rank correlation rho
    ## 
    ## data:  veg_pred.m$Gross_Cost and veg_pred.m$Quantity
    ## S = 446435302, p-value < 2.2e-16
    ## alternative hypothesis: true rho is not equal to 0
    ## sample estimates:
    ##       rho 
    ## 0.7568245

``` r
#rho=.75  #rank oreder corr

cor.test(log10(veg_pred.m$Gross_Cost), log10(veg_pred.m$Quantity))
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  log10(veg_pred.m$Gross_Cost) and log10(veg_pred.m$Quantity)
    ## t = 49.936, df = 2223, p-value < 2.2e-16
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.7069106 0.7461177
    ## sample estimates:
    ##       cor 
    ## 0.7271065

``` r
#r = .73
mean(veg_pred.m$Quantity)
```

    ## [1] 2064069

``` r
cor.test(scale(as.numeric(veg_pred.m$Population)), log(veg_pred.m$Quantity))
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  scale(as.numeric(veg_pred.m$Population)) and log(veg_pred.m$Quantity)
    ## t = 4.8078, df = 2223, p-value = 1.628e-06
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.06014428 0.14240093
    ## sample estimates:
    ##      cor 
    ## 0.101446

``` r
cor.test(veg_pred$RESL_SCORE, log(1+veg_pred$Quantity))
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  veg_pred$RESL_SCORE and log(1 + veg_pred$Quantity)
    ## t = 1.1465, df = 2375, p-value = 0.2517
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.01670020  0.06366419
    ## sample estimates:
    ##     cor 
    ## 0.02352

## Const-Debris

### The Data:

Here is the Grants Manager data for construction debris removals, again
aggregated to county level removals. If there are multiple projects
sites for a removal, they are gathered up so each row representes a
given Cat Work A - Sum of Quantity Removed and Sum of Cost of removal.

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:650px; ">

<table class="table table-striped" style="color: black; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Disaster_num
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
FIPS
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Dec_Date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Num_Removals
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Num_Damages
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Num_Projects
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Gross_Cost
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Net_Cost
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Quantity
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
year
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Population
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Infrastructure_Value
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
RISK_SCORE
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
RESL_SCORE
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
disasterName
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
incidentType
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
region
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
4284
</td>
<td style="text-align:left;">
13051
</td>
<td style="text-align:left;">
2016-10-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.705885e+06
</td>
<td style="text-align:right;">
3705885.12
</td>
<td style="text-align:right;">
5.040000e+02
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
293520
</td>
<td style="text-align:right;">
5.634188e+10
</td>
<td style="text-align:right;">
98.6636971
</td>
<td style="text-align:right;">
90.77
</td>
<td style="text-align:left;">
HURRICANE MATTHEW
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4344
</td>
<td style="text-align:left;">
06067
</td>
<td style="text-align:left;">
2017-10-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.887155e+06
</td>
<td style="text-align:right;">
2887155.27
</td>
<td style="text-align:right;">
1.579100e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1584652
</td>
<td style="text-align:right;">
2.637269e+11
</td>
<td style="text-align:right;">
97.6455616
</td>
<td style="text-align:right;">
65.53
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4344
</td>
<td style="text-align:left;">
06097
</td>
<td style="text-align:left;">
2017-10-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.068530e+03
</td>
<td style="text-align:right;">
4068.53
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
488330
</td>
<td style="text-align:right;">
1.046105e+11
</td>
<td style="text-align:right;">
99.1727649
</td>
<td style="text-align:right;">
72.98
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12001
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.910707e+06
</td>
<td style="text-align:right;">
3027397.24
</td>
<td style="text-align:right;">
1.429740e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
277984
</td>
<td style="text-align:right;">
4.273670e+10
</td>
<td style="text-align:right;">
93.8275533
</td>
<td style="text-align:right;">
74.44
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12007
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.966274e+04
</td>
<td style="text-align:right;">
59662.74
</td>
<td style="text-align:right;">
1.400000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
28233
</td>
<td style="text-align:right;">
3.560269e+09
</td>
<td style="text-align:right;">
70.2195355
</td>
<td style="text-align:right;">
28.39
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12009
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
3.529646e+06
</td>
<td style="text-align:right;">
3529646.08
</td>
<td style="text-align:right;">
1.135205e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
606067
</td>
<td style="text-align:right;">
1.055946e+11
</td>
<td style="text-align:right;">
99.3636653
</td>
<td style="text-align:right;">
59.83
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12011
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
2.210745e+07
</td>
<td style="text-align:right;">
22107450.30
</td>
<td style="text-align:right;">
4.353217e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1940829
</td>
<td style="text-align:right;">
2.681728e+11
</td>
<td style="text-align:right;">
99.7454661
</td>
<td style="text-align:right;">
44.84
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12019
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
8.039765e+06
</td>
<td style="text-align:right;">
7292302.87
</td>
<td style="text-align:right;">
1.303454e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
218078
</td>
<td style="text-align:right;">
2.936411e+10
</td>
<td style="text-align:right;">
88.7686923
</td>
<td style="text-align:right;">
57.67
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12021
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
6.718732e+05
</td>
<td style="text-align:right;">
569055.77
</td>
<td style="text-align:right;">
2.136282e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
375443
</td>
<td style="text-align:right;">
8.254240e+10
</td>
<td style="text-align:right;">
98.8545975
</td>
<td style="text-align:right;">
20.15
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12027
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.437593e+06
</td>
<td style="text-align:right;">
1437593.03
</td>
<td style="text-align:right;">
1.647947e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
33898
</td>
<td style="text-align:right;">
5.358888e+09
</td>
<td style="text-align:right;">
90.4231626
</td>
<td style="text-align:right;">
1.11
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12031
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1.364659e+06
</td>
<td style="text-align:right;">
1364659.15
</td>
<td style="text-align:right;">
2.211878e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
994675
</td>
<td style="text-align:right;">
1.584862e+11
</td>
<td style="text-align:right;">
97.8364620
</td>
<td style="text-align:right;">
67.19
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12035
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1.707548e+06
</td>
<td style="text-align:right;">
1707547.73
</td>
<td style="text-align:right;">
4.198133e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
115270
</td>
<td style="text-align:right;">
1.705646e+10
</td>
<td style="text-align:right;">
88.3550748
</td>
<td style="text-align:right;">
33.13
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12043
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.911640e+03
</td>
<td style="text-align:right;">
5911.64
</td>
<td style="text-align:right;">
5.667340e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
12052
</td>
<td style="text-align:right;">
2.247458e+09
</td>
<td style="text-align:right;">
74.9920458
</td>
<td style="text-align:right;">
0.70
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12051
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.305388e+05
</td>
<td style="text-align:right;">
430538.76
</td>
<td style="text-align:right;">
4.670000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
39371
</td>
<td style="text-align:right;">
6.080710e+09
</td>
<td style="text-align:right;">
91.1549475
</td>
<td style="text-align:right;">
0.89
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12055
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.340932e+07
</td>
<td style="text-align:right;">
13409317.59
</td>
<td style="text-align:right;">
5.263260e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
101136
</td>
<td style="text-align:right;">
1.753236e+10
</td>
<td style="text-align:right;">
90.2958956
</td>
<td style="text-align:right;">
4.81
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12057
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.834040e+03
</td>
<td style="text-align:right;">
4834.04
</td>
<td style="text-align:right;">
2.200000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1458282
</td>
<td style="text-align:right;">
2.038126e+11
</td>
<td style="text-align:right;">
99.5545657
</td>
<td style="text-align:right;">
40.90
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12061
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.078071e+05
</td>
<td style="text-align:right;">
307807.07
</td>
<td style="text-align:right;">
4.844200e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
159719
</td>
<td style="text-align:right;">
2.862201e+10
</td>
<td style="text-align:right;">
97.0728603
</td>
<td style="text-align:right;">
49.55
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12069
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.367834e+06
</td>
<td style="text-align:right;">
6367834.33
</td>
<td style="text-align:right;">
4.258000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
383042
</td>
<td style="text-align:right;">
5.896930e+10
</td>
<td style="text-align:right;">
95.2274897
</td>
<td style="text-align:right;">
48.09
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12071
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
1.992298e+07
</td>
<td style="text-align:right;">
19922980.08
</td>
<td style="text-align:right;">
3.560657e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
759922
</td>
<td style="text-align:right;">
1.233084e+11
</td>
<td style="text-align:right;">
99.4909322
</td>
<td style="text-align:right;">
9.17
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
3.198412e+07
</td>
<td style="text-align:right;">
31984115.95
</td>
<td style="text-align:right;">
1.301668e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
292157
</td>
<td style="text-align:right;">
4.688315e+10
</td>
<td style="text-align:right;">
94.5593382
</td>
<td style="text-align:right;">
74.76
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12081
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.173623e+07
</td>
<td style="text-align:right;">
11736228.42
</td>
<td style="text-align:right;">
8.925600e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
399485
</td>
<td style="text-align:right;">
6.672028e+10
</td>
<td style="text-align:right;">
98.4409800
</td>
<td style="text-align:right;">
23.90
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12083
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.118972e+05
</td>
<td style="text-align:right;">
55948.60
</td>
<td style="text-align:right;">
1.316000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
375566
</td>
<td style="text-align:right;">
5.623256e+10
</td>
<td style="text-align:right;">
96.2456252
</td>
<td style="text-align:right;">
20.05
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12086
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
28
</td>
<td style="text-align:right;">
25
</td>
<td style="text-align:right;">
24
</td>
<td style="text-align:right;">
2.112189e+07
</td>
<td style="text-align:right;">
21082923.83
</td>
<td style="text-align:right;">
1.263430e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
2698679
</td>
<td style="text-align:right;">
3.195744e+11
</td>
<td style="text-align:right;">
99.8090996
</td>
<td style="text-align:right;">
37.11
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12087
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
6.142769e+07
</td>
<td style="text-align:right;">
46879999.20
</td>
<td style="text-align:right;">
8.578856e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
82390
</td>
<td style="text-align:right;">
1.808080e+10
</td>
<td style="text-align:right;">
95.9274578
</td>
<td style="text-align:right;">
27.88
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12089
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.162066e+06
</td>
<td style="text-align:right;">
3162065.57
</td>
<td style="text-align:right;">
2.345950e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
90123
</td>
<td style="text-align:right;">
1.418695e+10
</td>
<td style="text-align:right;">
83.0734967
</td>
<td style="text-align:right;">
70.78
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12093
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.699323e+06
</td>
<td style="text-align:right;">
1699323.41
</td>
<td style="text-align:right;">
1.455700e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
39455
</td>
<td style="text-align:right;">
6.060952e+09
</td>
<td style="text-align:right;">
87.5914731
</td>
<td style="text-align:right;">
2.42
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12095
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.278189e+05
</td>
<td style="text-align:right;">
427818.87
</td>
<td style="text-align:right;">
4.963960e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1428790
</td>
<td style="text-align:right;">
2.324704e+11
</td>
<td style="text-align:right;">
98.5682469
</td>
<td style="text-align:right;">
43.92
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12097
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3.820086e+05
</td>
<td style="text-align:right;">
382008.61
</td>
<td style="text-align:right;">
1.861718e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
387795
</td>
<td style="text-align:right;">
5.045786e+10
</td>
<td style="text-align:right;">
95.1320395
</td>
<td style="text-align:right;">
35.74
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12099
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2.098244e+06
</td>
<td style="text-align:right;">
2098244.25
</td>
<td style="text-align:right;">
1.612295e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1489461
</td>
<td style="text-align:right;">
2.385678e+11
</td>
<td style="text-align:right;">
99.7136494
</td>
<td style="text-align:right;">
23.65
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12101
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.484174e+04
</td>
<td style="text-align:right;">
14841.74
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
561566
</td>
<td style="text-align:right;">
8.980181e+10
</td>
<td style="text-align:right;">
99.0454979
</td>
<td style="text-align:right;">
13.21
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12103
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
2.622531e+07
</td>
<td style="text-align:right;">
14089979.61
</td>
<td style="text-align:right;">
1.243350e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
958822
</td>
<td style="text-align:right;">
1.452981e+11
</td>
<td style="text-align:right;">
99.2045816
</td>
<td style="text-align:right;">
12.00
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12105
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.379514e+07
</td>
<td style="text-align:right;">
13795142.37
</td>
<td style="text-align:right;">
6.649810e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
721918
</td>
<td style="text-align:right;">
1.007445e+11
</td>
<td style="text-align:right;">
98.0273624
</td>
<td style="text-align:right;">
30.36
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12107
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.211588e+06
</td>
<td style="text-align:right;">
3211587.80
</td>
<td style="text-align:right;">
3.044940e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
73208
</td>
<td style="text-align:right;">
1.125835e+10
</td>
<td style="text-align:right;">
90.1686287
</td>
<td style="text-align:right;">
14.23
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12109
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1.758281e+06
</td>
<td style="text-align:right;">
1524340.54
</td>
<td style="text-align:right;">
1.446476e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
273175
</td>
<td style="text-align:right;">
5.135085e+10
</td>
<td style="text-align:right;">
94.1457206
</td>
<td style="text-align:right;">
81.99
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12115
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.948444e+04
</td>
<td style="text-align:right;">
69484.44
</td>
<td style="text-align:right;">
2.750000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
433908
</td>
<td style="text-align:right;">
8.064207e+10
</td>
<td style="text-align:right;">
98.4091632
</td>
<td style="text-align:right;">
9.23
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12117
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.372560e+06
</td>
<td style="text-align:right;">
1081807.04
</td>
<td style="text-align:right;">
1.974345e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
470586
</td>
<td style="text-align:right;">
7.253923e+10
</td>
<td style="text-align:right;">
95.6411072
</td>
<td style="text-align:right;">
64.58
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4337
</td>
<td style="text-align:left;">
12127
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
1.472103e+07
</td>
<td style="text-align:right;">
14721033.87
</td>
<td style="text-align:right;">
9.080085e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
551829
</td>
<td style="text-align:right;">
8.247022e+10
</td>
<td style="text-align:right;">
97.5182946
</td>
<td style="text-align:right;">
45.64
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4338
</td>
<td style="text-align:left;">
13039
</td>
<td style="text-align:left;">
2017-09-16
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.249740e+06
</td>
<td style="text-align:right;">
1249740.45
</td>
<td style="text-align:right;">
5.001874e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
54619
</td>
<td style="text-align:right;">
8.633305e+09
</td>
<td style="text-align:right;">
85.3006682
</td>
<td style="text-align:right;">
57.57
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4297
</td>
<td style="text-align:left;">
13081
</td>
<td style="text-align:left;">
2017-01-26
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.829343e+04
</td>
<td style="text-align:right;">
28293.43
</td>
<td style="text-align:right;">
5.210000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
20073
</td>
<td style="text-align:right;">
4.970131e+09
</td>
<td style="text-align:right;">
61.1199491
</td>
<td style="text-align:right;">
20.75
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4294
</td>
<td style="text-align:left;">
13095
</td>
<td style="text-align:left;">
2017-01-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.084616e+06
</td>
<td style="text-align:right;">
7084615.96
</td>
<td style="text-align:right;">
1.281275e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
85747
</td>
<td style="text-align:right;">
1.924929e+10
</td>
<td style="text-align:right;">
79.5100223
</td>
<td style="text-align:right;">
54.68
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4297
</td>
<td style="text-align:left;">
13095
</td>
<td style="text-align:left;">
2017-01-26
</td>
<td style="text-align:right;">
4677
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
4.188572e+08
</td>
<td style="text-align:right;">
21196961.65
</td>
<td style="text-align:right;">
1.196095e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
85747
</td>
<td style="text-align:right;">
1.924929e+10
</td>
<td style="text-align:right;">
79.5100223
</td>
<td style="text-align:right;">
54.68
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4338
</td>
<td style="text-align:left;">
13127
</td>
<td style="text-align:left;">
2017-09-16
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
7.850670e+06
</td>
<td style="text-align:right;">
7850670.10
</td>
<td style="text-align:right;">
6.964770e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
84424
</td>
<td style="text-align:right;">
1.615499e+10
</td>
<td style="text-align:right;">
90.8049634
</td>
<td style="text-align:right;">
83.99
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4350
</td>
<td style="text-align:left;">
28047
</td>
<td style="text-align:left;">
2017-11-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.444791e+04
</td>
<td style="text-align:right;">
24447.91
</td>
<td style="text-align:right;">
7.190000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
208275
</td>
<td style="text-align:right;">
3.732934e+10
</td>
<td style="text-align:right;">
97.2001273
</td>
<td style="text-align:right;">
67.41
</td>
<td style="text-align:left;">
HURRICANE NATE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4317
</td>
<td style="text-align:left;">
29051
</td>
<td style="text-align:left;">
2017-06-02
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.968734e+04
</td>
<td style="text-align:right;">
39687.34
</td>
<td style="text-align:right;">
1.950000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
76989
</td>
<td style="text-align:right;">
1.757678e+10
</td>
<td style="text-align:right;">
74.6102450
</td>
<td style="text-align:right;">
90.67
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4317
</td>
<td style="text-align:left;">
29091
</td>
<td style="text-align:left;">
2017-06-02
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.737840e+05
</td>
<td style="text-align:right;">
86892.00
</td>
<td style="text-align:right;">
1.100000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
39672
</td>
<td style="text-align:right;">
1.001590e+10
</td>
<td style="text-align:right;">
81.7690105
</td>
<td style="text-align:right;">
12.09
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4317
</td>
<td style="text-align:left;">
29099
</td>
<td style="text-align:left;">
2017-06-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.309160e+03
</td>
<td style="text-align:right;">
3309.16
</td>
<td style="text-align:right;">
1.754891e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
226703
</td>
<td style="text-align:right;">
3.437278e+10
</td>
<td style="text-align:right;">
87.2096723
</td>
<td style="text-align:right;">
74.86
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4317
</td>
<td style="text-align:left;">
29149
</td>
<td style="text-align:left;">
2017-06-02
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.666061e+05
</td>
<td style="text-align:right;">
166606.08
</td>
<td style="text-align:right;">
1.323305e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
8635
</td>
<td style="text-align:right;">
2.416828e+09
</td>
<td style="text-align:right;">
51.9885460
</td>
<td style="text-align:right;">
0.76
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4317
</td>
<td style="text-align:left;">
29189
</td>
<td style="text-align:left;">
2017-06-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.446020e+03
</td>
<td style="text-align:right;">
1446.02
</td>
<td style="text-align:right;">
8.000000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1003734
</td>
<td style="text-align:right;">
2.351181e+11
</td>
<td style="text-align:right;">
98.5364302
</td>
<td style="text-align:right;">
77.66
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4329
</td>
<td style="text-align:left;">
33009
</td>
<td style="text-align:left;">
2017-08-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.709950e+03
</td>
<td style="text-align:right;">
3709.95
</td>
<td style="text-align:right;">
2.220000e+00
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
91072
</td>
<td style="text-align:right;">
2.025017e+10
</td>
<td style="text-align:right;">
66.4015272
</td>
<td style="text-align:right;">
66.36
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4346
</td>
<td style="text-align:left;">
45013
</td>
<td style="text-align:left;">
2017-10-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.853906e+05
</td>
<td style="text-align:right;">
285390.56
</td>
<td style="text-align:right;">
8.880000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
186606
</td>
<td style="text-align:right;">
4.113781e+10
</td>
<td style="text-align:right;">
98.3455297
</td>
<td style="text-align:right;">
64.10
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4346
</td>
<td style="text-align:left;">
45019
</td>
<td style="text-align:left;">
2017-10-16
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.399184e+06
</td>
<td style="text-align:right;">
2399183.76
</td>
<td style="text-align:right;">
3.465800e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
407722
</td>
<td style="text-align:right;">
8.599560e+10
</td>
<td style="text-align:right;">
99.4591155
</td>
<td style="text-align:right;">
92.71
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48007
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.847465e+06
</td>
<td style="text-align:right;">
2847465.32
</td>
<td style="text-align:right;">
1.187878e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
23703
</td>
<td style="text-align:right;">
5.141210e+09
</td>
<td style="text-align:right;">
84.5052498
</td>
<td style="text-align:right;">
46.34
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48039
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
8.219233e+06
</td>
<td style="text-align:right;">
8191287.35
</td>
<td style="text-align:right;">
1.750138e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
371474
</td>
<td style="text-align:right;">
5.751482e+10
</td>
<td style="text-align:right;">
99.1091314
</td>
<td style="text-align:right;">
71.71
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48057
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.777138e+05
</td>
<td style="text-align:right;">
377713.75
</td>
<td style="text-align:right;">
6.056420e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
20057
</td>
<td style="text-align:right;">
4.802417e+09
</td>
<td style="text-align:right;">
85.1415845
</td>
<td style="text-align:right;">
75.43
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48071
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.157696e+05
</td>
<td style="text-align:right;">
115769.57
</td>
<td style="text-align:right;">
5.000000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
46507
</td>
<td style="text-align:right;">
1.201820e+10
</td>
<td style="text-align:right;">
89.7550111
</td>
<td style="text-align:right;">
91.88
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48089
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.358339e+05
</td>
<td style="text-align:right;">
135833.94
</td>
<td style="text-align:right;">
1.558000e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
20532
</td>
<td style="text-align:right;">
6.709205e+09
</td>
<td style="text-align:right;">
69.4877506
</td>
<td style="text-align:right;">
45.07
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48091
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.217175e+04
</td>
<td style="text-align:right;">
32171.75
</td>
<td style="text-align:right;">
1.720000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
161300
</td>
<td style="text-align:right;">
3.132962e+10
</td>
<td style="text-align:right;">
90.3595291
</td>
<td style="text-align:right;">
76.35
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48157
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.123830e+05
</td>
<td style="text-align:right;">
212382.96
</td>
<td style="text-align:right;">
3.083200e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
822581
</td>
<td style="text-align:right;">
1.389588e+11
</td>
<td style="text-align:right;">
99.0773147
</td>
<td style="text-align:right;">
56.43
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48167
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
11
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2.563490e+07
</td>
<td style="text-align:right;">
15840931.97
</td>
<td style="text-align:right;">
7.932393e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
350477
</td>
<td style="text-align:right;">
5.674698e+10
</td>
<td style="text-align:right;">
99.5227490
</td>
<td style="text-align:right;">
90.36
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48199
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
7.272911e+05
</td>
<td style="text-align:right;">
727291.13
</td>
<td style="text-align:right;">
7.928390e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
56129
</td>
<td style="text-align:right;">
1.037468e+10
</td>
<td style="text-align:right;">
83.8052816
</td>
<td style="text-align:right;">
46.50
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48201
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
4.383438e+07
</td>
<td style="text-align:right;">
43834377.28
</td>
<td style="text-align:right;">
1.439547e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
4726200
</td>
<td style="text-align:right;">
7.472565e+11
</td>
<td style="text-align:right;">
99.9681833
</td>
<td style="text-align:right;">
12.73
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48239
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.656425e+04
</td>
<td style="text-align:right;">
66564.25
</td>
<td style="text-align:right;">
7.638000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
14955
</td>
<td style="text-align:right;">
6.581488e+09
</td>
<td style="text-align:right;">
88.0369074
</td>
<td style="text-align:right;">
38.03
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48245
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1.557740e+07
</td>
<td style="text-align:right;">
15568113.41
</td>
<td style="text-align:right;">
1.105035e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
256386
</td>
<td style="text-align:right;">
3.854901e+10
</td>
<td style="text-align:right;">
97.7410118
</td>
<td style="text-align:right;">
68.01
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48287
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.533230e+03
</td>
<td style="text-align:right;">
6533.23
</td>
<td style="text-align:right;">
2.700000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
17463
</td>
<td style="text-align:right;">
3.773540e+09
</td>
<td style="text-align:right;">
38.9436844
</td>
<td style="text-align:right;">
40.67
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48291
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.152299e+04
</td>
<td style="text-align:right;">
41522.99
</td>
<td style="text-align:right;">
1.603400e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
91492
</td>
<td style="text-align:right;">
1.026688e+10
</td>
<td style="text-align:right;">
91.6321985
</td>
<td style="text-align:right;">
25.53
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48321
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3.178575e+05
</td>
<td style="text-align:right;">
317857.49
</td>
<td style="text-align:right;">
3.274200e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
36214
</td>
<td style="text-align:right;">
6.666911e+09
</td>
<td style="text-align:right;">
89.7868279
</td>
<td style="text-align:right;">
44.97
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48339
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.221158e+05
</td>
<td style="text-align:right;">
222115.80
</td>
<td style="text-align:right;">
3.736000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
620016
</td>
<td style="text-align:right;">
9.583079e+10
</td>
<td style="text-align:right;">
96.7228762
</td>
<td style="text-align:right;">
38.57
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48351
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.996190e+05
</td>
<td style="text-align:right;">
199618.97
</td>
<td style="text-align:right;">
8.824000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
12151
</td>
<td style="text-align:right;">
2.050022e+09
</td>
<td style="text-align:right;">
47.8205536
</td>
<td style="text-align:right;">
3.72
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48355
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1.273601e+08
</td>
<td style="text-align:right;">
43605609.19
</td>
<td style="text-align:right;">
1.611233e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
352623
</td>
<td style="text-align:right;">
5.585192e+10
</td>
<td style="text-align:right;">
97.5501114
</td>
<td style="text-align:right;">
76.70
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48361
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
5.844348e+07
</td>
<td style="text-align:right;">
29451483.35
</td>
<td style="text-align:right;">
1.711566e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
84645
</td>
<td style="text-align:right;">
1.477127e+10
</td>
<td style="text-align:right;">
96.5637926
</td>
<td style="text-align:right;">
63.75
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48373
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.586066e+04
</td>
<td style="text-align:right;">
65860.66
</td>
<td style="text-align:right;">
3.271700e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
49569
</td>
<td style="text-align:right;">
7.639450e+09
</td>
<td style="text-align:right;">
82.2144448
</td>
<td style="text-align:right;">
18.08
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48391
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.233052e+05
</td>
<td style="text-align:right;">
17939.78
</td>
<td style="text-align:right;">
7.000000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
6741
</td>
<td style="text-align:right;">
1.518277e+09
</td>
<td style="text-align:right;">
63.5062043
</td>
<td style="text-align:right;">
33.45
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48407
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.192560e+04
</td>
<td style="text-align:right;">
11925.60
</td>
<td style="text-align:right;">
8.800000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
27373
</td>
<td style="text-align:right;">
4.114451e+09
</td>
<td style="text-align:right;">
76.3919822
</td>
<td style="text-align:right;">
8.88
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48409
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
24
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1.018784e+08
</td>
<td style="text-align:right;">
36331134.66
</td>
<td style="text-align:right;">
1.297303e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
68653
</td>
<td style="text-align:right;">
1.095620e+10
</td>
<td style="text-align:right;">
89.1504932
</td>
<td style="text-align:right;">
46.56
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48453
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.087443e+07
</td>
<td style="text-align:right;">
18494026.05
</td>
<td style="text-align:right;">
6.116253e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
1285769
</td>
<td style="text-align:right;">
1.895386e+11
</td>
<td style="text-align:right;">
96.8819599
</td>
<td style="text-align:right;">
56.75
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48469
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.734620e+06
</td>
<td style="text-align:right;">
7734619.61
</td>
<td style="text-align:right;">
2.371645e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
91218
</td>
<td style="text-align:right;">
1.636339e+10
</td>
<td style="text-align:right;">
91.9503659
</td>
<td style="text-align:right;">
47.14
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48471
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.318225e+04
</td>
<td style="text-align:right;">
13182.25
</td>
<td style="text-align:right;">
2.692136e+06
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
76299
</td>
<td style="text-align:right;">
1.014816e+10
</td>
<td style="text-align:right;">
80.4963411
</td>
<td style="text-align:right;">
19.29
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48473
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.076687e+04
</td>
<td style="text-align:right;">
80766.87
</td>
<td style="text-align:right;">
1.158000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
56764
</td>
<td style="text-align:right;">
1.240359e+10
</td>
<td style="text-align:right;">
79.7645562
</td>
<td style="text-align:right;">
36.89
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4332
</td>
<td style="text-align:left;">
48481
</td>
<td style="text-align:left;">
2017-08-25
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.031395e+06
</td>
<td style="text-align:right;">
2056091.56
</td>
<td style="text-align:right;">
1.860052e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
41544
</td>
<td style="text-align:right;">
8.099223e+09
</td>
<td style="text-align:right;">
89.8822781
</td>
<td style="text-align:right;">
44.53
</td>
<td style="text-align:left;">
HURRICANE HARVEY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4343
</td>
<td style="text-align:left;">
55023
</td>
<td style="text-align:left;">
2017-10-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.067520e+03
</td>
<td style="text-align:right;">
2067.52
</td>
<td style="text-align:right;">
2.000000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
16088
</td>
<td style="text-align:right;">
7.987750e+09
</td>
<td style="text-align:right;">
66.1151766
</td>
<td style="text-align:right;">
72.69
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES, AND MUD
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4343
</td>
<td style="text-align:left;">
55043
</td>
<td style="text-align:left;">
2017-10-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.295321e+04
</td>
<td style="text-align:right;">
22953.21
</td>
<td style="text-align:right;">
1.972746e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
51299
</td>
<td style="text-align:right;">
1.162080e+10
</td>
<td style="text-align:right;">
61.7562838
</td>
<td style="text-align:right;">
68.87
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES, AND MUD
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4343
</td>
<td style="text-align:left;">
55123
</td>
<td style="text-align:left;">
2017-10-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.471690e+03
</td>
<td style="text-align:right;">
5471.69
</td>
<td style="text-align:right;">
1.200000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
30574
</td>
<td style="text-align:right;">
7.252761e+09
</td>
<td style="text-align:right;">
51.1294941
</td>
<td style="text-align:right;">
73.93
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES, AND MUD
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72003
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.117674e+07
</td>
<td style="text-align:right;">
11176744.73
</td>
<td style="text-align:right;">
2.311582e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
38125
</td>
<td style="text-align:right;">
4.957845e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72013
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.100000e+03
</td>
<td style="text-align:right;">
8100.00
</td>
<td style="text-align:right;">
1.598148e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
87581
</td>
<td style="text-align:right;">
1.190678e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72021
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.808207e+04
</td>
<td style="text-align:right;">
18082.07
</td>
<td style="text-align:right;">
1.230600e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
184904
</td>
<td style="text-align:right;">
2.888948e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72027
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.301887e+05
</td>
<td style="text-align:right;">
630188.73
</td>
<td style="text-align:right;">
1.266960e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
32797
</td>
<td style="text-align:right;">
3.642558e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72031
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
6.085016e+05
</td>
<td style="text-align:right;">
480450.04
</td>
<td style="text-align:right;">
5.396000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
154696
</td>
<td style="text-align:right;">
2.283049e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72045
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.115410e+05
</td>
<td style="text-align:right;">
111541.00
</td>
<td style="text-align:right;">
1.800000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
18883
</td>
<td style="text-align:right;">
1.969412e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72051
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.457270e+03
</td>
<td style="text-align:right;">
7457.27
</td>
<td style="text-align:right;">
3.500000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
35792
</td>
<td style="text-align:right;">
4.913842e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72057
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.091200e+03
</td>
<td style="text-align:right;">
4091.20
</td>
<td style="text-align:right;">
2.280000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
36538
</td>
<td style="text-align:right;">
5.954075e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72061
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.304567e+06
</td>
<td style="text-align:right;">
4304566.52
</td>
<td style="text-align:right;">
1.441217e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
89604
</td>
<td style="text-align:right;">
1.653781e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72065
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.216378e+05
</td>
<td style="text-align:right;">
109558.38
</td>
<td style="text-align:right;">
1.737360e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
38444
</td>
<td style="text-align:right;">
4.799716e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72075
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.799388e+04
</td>
<td style="text-align:right;">
27993.88
</td>
<td style="text-align:right;">
7.000000e+01
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
46316
</td>
<td style="text-align:right;">
4.988138e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4336
</td>
<td style="text-align:left;">
72077
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.529488e+05
</td>
<td style="text-align:right;">
152948.79
</td>
<td style="text-align:right;">
2.349000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
36808
</td>
<td style="text-align:right;">
4.359239e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72077
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.255565e+06
</td>
<td style="text-align:right;">
4255565.09
</td>
<td style="text-align:right;">
7.867000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
36808
</td>
<td style="text-align:right;">
4.359239e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72085
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.081998e+06
</td>
<td style="text-align:right;">
6081997.93
</td>
<td style="text-align:right;">
1.123299e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
35117
</td>
<td style="text-align:right;">
4.852675e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72093
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.902432e+06
</td>
<td style="text-align:right;">
3902432.07
</td>
<td style="text-align:right;">
4.015700e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
4733
</td>
<td style="text-align:right;">
5.949854e+08
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4336
</td>
<td style="text-align:left;">
72103
</td>
<td style="text-align:left;">
2017-09-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.336348e+05
</td>
<td style="text-align:right;">
233634.78
</td>
<td style="text-align:right;">
1.237900e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
23283
</td>
<td style="text-align:right;">
2.953017e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE IRMA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72103
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.970000e+04
</td>
<td style="text-align:right;">
29700.00
</td>
<td style="text-align:right;">
3.500000e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
23283
</td>
<td style="text-align:right;">
2.953017e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72105
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.434530e+06
</td>
<td style="text-align:right;">
1434529.99
</td>
<td style="text-align:right;">
3.230785e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
29241
</td>
<td style="text-align:right;">
2.934399e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72109
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.668247e+05
</td>
<td style="text-align:right;">
266824.67
</td>
<td style="text-align:right;">
5.347060e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
15929
</td>
<td style="text-align:right;">
2.009534e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72111
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.212950e+06
</td>
<td style="text-align:right;">
1212950.30
</td>
<td style="text-align:right;">
9.680060e+02
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
20320
</td>
<td style="text-align:right;">
2.518446e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72113
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
7.292549e+05
</td>
<td style="text-align:right;">
729254.89
</td>
<td style="text-align:right;">
2.246476e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
137316
</td>
<td style="text-align:right;">
2.102578e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72117
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.947540e+04
</td>
<td style="text-align:right;">
79475.40
</td>
<td style="text-align:right;">
1.572450e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
15187
</td>
<td style="text-align:right;">
2.056385e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72123
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.136379e+05
</td>
<td style="text-align:right;">
613637.94
</td>
<td style="text-align:right;">
7.337740e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
25742
</td>
<td style="text-align:right;">
3.835226e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72127
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
71
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
5.094905e+08
</td>
<td style="text-align:right;">
94516937.40
</td>
<td style="text-align:right;">
1.429344e+05
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
341981
</td>
<td style="text-align:right;">
6.618328e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72135
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.427339e+06
</td>
<td style="text-align:right;">
8427338.70
</td>
<td style="text-align:right;">
4.862000e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
66827
</td>
<td style="text-align:right;">
6.691613e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72151
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.692698e+06
</td>
<td style="text-align:right;">
1692698.04
</td>
<td style="text-align:right;">
2.060524e+04
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
30397
</td>
<td style="text-align:right;">
3.345120e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4339
</td>
<td style="text-align:left;">
72153
</td>
<td style="text-align:left;">
2017-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.313312e+05
</td>
<td style="text-align:right;">
631331.25
</td>
<td style="text-align:right;">
5.279580e+03
</td>
<td style="text-align:right;">
2017
</td>
<td style="text-align:left;">
34151
</td>
<td style="text-align:right;">
3.045488e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE MARIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4362
</td>
<td style="text-align:left;">
01015
</td>
<td style="text-align:left;">
2018-04-26
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5.949913e+06
</td>
<td style="text-align:right;">
5949913.22
</td>
<td style="text-align:right;">
1.064080e+06
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
116250
</td>
<td style="text-align:right;">
2.269150e+10
</td>
<td style="text-align:right;">
84.3779828
</td>
<td style="text-align:right;">
46.72
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4362
</td>
<td style="text-align:left;">
01055
</td>
<td style="text-align:left;">
2018-04-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.005575e+05
</td>
<td style="text-align:right;">
300557.48
</td>
<td style="text-align:right;">
2.800000e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
103320
</td>
<td style="text-align:right;">
1.900045e+10
</td>
<td style="text-align:right;">
82.2462615
</td>
<td style="text-align:right;">
53.66
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4406
</td>
<td style="text-align:left;">
01069
</td>
<td style="text-align:left;">
2018-11-05
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.046204e+06
</td>
<td style="text-align:right;">
5046204.31
</td>
<td style="text-align:right;">
7.605860e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
107125
</td>
<td style="text-align:right;">
2.376142e+10
</td>
<td style="text-align:right;">
90.6776965
</td>
<td style="text-align:right;">
64.00
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4362
</td>
<td style="text-align:left;">
01115
</td>
<td style="text-align:left;">
2018-04-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.067902e+05
</td>
<td style="text-align:right;">
306790.20
</td>
<td style="text-align:right;">
3.200000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
90899
</td>
<td style="text-align:right;">
1.311334e+10
</td>
<td style="text-align:right;">
70.7922367
</td>
<td style="text-align:right;">
48.50
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4407
</td>
<td style="text-align:left;">
06007
</td>
<td style="text-align:left;">
2018-11-12
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
5.845413e+05
</td>
<td style="text-align:right;">
455216.28
</td>
<td style="text-align:right;">
2.499960e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
211490
</td>
<td style="text-align:right;">
4.251521e+10
</td>
<td style="text-align:right;">
97.1683105
</td>
<td style="text-align:right;">
62.41
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4407
</td>
<td style="text-align:left;">
06037
</td>
<td style="text-align:left;">
2018-11-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.856366e+05
</td>
<td style="text-align:right;">
185636.61
</td>
<td style="text-align:right;">
4.628601e+01
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
10005712
</td>
<td style="text-align:right;">
1.517716e+12
</td>
<td style="text-align:right;">
100.0000000
</td>
<td style="text-align:right;">
19.67
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4382
</td>
<td style="text-align:left;">
06067
</td>
<td style="text-align:left;">
2018-08-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.605000e+07
</td>
<td style="text-align:right;">
66050000.00
</td>
<td style="text-align:right;">
9.840000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
1584652
</td>
<td style="text-align:right;">
2.637269e+11
</td>
<td style="text-align:right;">
97.6455616
</td>
<td style="text-align:right;">
65.53
</td>
<td style="text-align:left;">
WILDFIRES AND HIGH WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4407
</td>
<td style="text-align:left;">
06067
</td>
<td style="text-align:left;">
2018-11-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.380376e+07
</td>
<td style="text-align:right;">
23803762.44
</td>
<td style="text-align:right;">
2.871600e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
1584652
</td>
<td style="text-align:right;">
2.637269e+11
</td>
<td style="text-align:right;">
97.6455616
</td>
<td style="text-align:right;">
65.53
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4353
</td>
<td style="text-align:left;">
06083
</td>
<td style="text-align:left;">
2018-01-03
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.495064e+06
</td>
<td style="text-align:right;">
5495064.48
</td>
<td style="text-align:right;">
1.773000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
447998
</td>
<td style="text-align:right;">
7.845353e+10
</td>
<td style="text-align:right;">
99.3954820
</td>
<td style="text-align:right;">
47.93
</td>
<td style="text-align:left;">
WILDFIRES, FLOODING, MUDFLOWS, AND DEBRIS FLOWS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4382
</td>
<td style="text-align:left;">
06089
</td>
<td style="text-align:left;">
2018-08-05
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.360249e+04
</td>
<td style="text-align:right;">
73602.49
</td>
<td style="text-align:right;">
0.000000e+00
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
181918
</td>
<td style="text-align:right;">
3.728918e+10
</td>
<td style="text-align:right;">
95.2911231
</td>
<td style="text-align:right;">
50.95
</td>
<td style="text-align:left;">
WILDFIRES AND HIGH WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4353
</td>
<td style="text-align:left;">
06111
</td>
<td style="text-align:left;">
2018-01-03
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.917860e+05
</td>
<td style="text-align:right;">
591786.03
</td>
<td style="text-align:right;">
6.000000e+00
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
843136
</td>
<td style="text-align:right;">
1.513737e+11
</td>
<td style="text-align:right;">
99.4272988
</td>
<td style="text-align:right;">
47.04
</td>
<td style="text-align:left;">
WILDFIRES, FLOODING, MUDFLOWS, AND DEBRIS FLOWS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4407
</td>
<td style="text-align:left;">
06111
</td>
<td style="text-align:left;">
2018-11-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.299776e+05
</td>
<td style="text-align:right;">
229977.60
</td>
<td style="text-align:right;">
3.635760e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
843136
</td>
<td style="text-align:right;">
1.513737e+11
</td>
<td style="text-align:right;">
99.4272988
</td>
<td style="text-align:right;">
47.04
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12005
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
666
</td>
<td style="text-align:right;">
25
</td>
<td style="text-align:right;">
25
</td>
<td style="text-align:right;">
1.219259e+09
</td>
<td style="text-align:right;">
422451597\.80
</td>
<td style="text-align:right;">
7.065869e+06
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
174869
</td>
<td style="text-align:right;">
3.231735e+10
</td>
<td style="text-align:right;">
97.4864779
</td>
<td style="text-align:right;">
31.41
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12013
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.520674e+05
</td>
<td style="text-align:right;">
352067.43
</td>
<td style="text-align:right;">
2.986000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
13597
</td>
<td style="text-align:right;">
2.454658e+09
</td>
<td style="text-align:right;">
68.5332485
</td>
<td style="text-align:right;">
13.34
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12037
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.163034e+05
</td>
<td style="text-align:right;">
116303.39
</td>
<td style="text-align:right;">
4.480000e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
12426
</td>
<td style="text-align:right;">
2.829817e+09
</td>
<td style="text-align:right;">
70.6649698
</td>
<td style="text-align:right;">
28.55
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12039
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.011368e+04
</td>
<td style="text-align:right;">
10113.68
</td>
<td style="text-align:right;">
1.092592e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
43693
</td>
<td style="text-align:right;">
6.497843e+09
</td>
<td style="text-align:right;">
80.0509068
</td>
<td style="text-align:right;">
32.88
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12045
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.264253e+06
</td>
<td style="text-align:right;">
1264253.34
</td>
<td style="text-align:right;">
2.760000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
14176
</td>
<td style="text-align:right;">
2.743053e+09
</td>
<td style="text-align:right;">
76.2647152
</td>
<td style="text-align:right;">
43.22
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12063
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.075691e+07
</td>
<td style="text-align:right;">
10756905.86
</td>
<td style="text-align:right;">
4.307620e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
47249
</td>
<td style="text-align:right;">
8.012107e+09
</td>
<td style="text-align:right;">
85.8097359
</td>
<td style="text-align:right;">
20.91
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
116
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
3.972479e+08
</td>
<td style="text-align:right;">
267038661\.18
</td>
<td style="text-align:right;">
1.437230e+06
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
292157
</td>
<td style="text-align:right;">
4.688315e+10
</td>
<td style="text-align:right;">
94.5593382
</td>
<td style="text-align:right;">
74.76
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12123
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.833412e+04
</td>
<td style="text-align:right;">
78334.12
</td>
<td style="text-align:right;">
2.053206e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
21690
</td>
<td style="text-align:right;">
3.630806e+09
</td>
<td style="text-align:right;">
74.0057270
</td>
<td style="text-align:right;">
16.17
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4399
</td>
<td style="text-align:left;">
12133
</td>
<td style="text-align:left;">
2018-10-11
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0.000000e+00
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
9.500000e+01
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
25288
</td>
<td style="text-align:right;">
3.660356e+09
</td>
<td style="text-align:right;">
87.7187401
</td>
<td style="text-align:right;">
15.60
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13081
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.558951e+05
</td>
<td style="text-align:right;">
255895.10
</td>
<td style="text-align:right;">
6.533300e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
20073
</td>
<td style="text-align:right;">
4.970131e+09
</td>
<td style="text-align:right;">
61.1199491
</td>
<td style="text-align:right;">
20.75
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13087
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.251782e+06
</td>
<td style="text-align:right;">
6251782.29
</td>
<td style="text-align:right;">
1.231300e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
29305
</td>
<td style="text-align:right;">
5.487419e+09
</td>
<td style="text-align:right;">
80.7826917
</td>
<td style="text-align:right;">
18.36
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13095
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
7.658757e+06
</td>
<td style="text-align:right;">
7658757.34
</td>
<td style="text-align:right;">
4.982200e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
85747
</td>
<td style="text-align:right;">
1.924929e+10
</td>
<td style="text-align:right;">
79.5100223
</td>
<td style="text-align:right;">
54.68
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13099
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.034805e+05
</td>
<td style="text-align:right;">
203480.51
</td>
<td style="text-align:right;">
2.000000e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
10781
</td>
<td style="text-align:right;">
2.629068e+09
</td>
<td style="text-align:right;">
59.4018454
</td>
<td style="text-align:right;">
29.06
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13121
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.407570e+03
</td>
<td style="text-align:right;">
3407.57
</td>
<td style="text-align:right;">
5.000000e+00
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
1065539
</td>
<td style="text-align:right;">
2.317551e+11
</td>
<td style="text-align:right;">
92.3003500
</td>
<td style="text-align:right;">
55.28
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13201
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.250563e+05
</td>
<td style="text-align:right;">
125056.33
</td>
<td style="text-align:right;">
7.968000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
5989
</td>
<td style="text-align:right;">
1.442293e+09
</td>
<td style="text-align:right;">
45.5615654
</td>
<td style="text-align:right;">
25.08
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4400
</td>
<td style="text-align:left;">
13253
</td>
<td style="text-align:left;">
2018-10-14
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.209354e+05
</td>
<td style="text-align:right;">
220935.38
</td>
<td style="text-align:right;">
4.762900e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
9105
</td>
<td style="text-align:right;">
2.212101e+09
</td>
<td style="text-align:right;">
62.9971365
</td>
<td style="text-align:right;">
14.99
</td>
<td style="text-align:left;">
HURRICANE MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4365
</td>
<td style="text-align:left;">
15007
</td>
<td style="text-align:left;">
2018-05-08
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
9.381912e+05
</td>
<td style="text-align:right;">
938191.17
</td>
<td style="text-align:right;">
3.730000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
73136
</td>
<td style="text-align:right;">
1.453875e+10
</td>
<td style="text-align:right;">
74.4193446
</td>
<td style="text-align:right;">
84.85
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4363
</td>
<td style="text-align:left;">
18019
</td>
<td style="text-align:left;">
2018-05-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.492410e+03
</td>
<td style="text-align:right;">
6492.41
</td>
<td style="text-align:right;">
3.000000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
120950
</td>
<td style="text-align:right;">
2.158405e+10
</td>
<td style="text-align:right;">
81.9280942
</td>
<td style="text-align:right;">
76.16
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4363
</td>
<td style="text-align:left;">
18061
</td>
<td style="text-align:left;">
2018-05-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.732700e+03
</td>
<td style="text-align:right;">
3732.70
</td>
<td style="text-align:right;">
9.520000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
39636
</td>
<td style="text-align:right;">
7.284303e+09
</td>
<td style="text-align:right;">
60.1972638
</td>
<td style="text-align:right;">
45.77
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4363
</td>
<td style="text-align:left;">
18099
</td>
<td style="text-align:left;">
2018-05-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.291750e+03
</td>
<td style="text-align:right;">
5291.75
</td>
<td style="text-align:right;">
6.130000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
46062
</td>
<td style="text-align:right;">
1.017629e+10
</td>
<td style="text-align:right;">
42.5071588
</td>
<td style="text-align:right;">
70.59
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4386
</td>
<td style="text-align:left;">
19119
</td>
<td style="text-align:left;">
2018-08-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.327620e+03
</td>
<td style="text-align:right;">
6327.62
</td>
<td style="text-align:right;">
6.000000e+01
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
11934
</td>
<td style="text-align:right;">
3.533338e+09
</td>
<td style="text-align:right;">
34.2984410
</td>
<td style="text-align:right;">
85.42
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4392
</td>
<td style="text-align:left;">
19127
</td>
<td style="text-align:left;">
2018-09-12
</td>
<td style="text-align:right;">
4900
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.361042e+09
</td>
<td style="text-align:right;">
19443461.10
</td>
<td style="text-align:right;">
3.486432e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
40092
</td>
<td style="text-align:right;">
1.087537e+10
</td>
<td style="text-align:right;">
68.8514158
</td>
<td style="text-align:right;">
68.52
</td>
<td style="text-align:left;">
SEVERE STORM AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4386
</td>
<td style="text-align:left;">
19141
</td>
<td style="text-align:left;">
2018-08-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.628680e+04
</td>
<td style="text-align:right;">
26286.80
</td>
<td style="text-align:right;">
3.900000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
14182
</td>
<td style="text-align:right;">
4.720749e+09
</td>
<td style="text-align:right;">
67.4196627
</td>
<td style="text-align:right;">
95.80
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4386
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:left;">
2018-08-20
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
3.565789e+05
</td>
<td style="text-align:right;">
356578.94
</td>
<td style="text-align:right;">
3.800370e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
492316
</td>
<td style="text-align:right;">
9.393685e+10
</td>
<td style="text-align:right;">
91.5367483
</td>
<td style="text-align:right;">
95.58
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4392
</td>
<td style="text-align:left;">
19169
</td>
<td style="text-align:left;">
2018-09-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.682558e+05
</td>
<td style="text-align:right;">
168255.77
</td>
<td style="text-align:right;">
2.733750e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
98533
</td>
<td style="text-align:right;">
1.981740e+10
</td>
<td style="text-align:right;">
76.0101814
</td>
<td style="text-align:right;">
88.89
</td>
<td style="text-align:left;">
SEVERE STORM AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4386
</td>
<td style="text-align:left;">
19191
</td>
<td style="text-align:left;">
2018-08-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.111990e+03
</td>
<td style="text-align:right;">
8111.99
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
20051
</td>
<td style="text-align:right;">
6.671076e+09
</td>
<td style="text-align:right;">
45.1479478
</td>
<td style="text-align:right;">
95.45
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4361
</td>
<td style="text-align:left;">
21111
</td>
<td style="text-align:left;">
2018-04-26
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.206681e+06
</td>
<td style="text-align:right;">
603340.60
</td>
<td style="text-align:right;">
9.301000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
782833
</td>
<td style="text-align:right;">
1.432227e+11
</td>
<td style="text-align:right;">
95.9592746
</td>
<td style="text-align:right;">
77.94
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4376
</td>
<td style="text-align:left;">
24003
</td>
<td style="text-align:left;">
2018-07-03
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.318837e+04
</td>
<td style="text-align:right;">
43188.37
</td>
<td style="text-align:right;">
1.209226e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
587917
</td>
<td style="text-align:right;">
9.523544e+10
</td>
<td style="text-align:right;">
92.0139994
</td>
<td style="text-align:right;">
90.01
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4376
</td>
<td style="text-align:left;">
24027
</td>
<td style="text-align:left;">
2018-07-03
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.857715e+05
</td>
<td style="text-align:right;">
385771.50
</td>
<td style="text-align:right;">
1.344000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
332270
</td>
<td style="text-align:right;">
6.535684e+10
</td>
<td style="text-align:right;">
80.4008909
</td>
<td style="text-align:right;">
81.51
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4372
</td>
<td style="text-align:left;">
25009
</td>
<td style="text-align:left;">
2018-06-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.569497e+04
</td>
<td style="text-align:right;">
45694.97
</td>
<td style="text-align:right;">
9.625000e+01
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
808897
</td>
<td style="text-align:right;">
1.504420e+11
</td>
<td style="text-align:right;">
93.9548202
</td>
<td style="text-align:right;">
96.21
</td>
<td style="text-align:left;">
SEVERE WINTER STORM AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4372
</td>
<td style="text-align:left;">
25021
</td>
<td style="text-align:left;">
2018-06-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.212549e+05
</td>
<td style="text-align:right;">
221254.87
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
725648
</td>
<td style="text-align:right;">
1.489036e+11
</td>
<td style="text-align:right;">
92.9048680
</td>
<td style="text-align:right;">
93.98
</td>
<td style="text-align:left;">
SEVERE WINTER STORM AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4372
</td>
<td style="text-align:left;">
25023
</td>
<td style="text-align:left;">
2018-06-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.556252e+05
</td>
<td style="text-align:right;">
355625.20
</td>
<td style="text-align:right;">
9.508000e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
530320
</td>
<td style="text-align:right;">
1.025170e+11
</td>
<td style="text-align:right;">
90.9958638
</td>
<td style="text-align:right;">
99.01
</td>
<td style="text-align:left;">
SEVERE WINTER STORM AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4390
</td>
<td style="text-align:left;">
27101
</td>
<td style="text-align:left;">
2018-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.114522e+04
</td>
<td style="text-align:right;">
11145.22
</td>
<td style="text-align:right;">
6.930000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
8175
</td>
<td style="text-align:right;">
2.988917e+09
</td>
<td style="text-align:right;">
30.7986001
</td>
<td style="text-align:right;">
93.79
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4390
</td>
<td style="text-align:left;">
27127
</td>
<td style="text-align:left;">
2018-09-05
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.963801e+04
</td>
<td style="text-align:right;">
39638.01
</td>
<td style="text-align:right;">
1.117750e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
15425
</td>
<td style="text-align:right;">
4.482224e+09
</td>
<td style="text-align:right;">
33.4393891
</td>
<td style="text-align:right;">
96.31
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4397
</td>
<td style="text-align:left;">
36001
</td>
<td style="text-align:left;">
2018-10-01
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.121837e+06
</td>
<td style="text-align:right;">
4507982.53
</td>
<td style="text-align:right;">
3.164186e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
314684
</td>
<td style="text-align:right;">
6.262032e+10
</td>
<td style="text-align:right;">
75.5011136
</td>
<td style="text-align:right;">
94.02
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37013
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
8.602717e+05
</td>
<td style="text-align:right;">
826351.68
</td>
<td style="text-align:right;">
9.206397e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
44607
</td>
<td style="text-align:right;">
9.451940e+09
</td>
<td style="text-align:right;">
94.4957047
</td>
<td style="text-align:right;">
55.38
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37017
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.263996e+05
</td>
<td style="text-align:right;">
126399.56
</td>
<td style="text-align:right;">
2.798560e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
29597
</td>
<td style="text-align:right;">
8.417363e+09
</td>
<td style="text-align:right;">
90.9004136
</td>
<td style="text-align:right;">
23.87
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37019
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
1.331598e+07
</td>
<td style="text-align:right;">
13315982.85
</td>
<td style="text-align:right;">
3.588177e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
136245
</td>
<td style="text-align:right;">
2.994477e+10
</td>
<td style="text-align:right;">
97.0410436
</td>
<td style="text-align:right;">
64.93
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37031
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
2.107035e+07
</td>
<td style="text-align:right;">
21070350.44
</td>
<td style="text-align:right;">
4.328141e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
67625
</td>
<td style="text-align:right;">
1.740755e+10
</td>
<td style="text-align:right;">
97.1046771
</td>
<td style="text-align:right;">
83.10
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37047
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
3.406568e+05
</td>
<td style="text-align:right;">
340656.77
</td>
<td style="text-align:right;">
2.375821e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
50557
</td>
<td style="text-align:right;">
8.015801e+09
</td>
<td style="text-align:right;">
91.1231308
</td>
<td style="text-align:right;">
23.11
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37049
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1.337726e+07
</td>
<td style="text-align:right;">
11010303.55
</td>
<td style="text-align:right;">
8.857768e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
100042
</td>
<td style="text-align:right;">
1.736429e+10
</td>
<td style="text-align:right;">
96.4047089
</td>
<td style="text-align:right;">
65.63
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37051
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1.293207e+07
</td>
<td style="text-align:right;">
8203556.23
</td>
<td style="text-align:right;">
1.304293e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
334379
</td>
<td style="text-align:right;">
5.107665e+10
</td>
<td style="text-align:right;">
92.5230671
</td>
<td style="text-align:right;">
43.54
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37061
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.489586e+05
</td>
<td style="text-align:right;">
148958.59
</td>
<td style="text-align:right;">
3.223786e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
48624
</td>
<td style="text-align:right;">
8.582202e+09
</td>
<td style="text-align:right;">
93.3821190
</td>
<td style="text-align:right;">
8.34
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37103
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.056677e+05
</td>
<td style="text-align:right;">
305667.65
</td>
<td style="text-align:right;">
3.340000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
9165
</td>
<td style="text-align:right;">
1.485431e+09
</td>
<td style="text-align:right;">
79.9872733
</td>
<td style="text-align:right;">
20.34
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37107
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.623538e+05
</td>
<td style="text-align:right;">
962353.75
</td>
<td style="text-align:right;">
1.269900e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
55112
</td>
<td style="text-align:right;">
1.175561e+10
</td>
<td style="text-align:right;">
91.7912822
</td>
<td style="text-align:right;">
52.51
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37129
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1.647612e+07
</td>
<td style="text-align:right;">
16476124.28
</td>
<td style="text-align:right;">
5.290670e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
225344
</td>
<td style="text-align:right;">
5.126031e+10
</td>
<td style="text-align:right;">
98.5046134
</td>
<td style="text-align:right;">
82.27
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37133
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1.492910e+07
</td>
<td style="text-align:right;">
14929098.44
</td>
<td style="text-align:right;">
3.615121e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
202949
</td>
<td style="text-align:right;">
3.015480e+10
</td>
<td style="text-align:right;">
97.9319122
</td>
<td style="text-align:right;">
63.18
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37135
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.106464e+04
</td>
<td style="text-align:right;">
51064.64
</td>
<td style="text-align:right;">
1.284000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
148579
</td>
<td style="text-align:right;">
2.699894e+10
</td>
<td style="text-align:right;">
72.9557747
</td>
<td style="text-align:right;">
65.05
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37137
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
3.062022e+06
</td>
<td style="text-align:right;">
3062021.80
</td>
<td style="text-align:right;">
3.502578e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
12213
</td>
<td style="text-align:right;">
3.222184e+09
</td>
<td style="text-align:right;">
84.7916004
</td>
<td style="text-align:right;">
61.23
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37141
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.238024e+07
</td>
<td style="text-align:right;">
12380244.98
</td>
<td style="text-align:right;">
1.804919e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
60195
</td>
<td style="text-align:right;">
1.105170e+10
</td>
<td style="text-align:right;">
94.0184537
</td>
<td style="text-align:right;">
56.87
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37151
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.812469e+04
</td>
<td style="text-align:right;">
28124.69
</td>
<td style="text-align:right;">
7.353600e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
144115
</td>
<td style="text-align:right;">
2.604142e+10
</td>
<td style="text-align:right;">
75.8510977
</td>
<td style="text-align:right;">
33.35
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37155
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2.722674e+06
</td>
<td style="text-align:right;">
2705433.90
</td>
<td style="text-align:right;">
4.252400e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
116414
</td>
<td style="text-align:right;">
1.841898e+10
</td>
<td style="text-align:right;">
94.3684378
</td>
<td style="text-align:right;">
12.64
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37163
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.604271e+04
</td>
<td style="text-align:right;">
66042.71
</td>
<td style="text-align:right;">
2.400000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
59021
</td>
<td style="text-align:right;">
1.256312e+10
</td>
<td style="text-align:right;">
93.9866370
</td>
<td style="text-align:right;">
11.55
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37183
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
1.247636e+07
</td>
<td style="text-align:right;">
12476355.53
</td>
<td style="text-align:right;">
2.431282e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
1128357
</td>
<td style="text-align:right;">
2.152861e+11
</td>
<td style="text-align:right;">
93.6366529
</td>
<td style="text-align:right;">
72.02
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4393
</td>
<td style="text-align:left;">
37191
</td>
<td style="text-align:left;">
2018-09-14
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
7.201412e+04
</td>
<td style="text-align:right;">
72014.12
</td>
<td style="text-align:right;">
4.823320e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
116680
</td>
<td style="text-align:right;">
2.717472e+10
</td>
<td style="text-align:right;">
94.4320713
</td>
<td style="text-align:right;">
44.11
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4360
</td>
<td style="text-align:left;">
39061
</td>
<td style="text-align:left;">
2018-04-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.130350e+03
</td>
<td style="text-align:right;">
7130.35
</td>
<td style="text-align:right;">
1.800000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
830623
</td>
<td style="text-align:right;">
1.538887e+11
</td>
<td style="text-align:right;">
93.4457525
</td>
<td style="text-align:right;">
86.31
</td>
<td style="text-align:left;">
SEVERE STORMS, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4408
</td>
<td style="text-align:left;">
42015
</td>
<td style="text-align:left;">
2018-11-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.639055e+04
</td>
<td style="text-align:right;">
36390.55
</td>
<td style="text-align:right;">
1.970060e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
59840
</td>
<td style="text-align:right;">
1.440289e+10
</td>
<td style="text-align:right;">
71.5240216
</td>
<td style="text-align:right;">
64.96
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4408
</td>
<td style="text-align:left;">
42037
</td>
<td style="text-align:left;">
2018-11-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.833000e+03
</td>
<td style="text-align:right;">
4833.00
</td>
<td style="text-align:right;">
1.464000e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
64676
</td>
<td style="text-align:right;">
1.272554e+10
</td>
<td style="text-align:right;">
36.3665288
</td>
<td style="text-align:right;">
86.03
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4394
</td>
<td style="text-align:left;">
45051
</td>
<td style="text-align:left;">
2018-09-16
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1.133269e+06
</td>
<td style="text-align:right;">
1133268.54
</td>
<td style="text-align:right;">
2.318340e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
348574
</td>
<td style="text-align:right;">
6.455071e+10
</td>
<td style="text-align:right;">
98.9818645
</td>
<td style="text-align:right;">
61.30
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4394
</td>
<td style="text-align:left;">
45067
</td>
<td style="text-align:left;">
2018-09-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.117154e+05
</td>
<td style="text-align:right;">
311715.36
</td>
<td style="text-align:right;">
8.890000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
29137
</td>
<td style="text-align:right;">
6.028669e+09
</td>
<td style="text-align:right;">
85.9688196
</td>
<td style="text-align:right;">
22.53
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4394
</td>
<td style="text-align:left;">
45069
</td>
<td style="text-align:left;">
2018-09-16
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.618326e+05
</td>
<td style="text-align:right;">
361832.64
</td>
<td style="text-align:right;">
8.050788e+05
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
26656
</td>
<td style="text-align:right;">
3.804431e+09
</td>
<td style="text-align:right;">
65.6379255
</td>
<td style="text-align:right;">
17.79
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4394
</td>
<td style="text-align:left;">
45079
</td>
<td style="text-align:left;">
2018-09-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.803872e+05
</td>
<td style="text-align:right;">
380387.21
</td>
<td style="text-align:right;">
1.033740e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
415644
</td>
<td style="text-align:right;">
8.168495e+10
</td>
<td style="text-align:right;">
93.3503023
</td>
<td style="text-align:right;">
74.19
</td>
<td style="text-align:left;">
HURRICANE FLORENCE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4411
</td>
<td style="text-align:left;">
51760
</td>
<td style="text-align:left;">
2018-12-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.351660e+03
</td>
<td style="text-align:right;">
2351.66
</td>
<td style="text-align:right;">
2.000000e+01
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
226269
</td>
<td style="text-align:right;">
4.353533e+10
</td>
<td style="text-align:right;">
73.2421254
</td>
<td style="text-align:right;">
54.84
</td>
<td style="text-align:left;">
TROPICAL STORM MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4383
</td>
<td style="text-align:left;">
55025
</td>
<td style="text-align:left;">
2018-08-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.871043e+04
</td>
<td style="text-align:right;">
28710.43
</td>
<td style="text-align:right;">
9.000000e+01
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
561270
</td>
<td style="text-align:right;">
1.345952e+11
</td>
<td style="text-align:right;">
90.7413299
</td>
<td style="text-align:right;">
98.28
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4402
</td>
<td style="text-align:left;">
55025
</td>
<td style="text-align:left;">
2018-10-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.192215e+04
</td>
<td style="text-align:right;">
81922.15
</td>
<td style="text-align:right;">
3.497000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
561270
</td>
<td style="text-align:right;">
1.345952e+11
</td>
<td style="text-align:right;">
90.7413299
</td>
<td style="text-align:right;">
98.28
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, FLOODING, AND LANDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4402
</td>
<td style="text-align:left;">
55077
</td>
<td style="text-align:left;">
2018-10-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.180000e+03
</td>
<td style="text-align:right;">
9180.00
</td>
<td style="text-align:right;">
3.600000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
15562
</td>
<td style="text-align:right;">
4.275948e+09
</td>
<td style="text-align:right;">
24.3716195
</td>
<td style="text-align:right;">
56.30
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, FLOODING, AND LANDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4402
</td>
<td style="text-align:left;">
55081
</td>
<td style="text-align:left;">
2018-10-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.639500e+03
</td>
<td style="text-align:right;">
3639.50
</td>
<td style="text-align:right;">
1.767480e+03
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
46230
</td>
<td style="text-align:right;">
1.119403e+10
</td>
<td style="text-align:right;">
62.1380846
</td>
<td style="text-align:right;">
72.15
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, FLOODING, AND LANDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4402
</td>
<td style="text-align:left;">
55111
</td>
<td style="text-align:left;">
2018-10-18
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.592182e+04
</td>
<td style="text-align:right;">
25921.82
</td>
<td style="text-align:right;">
1.745324e+04
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
65699
</td>
<td style="text-align:right;">
1.686294e+10
</td>
<td style="text-align:right;">
59.6245625
</td>
<td style="text-align:right;">
89.78
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, FLOODING, AND LANDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4402
</td>
<td style="text-align:left;">
55123
</td>
<td style="text-align:left;">
2018-10-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.997230e+03
</td>
<td style="text-align:right;">
4997.23
</td>
<td style="text-align:right;">
2.300000e+02
</td>
<td style="text-align:right;">
2018
</td>
<td style="text-align:left;">
30574
</td>
<td style="text-align:right;">
7.252761e+09
</td>
<td style="text-align:right;">
51.1294941
</td>
<td style="text-align:right;">
73.93
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, FLOODING, AND LANDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4426
</td>
<td style="text-align:left;">
01019
</td>
<td style="text-align:left;">
2019-04-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.766280e+03
</td>
<td style="text-align:right;">
4766.28
</td>
<td style="text-align:right;">
8.576000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
24933
</td>
<td style="text-align:right;">
4.433991e+09
</td>
<td style="text-align:right;">
42.0617245
</td>
<td style="text-align:right;">
13.59
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4426
</td>
<td style="text-align:left;">
01059
</td>
<td style="text-align:left;">
2019-04-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.113192e+04
</td>
<td style="text-align:right;">
21131.92
</td>
<td style="text-align:right;">
1.499885e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
31999
</td>
<td style="text-align:right;">
5.620840e+09
</td>
<td style="text-align:right;">
65.4788419
</td>
<td style="text-align:right;">
19.10
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4419
</td>
<td style="text-align:left;">
01081
</td>
<td style="text-align:left;">
2019-03-05
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.255144e+07
</td>
<td style="text-align:right;">
6275717.54
</td>
<td style="text-align:right;">
3.364800e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
173871
</td>
<td style="text-align:right;">
2.952116e+10
</td>
<td style="text-align:right;">
70.8240535
</td>
<td style="text-align:right;">
46.66
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4413
</td>
<td style="text-align:left;">
02020
</td>
<td style="text-align:left;">
2019-01-31
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
5.578337e+05
</td>
<td style="text-align:right;">
557220.97
</td>
<td style="text-align:right;">
7.816600e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
290985
</td>
<td style="text-align:right;">
6.439078e+10
</td>
<td style="text-align:right;">
94.8456888
</td>
<td style="text-align:right;">
51.08
</td>
<td style="text-align:left;">
EARTHQUAKE
</td>
<td style="text-align:left;">
Earthquake
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4413
</td>
<td style="text-align:left;">
02122
</td>
<td style="text-align:left;">
2019-01-31
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.373895e+04
</td>
<td style="text-align:right;">
13738.95
</td>
<td style="text-align:right;">
2.800000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
58731
</td>
<td style="text-align:right;">
1.887690e+10
</td>
<td style="text-align:right;">
90.9640471
</td>
<td style="text-align:right;">
10.38
</td>
<td style="text-align:left;">
EARTHQUAKE
</td>
<td style="text-align:left;">
Earthquake
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4441
</td>
<td style="text-align:left;">
05069
</td>
<td style="text-align:left;">
2019-06-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.373240e+03
</td>
<td style="text-align:right;">
9373.24
</td>
<td style="text-align:right;">
1.800000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
67108
</td>
<td style="text-align:right;">
1.350750e+10
</td>
<td style="text-align:right;">
84.2507159
</td>
<td style="text-align:right;">
50.64
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4441
</td>
<td style="text-align:left;">
05105
</td>
<td style="text-align:left;">
2019-06-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.917481e+04
</td>
<td style="text-align:right;">
49174.81
</td>
<td style="text-align:right;">
2.261666e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
10019
</td>
<td style="text-align:right;">
1.476877e+09
</td>
<td style="text-align:right;">
40.7572383
</td>
<td style="text-align:right;">
19.45
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4441
</td>
<td style="text-align:left;">
05119
</td>
<td style="text-align:left;">
2019-06-08
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.177875e+05
</td>
<td style="text-align:right;">
358893.74
</td>
<td style="text-align:right;">
1.010320e+05
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
398830
</td>
<td style="text-align:right;">
7.392333e+10
</td>
<td style="text-align:right;">
95.0684060
</td>
<td style="text-align:right;">
73.68
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4441
</td>
<td style="text-align:left;">
05131
</td>
<td style="text-align:left;">
2019-06-08
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.248890e+05
</td>
<td style="text-align:right;">
324888.95
</td>
<td style="text-align:right;">
5.230000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
127706
</td>
<td style="text-align:right;">
2.475716e+10
</td>
<td style="text-align:right;">
83.9325485
</td>
<td style="text-align:right;">
42.52
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4434
</td>
<td style="text-align:left;">
06007
</td>
<td style="text-align:left;">
2019-05-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.030990e+05
</td>
<td style="text-align:right;">
103099.04
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
211490
</td>
<td style="text-align:right;">
4.251521e+10
</td>
<td style="text-align:right;">
97.1683105
</td>
<td style="text-align:right;">
62.41
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4431
</td>
<td style="text-align:left;">
06065
</td>
<td style="text-align:left;">
2019-05-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.413386e+04
</td>
<td style="text-align:right;">
64133.86
</td>
<td style="text-align:right;">
8.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
2416838
</td>
<td style="text-align:right;">
4.187359e+11
</td>
<td style="text-align:right;">
99.9363665
</td>
<td style="text-align:right;">
27.43
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4434
</td>
<td style="text-align:left;">
06097
</td>
<td style="text-align:left;">
2019-05-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.316370e+04
</td>
<td style="text-align:right;">
13163.70
</td>
<td style="text-align:right;">
7.200000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
488330
</td>
<td style="text-align:right;">
1.046105e+11
</td>
<td style="text-align:right;">
99.1727649
</td>
<td style="text-align:right;">
72.98
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4443
</td>
<td style="text-align:left;">
16049
</td>
<td style="text-align:left;">
2019-06-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.722360e+03
</td>
<td style="text-align:right;">
3722.36
</td>
<td style="text-align:right;">
6.000000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
16440
</td>
<td style="text-align:right;">
6.597193e+09
</td>
<td style="text-align:right;">
80.9099586
</td>
<td style="text-align:right;">
16.84
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4461
</td>
<td style="text-align:left;">
17149
</td>
<td style="text-align:left;">
2019-09-19
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.559100e+02
</td>
<td style="text-align:right;">
155.91
</td>
<td style="text-align:right;">
3.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
14724
</td>
<td style="text-align:right;">
1.604249e+09
</td>
<td style="text-align:right;">
18.5173401
</td>
<td style="text-align:right;">
53.91
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4461
</td>
<td style="text-align:left;">
17167
</td>
<td style="text-align:left;">
2019-09-19
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.298103e+05
</td>
<td style="text-align:right;">
329810.35
</td>
<td style="text-align:right;">
3.992000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
196182
</td>
<td style="text-align:right;">
5.681118e+10
</td>
<td style="text-align:right;">
88.5459752
</td>
<td style="text-align:right;">
91.31
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4461
</td>
<td style="text-align:left;">
17171
</td>
<td style="text-align:left;">
2019-09-19
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.268457e+05
</td>
<td style="text-align:right;">
113422.86
</td>
<td style="text-align:right;">
2.740000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
4937
</td>
<td style="text-align:right;">
4.463794e+08
</td>
<td style="text-align:right;">
7.9541839
</td>
<td style="text-align:right;">
73.71
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4421
</td>
<td style="text-align:left;">
19017
</td>
<td style="text-align:left;">
2019-03-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.409320e+04
</td>
<td style="text-align:right;">
24093.20
</td>
<td style="text-align:right;">
7.230000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
24970
</td>
<td style="text-align:right;">
6.061426e+09
</td>
<td style="text-align:right;">
38.3391664
</td>
<td style="text-align:right;">
99.20
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4421
</td>
<td style="text-align:left;">
19071
</td>
<td style="text-align:left;">
2019-03-23
</td>
<td style="text-align:right;">
35348
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.892466e+09
</td>
<td style="text-align:right;">
36853128.84
</td>
<td style="text-align:right;">
6.315800e+05
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
6605
</td>
<td style="text-align:right;">
2.092445e+09
</td>
<td style="text-align:right;">
38.1800827
</td>
<td style="text-align:right;">
64.19
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4421
</td>
<td style="text-align:left;">
19085
</td>
<td style="text-align:left;">
2019-03-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.015405e+04
</td>
<td style="text-align:right;">
40154.05
</td>
<td style="text-align:right;">
2.400000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
14559
</td>
<td style="text-align:right;">
3.415922e+09
</td>
<td style="text-align:right;">
49.4432071
</td>
<td style="text-align:right;">
75.33
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4421
</td>
<td style="text-align:left;">
19129
</td>
<td style="text-align:left;">
2019-03-23
</td>
<td style="text-align:right;">
51
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
8.031916e+06
</td>
<td style="text-align:right;">
1194916.74
</td>
<td style="text-align:right;">
2.314703e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
14475
</td>
<td style="text-align:right;">
3.454378e+09
</td>
<td style="text-align:right;">
38.7527840
</td>
<td style="text-align:right;">
86.28
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4421
</td>
<td style="text-align:left;">
19163
</td>
<td style="text-align:left;">
2019-03-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.460323e+04
</td>
<td style="text-align:right;">
14603.23
</td>
<td style="text-align:right;">
1.093400e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
174636
</td>
<td style="text-align:right;">
3.898570e+10
</td>
<td style="text-align:right;">
82.0553611
</td>
<td style="text-align:right;">
95.74
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4421
</td>
<td style="text-align:left;">
19193
</td>
<td style="text-align:left;">
2019-03-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.911463e+04
</td>
<td style="text-align:right;">
29114.63
</td>
<td style="text-align:right;">
4.920000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
105853
</td>
<td style="text-align:right;">
1.931667e+10
</td>
<td style="text-align:right;">
83.4552975
</td>
<td style="text-align:right;">
93.57
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20015
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.870808e+04
</td>
<td style="text-align:right;">
38708.08
</td>
<td style="text-align:right;">
9.600000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
67340
</td>
<td style="text-align:right;">
1.296721e+10
</td>
<td style="text-align:right;">
80.2099905
</td>
<td style="text-align:right;">
70.88
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20017
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.503620e+03
</td>
<td style="text-align:right;">
7503.62
</td>
<td style="text-align:right;">
3.000000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
2564
</td>
<td style="text-align:right;">
8.990594e+08
</td>
<td style="text-align:right;">
25.1352211
</td>
<td style="text-align:right;">
45.45
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20035
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.308647e+05
</td>
<td style="text-align:right;">
130864.71
</td>
<td style="text-align:right;">
7.680000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
34541
</td>
<td style="text-align:right;">
7.569744e+09
</td>
<td style="text-align:right;">
74.5466115
</td>
<td style="text-align:right;">
70.62
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20045
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.426368e+05
</td>
<td style="text-align:right;">
542636.85
</td>
<td style="text-align:right;">
1.463560e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
118666
</td>
<td style="text-align:right;">
2.450489e+10
</td>
<td style="text-align:right;">
79.6372892
</td>
<td style="text-align:right;">
79.79
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20047
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.323396e+04
</td>
<td style="text-align:right;">
33233.96
</td>
<td style="text-align:right;">
3.240000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
2905
</td>
<td style="text-align:right;">
1.061872e+09
</td>
<td style="text-align:right;">
16.0038180
</td>
<td style="text-align:right;">
50.57
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20103
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.713647e+06
</td>
<td style="text-align:right;">
804904.23
</td>
<td style="text-align:right;">
1.475522e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
81853
</td>
<td style="text-align:right;">
1.587767e+10
</td>
<td style="text-align:right;">
74.1966274
</td>
<td style="text-align:right;">
73.81
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4449
</td>
<td style="text-align:left;">
20165
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.980870e+03
</td>
<td style="text-align:right;">
3980.87
</td>
<td style="text-align:right;">
1.999800e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
2952
</td>
<td style="text-align:right;">
1.656550e+09
</td>
<td style="text-align:right;">
23.6080178
</td>
<td style="text-align:right;">
82.69
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING,LANDSLIDES,AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4458
</td>
<td style="text-align:left;">
22007
</td>
<td style="text-align:left;">
2019-08-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.031278e+05
</td>
<td style="text-align:right;">
103127.79
</td>
<td style="text-align:right;">
1.022700e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
21017
</td>
<td style="text-align:right;">
3.291188e+09
</td>
<td style="text-align:right;">
86.3506204
</td>
<td style="text-align:right;">
48.89
</td>
<td style="text-align:left;">
HURRICANE BARRY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4458
</td>
<td style="text-align:left;">
22033
</td>
<td style="text-align:left;">
2019-08-27
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.469997e+06
</td>
<td style="text-align:right;">
1234998.62
</td>
<td style="text-align:right;">
2.822000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
456512
</td>
<td style="text-align:right;">
1.120657e+11
</td>
<td style="text-align:right;">
98.8864143
</td>
<td style="text-align:right;">
60.09
</td>
<td style="text-align:left;">
HURRICANE BARRY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4439
</td>
<td style="text-align:left;">
22061
</td>
<td style="text-align:left;">
2019-06-03
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.330467e+05
</td>
<td style="text-align:right;">
833046.71
</td>
<td style="text-align:right;">
4.647900e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
48370
</td>
<td style="text-align:right;">
8.006832e+09
</td>
<td style="text-align:right;">
68.6605154
</td>
<td style="text-align:right;">
40.74
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4458
</td>
<td style="text-align:left;">
22075
</td>
<td style="text-align:left;">
2019-08-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.609046e+04
</td>
<td style="text-align:right;">
36090.46
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
23448
</td>
<td style="text-align:right;">
4.870262e+09
</td>
<td style="text-align:right;">
82.6916958
</td>
<td style="text-align:right;">
94.33
</td>
<td style="text-align:left;">
HURRICANE BARRY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4458
</td>
<td style="text-align:left;">
22101
</td>
<td style="text-align:left;">
2019-08-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.739858e+05
</td>
<td style="text-align:right;">
573985.77
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
49313
</td>
<td style="text-align:right;">
1.172348e+10
</td>
<td style="text-align:right;">
94.7184219
</td>
<td style="text-align:right;">
78.13
</td>
<td style="text-align:left;">
HURRICANE BARRY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4458
</td>
<td style="text-align:left;">
22109
</td>
<td style="text-align:left;">
2019-08-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.443827e+05
</td>
<td style="text-align:right;">
644382.71
</td>
<td style="text-align:right;">
1.625000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
109484
</td>
<td style="text-align:right;">
2.195670e+10
</td>
<td style="text-align:right;">
96.3728921
</td>
<td style="text-align:right;">
94.75
</td>
<td style="text-align:left;">
HURRICANE BARRY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
27013
</td>
<td style="text-align:left;">
2019-06-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.025361e+04
</td>
<td style="text-align:right;">
10253.61
</td>
<td style="text-align:right;">
2.612800e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
69073
</td>
<td style="text-align:right;">
1.557987e+10
</td>
<td style="text-align:right;">
68.0559975
</td>
<td style="text-align:right;">
98.76
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
27027
</td>
<td style="text-align:left;">
2019-06-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.259967e+04
</td>
<td style="text-align:right;">
22599.67
</td>
<td style="text-align:right;">
1.000000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
65291
</td>
<td style="text-align:right;">
1.448070e+10
</td>
<td style="text-align:right;">
58.6700605
</td>
<td style="text-align:right;">
85.74
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4414
</td>
<td style="text-align:left;">
27137
</td>
<td style="text-align:left;">
2019-02-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.187556e+05
</td>
<td style="text-align:right;">
218755.60
</td>
<td style="text-align:right;">
2.027750e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
200067
</td>
<td style="text-align:right;">
5.200865e+10
</td>
<td style="text-align:right;">
66.5287941
</td>
<td style="text-align:right;">
98.73
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
27173
</td>
<td style="text-align:left;">
2019-06-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.070390e+03
</td>
<td style="text-align:right;">
7070.39
</td>
<td style="text-align:right;">
4.980000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
9528
</td>
<td style="text-align:right;">
2.993936e+09
</td>
<td style="text-align:right;">
24.6579701
</td>
<td style="text-align:right;">
94.14
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4450
</td>
<td style="text-align:left;">
28023
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.185000e+04
</td>
<td style="text-align:right;">
11850.00
</td>
<td style="text-align:right;">
1.170000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
15570
</td>
<td style="text-align:right;">
2.563118e+09
</td>
<td style="text-align:right;">
50.6522431
</td>
<td style="text-align:right;">
21.16
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4415
</td>
<td style="text-align:left;">
28035
</td>
<td style="text-align:left;">
2019-02-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.032558e+04
</td>
<td style="text-align:right;">
10325.58
</td>
<td style="text-align:right;">
3.600000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
78050
</td>
<td style="text-align:right;">
1.342654e+10
</td>
<td style="text-align:right;">
86.5415208
</td>
<td style="text-align:right;">
59.10
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, AND TORNADO
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4470
</td>
<td style="text-align:left;">
28081
</td>
<td style="text-align:left;">
2019-12-06
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.390359e+05
</td>
<td style="text-align:right;">
239035.94
</td>
<td style="text-align:right;">
1.610000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
83269
</td>
<td style="text-align:right;">
1.698476e+10
</td>
<td style="text-align:right;">
79.0009545
</td>
<td style="text-align:right;">
62.38
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4429
</td>
<td style="text-align:left;">
28087
</td>
<td style="text-align:left;">
2019-04-23
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.591174e+05
</td>
<td style="text-align:right;">
759117.40
</td>
<td style="text-align:right;">
4.321300e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
58845
</td>
<td style="text-align:right;">
1.178985e+10
</td>
<td style="text-align:right;">
77.6328349
</td>
<td style="text-align:right;">
56.91
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4450
</td>
<td style="text-align:left;">
28095
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.144422e+06
</td>
<td style="text-align:right;">
2144422.14
</td>
<td style="text-align:right;">
8.078800e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
34055
</td>
<td style="text-align:right;">
5.885635e+09
</td>
<td style="text-align:right;">
63.0607700
</td>
<td style="text-align:right;">
43.76
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4429
</td>
<td style="text-align:left;">
28163
</td>
<td style="text-align:left;">
2019-04-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.868720e+03
</td>
<td style="text-align:right;">
4868.72
</td>
<td style="text-align:right;">
4.940000e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
26703
</td>
<td style="text-align:right;">
3.860836e+09
</td>
<td style="text-align:right;">
71.7149220
</td>
<td style="text-align:right;">
7.45
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4435
</td>
<td style="text-align:left;">
29005
</td>
<td style="text-align:left;">
2019-05-20
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.077995e+06
</td>
<td style="text-align:right;">
312080.06
</td>
<td style="text-align:right;">
6.880000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
5305
</td>
<td style="text-align:right;">
1.510889e+09
</td>
<td style="text-align:right;">
30.2258988
</td>
<td style="text-align:right;">
66.77
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29009
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.475490e+04
</td>
<td style="text-align:right;">
14754.90
</td>
<td style="text-align:right;">
1.199908e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
34451
</td>
<td style="text-align:right;">
8.444012e+09
</td>
<td style="text-align:right;">
77.9510022
</td>
<td style="text-align:right;">
22.69
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4435
</td>
<td style="text-align:left;">
29021
</td>
<td style="text-align:left;">
2019-05-20
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.153960e+04
</td>
<td style="text-align:right;">
15769.80
</td>
<td style="text-align:right;">
1.518520e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
84731
</td>
<td style="text-align:right;">
1.765838e+10
</td>
<td style="text-align:right;">
69.3923003
</td>
<td style="text-align:right;">
89.50
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29033
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.624925e+05
</td>
<td style="text-align:right;">
162492.50
</td>
<td style="text-align:right;">
6.131000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
8482
</td>
<td style="text-align:right;">
2.270868e+09
</td>
<td style="text-align:right;">
42.4117086
</td>
<td style="text-align:right;">
77.08
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29041
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.824655e+04
</td>
<td style="text-align:right;">
48246.55
</td>
<td style="text-align:right;">
3.670000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
7366
</td>
<td style="text-align:right;">
2.765102e+09
</td>
<td style="text-align:right;">
21.5717467
</td>
<td style="text-align:right;">
72.12
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4435
</td>
<td style="text-align:left;">
29051
</td>
<td style="text-align:left;">
2019-05-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.047692e+04
</td>
<td style="text-align:right;">
40476.92
</td>
<td style="text-align:right;">
8.000000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
76989
</td>
<td style="text-align:right;">
1.757678e+10
</td>
<td style="text-align:right;">
74.6102450
</td>
<td style="text-align:right;">
90.67
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29051
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.755094e+06
</td>
<td style="text-align:right;">
1755093.68
</td>
<td style="text-align:right;">
3.000406e+06
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
76989
</td>
<td style="text-align:right;">
1.757678e+10
</td>
<td style="text-align:right;">
74.6102450
</td>
<td style="text-align:right;">
90.67
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4435
</td>
<td style="text-align:left;">
29087
</td>
<td style="text-align:left;">
2019-05-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.245427e+04
</td>
<td style="text-align:right;">
42454.27
</td>
<td style="text-align:right;">
2.581000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
4210
</td>
<td style="text-align:right;">
1.812408e+09
</td>
<td style="text-align:right;">
23.4171174
</td>
<td style="text-align:right;">
63.34
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29087
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.051100e+04
</td>
<td style="text-align:right;">
30511.00
</td>
<td style="text-align:right;">
3.940000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
4210
</td>
<td style="text-align:right;">
1.812408e+09
</td>
<td style="text-align:right;">
23.4171174
</td>
<td style="text-align:right;">
63.34
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29095
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.300000e+04
</td>
<td style="text-align:right;">
13000.00
</td>
<td style="text-align:right;">
5.000000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
716764
</td>
<td style="text-align:right;">
1.359077e+11
</td>
<td style="text-align:right;">
95.6092905
</td>
<td style="text-align:right;">
61.52
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29097
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.910665e+05
</td>
<td style="text-align:right;">
191066.47
</td>
<td style="text-align:right;">
2.040000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
122722
</td>
<td style="text-align:right;">
2.462236e+10
</td>
<td style="text-align:right;">
84.6961502
</td>
<td style="text-align:right;">
57.80
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29113
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.299964e+04
</td>
<td style="text-align:right;">
32999.64
</td>
<td style="text-align:right;">
2.160000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
59458
</td>
<td style="text-align:right;">
1.051751e+10
</td>
<td style="text-align:right;">
73.1148584
</td>
<td style="text-align:right;">
78.49
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29131
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.319481e+04
</td>
<td style="text-align:right;">
33194.81
</td>
<td style="text-align:right;">
8.162275e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
24682
</td>
<td style="text-align:right;">
6.380061e+09
</td>
<td style="text-align:right;">
60.8017817
</td>
<td style="text-align:right;">
36.95
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29145
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.335495e+04
</td>
<td style="text-align:right;">
13354.95
</td>
<td style="text-align:right;">
2.440000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
58493
</td>
<td style="text-align:right;">
1.241102e+10
</td>
<td style="text-align:right;">
81.8008272
</td>
<td style="text-align:right;">
44.56
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29163
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.439171e+04
</td>
<td style="text-align:right;">
84391.71
</td>
<td style="text-align:right;">
2.000000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
17548
</td>
<td style="text-align:right;">
4.436404e+09
</td>
<td style="text-align:right;">
41.1708559
</td>
<td style="text-align:right;">
56.94
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29177
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.927000e+05
</td>
<td style="text-align:right;">
96350.00
</td>
<td style="text-align:right;">
7.666666e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
23141
</td>
<td style="text-align:right;">
4.416857e+09
</td>
<td style="text-align:right;">
30.0668151
</td>
<td style="text-align:right;">
78.58
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29183
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.358411e+06
</td>
<td style="text-align:right;">
1032853.07
</td>
<td style="text-align:right;">
8.800000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
405183
</td>
<td style="text-align:right;">
8.092181e+10
</td>
<td style="text-align:right;">
93.0003182
</td>
<td style="text-align:right;">
96.24
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4451
</td>
<td style="text-align:left;">
29195
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.200000e+04
</td>
<td style="text-align:right;">
12000.00
</td>
<td style="text-align:right;">
6.920000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
23279
</td>
<td style="text-align:right;">
4.980690e+09
</td>
<td style="text-align:right;">
46.4842507
</td>
<td style="text-align:right;">
72.66
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31015
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
36
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.304275e+05
</td>
<td style="text-align:right;">
71737.92
</td>
<td style="text-align:right;">
1.764000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
1810
</td>
<td style="text-align:right;">
8.006031e+08
</td>
<td style="text-align:right;">
5.4088451
</td>
<td style="text-align:right;">
42.65
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31019
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.907830e+04
</td>
<td style="text-align:right;">
29078.30
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
50007
</td>
<td style="text-align:right;">
1.014431e+10
</td>
<td style="text-align:right;">
79.4463888
</td>
<td style="text-align:right;">
75.59
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31023
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.585415e+05
</td>
<td style="text-align:right;">
158541.48
</td>
<td style="text-align:right;">
3.029140e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
8354
</td>
<td style="text-align:right;">
2.690794e+09
</td>
<td style="text-align:right;">
38.5618836
</td>
<td style="text-align:right;">
80.78
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31027
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.846910e+03
</td>
<td style="text-align:right;">
8846.91
</td>
<td style="text-align:right;">
2.724560e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
8361
</td>
<td style="text-align:right;">
2.960310e+09
</td>
<td style="text-align:right;">
32.8985046
</td>
<td style="text-align:right;">
55.51
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31053
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.373705e+05
</td>
<td style="text-align:right;">
337370.49
</td>
<td style="text-align:right;">
5.356880e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
37166
</td>
<td style="text-align:right;">
8.678864e+09
</td>
<td style="text-align:right;">
79.4782055
</td>
<td style="text-align:right;">
84.31
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31055
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
5.094115e+05
</td>
<td style="text-align:right;">
509411.54
</td>
<td style="text-align:right;">
8.864000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
584427
</td>
<td style="text-align:right;">
1.025738e+11
</td>
<td style="text-align:right;">
95.6729240
</td>
<td style="text-align:right;">
82.78
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31079
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.773112e+04
</td>
<td style="text-align:right;">
57731.12
</td>
<td style="text-align:right;">
1.006000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
62826
</td>
<td style="text-align:right;">
1.107112e+10
</td>
<td style="text-align:right;">
84.2825326
</td>
<td style="text-align:right;">
69.26
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31093
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.283799e+04
</td>
<td style="text-align:right;">
12837.99
</td>
<td style="text-align:right;">
3.290060e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
6475
</td>
<td style="text-align:right;">
1.721276e+09
</td>
<td style="text-align:right;">
52.1476297
</td>
<td style="text-align:right;">
56.11
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31097
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.448106e+04
</td>
<td style="text-align:right;">
44481.06
</td>
<td style="text-align:right;">
8.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
5288
</td>
<td style="text-align:right;">
1.325388e+09
</td>
<td style="text-align:right;">
13.6493796
</td>
<td style="text-align:right;">
67.85
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31107
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.877048e+04
</td>
<td style="text-align:right;">
28770.48
</td>
<td style="text-align:right;">
1.500000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
8383
</td>
<td style="text-align:right;">
3.127981e+09
</td>
<td style="text-align:right;">
35.8574610
</td>
<td style="text-align:right;">
47.71
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31127
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
125
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.036525e+07
</td>
<td style="text-align:right;">
4174301.65
</td>
<td style="text-align:right;">
5.708050e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
7072
</td>
<td style="text-align:right;">
1.812901e+09
</td>
<td style="text-align:right;">
24.4670697
</td>
<td style="text-align:right;">
66.61
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31139
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.810620e+04
</td>
<td style="text-align:right;">
18106.20
</td>
<td style="text-align:right;">
1.146000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
7312
</td>
<td style="text-align:right;">
2.229682e+09
</td>
<td style="text-align:right;">
19.5991091
</td>
<td style="text-align:right;">
73.39
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31141
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.167394e+04
</td>
<td style="text-align:right;">
11673.94
</td>
<td style="text-align:right;">
2.920000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
34261
</td>
<td style="text-align:right;">
7.898105e+09
</td>
<td style="text-align:right;">
75.2465797
</td>
<td style="text-align:right;">
67.03
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
31153
</td>
<td style="text-align:left;">
2019-03-21
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.905860e+05
</td>
<td style="text-align:right;">
190585.96
</td>
<td style="text-align:right;">
2.621230e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
190379
</td>
<td style="text-align:right;">
3.342189e+10
</td>
<td style="text-align:right;">
88.1641744
</td>
<td style="text-align:right;">
91.50
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4472
</td>
<td style="text-align:left;">
36043
</td>
<td style="text-align:left;">
2019-12-19
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.956687e+04
</td>
<td style="text-align:right;">
79566.87
</td>
<td style="text-align:right;">
2.690000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
60103
</td>
<td style="text-align:right;">
1.338981e+10
</td>
<td style="text-align:right;">
34.7438753
</td>
<td style="text-align:right;">
84.40
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4472
</td>
<td style="text-align:left;">
36065
</td>
<td style="text-align:left;">
2019-12-19
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.987899e+04
</td>
<td style="text-align:right;">
69878.99
</td>
<td style="text-align:right;">
7.116400e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
232037
</td>
<td style="text-align:right;">
4.466933e+10
</td>
<td style="text-align:right;">
72.0330894
</td>
<td style="text-align:right;">
89.62
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4472
</td>
<td style="text-align:left;">
36075
</td>
<td style="text-align:left;">
2019-12-19
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.467824e+04
</td>
<td style="text-align:right;">
14678.24
</td>
<td style="text-align:right;">
1.578900e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
117511
</td>
<td style="text-align:right;">
2.311243e+10
</td>
<td style="text-align:right;">
28.7305122
</td>
<td style="text-align:right;">
83.67
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37031
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.620020e+05
</td>
<td style="text-align:right;">
162002.05
</td>
<td style="text-align:right;">
3.329200e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
67625
</td>
<td style="text-align:right;">
1.740755e+10
</td>
<td style="text-align:right;">
97.1046771
</td>
<td style="text-align:right;">
83.10
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37049
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.245355e+05
</td>
<td style="text-align:right;">
424535.48
</td>
<td style="text-align:right;">
7.320000e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
100042
</td>
<td style="text-align:right;">
1.736429e+10
</td>
<td style="text-align:right;">
96.4047089
</td>
<td style="text-align:right;">
65.63
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4412
</td>
<td style="text-align:left;">
37055
</td>
<td style="text-align:left;">
2019-01-31
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2.888472e+05
</td>
<td style="text-align:right;">
288847.15
</td>
<td style="text-align:right;">
4.965800e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
36860
</td>
<td style="text-align:right;">
1.259747e+10
</td>
<td style="text-align:right;">
95.4820235
</td>
<td style="text-align:right;">
99.59
</td>
<td style="text-align:left;">
TROPICAL STORM MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37055
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
3.087902e+06
</td>
<td style="text-align:right;">
3087902.38
</td>
<td style="text-align:right;">
9.388458e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
36860
</td>
<td style="text-align:right;">
1.259747e+10
</td>
<td style="text-align:right;">
95.4820235
</td>
<td style="text-align:right;">
99.59
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37095
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.121502e+06
</td>
<td style="text-align:right;">
3121502.49
</td>
<td style="text-align:right;">
4.666667e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
4573
</td>
<td style="text-align:right;">
1.146907e+09
</td>
<td style="text-align:right;">
82.5962456
</td>
<td style="text-align:right;">
35.84
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37107
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.053932e+04
</td>
<td style="text-align:right;">
10539.32
</td>
<td style="text-align:right;">
7.128000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
55112
</td>
<td style="text-align:right;">
1.175561e+10
</td>
<td style="text-align:right;">
91.7912822
</td>
<td style="text-align:right;">
52.51
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37177
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.772494e+05
</td>
<td style="text-align:right;">
877249.43
</td>
<td style="text-align:right;">
9.887500e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
3245
</td>
<td style="text-align:right;">
1.018921e+09
</td>
<td style="text-align:right;">
74.2920776
</td>
<td style="text-align:right;">
39.12
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4412
</td>
<td style="text-align:left;">
37183
</td>
<td style="text-align:left;">
2019-01-31
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.348062e+06
</td>
<td style="text-align:right;">
1348061.53
</td>
<td style="text-align:right;">
3.384440e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
1128357
</td>
<td style="text-align:right;">
2.152861e+11
</td>
<td style="text-align:right;">
93.6366529
</td>
<td style="text-align:right;">
72.02
</td>
<td style="text-align:left;">
TROPICAL STORM MICHAEL
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4465
</td>
<td style="text-align:left;">
37183
</td>
<td style="text-align:left;">
2019-10-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.650087e+05
</td>
<td style="text-align:right;">
865008.67
</td>
<td style="text-align:right;">
1.400000e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
1128357
</td>
<td style="text-align:right;">
2.152861e+11
</td>
<td style="text-align:right;">
93.6366529
</td>
<td style="text-align:right;">
72.02
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4447
</td>
<td style="text-align:left;">
39057
</td>
<td style="text-align:left;">
2019-06-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.769586e+04
</td>
<td style="text-align:right;">
37695.86
</td>
<td style="text-align:right;">
5.000000e+00
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
167939
</td>
<td style="text-align:right;">
3.290457e+10
</td>
<td style="text-align:right;">
64.2061724
</td>
<td style="text-align:right;">
87.49
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDE
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4447
</td>
<td style="text-align:left;">
39107
</td>
<td style="text-align:left;">
2019-06-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.019599e+04
</td>
<td style="text-align:right;">
20195.99
</td>
<td style="text-align:right;">
1.232000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
42522
</td>
<td style="text-align:right;">
1.348284e+10
</td>
<td style="text-align:right;">
57.6519249
</td>
<td style="text-align:right;">
97.33
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDE
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4447
</td>
<td style="text-align:left;">
39113
</td>
<td style="text-align:left;">
2019-06-18
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2.472149e+06
</td>
<td style="text-align:right;">
2472148.94
</td>
<td style="text-align:right;">
6.668112e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
537193
</td>
<td style="text-align:right;">
9.945173e+10
</td>
<td style="text-align:right;">
86.0960865
</td>
<td style="text-align:right;">
76.00
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDE
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40017
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.616910e+03
</td>
<td style="text-align:right;">
8616.91
</td>
<td style="text-align:right;">
6.840000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
153947
</td>
<td style="text-align:right;">
2.257507e+10
</td>
<td style="text-align:right;">
87.7823735
</td>
<td style="text-align:right;">
69.06
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40101
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.644650e+05
</td>
<td style="text-align:right;">
264465.01
</td>
<td style="text-align:right;">
3.467000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
66200
</td>
<td style="text-align:right;">
1.142326e+10
</td>
<td style="text-align:right;">
83.1689469
</td>
<td style="text-align:right;">
40.48
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40109
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.379840e+04
</td>
<td style="text-align:right;">
83798.40
</td>
<td style="text-align:right;">
1.000000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
795336
</td>
<td style="text-align:right;">
1.289742e+11
</td>
<td style="text-align:right;">
97.2637607
</td>
<td style="text-align:right;">
45.86
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40113
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.286860e+04
</td>
<td style="text-align:right;">
22868.60
</td>
<td style="text-align:right;">
8.850000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
45705
</td>
<td style="text-align:right;">
6.963588e+09
</td>
<td style="text-align:right;">
73.7511931
</td>
<td style="text-align:right;">
37.14
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40131
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.210613e+04
</td>
<td style="text-align:right;">
32106.13
</td>
<td style="text-align:right;">
5.700000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
95011
</td>
<td style="text-align:right;">
1.477543e+10
</td>
<td style="text-align:right;">
80.5281578
</td>
<td style="text-align:right;">
65.12
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40135
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.789225e+04
</td>
<td style="text-align:right;">
87892.25
</td>
<td style="text-align:right;">
4.500000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
39237
</td>
<td style="text-align:right;">
4.932920e+09
</td>
<td style="text-align:right;">
77.0601336
</td>
<td style="text-align:right;">
7.10
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
40143
</td>
<td style="text-align:left;">
2019-06-01
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.772521e+05
</td>
<td style="text-align:right;">
377252.08
</td>
<td style="text-align:right;">
5.694400e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
668481
</td>
<td style="text-align:right;">
1.157878e+11
</td>
<td style="text-align:right;">
96.6274260
</td>
<td style="text-align:right;">
53.69
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4452
</td>
<td style="text-align:left;">
41059
</td>
<td style="text-align:left;">
2019-07-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.186247e+04
</td>
<td style="text-align:right;">
51862.47
</td>
<td style="text-align:right;">
1.744720e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
79888
</td>
<td style="text-align:right;">
1.861261e+10
</td>
<td style="text-align:right;">
76.5510659
</td>
<td style="text-align:right;">
23.52
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4464
</td>
<td style="text-align:left;">
45051
</td>
<td style="text-align:left;">
2019-09-30
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.850597e+05
</td>
<td style="text-align:right;">
185059.66
</td>
<td style="text-align:right;">
6.648000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
348574
</td>
<td style="text-align:right;">
6.455071e+10
</td>
<td style="text-align:right;">
98.9818645
</td>
<td style="text-align:right;">
61.30
</td>
<td style="text-align:left;">
HURRICANE DORIAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4440
</td>
<td style="text-align:left;">
46035
</td>
<td style="text-align:left;">
2019-06-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.642263e+04
</td>
<td style="text-align:right;">
66422.63
</td>
<td style="text-align:right;">
4.440000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
19950
</td>
<td style="text-align:right;">
5.204129e+09
</td>
<td style="text-align:right;">
63.6334712
</td>
<td style="text-align:right;">
87.78
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, SNOWSTORM, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4469
</td>
<td style="text-align:left;">
46061
</td>
<td style="text-align:left;">
2019-11-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.040610e+03
</td>
<td style="text-align:right;">
4040.61
</td>
<td style="text-align:right;">
2.100000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
3461
</td>
<td style="text-align:right;">
1.064278e+09
</td>
<td style="text-align:right;">
12.7903277
</td>
<td style="text-align:right;">
96.02
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4440
</td>
<td style="text-align:left;">
46065
</td>
<td style="text-align:left;">
2019-06-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.311930e+04
</td>
<td style="text-align:right;">
73119.30
</td>
<td style="text-align:right;">
2.600000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
17751
</td>
<td style="text-align:right;">
3.816304e+09
</td>
<td style="text-align:right;">
47.1205854
</td>
<td style="text-align:right;">
85.90
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, SNOWSTORM, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4469
</td>
<td style="text-align:left;">
46087
</td>
<td style="text-align:left;">
2019-11-18
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.704700e+03
</td>
<td style="text-align:right;">
9704.70
</td>
<td style="text-align:right;">
5.400000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
5682
</td>
<td style="text-align:right;">
1.196086e+10
</td>
<td style="text-align:right;">
69.1059497
</td>
<td style="text-align:right;">
88.70
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4440
</td>
<td style="text-align:left;">
46099
</td>
<td style="text-align:left;">
2019-06-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.320918e+05
</td>
<td style="text-align:right;">
132091.75
</td>
<td style="text-align:right;">
1.093400e+04
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
196891
</td>
<td style="text-align:right;">
3.733547e+10
</td>
<td style="text-align:right;">
92.5867006
</td>
<td style="text-align:right;">
93.13
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, SNOWSTORM, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4427
</td>
<td style="text-align:left;">
47071
</td>
<td style="text-align:left;">
2019-04-17
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.440183e+04
</td>
<td style="text-align:right;">
74401.83
</td>
<td style="text-align:right;">
1.376620e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
26745
</td>
<td style="text-align:right;">
4.834413e+09
</td>
<td style="text-align:right;">
52.4021635
</td>
<td style="text-align:right;">
34.09
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4427
</td>
<td style="text-align:left;">
47093
</td>
<td style="text-align:left;">
2019-04-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.079972e+04
</td>
<td style="text-align:right;">
10799.72
</td>
<td style="text-align:right;">
3.140000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
478842
</td>
<td style="text-align:right;">
8.234800e+10
</td>
<td style="text-align:right;">
89.4368438
</td>
<td style="text-align:right;">
62.22
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4471
</td>
<td style="text-align:left;">
47125
</td>
<td style="text-align:left;">
2019-12-06
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.461950e+06
</td>
<td style="text-align:right;">
1461949.54
</td>
<td style="text-align:right;">
6.600000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
220041
</td>
<td style="text-align:right;">
3.013971e+10
</td>
<td style="text-align:right;">
85.5870188
</td>
<td style="text-align:right;">
50.54
</td>
<td style="text-align:left;">
SEVERE STORM AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4427
</td>
<td style="text-align:left;">
47143
</td>
<td style="text-align:left;">
2019-04-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.096225e+04
</td>
<td style="text-align:right;">
20962.25
</td>
<td style="text-align:right;">
1.600000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
32839
</td>
<td style="text-align:right;">
5.385350e+09
</td>
<td style="text-align:right;">
49.1250398
</td>
<td style="text-align:right;">
34.88
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4416
</td>
<td style="text-align:left;">
48053
</td>
<td style="text-align:left;">
2019-02-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.982315e+05
</td>
<td style="text-align:right;">
298231.52
</td>
<td style="text-align:right;">
5.002000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
48986
</td>
<td style="text-align:right;">
9.610927e+09
</td>
<td style="text-align:right;">
80.1463570
</td>
<td style="text-align:right;">
42.39
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4416
</td>
<td style="text-align:left;">
48207
</td>
<td style="text-align:left;">
2019-02-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.967730e+03
</td>
<td style="text-align:right;">
4967.73
</td>
<td style="text-align:right;">
2.790000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
5414
</td>
<td style="text-align:right;">
2.023697e+09
</td>
<td style="text-align:right;">
23.1307668
</td>
<td style="text-align:right;">
20.24
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4416
</td>
<td style="text-align:left;">
48267
</td>
<td style="text-align:left;">
2019-02-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.224471e+04
</td>
<td style="text-align:right;">
12244.71
</td>
<td style="text-align:right;">
1.900000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
4271
</td>
<td style="text-align:right;">
2.532014e+09
</td>
<td style="text-align:right;">
46.0388164
</td>
<td style="text-align:right;">
21.29
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4416
</td>
<td style="text-align:left;">
48271
</td>
<td style="text-align:left;">
2019-02-25
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.613436e+04
</td>
<td style="text-align:right;">
36134.36
</td>
<td style="text-align:right;">
3.330000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
3126
</td>
<td style="text-align:right;">
5.383794e+08
</td>
<td style="text-align:right;">
1.5590200
</td>
<td style="text-align:right;">
1.05
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4416
</td>
<td style="text-align:left;">
48299
</td>
<td style="text-align:left;">
2019-02-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.316702e+06
</td>
<td style="text-align:right;">
1316701.55
</td>
<td style="text-align:right;">
6.616000e+03
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
21199
</td>
<td style="text-align:right;">
5.462515e+09
</td>
<td style="text-align:right;">
68.5650652
</td>
<td style="text-align:right;">
19.92
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4416
</td>
<td style="text-align:left;">
48453
</td>
<td style="text-align:left;">
2019-02-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.416851e+05
</td>
<td style="text-align:right;">
341685.14
</td>
<td style="text-align:right;">
7.812000e+02
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
1285769
</td>
<td style="text-align:right;">
1.895386e+11
</td>
<td style="text-align:right;">
96.8819599
</td>
<td style="text-align:right;">
56.75
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4418
</td>
<td style="text-align:left;">
53009
</td>
<td style="text-align:left;">
2019-03-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.171479e+04
</td>
<td style="text-align:right;">
11714.79
</td>
<td style="text-align:right;">
2.000000e+01
</td>
<td style="text-align:right;">
2019
</td>
<td style="text-align:left;">
77054
</td>
<td style="text-align:right;">
1.511825e+10
</td>
<td style="text-align:right;">
87.4960229
</td>
<td style="text-align:right;">
56.37
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES,
MUDSLIDES, TORNADO
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4563
</td>
<td style="text-align:left;">
01003
</td>
<td style="text-align:left;">
2020-09-20
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
6.077548e+07
</td>
<td style="text-align:right;">
60775477.28
</td>
<td style="text-align:right;">
3.868044e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
231365
</td>
<td style="text-align:right;">
4.596848e+10
</td>
<td style="text-align:right;">
97.7091950
</td>
<td style="text-align:right;">
86.12
</td>
<td style="text-align:left;">
HURRICANE SALLY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4554
</td>
<td style="text-align:left;">
01037
</td>
<td style="text-align:left;">
2020-07-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.173505e+04
</td>
<td style="text-align:right;">
1000.00
</td>
<td style="text-align:right;">
2.200000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
10320
</td>
<td style="text-align:right;">
1.954999e+09
</td>
<td style="text-align:right;">
7.5087496
</td>
<td style="text-align:right;">
21.71
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4554
</td>
<td style="text-align:left;">
01041
</td>
<td style="text-align:left;">
2020-07-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.267207e+05
</td>
<td style="text-align:right;">
126720.69
</td>
<td style="text-align:right;">
2.880000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
13179
</td>
<td style="text-align:right;">
2.563555e+09
</td>
<td style="text-align:right;">
46.2615336
</td>
<td style="text-align:right;">
21.20
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4555
</td>
<td style="text-align:left;">
01055
</td>
<td style="text-align:left;">
2020-07-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.220261e+05
</td>
<td style="text-align:right;">
322026.12
</td>
<td style="text-align:right;">
7.111200e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
103320
</td>
<td style="text-align:right;">
1.900045e+10
</td>
<td style="text-align:right;">
82.2462615
</td>
<td style="text-align:right;">
53.66
</td>
<td style="text-align:left;">
SEVERE THUNDERSTORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4555
</td>
<td style="text-align:left;">
01095
</td>
<td style="text-align:left;">
2020-07-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.102030e+05
</td>
<td style="text-align:right;">
310203.01
</td>
<td style="text-align:right;">
5.422000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
97552
</td>
<td style="text-align:right;">
1.736259e+10
</td>
<td style="text-align:right;">
85.1734012
</td>
<td style="text-align:right;">
40.39
</td>
<td style="text-align:left;">
SEVERE THUNDERSTORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4563
</td>
<td style="text-align:left;">
01097
</td>
<td style="text-align:left;">
2020-09-20
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1.465248e+07
</td>
<td style="text-align:right;">
14652476.51
</td>
<td style="text-align:right;">
6.677400e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
414514
</td>
<td style="text-align:right;">
7.602302e+10
</td>
<td style="text-align:right;">
98.7909640
</td>
<td style="text-align:right;">
74.60
</td>
<td style="text-align:left;">
HURRICANE SALLY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4573
</td>
<td style="text-align:left;">
01097
</td>
<td style="text-align:left;">
2020-12-10
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.170848e+06
</td>
<td style="text-align:right;">
6170848.44
</td>
<td style="text-align:right;">
4.713560e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
414514
</td>
<td style="text-align:right;">
7.602302e+10
</td>
<td style="text-align:right;">
98.7909640
</td>
<td style="text-align:right;">
74.60
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4563
</td>
<td style="text-align:left;">
01101
</td>
<td style="text-align:left;">
2020-09-20
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.361660e+07
</td>
<td style="text-align:right;">
13616603.44
</td>
<td style="text-align:right;">
6.238100e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
228847
</td>
<td style="text-align:right;">
4.622347e+10
</td>
<td style="text-align:right;">
86.2551702
</td>
<td style="text-align:right;">
67.92
</td>
<td style="text-align:left;">
HURRICANE SALLY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4573
</td>
<td style="text-align:left;">
01101
</td>
<td style="text-align:left;">
2020-12-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.884084e+06
</td>
<td style="text-align:right;">
6884084.00
</td>
<td style="text-align:right;">
3.200000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
228847
</td>
<td style="text-align:right;">
4.622347e+10
</td>
<td style="text-align:right;">
86.2551702
</td>
<td style="text-align:right;">
67.92
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4554
</td>
<td style="text-align:left;">
01109
</td>
<td style="text-align:left;">
2020-07-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.247833e+05
</td>
<td style="text-align:right;">
324783.32
</td>
<td style="text-align:right;">
1.250000e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
32965
</td>
<td style="text-align:right;">
6.512703e+09
</td>
<td style="text-align:right;">
63.2516704
</td>
<td style="text-align:right;">
49.52
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4544
</td>
<td style="text-align:left;">
05031
</td>
<td style="text-align:left;">
2020-05-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.296069e+05
</td>
<td style="text-align:right;">
229606.89
</td>
<td style="text-align:right;">
1.046000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
111202
</td>
<td style="text-align:right;">
1.973483e+10
</td>
<td style="text-align:right;">
92.3321667
</td>
<td style="text-align:right;">
52.10
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4556
</td>
<td style="text-align:left;">
05107
</td>
<td style="text-align:left;">
2020-07-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.386589e+05
</td>
<td style="text-align:right;">
138658.92
</td>
<td style="text-align:right;">
1.200000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
16523
</td>
<td style="text-align:right;">
3.246568e+09
</td>
<td style="text-align:right;">
65.2879415
</td>
<td style="text-align:right;">
32.27
</td>
<td style="text-align:left;">
SEVERE STORMS AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4544
</td>
<td style="text-align:left;">
05119
</td>
<td style="text-align:left;">
2020-05-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.737772e+04
</td>
<td style="text-align:right;">
57377.72
</td>
<td style="text-align:right;">
2.470000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
398830
</td>
<td style="text-align:right;">
7.392333e+10
</td>
<td style="text-align:right;">
95.0684060
</td>
<td style="text-align:right;">
73.68
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4558
</td>
<td style="text-align:left;">
06053
</td>
<td style="text-align:left;">
2020-08-22
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.490270e+03
</td>
<td style="text-align:right;">
1490.27
</td>
<td style="text-align:right;">
4.000000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
438599
</td>
<td style="text-align:right;">
7.640694e+10
</td>
<td style="text-align:right;">
98.3773465
</td>
<td style="text-align:right;">
28.90
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4558
</td>
<td style="text-align:left;">
06107
</td>
<td style="text-align:left;">
2020-08-22
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.269005e+04
</td>
<td style="text-align:right;">
12690.05
</td>
<td style="text-align:right;">
1.069444e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
472585
</td>
<td style="text-align:right;">
7.447368e+10
</td>
<td style="text-align:right;">
95.7683742
</td>
<td style="text-align:right;">
12.83
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4566
</td>
<td style="text-align:left;">
10001
</td>
<td style="text-align:left;">
2020-10-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.155000e+04
</td>
<td style="text-align:right;">
41550.00
</td>
<td style="text-align:right;">
1.069444e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
181705
</td>
<td style="text-align:right;">
3.268465e+10
</td>
<td style="text-align:right;">
84.7597836
</td>
<td style="text-align:right;">
72.95
</td>
<td style="text-align:left;">
TROPICAL STORM ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4566
</td>
<td style="text-align:left;">
10003
</td>
<td style="text-align:left;">
2020-10-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.055757e+05
</td>
<td style="text-align:right;">
305575.72
</td>
<td style="text-align:right;">
0.000000e+00
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
570089
</td>
<td style="text-align:right;">
1.221791e+11
</td>
<td style="text-align:right;">
90.7731467
</td>
<td style="text-align:right;">
83.26
</td>
<td style="text-align:left;">
TROPICAL STORM ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4564
</td>
<td style="text-align:left;">
12033
</td>
<td style="text-align:left;">
2020-09-23
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.909541e+07
</td>
<td style="text-align:right;">
29095409.43
</td>
<td style="text-align:right;">
2.338336e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
321367
</td>
<td style="text-align:right;">
5.111437e+10
</td>
<td style="text-align:right;">
97.8682787
</td>
<td style="text-align:right;">
45.51
</td>
<td style="text-align:left;">
HURRICANE SALLY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4564
</td>
<td style="text-align:left;">
12091
</td>
<td style="text-align:left;">
2020-09-23
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.797345e+05
</td>
<td style="text-align:right;">
379734.48
</td>
<td style="text-align:right;">
1.500740e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
211293
</td>
<td style="text-align:right;">
3.597393e+10
</td>
<td style="text-align:right;">
97.4228444
</td>
<td style="text-align:right;">
45.23
</td>
<td style="text-align:left;">
HURRICANE SALLY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4564
</td>
<td style="text-align:left;">
12113
</td>
<td style="text-align:left;">
2020-09-23
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1.126164e+07
</td>
<td style="text-align:right;">
11261636.35
</td>
<td style="text-align:right;">
1.832020e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
187714
</td>
<td style="text-align:right;">
2.541006e+10
</td>
<td style="text-align:right;">
95.5138403
</td>
<td style="text-align:right;">
53.44
</td>
<td style="text-align:left;">
HURRICANE SALLY
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19011
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
9.713339e+05
</td>
<td style="text-align:right;">
831333.87
</td>
<td style="text-align:right;">
9.875060e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
25558
</td>
<td style="text-align:right;">
6.813448e+09
</td>
<td style="text-align:right;">
48.2978046
</td>
<td style="text-align:right;">
96.63
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19091
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.134500e+04
</td>
<td style="text-align:right;">
41345.00
</td>
<td style="text-align:right;">
3.083050e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
9597
</td>
<td style="text-align:right;">
2.200834e+09
</td>
<td style="text-align:right;">
29.6531976
</td>
<td style="text-align:right;">
84.66
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19113
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
3.943399e+07
</td>
<td style="text-align:right;">
39044990.73
</td>
<td style="text-align:right;">
3.049568e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
230253
</td>
<td style="text-align:right;">
4.843057e+10
</td>
<td style="text-align:right;">
91.7276487
</td>
<td style="text-align:right;">
99.24
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19127
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.989031e+04
</td>
<td style="text-align:right;">
29890.31
</td>
<td style="text-align:right;">
6.166100e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
40092
</td>
<td style="text-align:right;">
1.087537e+10
</td>
<td style="text-align:right;">
68.8514158
</td>
<td style="text-align:right;">
68.52
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19153
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.364581e+05
</td>
<td style="text-align:right;">
136458.10
</td>
<td style="text-align:right;">
5.170000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
492316
</td>
<td style="text-align:right;">
9.393685e+10
</td>
<td style="text-align:right;">
91.5367483
</td>
<td style="text-align:right;">
95.58
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19169
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.224314e+05
</td>
<td style="text-align:right;">
143431.44
</td>
<td style="text-align:right;">
8.834000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
98533
</td>
<td style="text-align:right;">
1.981740e+10
</td>
<td style="text-align:right;">
76.0101814
</td>
<td style="text-align:right;">
88.89
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4557
</td>
<td style="text-align:left;">
19171
</td>
<td style="text-align:left;">
2020-08-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.856669e+06
</td>
<td style="text-align:right;">
1831668.66
</td>
<td style="text-align:right;">
1.224500e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
17081
</td>
<td style="text-align:right;">
4.591618e+09
</td>
<td style="text-align:right;">
52.7203309
</td>
<td style="text-align:right;">
72.60
</td>
<td style="text-align:left;">
SEVERE STORMS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4561
</td>
<td style="text-align:left;">
19171
</td>
<td style="text-align:left;">
2020-09-10
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.090607e+06
</td>
<td style="text-align:right;">
2045303.40
</td>
<td style="text-align:right;">
9.537600e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
17081
</td>
<td style="text-align:right;">
4.591618e+09
</td>
<td style="text-align:right;">
52.7203309
</td>
<td style="text-align:right;">
72.60
</td>
<td style="text-align:left;">
SEVERE STORMS AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22001
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.386554e+06
</td>
<td style="text-align:right;">
3916511.30
</td>
<td style="text-align:right;">
1.418255e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
57475
</td>
<td style="text-align:right;">
1.082074e+10
</td>
<td style="text-align:right;">
90.5186128
</td>
<td style="text-align:right;">
29.76
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22003
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
1.616527e+07
</td>
<td style="text-align:right;">
16165273.67
</td>
<td style="text-align:right;">
1.394995e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
22708
</td>
<td style="text-align:right;">
3.900954e+09
</td>
<td style="text-align:right;">
64.0470888
</td>
<td style="text-align:right;">
30.68
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22011
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.026766e+07
</td>
<td style="text-align:right;">
10267664.01
</td>
<td style="text-align:right;">
7.287010e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
36508
</td>
<td style="text-align:right;">
6.482271e+09
</td>
<td style="text-align:right;">
71.8421890
</td>
<td style="text-align:right;">
32.24
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22019
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
44
</td>
<td style="text-align:right;">
35
</td>
<td style="text-align:right;">
35
</td>
<td style="text-align:right;">
6.907878e+08
</td>
<td style="text-align:right;">
555991104\.37
</td>
<td style="text-align:right;">
5.851407e+06
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
216652
</td>
<td style="text-align:right;">
4.227924e+10
</td>
<td style="text-align:right;">
96.9137766
</td>
<td style="text-align:right;">
32.40
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22023
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.295674e+08
</td>
<td style="text-align:right;">
64864742.74
</td>
<td style="text-align:right;">
1.603246e+06
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
5615
</td>
<td style="text-align:right;">
1.108910e+09
</td>
<td style="text-align:right;">
55.8065542
</td>
<td style="text-align:right;">
95.04
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22033
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
2.262258e+08
</td>
<td style="text-align:right;">
116704985\.16
</td>
<td style="text-align:right;">
6.778400e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
456512
</td>
<td style="text-align:right;">
1.120657e+11
</td>
<td style="text-align:right;">
98.8864143
</td>
<td style="text-align:right;">
60.09
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4570
</td>
<td style="text-align:left;">
22033
</td>
<td style="text-align:left;">
2020-10-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.712772e+05
</td>
<td style="text-align:right;">
271277.18
</td>
<td style="text-align:right;">
5.800000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
456512
</td>
<td style="text-align:right;">
1.120657e+11
</td>
<td style="text-align:right;">
98.8864143
</td>
<td style="text-align:right;">
60.09
</td>
<td style="text-align:left;">
HURRICANE DELTA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22043
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.588890e+07
</td>
<td style="text-align:right;">
15888904.87
</td>
<td style="text-align:right;">
4.963000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
22122
</td>
<td style="text-align:right;">
3.090163e+09
</td>
<td style="text-align:right;">
33.6939230
</td>
<td style="text-align:right;">
20.78
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22053
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7.302015e+06
</td>
<td style="text-align:right;">
7302015.49
</td>
<td style="text-align:right;">
3.913800e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
32235
</td>
<td style="text-align:right;">
5.698292e+09
</td>
<td style="text-align:right;">
88.4505250
</td>
<td style="text-align:right;">
30.11
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22055
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.092992e+04
</td>
<td style="text-align:right;">
60929.92
</td>
<td style="text-align:right;">
2.462600e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
241618
</td>
<td style="text-align:right;">
4.511250e+10
</td>
<td style="text-align:right;">
97.9637289
</td>
<td style="text-align:right;">
64.35
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4570
</td>
<td style="text-align:left;">
22055
</td>
<td style="text-align:left;">
2020-10-16
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
7.435385e+06
</td>
<td style="text-align:right;">
7435385.23
</td>
<td style="text-align:right;">
9.698600e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
241618
</td>
<td style="text-align:right;">
4.511250e+10
</td>
<td style="text-align:right;">
97.9637289
</td>
<td style="text-align:right;">
64.35
</td>
<td style="text-align:left;">
HURRICANE DELTA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22073
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3.422428e+06
</td>
<td style="text-align:right;">
3422427.84
</td>
<td style="text-align:right;">
1.416077e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
160298
</td>
<td style="text-align:right;">
2.846186e+10
</td>
<td style="text-align:right;">
85.6188355
</td>
<td style="text-align:right;">
69.29
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22079
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.254576e+06
</td>
<td style="text-align:right;">
4254575.96
</td>
<td style="text-align:right;">
5.706400e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
129664
</td>
<td style="text-align:right;">
2.330566e+10
</td>
<td style="text-align:right;">
83.3916640
</td>
<td style="text-align:right;">
71.26
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4570
</td>
<td style="text-align:left;">
22079
</td>
<td style="text-align:left;">
2020-10-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.058359e+04
</td>
<td style="text-align:right;">
70583.59
</td>
<td style="text-align:right;">
2.468800e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
129664
</td>
<td style="text-align:right;">
2.330566e+10
</td>
<td style="text-align:right;">
83.3916640
</td>
<td style="text-align:right;">
71.26
</td>
<td style="text-align:left;">
HURRICANE DELTA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4570
</td>
<td style="text-align:left;">
22099
</td>
<td style="text-align:left;">
2020-10-16
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.980032e+05
</td>
<td style="text-align:right;">
898003.24
</td>
<td style="text-align:right;">
7.142000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
51686
</td>
<td style="text-align:right;">
8.428679e+09
</td>
<td style="text-align:right;">
91.3140312
</td>
<td style="text-align:right;">
75.75
</td>
<td style="text-align:left;">
HURRICANE DELTA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22113
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.045695e+06
</td>
<td style="text-align:right;">
6045694.95
</td>
<td style="text-align:right;">
2.477365e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
57317
</td>
<td style="text-align:right;">
7.237194e+09
</td>
<td style="text-align:right;">
89.8504613
</td>
<td style="text-align:right;">
70.69
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4570
</td>
<td style="text-align:left;">
22113
</td>
<td style="text-align:left;">
2020-10-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.183000e+03
</td>
<td style="text-align:right;">
6183.00
</td>
<td style="text-align:right;">
7.000000e+00
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
57317
</td>
<td style="text-align:right;">
7.237194e+09
</td>
<td style="text-align:right;">
89.8504613
</td>
<td style="text-align:right;">
70.69
</td>
<td style="text-align:left;">
HURRICANE DELTA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22115
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.323466e+07
</td>
<td style="text-align:right;">
13204536.75
</td>
<td style="text-align:right;">
3.839000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
48626
</td>
<td style="text-align:right;">
7.312746e+09
</td>
<td style="text-align:right;">
73.3057588
</td>
<td style="text-align:right;">
14.13
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4559
</td>
<td style="text-align:left;">
22127
</td>
<td style="text-align:left;">
2020-08-28
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.709224e+06
</td>
<td style="text-align:right;">
2709224.08
</td>
<td style="text-align:right;">
1.168000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
13718
</td>
<td style="text-align:right;">
2.335246e+09
</td>
<td style="text-align:right;">
22.1444480
</td>
<td style="text-align:right;">
37.84
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4547
</td>
<td style="text-align:left;">
26051
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.552391e+04
</td>
<td style="text-align:right;">
35523.91
</td>
<td style="text-align:right;">
8.000000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
25371
</td>
<td style="text-align:right;">
6.556040e+09
</td>
<td style="text-align:right;">
15.1447661
</td>
<td style="text-align:right;">
52.45
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Dam/Levee Break
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4547
</td>
<td style="text-align:left;">
26111
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.983990e+03
</td>
<td style="text-align:right;">
3983.99
</td>
<td style="text-align:right;">
2.240000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
83456
</td>
<td style="text-align:right;">
1.828456e+10
</td>
<td style="text-align:right;">
64.9061406
</td>
<td style="text-align:right;">
94.05
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Dam/Levee Break
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4547
</td>
<td style="text-align:left;">
26145
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.130423e+05
</td>
<td style="text-align:right;">
113042.31
</td>
<td style="text-align:right;">
1.027300e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
190067
</td>
<td style="text-align:right;">
3.667700e+10
</td>
<td style="text-align:right;">
79.0327712
</td>
<td style="text-align:right;">
91.63
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Dam/Levee Break
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4478
</td>
<td style="text-align:left;">
28011
</td>
<td style="text-align:left;">
2020-03-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.523330e+03
</td>
<td style="text-align:right;">
5523.33
</td>
<td style="text-align:right;">
5.700000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
30953
</td>
<td style="text-align:right;">
8.264811e+09
</td>
<td style="text-align:right;">
74.5147948
</td>
<td style="text-align:right;">
31.09
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28031
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.324141e+06
</td>
<td style="text-align:right;">
4324140.81
</td>
<td style="text-align:right;">
9.209000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
18330
</td>
<td style="text-align:right;">
2.645445e+09
</td>
<td style="text-align:right;">
62.5198855
</td>
<td style="text-align:right;">
12.73
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4478
</td>
<td style="text-align:left;">
28033
</td>
<td style="text-align:left;">
2020-03-12
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.221308e+07
</td>
<td style="text-align:right;">
10737694.08
</td>
<td style="text-align:right;">
8.400000e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
185280
</td>
<td style="text-align:right;">
3.055907e+10
</td>
<td style="text-align:right;">
90.8367801
</td>
<td style="text-align:right;">
58.85
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4576
</td>
<td style="text-align:left;">
28035
</td>
<td style="text-align:left;">
2020-12-31
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.132038e+04
</td>
<td style="text-align:right;">
11320.38
</td>
<td style="text-align:right;">
3.000000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
78050
</td>
<td style="text-align:right;">
1.342654e+10
</td>
<td style="text-align:right;">
86.5415208
</td>
<td style="text-align:right;">
59.10
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4576
</td>
<td style="text-align:left;">
28039
</td>
<td style="text-align:left;">
2020-12-31
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.130316e+05
</td>
<td style="text-align:right;">
813031.57
</td>
<td style="text-align:right;">
4.674000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
24246
</td>
<td style="text-align:right;">
3.603370e+09
</td>
<td style="text-align:right;">
87.0505886
</td>
<td style="text-align:right;">
21.13
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4576
</td>
<td style="text-align:left;">
28045
</td>
<td style="text-align:left;">
2020-12-31
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
5.240651e+06
</td>
<td style="text-align:right;">
5240651.43
</td>
<td style="text-align:right;">
1.685384e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
46025
</td>
<td style="text-align:right;">
8.381827e+09
</td>
<td style="text-align:right;">
88.0687241
</td>
<td style="text-align:right;">
60.31
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4576
</td>
<td style="text-align:left;">
28047
</td>
<td style="text-align:left;">
2020-12-31
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
2.185101e+07
</td>
<td style="text-align:right;">
21851008.92
</td>
<td style="text-align:right;">
3.431663e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
208275
</td>
<td style="text-align:right;">
3.732934e+10
</td>
<td style="text-align:right;">
97.2001273
</td>
<td style="text-align:right;">
67.41
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28049
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.524624e+06
</td>
<td style="text-align:right;">
2524623.54
</td>
<td style="text-align:right;">
3.000000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
227510
</td>
<td style="text-align:right;">
3.931969e+10
</td>
<td style="text-align:right;">
92.5548839
</td>
<td style="text-align:right;">
77.37
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4538
</td>
<td style="text-align:left;">
28049
</td>
<td style="text-align:left;">
2020-04-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.462838e+04
</td>
<td style="text-align:right;">
94628.38
</td>
<td style="text-align:right;">
1.104610e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
227510
</td>
<td style="text-align:right;">
3.931969e+10
</td>
<td style="text-align:right;">
92.5548839
</td>
<td style="text-align:right;">
77.37
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, AND MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4576
</td>
<td style="text-align:left;">
28059
</td>
<td style="text-align:left;">
2020-12-31
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.319181e+06
</td>
<td style="text-align:right;">
7319180.97
</td>
<td style="text-align:right;">
5.523345e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
143149
</td>
<td style="text-align:right;">
2.306976e+10
</td>
<td style="text-align:right;">
95.9910913
</td>
<td style="text-align:right;">
75.88
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28061
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.721371e+06
</td>
<td style="text-align:right;">
1721371.35
</td>
<td style="text-align:right;">
1.481800e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
16343
</td>
<td style="text-align:right;">
3.036074e+09
</td>
<td style="text-align:right;">
48.7750557
</td>
<td style="text-align:right;">
21.01
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28065
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.807595e+06
</td>
<td style="text-align:right;">
3807595.45
</td>
<td style="text-align:right;">
2.812100e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
11298
</td>
<td style="text-align:right;">
1.934606e+09
</td>
<td style="text-align:right;">
29.9395482
</td>
<td style="text-align:right;">
19.03
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28067
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.779986e+06
</td>
<td style="text-align:right;">
3779986.23
</td>
<td style="text-align:right;">
5.484920e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
67168
</td>
<td style="text-align:right;">
1.431544e+10
</td>
<td style="text-align:right;">
87.1460388
</td>
<td style="text-align:right;">
34.91
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4551
</td>
<td style="text-align:left;">
28067
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.849760e+05
</td>
<td style="text-align:right;">
884975.98
</td>
<td style="text-align:right;">
2.179000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
67168
</td>
<td style="text-align:right;">
1.431544e+10
</td>
<td style="text-align:right;">
87.1460388
</td>
<td style="text-align:right;">
34.91
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28077
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.800084e+05
</td>
<td style="text-align:right;">
580008.37
</td>
<td style="text-align:right;">
8.120000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
12013
</td>
<td style="text-align:right;">
2.409621e+09
</td>
<td style="text-align:right;">
46.2933503
</td>
<td style="text-align:right;">
5.70
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4551
</td>
<td style="text-align:left;">
28077
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.262454e+06
</td>
<td style="text-align:right;">
1262454.27
</td>
<td style="text-align:right;">
4.060000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
12013
</td>
<td style="text-align:right;">
2.409621e+09
</td>
<td style="text-align:right;">
46.2933503
</td>
<td style="text-align:right;">
5.70
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4551
</td>
<td style="text-align:left;">
28113
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.438483e+06
</td>
<td style="text-align:right;">
1438482.89
</td>
<td style="text-align:right;">
2.400000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
40285
</td>
<td style="text-align:right;">
7.208143e+09
</td>
<td style="text-align:right;">
77.2192173
</td>
<td style="text-align:right;">
23.81
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4576
</td>
<td style="text-align:left;">
28131
</td>
<td style="text-align:left;">
2020-12-31
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.625053e+06
</td>
<td style="text-align:right;">
1625052.75
</td>
<td style="text-align:right;">
1.302000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
18312
</td>
<td style="text-align:right;">
2.965992e+09
</td>
<td style="text-align:right;">
82.5326122
</td>
<td style="text-align:right;">
27.66
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4478
</td>
<td style="text-align:left;">
28135
</td>
<td style="text-align:left;">
2020-03-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.100000e+04
</td>
<td style="text-align:right;">
33000.00
</td>
<td style="text-align:right;">
2.194000e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
12658
</td>
<td style="text-align:right;">
1.655110e+09
</td>
<td style="text-align:right;">
45.2115813
</td>
<td style="text-align:right;">
9.87
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4536
</td>
<td style="text-align:left;">
28147
</td>
<td style="text-align:left;">
2020-04-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.336377e+05
</td>
<td style="text-align:right;">
933637.73
</td>
<td style="text-align:right;">
1.421300e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
13882
</td>
<td style="text-align:right;">
2.760510e+09
</td>
<td style="text-align:right;">
48.9341394
</td>
<td style="text-align:right;">
9.90
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4552
</td>
<td style="text-align:left;">
29143
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.512325e+04
</td>
<td style="text-align:right;">
65123.25
</td>
<td style="text-align:right;">
9.000000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
16354
</td>
<td style="text-align:right;">
3.863169e+09
</td>
<td style="text-align:right;">
82.8825962
</td>
<td style="text-align:right;">
28.36
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4574
</td>
<td style="text-align:left;">
34003
</td>
<td style="text-align:left;">
2020-12-11
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.672281e+05
</td>
<td style="text-align:right;">
167228.12
</td>
<td style="text-align:right;">
1.200000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
955459
</td>
<td style="text-align:right;">
1.836780e+11
</td>
<td style="text-align:right;">
98.6955138
</td>
<td style="text-align:right;">
62.64
</td>
<td style="text-align:left;">
TROPICAL STORM ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4567
</td>
<td style="text-align:left;">
36059
</td>
<td style="text-align:left;">
2020-10-02
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.931242e+05
</td>
<td style="text-align:right;">
493124.20
</td>
<td style="text-align:right;">
1.357000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
1395584
</td>
<td style="text-align:right;">
2.703602e+11
</td>
<td style="text-align:right;">
96.6592428
</td>
<td style="text-align:right;">
95.00
</td>
<td style="text-align:left;">
TROPICAL STORM ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4568
</td>
<td style="text-align:left;">
37015
</td>
<td style="text-align:left;">
2020-10-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.983166e+04
</td>
<td style="text-align:right;">
59831.66
</td>
<td style="text-align:right;">
6.000000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
17933
</td>
<td style="text-align:right;">
4.394099e+09
</td>
<td style="text-align:right;">
82.4371619
</td>
<td style="text-align:right;">
15.69
</td>
<td style="text-align:left;">
HURRICANE ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4568
</td>
<td style="text-align:left;">
37019
</td>
<td style="text-align:left;">
2020-10-14
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.274001e+06
</td>
<td style="text-align:right;">
2274000.90
</td>
<td style="text-align:right;">
5.887770e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
136245
</td>
<td style="text-align:right;">
2.994477e+10
</td>
<td style="text-align:right;">
97.0410436
</td>
<td style="text-align:right;">
64.93
</td>
<td style="text-align:left;">
HURRICANE ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4568
</td>
<td style="text-align:left;">
37129
</td>
<td style="text-align:left;">
2020-10-14
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.699046e+06
</td>
<td style="text-align:right;">
4699046.36
</td>
<td style="text-align:right;">
6.891000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
225344
</td>
<td style="text-align:right;">
5.126031e+10
</td>
<td style="text-align:right;">
98.5046134
</td>
<td style="text-align:right;">
82.27
</td>
<td style="text-align:left;">
HURRICANE ISAIAS
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4575
</td>
<td style="text-align:left;">
40051
</td>
<td style="text-align:left;">
2020-12-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.050575e+05
</td>
<td style="text-align:right;">
105057.50
</td>
<td style="text-align:right;">
7.200000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
54565
</td>
<td style="text-align:right;">
8.711522e+09
</td>
<td style="text-align:right;">
78.0782692
</td>
<td style="text-align:right;">
49.01
</td>
<td style="text-align:left;">
SEVERE WINTER STORM
</td>
<td style="text-align:left;">
Severe Ice Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4562
</td>
<td style="text-align:left;">
41039
</td>
<td style="text-align:left;">
2020-09-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.213909e+04
</td>
<td style="text-align:right;">
12139.09
</td>
<td style="text-align:right;">
3.700000e+01
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
382865
</td>
<td style="text-align:right;">
8.188156e+10
</td>
<td style="text-align:right;">
96.7865097
</td>
<td style="text-align:right;">
63.84
</td>
<td style="text-align:left;">
WILDFIRES AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4562
</td>
<td style="text-align:left;">
41047
</td>
<td style="text-align:left;">
2020-09-15
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
8.992413e+06
</td>
<td style="text-align:right;">
8992413.15
</td>
<td style="text-align:right;">
6.880040e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
344064
</td>
<td style="text-align:right;">
6.280993e+10
</td>
<td style="text-align:right;">
96.7546930
</td>
<td style="text-align:right;">
50.06
</td>
<td style="text-align:left;">
WILDFIRES AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4542
</td>
<td style="text-align:left;">
45073
</td>
<td style="text-align:left;">
2020-05-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.369333e+05
</td>
<td style="text-align:right;">
436933.33
</td>
<td style="text-align:right;">
8.550000e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
78560
</td>
<td style="text-align:right;">
1.733914e+10
</td>
<td style="text-align:right;">
65.8924594
</td>
<td style="text-align:right;">
40.04
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4542
</td>
<td style="text-align:left;">
45077
</td>
<td style="text-align:left;">
2020-05-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.228122e+04
</td>
<td style="text-align:right;">
22281.22
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
131341
</td>
<td style="text-align:right;">
2.464922e+10
</td>
<td style="text-align:right;">
59.0836780
</td>
<td style="text-align:right;">
47.17
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4542
</td>
<td style="text-align:left;">
45079
</td>
<td style="text-align:left;">
2020-05-01
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.182857e+06
</td>
<td style="text-align:right;">
5182856.75
</td>
<td style="text-align:right;">
1.740600e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
415644
</td>
<td style="text-align:right;">
8.168495e+10
</td>
<td style="text-align:right;">
93.3503023
</td>
<td style="text-align:right;">
74.19
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4476
</td>
<td style="text-align:left;">
47005
</td>
<td style="text-align:left;">
2020-03-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.978907e+04
</td>
<td style="text-align:right;">
79789.07
</td>
<td style="text-align:right;">
5.150000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
15849
</td>
<td style="text-align:right;">
2.410436e+09
</td>
<td style="text-align:right;">
37.0346802
</td>
<td style="text-align:right;">
12.57
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4541
</td>
<td style="text-align:left;">
47011
</td>
<td style="text-align:left;">
2020-04-24
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.293443e+06
</td>
<td style="text-align:right;">
1293442.96
</td>
<td style="text-align:right;">
5.083000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
108482
</td>
<td style="text-align:right;">
1.789671e+10
</td>
<td style="text-align:right;">
77.4101177
</td>
<td style="text-align:right;">
49.14
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4476
</td>
<td style="text-align:left;">
47037
</td>
<td style="text-align:left;">
2020-03-05
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
3.338607e+06
</td>
<td style="text-align:right;">
3338606.98
</td>
<td style="text-align:right;">
5.377850e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
715485
</td>
<td style="text-align:right;">
1.402987e+11
</td>
<td style="text-align:right;">
96.5001591
</td>
<td style="text-align:right;">
63.08
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4550
</td>
<td style="text-align:left;">
47037
</td>
<td style="text-align:left;">
2020-07-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.015124e+05
</td>
<td style="text-align:right;">
801512.44
</td>
<td style="text-align:right;">
2.500000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
715485
</td>
<td style="text-align:right;">
1.402987e+11
</td>
<td style="text-align:right;">
96.5001591
</td>
<td style="text-align:right;">
63.08
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4541
</td>
<td style="text-align:left;">
47065
</td>
<td style="text-align:left;">
2020-04-24
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.360949e+07
</td>
<td style="text-align:right;">
13603360.40
</td>
<td style="text-align:right;">
3.447911e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
365901
</td>
<td style="text-align:right;">
6.771230e+10
</td>
<td style="text-align:right;">
92.3958002
</td>
<td style="text-align:right;">
76.93
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4476
</td>
<td style="text-align:left;">
47141
</td>
<td style="text-align:left;">
2020-03-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.051809e+05
</td>
<td style="text-align:right;">
605180.94
</td>
<td style="text-align:right;">
2.792500e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
79809
</td>
<td style="text-align:right;">
1.142029e+10
</td>
<td style="text-align:right;">
67.1333121
</td>
<td style="text-align:right;">
46.75
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4476
</td>
<td style="text-align:left;">
47149
</td>
<td style="text-align:left;">
2020-03-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.839498e+05
</td>
<td style="text-align:right;">
783949.84
</td>
<td style="text-align:right;">
2.815940e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
341253
</td>
<td style="text-align:right;">
6.433455e+10
</td>
<td style="text-align:right;">
88.2278078
</td>
<td style="text-align:right;">
70.53
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4476
</td>
<td style="text-align:left;">
47189
</td>
<td style="text-align:left;">
2020-03-05
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.390411e+06
</td>
<td style="text-align:right;">
2390410.80
</td>
<td style="text-align:right;">
1.349610e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
147693
</td>
<td style="text-align:right;">
2.799064e+10
</td>
<td style="text-align:right;">
79.7009227
</td>
<td style="text-align:right;">
66.07
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4572
</td>
<td style="text-align:left;">
48245
</td>
<td style="text-align:left;">
2020-12-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.058362e+05
</td>
<td style="text-align:right;">
205836.22
</td>
<td style="text-align:right;">
1.450000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
256386
</td>
<td style="text-align:right;">
3.854901e+10
</td>
<td style="text-align:right;">
97.7410118
</td>
<td style="text-align:right;">
68.01
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4572
</td>
<td style="text-align:left;">
48351
</td>
<td style="text-align:left;">
2020-12-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.127118e+05
</td>
<td style="text-align:right;">
312711.84
</td>
<td style="text-align:right;">
3.560000e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
12151
</td>
<td style="text-align:right;">
2.050022e+09
</td>
<td style="text-align:right;">
47.8205536
</td>
<td style="text-align:right;">
3.72
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4572
</td>
<td style="text-align:left;">
48361
</td>
<td style="text-align:left;">
2020-12-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.385839e+07
</td>
<td style="text-align:right;">
13858386.42
</td>
<td style="text-align:right;">
7.337196e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
84645
</td>
<td style="text-align:right;">
1.477127e+10
</td>
<td style="text-align:right;">
96.5637926
</td>
<td style="text-align:right;">
63.75
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4572
</td>
<td style="text-align:left;">
48453
</td>
<td style="text-align:left;">
2020-12-09
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.896741e+07
</td>
<td style="text-align:right;">
28967410.41
</td>
<td style="text-align:right;">
2.809086e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
1285769
</td>
<td style="text-align:right;">
1.895386e+11
</td>
<td style="text-align:right;">
96.8819599
</td>
<td style="text-align:right;">
56.75
</td>
<td style="text-align:left;">
HURRICANE LAURA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4539
</td>
<td style="text-align:left;">
53033
</td>
<td style="text-align:left;">
2020-04-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.313171e+05
</td>
<td style="text-align:right;">
131317.08
</td>
<td style="text-align:right;">
1.866200e+02
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
2268178
</td>
<td style="text-align:right;">
4.701211e+11
</td>
<td style="text-align:right;">
99.6500159
</td>
<td style="text-align:right;">
78.36
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4539
</td>
<td style="text-align:left;">
53067
</td>
<td style="text-align:left;">
2020-04-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.571660e+04
</td>
<td style="text-align:right;">
25716.60
</td>
<td style="text-align:right;">
1.400000e+04
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
294398
</td>
<td style="text-align:right;">
5.744299e+10
</td>
<td style="text-align:right;">
96.2138085
</td>
<td style="text-align:right;">
71.90
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4473
</td>
<td style="text-align:left;">
72059
</td>
<td style="text-align:left;">
2020-01-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.936124e+05
</td>
<td style="text-align:right;">
493612.44
</td>
<td style="text-align:right;">
6.188000e+03
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
17690
</td>
<td style="text-align:right;">
1.798984e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
EARTHQUAKES
</td>
<td style="text-align:left;">
Earthquake
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4473
</td>
<td style="text-align:left;">
72113
</td>
<td style="text-align:left;">
2020-01-16
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
8.767199e+04
</td>
<td style="text-align:right;">
87671.99
</td>
<td style="text-align:right;">
4.296124e+05
</td>
<td style="text-align:right;">
2020
</td>
<td style="text-align:left;">
137316
</td>
<td style="text-align:right;">
2.102578e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
EARTHQUAKES
</td>
<td style="text-align:left;">
Earthquake
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4596
</td>
<td style="text-align:left;">
01015
</td>
<td style="text-align:left;">
2021-04-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.238120e+06
</td>
<td style="text-align:right;">
3238120.48
</td>
<td style="text-align:right;">
1.215400e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
116250
</td>
<td style="text-align:right;">
2.269150e+10
</td>
<td style="text-align:right;">
84.3779828
</td>
<td style="text-align:right;">
46.72
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4596
</td>
<td style="text-align:left;">
01105
</td>
<td style="text-align:left;">
2021-04-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.302500e+05
</td>
<td style="text-align:right;">
730250.00
</td>
<td style="text-align:right;">
2.660000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
8480
</td>
<td style="text-align:right;">
1.455191e+09
</td>
<td style="text-align:right;">
13.7130130
</td>
<td style="text-align:right;">
10.88
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4596
</td>
<td style="text-align:left;">
01117
</td>
<td style="text-align:left;">
2021-04-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.805876e+05
</td>
<td style="text-align:right;">
180587.60
</td>
<td style="text-align:right;">
1.500000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
222782
</td>
<td style="text-align:right;">
4.221177e+10
</td>
<td style="text-align:right;">
87.4005727
</td>
<td style="text-align:right;">
74.09
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4585
</td>
<td style="text-align:left;">
02100
</td>
<td style="text-align:left;">
2021-02-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.884985e+05
</td>
<td style="text-align:right;">
288498.54
</td>
<td style="text-align:right;">
1.650802e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
2080
</td>
<td style="text-align:right;">
8.783506e+08
</td>
<td style="text-align:right;">
3.7225581
</td>
<td style="text-align:right;">
25.02
</td>
<td style="text-align:left;">
SEVERE STORM, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Mud/Landslide
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4610
</td>
<td style="text-align:left;">
06063
</td>
<td style="text-align:left;">
2021-08-24
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.300000e+03
</td>
<td style="text-align:right;">
4300.00
</td>
<td style="text-align:right;">
1.657000e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
19746
</td>
<td style="text-align:right;">
8.405356e+09
</td>
<td style="text-align:right;">
78.9691378
</td>
<td style="text-align:right;">
62.95
</td>
<td style="text-align:left;">
WILDFIRES
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4634
</td>
<td style="text-align:left;">
08013
</td>
<td style="text-align:left;">
2021-12-31
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.156198e+06
</td>
<td style="text-align:right;">
1156197.82
</td>
<td style="text-align:right;">
1.142980e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
330652
</td>
<td style="text-align:right;">
6.223313e+10
</td>
<td style="text-align:right;">
89.5004773
</td>
<td style="text-align:right;">
71.01
</td>
<td style="text-align:left;">
WILDFIRES AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4627
</td>
<td style="text-align:left;">
10003
</td>
<td style="text-align:left;">
2021-10-24
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.598896e+05
</td>
<td style="text-align:right;">
259889.60
</td>
<td style="text-align:right;">
6.838600e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
570089
</td>
<td style="text-align:right;">
1.221791e+11
</td>
<td style="text-align:right;">
90.7731467
</td>
<td style="text-align:right;">
83.26
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
3560
</td>
<td style="text-align:left;">
12086
</td>
<td style="text-align:left;">
2021-06-25
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.072216e+07
</td>
<td style="text-align:right;">
10722161.01
</td>
<td style="text-align:right;">
2.600000e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
2698679
</td>
<td style="text-align:right;">
3.195744e+11
</td>
<td style="text-align:right;">
99.8090996
</td>
<td style="text-align:right;">
37.11
</td>
<td style="text-align:left;">
SURFSIDE BUILDING COLLAPSE
</td>
<td style="text-align:left;">
Other
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4600
</td>
<td style="text-align:left;">
13077
</td>
<td style="text-align:left;">
2021-05-05
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
4.371243e+06
</td>
<td style="text-align:right;">
4371242.55
</td>
<td style="text-align:right;">
7.118797e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
146099
</td>
<td style="text-align:right;">
2.776655e+10
</td>
<td style="text-align:right;">
67.6423799
</td>
<td style="text-align:right;">
59.99
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4589
</td>
<td style="text-align:left;">
16055
</td>
<td style="text-align:left;">
2021-03-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.438445e+05
</td>
<td style="text-align:right;">
343844.52
</td>
<td style="text-align:right;">
1.680000e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
171251
</td>
<td style="text-align:right;">
3.499817e+10
</td>
<td style="text-align:right;">
61.3108495
</td>
<td style="text-align:right;">
54.14
</td>
<td style="text-align:left;">
STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4595
</td>
<td style="text-align:left;">
21007
</td>
<td style="text-align:left;">
2021-04-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.700000e+04
</td>
<td style="text-align:right;">
27000.00
</td>
<td style="text-align:right;">
5.220000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
7718
</td>
<td style="text-align:right;">
1.561990e+09
</td>
<td style="text-align:right;">
44.6070633
</td>
<td style="text-align:right;">
42.23
</td>
<td style="text-align:left;">
SEVERE, STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21009
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.553630e+03
</td>
<td style="text-align:right;">
2553.63
</td>
<td style="text-align:right;">
1.440000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
44465
</td>
<td style="text-align:right;">
8.877867e+09
</td>
<td style="text-align:right;">
66.4333439
</td>
<td style="text-align:right;">
47.20
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21033
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.565992e+05
</td>
<td style="text-align:right;">
656599.23
</td>
<td style="text-align:right;">
4.155400e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
12649
</td>
<td style="text-align:right;">
2.503478e+09
</td>
<td style="text-align:right;">
53.7384664
</td>
<td style="text-align:right;">
32.94
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21047
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.378318e+05
</td>
<td style="text-align:right;">
137831.79
</td>
<td style="text-align:right;">
3.264940e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
72692
</td>
<td style="text-align:right;">
1.162218e+10
</td>
<td style="text-align:right;">
84.3143493
</td>
<td style="text-align:right;">
51.56
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4595
</td>
<td style="text-align:left;">
21065
</td>
<td style="text-align:left;">
2021-04-23
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.166988e+04
</td>
<td style="text-align:right;">
71669.88
</td>
<td style="text-align:right;">
3.002000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
14163
</td>
<td style="text-align:right;">
2.046989e+09
</td>
<td style="text-align:right;">
25.3897550
</td>
<td style="text-align:right;">
10.98
</td>
<td style="text-align:left;">
SEVERE, STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21067
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
65
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.116366e+07
</td>
<td style="text-align:right;">
2287470.95
</td>
<td style="text-align:right;">
5.827680e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
322301
</td>
<td style="text-align:right;">
5.665312e+10
</td>
<td style="text-align:right;">
88.2914413
</td>
<td style="text-align:right;">
72.09
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21073
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
4.508859e+06
</td>
<td style="text-align:right;">
4508859.06
</td>
<td style="text-align:right;">
8.094045e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
51521
</td>
<td style="text-align:right;">
1.006136e+10
</td>
<td style="text-align:right;">
62.8062361
</td>
<td style="text-align:right;">
71.55
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21075
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.228770e+05
</td>
<td style="text-align:right;">
422877.01
</td>
<td style="text-align:right;">
2.408400e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
6515
</td>
<td style="text-align:right;">
1.438054e+09
</td>
<td style="text-align:right;">
58.3200764
</td>
<td style="text-align:right;">
20.50
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21083
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.526278e+04
</td>
<td style="text-align:right;">
3626.00
</td>
<td style="text-align:right;">
5.140000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
36623
</td>
<td style="text-align:right;">
8.112734e+09
</td>
<td style="text-align:right;">
83.7734648
</td>
<td style="text-align:right;">
32.56
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21099
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.256349e+04
</td>
<td style="text-align:right;">
62563.49
</td>
<td style="text-align:right;">
1.695000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
19277
</td>
<td style="text-align:right;">
3.607581e+09
</td>
<td style="text-align:right;">
41.0435889
</td>
<td style="text-align:right;">
36.60
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21107
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
8.368929e+06
</td>
<td style="text-align:right;">
8289847.64
</td>
<td style="text-align:right;">
2.587903e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
45395
</td>
<td style="text-align:right;">
9.084016e+09
</td>
<td style="text-align:right;">
75.4056634
</td>
<td style="text-align:right;">
58.31
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4595
</td>
<td style="text-align:left;">
21115
</td>
<td style="text-align:left;">
2021-04-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.840255e+04
</td>
<td style="text-align:right;">
28402.55
</td>
<td style="text-align:right;">
2.004440e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
22621
</td>
<td style="text-align:right;">
3.501458e+09
</td>
<td style="text-align:right;">
46.8978683
</td>
<td style="text-align:right;">
13.11
</td>
<td style="text-align:left;">
SEVERE, STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21141
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.720949e+04
</td>
<td style="text-align:right;">
67209.49
</td>
<td style="text-align:right;">
1.817030e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
27382
</td>
<td style="text-align:right;">
4.856890e+09
</td>
<td style="text-align:right;">
55.4247534
</td>
<td style="text-align:right;">
39.66
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21143
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.288122e+06
</td>
<td style="text-align:right;">
2288121.96
</td>
<td style="text-align:right;">
5.186950e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
8663
</td>
<td style="text-align:right;">
1.923462e+09
</td>
<td style="text-align:right;">
32.0394528
</td>
<td style="text-align:right;">
19.41
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21155
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.249877e+04
</td>
<td style="text-align:right;">
42498.77
</td>
<td style="text-align:right;">
3.304000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
19521
</td>
<td style="text-align:right;">
4.044451e+09
</td>
<td style="text-align:right;">
33.3439389
</td>
<td style="text-align:right;">
61.90
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21157
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
6.375770e+06
</td>
<td style="text-align:right;">
6375770.17
</td>
<td style="text-align:right;">
1.227147e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
31629
</td>
<td style="text-align:right;">
6.869117e+09
</td>
<td style="text-align:right;">
71.5876551
</td>
<td style="text-align:right;">
62.32
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21177
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.314805e+06
</td>
<td style="text-align:right;">
1314805.15
</td>
<td style="text-align:right;">
2.451000e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
30928
</td>
<td style="text-align:right;">
5.279739e+09
</td>
<td style="text-align:right;">
50.7476933
</td>
<td style="text-align:right;">
36.44
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21183
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.166854e+05
</td>
<td style="text-align:right;">
200593.15
</td>
<td style="text-align:right;">
6.134830e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
23745
</td>
<td style="text-align:right;">
5.697402e+09
</td>
<td style="text-align:right;">
52.4657970
</td>
<td style="text-align:right;">
50.32
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4630
</td>
<td style="text-align:left;">
21227
</td>
<td style="text-align:left;">
2021-12-12
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
4.548012e+06
</td>
<td style="text-align:right;">
4548012.41
</td>
<td style="text-align:right;">
7.190880e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
134482
</td>
<td style="text-align:right;">
2.463868e+10
</td>
<td style="text-align:right;">
83.0098632
</td>
<td style="text-align:right;">
64.99
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4606
</td>
<td style="text-align:left;">
22005
</td>
<td style="text-align:left;">
2021-06-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.904640e+03
</td>
<td style="text-align:right;">
5904.64
</td>
<td style="text-align:right;">
2.930000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
126426
</td>
<td style="text-align:right;">
3.019167e+10
</td>
<td style="text-align:right;">
96.2774419
</td>
<td style="text-align:right;">
88.45
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22005
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
1.647677e+07
</td>
<td style="text-align:right;">
14289528.63
</td>
<td style="text-align:right;">
3.485034e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
126426
</td>
<td style="text-align:right;">
3.019167e+10
</td>
<td style="text-align:right;">
96.2774419
</td>
<td style="text-align:right;">
88.45
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22007
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
4.065902e+05
</td>
<td style="text-align:right;">
406590.23
</td>
<td style="text-align:right;">
8.911550e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
21017
</td>
<td style="text-align:right;">
3.291188e+09
</td>
<td style="text-align:right;">
86.3506204
</td>
<td style="text-align:right;">
48.89
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4606
</td>
<td style="text-align:left;">
22019
</td>
<td style="text-align:left;">
2021-06-02
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.104108e+06
</td>
<td style="text-align:right;">
1104107.98
</td>
<td style="text-align:right;">
4.183475e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
216652
</td>
<td style="text-align:right;">
4.227924e+10
</td>
<td style="text-align:right;">
96.9137766
</td>
<td style="text-align:right;">
32.40
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22033
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.017367e+06
</td>
<td style="text-align:right;">
3017366.97
</td>
<td style="text-align:right;">
1.708700e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
456512
</td>
<td style="text-align:right;">
1.120657e+11
</td>
<td style="text-align:right;">
98.8864143
</td>
<td style="text-align:right;">
60.09
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4606
</td>
<td style="text-align:left;">
22033
</td>
<td style="text-align:left;">
2021-06-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.457675e+05
</td>
<td style="text-align:right;">
645767.50
</td>
<td style="text-align:right;">
2.963400e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
456512
</td>
<td style="text-align:right;">
1.120657e+11
</td>
<td style="text-align:right;">
98.8864143
</td>
<td style="text-align:right;">
60.09
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22033
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1.156044e+08
</td>
<td style="text-align:right;">
115604437\.67
</td>
<td style="text-align:right;">
7.387389e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
456512
</td>
<td style="text-align:right;">
1.120657e+11
</td>
<td style="text-align:right;">
98.8864143
</td>
<td style="text-align:right;">
60.09
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22051
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
5.835583e+06
</td>
<td style="text-align:right;">
5835582.78
</td>
<td style="text-align:right;">
1.478800e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
440590
</td>
<td style="text-align:right;">
6.529661e+10
</td>
<td style="text-align:right;">
98.1546293
</td>
<td style="text-align:right;">
96.05
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22051
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
89
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
2.649678e+08
</td>
<td style="text-align:right;">
99902962.58
</td>
<td style="text-align:right;">
1.250207e+06
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
440590
</td>
<td style="text-align:right;">
6.529661e+10
</td>
<td style="text-align:right;">
98.1546293
</td>
<td style="text-align:right;">
96.05
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22057
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.131015e+06
</td>
<td style="text-align:right;">
1131015.01
</td>
<td style="text-align:right;">
2.927500e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
97393
</td>
<td style="text-align:right;">
1.283919e+10
</td>
<td style="text-align:right;">
94.4002545
</td>
<td style="text-align:right;">
91.34
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22057
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
135
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
1.004245e+08
</td>
<td style="text-align:right;">
58715475.40
</td>
<td style="text-align:right;">
1.685344e+06
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
97393
</td>
<td style="text-align:right;">
1.283919e+10
</td>
<td style="text-align:right;">
94.4002545
</td>
<td style="text-align:right;">
91.34
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22063
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2.107511e+07
</td>
<td style="text-align:right;">
21075105.63
</td>
<td style="text-align:right;">
1.950200e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
142163
</td>
<td style="text-align:right;">
2.617953e+10
</td>
<td style="text-align:right;">
96.8501432
</td>
<td style="text-align:right;">
57.48
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22071
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.092847e+06
</td>
<td style="text-align:right;">
3092847.22
</td>
<td style="text-align:right;">
1.125400e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
383606
</td>
<td style="text-align:right;">
5.814887e+10
</td>
<td style="text-align:right;">
96.9774101
</td>
<td style="text-align:right;">
99.40
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22071
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
1.316914e+07
</td>
<td style="text-align:right;">
13169142.82
</td>
<td style="text-align:right;">
1.539474e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
383606
</td>
<td style="text-align:right;">
5.814887e+10
</td>
<td style="text-align:right;">
96.9774101
</td>
<td style="text-align:right;">
99.40
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22075
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.771487e+05
</td>
<td style="text-align:right;">
677148.67
</td>
<td style="text-align:right;">
1.914000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
23448
</td>
<td style="text-align:right;">
4.870262e+09
</td>
<td style="text-align:right;">
82.6916958
</td>
<td style="text-align:right;">
94.33
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22075
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.631102e+07
</td>
<td style="text-align:right;">
16311021.01
</td>
<td style="text-align:right;">
5.243400e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
23448
</td>
<td style="text-align:right;">
4.870262e+09
</td>
<td style="text-align:right;">
82.6916958
</td>
<td style="text-align:right;">
94.33
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22087
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.014001e+05
</td>
<td style="text-align:right;">
801400.14
</td>
<td style="text-align:right;">
9.757830e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
43709
</td>
<td style="text-align:right;">
6.549944e+09
</td>
<td style="text-align:right;">
86.6051543
</td>
<td style="text-align:right;">
91.79
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22087
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
2.141927e+06
</td>
<td style="text-align:right;">
2141926.92
</td>
<td style="text-align:right;">
2.646608e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
43709
</td>
<td style="text-align:right;">
6.549944e+09
</td>
<td style="text-align:right;">
86.6051543
</td>
<td style="text-align:right;">
91.79
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22089
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.237694e+05
</td>
<td style="text-align:right;">
323769.37
</td>
<td style="text-align:right;">
3.958000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
52479
</td>
<td style="text-align:right;">
8.228203e+09
</td>
<td style="text-align:right;">
90.3277124
</td>
<td style="text-align:right;">
99.97
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22089
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
23
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
2.490244e+07
</td>
<td style="text-align:right;">
24780398.10
</td>
<td style="text-align:right;">
5.848307e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
52479
</td>
<td style="text-align:right;">
8.228203e+09
</td>
<td style="text-align:right;">
90.3277124
</td>
<td style="text-align:right;">
99.97
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22091
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.508953e+06
</td>
<td style="text-align:right;">
3508953.44
</td>
<td style="text-align:right;">
3.997500e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
10911
</td>
<td style="text-align:right;">
1.816039e+09
</td>
<td style="text-align:right;">
48.2659879
</td>
<td style="text-align:right;">
24.25
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22093
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
7.269766e+06
</td>
<td style="text-align:right;">
4535671.86
</td>
<td style="text-align:right;">
3.920930e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
20169
</td>
<td style="text-align:right;">
4.201279e+09
</td>
<td style="text-align:right;">
76.8692332
</td>
<td style="text-align:right;">
96.82
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22095
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1.928605e+08
</td>
<td style="text-align:right;">
74935394.21
</td>
<td style="text-align:right;">
2.070212e+06
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
42466
</td>
<td style="text-align:right;">
8.175506e+09
</td>
<td style="text-align:right;">
86.5733376
</td>
<td style="text-align:right;">
99.90
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22101
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.020996e+05
</td>
<td style="text-align:right;">
302099.61
</td>
<td style="text-align:right;">
4.000000e+00
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
49313
</td>
<td style="text-align:right;">
1.172348e+10
</td>
<td style="text-align:right;">
94.7184219
</td>
<td style="text-align:right;">
78.13
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22103
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
5.641922e+07
</td>
<td style="text-align:right;">
56419218.21
</td>
<td style="text-align:right;">
1.991214e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
263870
</td>
<td style="text-align:right;">
4.368477e+10
</td>
<td style="text-align:right;">
97.8046452
</td>
<td style="text-align:right;">
87.91
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22105
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.219528e+07
</td>
<td style="text-align:right;">
52195275.17
</td>
<td style="text-align:right;">
9.608625e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
132858
</td>
<td style="text-align:right;">
2.072858e+10
</td>
<td style="text-align:right;">
96.1501750
</td>
<td style="text-align:right;">
57.73
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4577
</td>
<td style="text-align:left;">
22109
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.438236e+05
</td>
<td style="text-align:right;">
143823.58
</td>
<td style="text-align:right;">
1.600000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
109484
</td>
<td style="text-align:right;">
2.195670e+10
</td>
<td style="text-align:right;">
96.3728921
</td>
<td style="text-align:right;">
94.75
</td>
<td style="text-align:left;">
HURRICANE ZETA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22109
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1.506119e+07
</td>
<td style="text-align:right;">
7449699.92
</td>
<td style="text-align:right;">
2.607010e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
109484
</td>
<td style="text-align:right;">
2.195670e+10
</td>
<td style="text-align:right;">
96.3728921
</td>
<td style="text-align:right;">
94.75
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4611
</td>
<td style="text-align:left;">
22117
</td>
<td style="text-align:left;">
2021-08-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.580516e+05
</td>
<td style="text-align:right;">
258051.58
</td>
<td style="text-align:right;">
8.925000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
45382
</td>
<td style="text-align:right;">
7.554000e+09
</td>
<td style="text-align:right;">
89.3732103
</td>
<td style="text-align:right;">
25.05
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4607
</td>
<td style="text-align:left;">
26163
</td>
<td style="text-align:left;">
2021-07-15
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
1.203159e+07
</td>
<td style="text-align:right;">
12031592.63
</td>
<td style="text-align:right;">
3.671430e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1792549
</td>
<td style="text-align:right;">
3.174907e+11
</td>
<td style="text-align:right;">
96.6910595
</td>
<td style="text-align:right;">
56.56
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4598
</td>
<td style="text-align:left;">
28001
</td>
<td style="text-align:left;">
2021-05-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.753157e+05
</td>
<td style="text-align:right;">
775315.71
</td>
<td style="text-align:right;">
1.632000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
29474
</td>
<td style="text-align:right;">
4.966114e+09
</td>
<td style="text-align:right;">
54.3748011
</td>
<td style="text-align:right;">
26.58
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS
</td>
<td style="text-align:left;">
Severe Ice Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4626
</td>
<td style="text-align:left;">
28047
</td>
<td style="text-align:left;">
2021-10-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.200653e+05
</td>
<td style="text-align:right;">
520065.35
</td>
<td style="text-align:right;">
2.913900e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
208275
</td>
<td style="text-align:right;">
3.732934e+10
</td>
<td style="text-align:right;">
97.2001273
</td>
<td style="text-align:right;">
67.41
</td>
<td style="text-align:left;">
HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34003
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
34
</td>
<td style="text-align:right;">
26
</td>
<td style="text-align:right;">
26
</td>
<td style="text-align:right;">
4.099809e+06
</td>
<td style="text-align:right;">
3208893.03
</td>
<td style="text-align:right;">
2.447749e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
955459
</td>
<td style="text-align:right;">
1.836780e+11
</td>
<td style="text-align:right;">
98.6955138
</td>
<td style="text-align:right;">
62.64
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34013
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
3.761962e+06
</td>
<td style="text-align:right;">
3761961.75
</td>
<td style="text-align:right;">
4.349900e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
863448
</td>
<td style="text-align:right;">
1.279945e+11
</td>
<td style="text-align:right;">
92.7457843
</td>
<td style="text-align:right;">
31.29
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34015
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
4.259833e+05
</td>
<td style="text-align:right;">
425983.30
</td>
<td style="text-align:right;">
6.922440e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
302128
</td>
<td style="text-align:right;">
6.000103e+10
</td>
<td style="text-align:right;">
84.7279669
</td>
<td style="text-align:right;">
87.21
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34017
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1.538086e+06
</td>
<td style="text-align:right;">
1538086.36
</td>
<td style="text-align:right;">
2.920270e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
723485
</td>
<td style="text-align:right;">
8.143320e+10
</td>
<td style="text-align:right;">
95.3865733
</td>
<td style="text-align:right;">
38.16
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34019
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
6.150691e+05
</td>
<td style="text-align:right;">
615069.08
</td>
<td style="text-align:right;">
1.364898e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
128867
</td>
<td style="text-align:right;">
3.828055e+10
</td>
<td style="text-align:right;">
79.3827553
</td>
<td style="text-align:right;">
94.46
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34021
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.414260e+05
</td>
<td style="text-align:right;">
241425.98
</td>
<td style="text-align:right;">
1.634000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
386483
</td>
<td style="text-align:right;">
8.246538e+10
</td>
<td style="text-align:right;">
93.0321349
</td>
<td style="text-align:right;">
73.65
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34023
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1.488941e+06
</td>
<td style="text-align:right;">
1488940.52
</td>
<td style="text-align:right;">
2.533134e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
862614
</td>
<td style="text-align:right;">
1.753605e+11
</td>
<td style="text-align:right;">
95.8638244
</td>
<td style="text-align:right;">
59.20
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34027
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.852445e+05
</td>
<td style="text-align:right;">
185244.52
</td>
<td style="text-align:right;">
4.800000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
508868
</td>
<td style="text-align:right;">
1.186671e+11
</td>
<td style="text-align:right;">
90.7095132
</td>
<td style="text-align:right;">
92.01
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4597
</td>
<td style="text-align:left;">
34029
</td>
<td style="text-align:left;">
2021-04-28
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.492861e+04
</td>
<td style="text-align:right;">
24928.61
</td>
<td style="text-align:right;">
1.150000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
636993
</td>
<td style="text-align:right;">
1.058262e+11
</td>
<td style="text-align:right;">
97.2319440
</td>
<td style="text-align:right;">
89.27
</td>
<td style="text-align:left;">
SEVERE WINTER STORM AND SNOWSTORM
</td>
<td style="text-align:left;">
Snowstorm
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34031
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
1.659818e+06
</td>
<td style="text-align:right;">
1659818.14
</td>
<td style="text-align:right;">
1.883069e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
523958
</td>
<td style="text-align:right;">
7.427218e+10
</td>
<td style="text-align:right;">
89.6595609
</td>
<td style="text-align:right;">
41.88
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34035
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
21
</td>
<td style="text-align:right;">
21
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
7.212616e+06
</td>
<td style="text-align:right;">
7212616.30
</td>
<td style="text-align:right;">
1.544265e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
345266
</td>
<td style="text-align:right;">
8.593257e+10
</td>
<td style="text-align:right;">
93.4139357
</td>
<td style="text-align:right;">
88.67
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4614
</td>
<td style="text-align:left;">
34039
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
16
</td>
<td style="text-align:right;">
3.877424e+06
</td>
<td style="text-align:right;">
3877424.30
</td>
<td style="text-align:right;">
1.535021e+06
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
575179
</td>
<td style="text-align:right;">
9.266494e+10
</td>
<td style="text-align:right;">
92.4276169
</td>
<td style="text-align:right;">
38.29
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36005
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.832720e+03
</td>
<td style="text-align:right;">
3832.72
</td>
<td style="text-align:right;">
4.000000e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1471631
</td>
<td style="text-align:right;">
1.365967e+11
</td>
<td style="text-align:right;">
96.3092587
</td>
<td style="text-align:right;">
6.46
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36027
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.614820e+03
</td>
<td style="text-align:right;">
5614.82
</td>
<td style="text-align:right;">
2.710300e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
295788
</td>
<td style="text-align:right;">
6.225758e+10
</td>
<td style="text-align:right;">
76.9328667
</td>
<td style="text-align:right;">
78.39
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36047
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.131723e+04
</td>
<td style="text-align:right;">
11317.23
</td>
<td style="text-align:right;">
2.056400e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
2735309
</td>
<td style="text-align:right;">
3.306987e+11
</td>
<td style="text-align:right;">
98.1228126
</td>
<td style="text-align:right;">
9.45
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36059
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.137343e+05
</td>
<td style="text-align:right;">
113734.29
</td>
<td style="text-align:right;">
7.278000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1395584
</td>
<td style="text-align:right;">
2.703602e+11
</td>
<td style="text-align:right;">
96.6592428
</td>
<td style="text-align:right;">
95.00
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36061
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.063000e+04
</td>
<td style="text-align:right;">
30630.00
</td>
<td style="text-align:right;">
1.000000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1693650
</td>
<td style="text-align:right;">
2.601753e+11
</td>
<td style="text-align:right;">
96.1819917
</td>
<td style="text-align:right;">
49.97
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4625
</td>
<td style="text-align:left;">
36067
</td>
<td style="text-align:left;">
2021-10-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.111608e+04
</td>
<td style="text-align:right;">
91116.08
</td>
<td style="text-align:right;">
1.295800e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
476419
</td>
<td style="text-align:right;">
9.074455e+10
</td>
<td style="text-align:right;">
76.6146993
</td>
<td style="text-align:right;">
90.29
</td>
<td style="text-align:left;">
REMNANTS OF TROPICAL STORM FRED
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36071
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.258620e+03
</td>
<td style="text-align:right;">
8258.62
</td>
<td style="text-align:right;">
6.180000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
401096
</td>
<td style="text-align:right;">
7.171433e+10
</td>
<td style="text-align:right;">
85.2052179
</td>
<td style="text-align:right;">
70.97
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36079
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.198772e+04
</td>
<td style="text-align:right;">
11987.72
</td>
<td style="text-align:right;">
1.468000e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
97640
</td>
<td style="text-align:right;">
2.033413e+10
</td>
<td style="text-align:right;">
50.8431435
</td>
<td style="text-align:right;">
87.52
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4625
</td>
<td style="text-align:left;">
36101
</td>
<td style="text-align:left;">
2021-10-08
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.437029e+05
</td>
<td style="text-align:right;">
143702.90
</td>
<td style="text-align:right;">
5.687660e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
93544
</td>
<td style="text-align:right;">
1.955800e+10
</td>
<td style="text-align:right;">
64.4288896
</td>
<td style="text-align:right;">
70.50
</td>
<td style="text-align:left;">
REMNANTS OF TROPICAL STORM FRED
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36103
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.395300e+04
</td>
<td style="text-align:right;">
13953.00
</td>
<td style="text-align:right;">
1.940000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1524969
</td>
<td style="text-align:right;">
3.491028e+11
</td>
<td style="text-align:right;">
97.1364938
</td>
<td style="text-align:right;">
94.81
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4615
</td>
<td style="text-align:left;">
36119
</td>
<td style="text-align:left;">
2021-09-05
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
4.703006e+06
</td>
<td style="text-align:right;">
4703005.94
</td>
<td style="text-align:right;">
8.306886e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1002867
</td>
<td style="text-align:right;">
1.800880e+11
</td>
<td style="text-align:right;">
94.3048043
</td>
<td style="text-align:right;">
77.72
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4617
</td>
<td style="text-align:left;">
37021
</td>
<td style="text-align:left;">
2021-09-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.450863e+05
</td>
<td style="text-align:right;">
345086.33
</td>
<td style="text-align:right;">
1.230000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
269305
</td>
<td style="text-align:right;">
4.863094e+10
</td>
<td style="text-align:right;">
81.4826599
</td>
<td style="text-align:right;">
59.61
</td>
<td style="text-align:left;">
REMNANTS OF TROPICAL STORM FRED
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4617
</td>
<td style="text-align:left;">
37087
</td>
<td style="text-align:left;">
2021-09-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.082492e+04
</td>
<td style="text-align:right;">
30824.92
</td>
<td style="text-align:right;">
2.274000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
62051
</td>
<td style="text-align:right;">
1.040115e+10
</td>
<td style="text-align:right;">
61.8835507
</td>
<td style="text-align:right;">
50.80
</td>
<td style="text-align:left;">
REMNANTS OF TROPICAL STORM FRED
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4588
</td>
<td style="text-align:left;">
37183
</td>
<td style="text-align:left;">
2021-03-03
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.166113e+05
</td>
<td style="text-align:right;">
205537.11
</td>
<td style="text-align:right;">
1.170600e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1128357
</td>
<td style="text-align:right;">
2.152861e+11
</td>
<td style="text-align:right;">
93.6366529
</td>
<td style="text-align:right;">
72.02
</td>
<td style="text-align:left;">
TROPICAL STORM ETA
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4618
</td>
<td style="text-align:left;">
42017
</td>
<td style="text-align:left;">
2021-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.178184e+04
</td>
<td style="text-align:right;">
31781.84
</td>
<td style="text-align:right;">
1.500000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
646216
</td>
<td style="text-align:right;">
1.587404e+11
</td>
<td style="text-align:right;">
92.2367165
</td>
<td style="text-align:right;">
80.33
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4618
</td>
<td style="text-align:left;">
42029
</td>
<td style="text-align:left;">
2021-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.787584e+04
</td>
<td style="text-align:right;">
47875.84
</td>
<td style="text-align:right;">
0.000000e+00
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
534152
</td>
<td style="text-align:right;">
1.458534e+11
</td>
<td style="text-align:right;">
91.9821826
</td>
<td style="text-align:right;">
86.00
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4618
</td>
<td style="text-align:left;">
42043
</td>
<td style="text-align:left;">
2021-09-10
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.747946e+05
</td>
<td style="text-align:right;">
274794.56
</td>
<td style="text-align:right;">
5.555840e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
286115
</td>
<td style="text-align:right;">
6.227298e+10
</td>
<td style="text-align:right;">
89.3095768
</td>
<td style="text-align:right;">
97.49
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4618
</td>
<td style="text-align:left;">
42091
</td>
<td style="text-align:left;">
2021-09-10
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
6.569060e+06
</td>
<td style="text-align:right;">
6569060.33
</td>
<td style="text-align:right;">
1.509200e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
856020
</td>
<td style="text-align:right;">
2.069151e+11
</td>
<td style="text-align:right;">
94.0820872
</td>
<td style="text-align:right;">
94.08
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4618
</td>
<td style="text-align:left;">
42101
</td>
<td style="text-align:left;">
2021-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.099364e+05
</td>
<td style="text-align:right;">
108533.89
</td>
<td style="text-align:right;">
5.135800e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
1602305
</td>
<td style="text-align:right;">
2.598294e+11
</td>
<td style="text-align:right;">
98.1864461
</td>
<td style="text-align:right;">
42.84
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4618
</td>
<td style="text-align:left;">
42133
</td>
<td style="text-align:left;">
2021-09-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.084047e+04
</td>
<td style="text-align:right;">
10840.47
</td>
<td style="text-align:right;">
2.000000e+00
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
456310
</td>
<td style="text-align:right;">
8.409368e+10
</td>
<td style="text-align:right;">
83.9961820
</td>
<td style="text-align:right;">
90.48
</td>
<td style="text-align:left;">
REMNANTS OF HURRICANE IDA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4601
</td>
<td style="text-align:left;">
47037
</td>
<td style="text-align:left;">
2021-05-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.466241e+05
</td>
<td style="text-align:right;">
146624.09
</td>
<td style="text-align:right;">
2.916000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
715485
</td>
<td style="text-align:right;">
1.402987e+11
</td>
<td style="text-align:right;">
96.5001591
</td>
<td style="text-align:right;">
63.08
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4609
</td>
<td style="text-align:left;">
47037
</td>
<td style="text-align:left;">
2021-08-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.432929e+05
</td>
<td style="text-align:right;">
243292.87
</td>
<td style="text-align:right;">
1.593000e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
715485
</td>
<td style="text-align:right;">
1.402987e+11
</td>
<td style="text-align:right;">
96.5001591
</td>
<td style="text-align:right;">
63.08
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4609
</td>
<td style="text-align:left;">
47081
</td>
<td style="text-align:left;">
2021-08-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.308486e+04
</td>
<td style="text-align:right;">
43084.86
</td>
<td style="text-align:right;">
1.218100e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
24872
</td>
<td style="text-align:right;">
4.530279e+09
</td>
<td style="text-align:right;">
47.5660197
</td>
<td style="text-align:right;">
14.51
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4609
</td>
<td style="text-align:left;">
47085
</td>
<td style="text-align:left;">
2021-08-23
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
5.117235e+06
</td>
<td style="text-align:right;">
4202070.67
</td>
<td style="text-align:right;">
2.209380e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
18964
</td>
<td style="text-align:right;">
3.290526e+09
</td>
<td style="text-align:right;">
43.5252943
</td>
<td style="text-align:right;">
51.88
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4601
</td>
<td style="text-align:left;">
47187
</td>
<td style="text-align:left;">
2021-05-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.670780e+03
</td>
<td style="text-align:right;">
9670.78
</td>
<td style="text-align:right;">
1.680000e+02
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
247523
</td>
<td style="text-align:right;">
5.469015e+10
</td>
<td style="text-align:right;">
77.9828190
</td>
<td style="text-align:right;">
86.19
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4601
</td>
<td style="text-align:left;">
47189
</td>
<td style="text-align:left;">
2021-05-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.529950e+03
</td>
<td style="text-align:right;">
6529.95
</td>
<td style="text-align:right;">
4.259168e+05
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
147693
</td>
<td style="text-align:right;">
2.799064e+10
</td>
<td style="text-align:right;">
79.7009227
</td>
<td style="text-align:right;">
66.07
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4586
</td>
<td style="text-align:left;">
48167
</td>
<td style="text-align:left;">
2021-02-19
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.389754e+04
</td>
<td style="text-align:right;">
83897.54
</td>
<td style="text-align:right;">
3.464100e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
350477
</td>
<td style="text-align:right;">
5.674698e+10
</td>
<td style="text-align:right;">
99.5227490
</td>
<td style="text-align:right;">
90.36
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS
</td>
<td style="text-align:left;">
Severe Ice Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4578
</td>
<td style="text-align:left;">
49011
</td>
<td style="text-align:left;">
2021-01-12
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.270630e+05
</td>
<td style="text-align:right;">
227062.95
</td>
<td style="text-align:right;">
6.985800e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
362649
</td>
<td style="text-align:right;">
4.792347e+10
</td>
<td style="text-align:right;">
93.7957366
</td>
<td style="text-align:right;">
98.31
</td>
<td style="text-align:left;">
STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4628
</td>
<td style="text-align:left;">
51027
</td>
<td style="text-align:left;">
2021-10-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.926114e+05
</td>
<td style="text-align:right;">
292611.44
</td>
<td style="text-align:right;">
1.100000e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
20340
</td>
<td style="text-align:right;">
2.256744e+09
</td>
<td style="text-align:right;">
46.8024181
</td>
<td style="text-align:right;">
3.63
</td>
<td style="text-align:left;">
FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4628
</td>
<td style="text-align:left;">
51760
</td>
<td style="text-align:left;">
2021-10-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.528272e+05
</td>
<td style="text-align:right;">
352827.20
</td>
<td style="text-align:right;">
8.500000e+01
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
226269
</td>
<td style="text-align:right;">
4.353533e+10
</td>
<td style="text-align:right;">
73.2421254
</td>
<td style="text-align:right;">
54.84
</td>
<td style="text-align:left;">
FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4584
</td>
<td style="text-align:left;">
53021
</td>
<td style="text-align:left;">
2021-02-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.142333e+04
</td>
<td style="text-align:right;">
11423.33
</td>
<td style="text-align:right;">
5.110000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
96653
</td>
<td style="text-align:right;">
1.402875e+10
</td>
<td style="text-align:right;">
68.4696150
</td>
<td style="text-align:right;">
28.61
</td>
<td style="text-align:left;">
WILDFIRES AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4593
</td>
<td style="text-align:left;">
53047
</td>
<td style="text-align:left;">
2021-04-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.682250e+03
</td>
<td style="text-align:right;">
3682.25
</td>
<td style="text-align:right;">
8.000000e+00
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
42016
</td>
<td style="text-align:right;">
9.411749e+09
</td>
<td style="text-align:right;">
85.0779510
</td>
<td style="text-align:right;">
19.70
</td>
<td style="text-align:left;">
SEVERE WINTER STORM, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES, AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4584
</td>
<td style="text-align:left;">
53053
</td>
<td style="text-align:left;">
2021-02-04
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.567225e+06
</td>
<td style="text-align:right;">
4567224.89
</td>
<td style="text-align:right;">
1.277000e+03
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
920483
</td>
<td style="text-align:right;">
1.607052e+11
</td>
<td style="text-align:right;">
98.7273306
</td>
<td style="text-align:right;">
70.56
</td>
<td style="text-align:left;">
WILDFIRES AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4584
</td>
<td style="text-align:left;">
53075
</td>
<td style="text-align:left;">
2021-02-04
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
8.158387e+04
</td>
<td style="text-align:right;">
81583.87
</td>
<td style="text-align:right;">
2.593900e+04
</td>
<td style="text-align:right;">
2021
</td>
<td style="text-align:left;">
47922
</td>
<td style="text-align:right;">
9.223151e+09
</td>
<td style="text-align:right;">
23.8307350
</td>
<td style="text-align:right;">
43.48
</td>
<td style="text-align:left;">
WILDFIRES AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4672
</td>
<td style="text-align:left;">
02180
</td>
<td style="text-align:left;">
2022-09-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.482220e+03
</td>
<td style="text-align:right;">
5482.22
</td>
<td style="text-align:right;">
5.000000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
10031
</td>
<td style="text-align:right;">
1.993288e+09
</td>
<td style="text-align:right;">
3.4043907
</td>
<td style="text-align:right;">
25.18
</td>
<td style="text-align:left;">
SEVERE STORM, FLOODING, AND LANDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12009
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.011795e+05
</td>
<td style="text-align:right;">
201179.46
</td>
<td style="text-align:right;">
2.137500e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
606067
</td>
<td style="text-align:right;">
1.055946e+11
</td>
<td style="text-align:right;">
99.3636653
</td>
<td style="text-align:right;">
59.83
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12015
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
4.226326e+07
</td>
<td style="text-align:right;">
42263262.65
</td>
<td style="text-align:right;">
5.860211e+05
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
186734
</td>
<td style="text-align:right;">
3.146261e+10
</td>
<td style="text-align:right;">
96.5956093
</td>
<td style="text-align:right;">
11.94
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12021
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.071419e+08
</td>
<td style="text-align:right;">
53598049.05
</td>
<td style="text-align:right;">
1.068079e+06
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
375443
</td>
<td style="text-align:right;">
8.254240e+10
</td>
<td style="text-align:right;">
98.8545975
</td>
<td style="text-align:right;">
20.15
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12027
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.591684e+07
</td>
<td style="text-align:right;">
15916838.67
</td>
<td style="text-align:right;">
5.358010e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
33898
</td>
<td style="text-align:right;">
5.358888e+09
</td>
<td style="text-align:right;">
90.4231626
</td>
<td style="text-align:right;">
1.11
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12049
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.906879e+06
</td>
<td style="text-align:right;">
4906879.32
</td>
<td style="text-align:right;">
9.000000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
25302
</td>
<td style="text-align:right;">
3.815820e+09
</td>
<td style="text-align:right;">
80.2736239
</td>
<td style="text-align:right;">
3.53
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12051
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.348518e+05
</td>
<td style="text-align:right;">
234851.81
</td>
<td style="text-align:right;">
1.280000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
39371
</td>
<td style="text-align:right;">
6.080710e+09
</td>
<td style="text-align:right;">
91.1549475
</td>
<td style="text-align:right;">
0.89
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12055
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
8.649117e+06
</td>
<td style="text-align:right;">
8649116.66
</td>
<td style="text-align:right;">
1.072570e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
101136
</td>
<td style="text-align:right;">
1.753236e+10
</td>
<td style="text-align:right;">
90.2958956
</td>
<td style="text-align:right;">
4.81
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12069
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.741030e+05
</td>
<td style="text-align:right;">
274103.02
</td>
<td style="text-align:right;">
1.590000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
383042
</td>
<td style="text-align:right;">
5.896930e+10
</td>
<td style="text-align:right;">
95.2274897
</td>
<td style="text-align:right;">
48.09
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12071
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
21
</td>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
1.530937e+08
</td>
<td style="text-align:right;">
153093719\.52
</td>
<td style="text-align:right;">
1.775402e+06
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
759922
</td>
<td style="text-align:right;">
1.233084e+11
</td>
<td style="text-align:right;">
99.4909322
</td>
<td style="text-align:right;">
9.17
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12073
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
33
</td>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
18
</td>
<td style="text-align:right;">
3.930296e+08
</td>
<td style="text-align:right;">
250301311\.25
</td>
<td style="text-align:right;">
6.847536e+06
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
292157
</td>
<td style="text-align:right;">
4.688315e+10
</td>
<td style="text-align:right;">
94.5593382
</td>
<td style="text-align:right;">
74.76
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12081
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.391334e+07
</td>
<td style="text-align:right;">
13913335.48
</td>
<td style="text-align:right;">
6.184630e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
399485
</td>
<td style="text-align:right;">
6.672028e+10
</td>
<td style="text-align:right;">
98.4409800
</td>
<td style="text-align:right;">
23.90
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12087
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.693228e+06
</td>
<td style="text-align:right;">
1693227.63
</td>
<td style="text-align:right;">
1.323848e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
82390
</td>
<td style="text-align:right;">
1.808080e+10
</td>
<td style="text-align:right;">
95.9274578
</td>
<td style="text-align:right;">
27.88
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12093
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.217873e+06
</td>
<td style="text-align:right;">
4217873.08
</td>
<td style="text-align:right;">
5.363350e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
39455
</td>
<td style="text-align:right;">
6.060952e+09
</td>
<td style="text-align:right;">
87.5914731
</td>
<td style="text-align:right;">
2.42
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12095
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.876846e+06
</td>
<td style="text-align:right;">
2876846.22
</td>
<td style="text-align:right;">
1.024580e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
1428790
</td>
<td style="text-align:right;">
2.324704e+11
</td>
<td style="text-align:right;">
98.5682469
</td>
<td style="text-align:right;">
43.92
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12097
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
7.045912e+05
</td>
<td style="text-align:right;">
704591.23
</td>
<td style="text-align:right;">
1.132150e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
387795
</td>
<td style="text-align:right;">
5.045786e+10
</td>
<td style="text-align:right;">
95.1320395
</td>
<td style="text-align:right;">
35.74
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4680
</td>
<td style="text-align:left;">
12099
</td>
<td style="text-align:left;">
2022-12-13
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.125666e+05
</td>
<td style="text-align:right;">
112566.65
</td>
<td style="text-align:right;">
4.880000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
1489461
</td>
<td style="text-align:right;">
2.385678e+11
</td>
<td style="text-align:right;">
99.7136494
</td>
<td style="text-align:right;">
23.65
</td>
<td style="text-align:left;">
HURRICANE NICOLE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12103
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.324950e+05
</td>
<td style="text-align:right;">
132495.00
</td>
<td style="text-align:right;">
3.611600e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
958822
</td>
<td style="text-align:right;">
1.452981e+11
</td>
<td style="text-align:right;">
99.2045816
</td>
<td style="text-align:right;">
12.00
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12109
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
7.317271e+05
</td>
<td style="text-align:right;">
731727.09
</td>
<td style="text-align:right;">
3.242650e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
273175
</td>
<td style="text-align:right;">
5.135085e+10
</td>
<td style="text-align:right;">
94.1457206
</td>
<td style="text-align:right;">
81.99
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4680
</td>
<td style="text-align:left;">
12111
</td>
<td style="text-align:left;">
2022-12-13
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.950000e+03
</td>
<td style="text-align:right;">
4950.00
</td>
<td style="text-align:right;">
9.000000e+00
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
328876
</td>
<td style="text-align:right;">
4.431051e+10
</td>
<td style="text-align:right;">
98.3137130
</td>
<td style="text-align:right;">
46.21
</td>
<td style="text-align:left;">
HURRICANE NICOLE
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12115
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2.624248e+07
</td>
<td style="text-align:right;">
26242484.88
</td>
<td style="text-align:right;">
9.647222e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
433908
</td>
<td style="text-align:right;">
8.064207e+10
</td>
<td style="text-align:right;">
98.4091632
</td>
<td style="text-align:right;">
9.23
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12117
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
9.515614e+06
</td>
<td style="text-align:right;">
9515614.45
</td>
<td style="text-align:right;">
8.698750e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
470586
</td>
<td style="text-align:right;">
7.253923e+10
</td>
<td style="text-align:right;">
95.6411072
</td>
<td style="text-align:right;">
64.58
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12119
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.906994e+04
</td>
<td style="text-align:right;">
69069.94
</td>
<td style="text-align:right;">
1.000000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
129698
</td>
<td style="text-align:right;">
2.082162e+10
</td>
<td style="text-align:right;">
88.9277760
</td>
<td style="text-align:right;">
38.67
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4673
</td>
<td style="text-align:left;">
12127
</td>
<td style="text-align:left;">
2022-09-29
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
1.633062e+07
</td>
<td style="text-align:right;">
16330624.32
</td>
<td style="text-align:right;">
2.140567e+05
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
551829
</td>
<td style="text-align:right;">
8.247022e+10
</td>
<td style="text-align:right;">
97.5182946
</td>
<td style="text-align:right;">
45.64
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4642
</td>
<td style="text-align:left;">
19067
</td>
<td style="text-align:left;">
2022-02-23
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.589096e+04
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
2.623704e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
15615
</td>
<td style="text-align:right;">
3.686895e+09
</td>
<td style="text-align:right;">
42.9525931
</td>
<td style="text-align:right;">
86.76
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4640
</td>
<td style="text-align:left;">
20053
</td>
<td style="text-align:left;">
2022-02-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.400319e+04
</td>
<td style="text-align:right;">
14003.19
</td>
<td style="text-align:right;">
1.120000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
6367
</td>
<td style="text-align:right;">
2.468475e+09
</td>
<td style="text-align:right;">
40.0254534
</td>
<td style="text-align:right;">
59.36
</td>
<td style="text-align:left;">
SEVERE STORMS AND STRAIGHT LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4640
</td>
<td style="text-align:left;">
20169
</td>
<td style="text-align:left;">
2022-02-17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.983770e+03
</td>
<td style="text-align:right;">
3983.77
</td>
<td style="text-align:right;">
1.900000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
54303
</td>
<td style="text-align:right;">
1.186593e+10
</td>
<td style="text-align:right;">
77.8873688
</td>
<td style="text-align:right;">
86.38
</td>
<td style="text-align:left;">
SEVERE STORMS AND STRAIGHT LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
21025
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2.769438e+05
</td>
<td style="text-align:right;">
183864.59
</td>
<td style="text-align:right;">
9.392259e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
13610
</td>
<td style="text-align:right;">
1.933137e+09
</td>
<td style="text-align:right;">
29.8759147
</td>
<td style="text-align:right;">
2.77
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
21073
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5.998917e+07
</td>
<td style="text-align:right;">
59989172.12
</td>
<td style="text-align:right;">
1.014424e+06
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
51521
</td>
<td style="text-align:right;">
1.006136e+10
</td>
<td style="text-align:right;">
62.8062361
</td>
<td style="text-align:right;">
71.55
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
21133
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.431930e+05
</td>
<td style="text-align:right;">
243192.99
</td>
<td style="text-align:right;">
1.498740e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
21438
</td>
<td style="text-align:right;">
3.854601e+09
</td>
<td style="text-align:right;">
29.8440980
</td>
<td style="text-align:right;">
5.25
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4663
</td>
<td style="text-align:left;">
21159
</td>
<td style="text-align:left;">
2022-07-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.793846e+04
</td>
<td style="text-align:right;">
17938.46
</td>
<td style="text-align:right;">
9.372000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
11285
</td>
<td style="text-align:right;">
1.444151e+09
</td>
<td style="text-align:right;">
33.6302895
</td>
<td style="text-align:right;">
1.65
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4658
</td>
<td style="text-align:left;">
27001
</td>
<td style="text-align:left;">
2022-07-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.497580e+04
</td>
<td style="text-align:right;">
14975.80
</td>
<td style="text-align:right;">
5.000000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
15674
</td>
<td style="text-align:right;">
5.084602e+09
</td>
<td style="text-align:right;">
10.6904232
</td>
<td style="text-align:right;">
95.96
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4666
</td>
<td style="text-align:left;">
27007
</td>
<td style="text-align:left;">
2022-08-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.079546e+04
</td>
<td style="text-align:right;">
10795.46
</td>
<td style="text-align:right;">
5.000000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
46103
</td>
<td style="text-align:right;">
1.173116e+10
</td>
<td style="text-align:right;">
36.3028953
</td>
<td style="text-align:right;">
88.96
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4666
</td>
<td style="text-align:left;">
27041
</td>
<td style="text-align:left;">
2022-08-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.137020e+03
</td>
<td style="text-align:right;">
9137.02
</td>
<td style="text-align:right;">
1.503800e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
38992
</td>
<td style="text-align:right;">
1.160771e+10
</td>
<td style="text-align:right;">
31.9121858
</td>
<td style="text-align:right;">
97.96
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4659
</td>
<td style="text-align:left;">
27071
</td>
<td style="text-align:left;">
2022-07-13
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.733919e+05
</td>
<td style="text-align:right;">
873391.90
</td>
<td style="text-align:right;">
1.350000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
12059
</td>
<td style="text-align:right;">
2.782644e+09
</td>
<td style="text-align:right;">
0.6681514
</td>
<td style="text-align:right;">
85.65
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4658
</td>
<td style="text-align:left;">
27073
</td>
<td style="text-align:left;">
2022-07-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.950788e+04
</td>
<td style="text-align:right;">
29507.88
</td>
<td style="text-align:right;">
5.400000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
6711
</td>
<td style="text-align:right;">
2.455724e+09
</td>
<td style="text-align:right;">
14.7947821
</td>
<td style="text-align:right;">
86.16
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4658
</td>
<td style="text-align:left;">
27123
</td>
<td style="text-align:left;">
2022-07-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.467121e+04
</td>
<td style="text-align:right;">
64671.21
</td>
<td style="text-align:right;">
1.200000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
552246
</td>
<td style="text-align:right;">
1.001147e+11
</td>
<td style="text-align:right;">
92.1094496
</td>
<td style="text-align:right;">
95.07
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4658
</td>
<td style="text-align:left;">
27149
</td>
<td style="text-align:left;">
2022-07-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.313251e+04
</td>
<td style="text-align:right;">
33132.51
</td>
<td style="text-align:right;">
3.000000e+00
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
9670
</td>
<td style="text-align:right;">
3.506548e+09
</td>
<td style="text-align:right;">
14.6675151
</td>
<td style="text-align:right;">
93.92
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4666
</td>
<td style="text-align:left;">
27159
</td>
<td style="text-align:left;">
2022-08-09
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.376360e+03
</td>
<td style="text-align:right;">
5376.36
</td>
<td style="text-align:right;">
1.000000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
14054
</td>
<td style="text-align:right;">
3.447234e+09
</td>
<td style="text-align:right;">
16.1947184
</td>
<td style="text-align:right;">
85.81
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4665
</td>
<td style="text-align:left;">
29183
</td>
<td style="text-align:left;">
2022-08-08
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.106059e+05
</td>
<td style="text-align:right;">
110605.87
</td>
<td style="text-align:right;">
1.571400e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
405183
</td>
<td style="text-align:right;">
8.092181e+10
</td>
<td style="text-align:right;">
93.0003182
</td>
<td style="text-align:right;">
96.24
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4665
</td>
<td style="text-align:left;">
29189
</td>
<td style="text-align:left;">
2022-08-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.694460e+03
</td>
<td style="text-align:right;">
3694.46
</td>
<td style="text-align:right;">
4.000000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
1003734
</td>
<td style="text-align:right;">
2.351181e+11
</td>
<td style="text-align:right;">
98.5364302
</td>
<td style="text-align:right;">
77.66
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4665
</td>
<td style="text-align:left;">
29510
</td>
<td style="text-align:left;">
2022-08-08
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.184803e+04
</td>
<td style="text-align:right;">
91848.03
</td>
<td style="text-align:right;">
4.000000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
301414
</td>
<td style="text-align:right;">
6.615276e+10
</td>
<td style="text-align:right;">
95.3547566
</td>
<td style="text-align:right;">
61.14
</td>
<td style="text-align:left;">
SEVERE STORMS AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
30049
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.849497e+07
</td>
<td style="text-align:right;">
9247486.18
</td>
<td style="text-align:right;">
1.306541e+06
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
70922
</td>
<td style="text-align:right;">
1.384361e+10
</td>
<td style="text-align:right;">
51.1613108
</td>
<td style="text-align:right;">
76.61
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4655
</td>
<td style="text-align:left;">
30067
</td>
<td style="text-align:left;">
2022-06-16
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.112039e+05
</td>
<td style="text-align:right;">
111203.85
</td>
<td style="text-align:right;">
1.962795e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
17153
</td>
<td style="text-align:right;">
5.181397e+09
</td>
<td style="text-align:right;">
23.7352848
</td>
<td style="text-align:right;">
76.51
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4662
</td>
<td style="text-align:left;">
31107
</td>
<td style="text-align:left;">
2022-07-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.793602e+04
</td>
<td style="text-align:right;">
11300.00
</td>
<td style="text-align:right;">
1.300000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
8383
</td>
<td style="text-align:right;">
3.127981e+09
</td>
<td style="text-align:right;">
35.8574610
</td>
<td style="text-align:right;">
47.71
</td>
<td style="text-align:left;">
SEVERE STORMS AND STRAIGHT-LINE WINDS
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4652
</td>
<td style="text-align:left;">
35049
</td>
<td style="text-align:left;">
2022-05-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.042694e+07
</td>
<td style="text-align:right;">
20426936.70
</td>
<td style="text-align:right;">
4.000000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
154509
</td>
<td style="text-align:right;">
3.089753e+10
</td>
<td style="text-align:right;">
81.1963093
</td>
<td style="text-align:right;">
34.21
</td>
<td style="text-align:left;">
WILDFIRES, STRAIGHT-LINE WINDS, FLOODING, MUDFLOWS, AND DEBRIS FLOWS
</td>
<td style="text-align:left;">
Fire
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4677
</td>
<td style="text-align:left;">
45019
</td>
<td style="text-align:left;">
2022-11-21
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.453644e+06
</td>
<td style="text-align:right;">
1453644.48
</td>
<td style="text-align:right;">
1.643896e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
407722
</td>
<td style="text-align:right;">
8.599560e+10
</td>
<td style="text-align:right;">
99.4591155
</td>
<td style="text-align:right;">
92.71
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4677
</td>
<td style="text-align:left;">
45043
</td>
<td style="text-align:left;">
2022-11-21
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
8.231594e+04
</td>
<td style="text-align:right;">
82315.94
</td>
<td style="text-align:right;">
3.525200e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
63262
</td>
<td style="text-align:right;">
1.409163e+10
</td>
<td style="text-align:right;">
95.2593064
</td>
<td style="text-align:right;">
58.59
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4677
</td>
<td style="text-align:left;">
45051
</td>
<td style="text-align:left;">
2022-11-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.352706e+05
</td>
<td style="text-align:right;">
135270.62
</td>
<td style="text-align:right;">
3.200000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
348574
</td>
<td style="text-align:right;">
6.455071e+10
</td>
<td style="text-align:right;">
98.9818645
</td>
<td style="text-align:right;">
61.30
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4677
</td>
<td style="text-align:left;">
45079
</td>
<td style="text-align:left;">
2022-11-21
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
9.290531e+05
</td>
<td style="text-align:right;">
929053.06
</td>
<td style="text-align:right;">
4.222000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
415644
</td>
<td style="text-align:right;">
8.168495e+10
</td>
<td style="text-align:right;">
93.3503023
</td>
<td style="text-align:right;">
74.19
</td>
<td style="text-align:left;">
HURRICANE IAN
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46009
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.543667e+04
</td>
<td style="text-align:right;">
45436.67
</td>
<td style="text-align:right;">
6.225000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
6996
</td>
<td style="text-align:right;">
2.156272e+09
</td>
<td style="text-align:right;">
17.1174038
</td>
<td style="text-align:right;">
58.63
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46011
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.225375e+04
</td>
<td style="text-align:right;">
22253.75
</td>
<td style="text-align:right;">
1.600000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
34375
</td>
<td style="text-align:right;">
1.010820e+10
</td>
<td style="text-align:right;">
76.2965320
</td>
<td style="text-align:right;">
74.00
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46057
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.860099e+05
</td>
<td style="text-align:right;">
286009.86
</td>
<td style="text-align:right;">
3.328810e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
6164
</td>
<td style="text-align:right;">
2.428631e+09
</td>
<td style="text-align:right;">
33.9484569
</td>
<td style="text-align:right;">
79.09
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46065
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.104682e+05
</td>
<td style="text-align:right;">
110468.23
</td>
<td style="text-align:right;">
3.773000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
17751
</td>
<td style="text-align:right;">
3.816304e+09
</td>
<td style="text-align:right;">
47.1205854
</td>
<td style="text-align:right;">
85.90
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46077
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.252497e+05
</td>
<td style="text-align:right;">
125249.72
</td>
<td style="text-align:right;">
6.525000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
5183
</td>
<td style="text-align:right;">
2.874478e+09
</td>
<td style="text-align:right;">
33.7257397
</td>
<td style="text-align:right;">
91.57
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46087
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.326145e+05
</td>
<td style="text-align:right;">
232614.46
</td>
<td style="text-align:right;">
2.944140e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
5682
</td>
<td style="text-align:right;">
1.196086e+10
</td>
<td style="text-align:right;">
69.1059497
</td>
<td style="text-align:right;">
88.70
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46099
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2.198004e+05
</td>
<td style="text-align:right;">
219800.43
</td>
<td style="text-align:right;">
7.542000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
196891
</td>
<td style="text-align:right;">
3.733547e+10
</td>
<td style="text-align:right;">
92.5867006
</td>
<td style="text-align:right;">
93.13
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4656
</td>
<td style="text-align:left;">
46101
</td>
<td style="text-align:left;">
2022-06-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.411780e+03
</td>
<td style="text-align:right;">
7411.78
</td>
<td style="text-align:right;">
5.000000e+01
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
6336
</td>
<td style="text-align:right;">
3.337204e+09
</td>
<td style="text-align:right;">
53.2930321
</td>
<td style="text-align:right;">
74.35
</td>
<td style="text-align:left;">
SEVERE STORM, STRAIGHT-LINE WINDS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:right;">
4637
</td>
<td style="text-align:left;">
47037
</td>
<td style="text-align:left;">
2022-01-14
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.223647e+06
</td>
<td style="text-align:right;">
1223647.28
</td>
<td style="text-align:right;">
1.823000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
715485
</td>
<td style="text-align:right;">
1.402987e+11
</td>
<td style="text-align:right;">
96.5001591
</td>
<td style="text-align:right;">
63.08
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4637
</td>
<td style="text-align:left;">
47043
</td>
<td style="text-align:left;">
2022-01-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.417185e+05
</td>
<td style="text-align:right;">
241718.45
</td>
<td style="text-align:right;">
4.200000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
54292
</td>
<td style="text-align:right;">
8.838771e+09
</td>
<td style="text-align:right;">
64.8425072
</td>
<td style="text-align:right;">
45.16
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4637
</td>
<td style="text-align:left;">
47131
</td>
<td style="text-align:left;">
2022-01-14
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.045821e+05
</td>
<td style="text-align:right;">
152291.06
</td>
<td style="text-align:right;">
1.122222e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
30777
</td>
<td style="text-align:right;">
4.898492e+09
</td>
<td style="text-align:right;">
77.7601018
</td>
<td style="text-align:right;">
36.76
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4637
</td>
<td style="text-align:left;">
47183
</td>
<td style="text-align:left;">
2022-01-14
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.955783e+07
</td>
<td style="text-align:right;">
9852608.79
</td>
<td style="text-align:right;">
2.925000e+05
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
32891
</td>
<td style="text-align:right;">
5.108212e+09
</td>
<td style="text-align:right;">
78.4918867
</td>
<td style="text-align:right;">
31.19
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4674
</td>
<td style="text-align:left;">
51760
</td>
<td style="text-align:left;">
2022-09-30
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.720723e+06
</td>
<td style="text-align:right;">
1720722.95
</td>
<td style="text-align:right;">
6.744000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
226269
</td>
<td style="text-align:right;">
4.353533e+10
</td>
<td style="text-align:right;">
73.2421254
</td>
<td style="text-align:right;">
54.84
</td>
<td style="text-align:left;">
FLOODING AND MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4650
</td>
<td style="text-align:left;">
53021
</td>
<td style="text-align:left;">
2022-03-29
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.176734e+04
</td>
<td style="text-align:right;">
81767.34
</td>
<td style="text-align:right;">
3.600000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
96653
</td>
<td style="text-align:right;">
1.402875e+10
</td>
<td style="text-align:right;">
68.4696150
</td>
<td style="text-align:right;">
28.61
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS, SNOWSTORMS, STRAIGHT-LINE WINDS, FLOODIN
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4635
</td>
<td style="text-align:left;">
53057
</td>
<td style="text-align:left;">
2022-01-05
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.477661e+04
</td>
<td style="text-align:right;">
64776.61
</td>
<td style="text-align:right;">
2.431746e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
129360
</td>
<td style="text-align:right;">
2.649981e+10
</td>
<td style="text-align:right;">
89.1186764
</td>
<td style="text-align:right;">
77.02
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4635
</td>
<td style="text-align:left;">
53073
</td>
<td style="text-align:left;">
2022-01-05
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.715787e+05
</td>
<td style="text-align:right;">
311774.71
</td>
<td style="text-align:right;">
1.801400e+05
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
226681
</td>
<td style="text-align:right;">
4.568536e+10
</td>
<td style="text-align:right;">
90.9322304
</td>
<td style="text-align:right;">
79.50
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
10
</td>
</tr>
<tr>
<td style="text-align:right;">
4679
</td>
<td style="text-align:left;">
54019
</td>
<td style="text-align:left;">
2022-11-28
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.311186e+04
</td>
<td style="text-align:right;">
53111.86
</td>
<td style="text-align:right;">
4.870600e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
40383
</td>
<td style="text-align:right;">
5.823612e+09
</td>
<td style="text-align:right;">
24.7534203
</td>
<td style="text-align:right;">
26.61
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72005
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.574480e+03
</td>
<td style="text-align:right;">
9574.48
</td>
<td style="text-align:right;">
6.000000e+00
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
54977
</td>
<td style="text-align:right;">
9.841042e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72009
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.721767e+05
</td>
<td style="text-align:right;">
672176.66
</td>
<td style="text-align:right;">
1.600000e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
24637
</td>
<td style="text-align:right;">
3.015895e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72011
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.237893e+06
</td>
<td style="text-align:right;">
1237893.47
</td>
<td style="text-align:right;">
8.295000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
25546
</td>
<td style="text-align:right;">
3.097504e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72027
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.468564e+05
</td>
<td style="text-align:right;">
746856.39
</td>
<td style="text-align:right;">
8.116600e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
32797
</td>
<td style="text-align:right;">
3.642558e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72031
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.234365e+04
</td>
<td style="text-align:right;">
62343.65
</td>
<td style="text-align:right;">
4.000000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
154696
</td>
<td style="text-align:right;">
2.283049e+10
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72059
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.633778e+05
</td>
<td style="text-align:right;">
863377.78
</td>
<td style="text-align:right;">
6.711000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
17690
</td>
<td style="text-align:right;">
1.798984e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72105
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.860372e+05
</td>
<td style="text-align:right;">
286037.25
</td>
<td style="text-align:right;">
1.009000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
29241
</td>
<td style="text-align:right;">
2.934399e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72109
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.258948e+06
</td>
<td style="text-align:right;">
2258948.25
</td>
<td style="text-align:right;">
3.930802e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
15929
</td>
<td style="text-align:right;">
2.009534e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72111
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.559558e+06
</td>
<td style="text-align:right;">
4559557.59
</td>
<td style="text-align:right;">
2.068581e+04
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
20320
</td>
<td style="text-align:right;">
2.518446e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72117
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.232206e+05
</td>
<td style="text-align:right;">
123220.62
</td>
<td style="text-align:right;">
5.532000e+02
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
15187
</td>
<td style="text-align:right;">
2.056385e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4671
</td>
<td style="text-align:left;">
72153
</td>
<td style="text-align:left;">
2022-09-21
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.287350e+05
</td>
<td style="text-align:right;">
928735.00
</td>
<td style="text-align:right;">
2.809000e+03
</td>
<td style="text-align:right;">
2022
</td>
<td style="text-align:left;">
34151
</td>
<td style="text-align:right;">
3.045488e+09
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
HURRICANE FIONA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01001
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.067286e+06
</td>
<td style="text-align:right;">
1533642.78
</td>
<td style="text-align:right;">
4.256000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
58764
</td>
<td style="text-align:right;">
9.123274e+09
</td>
<td style="text-align:right;">
49.2204900
</td>
<td style="text-align:right;">
51.81
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01017
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.655437e+05
</td>
<td style="text-align:right;">
165543.74
</td>
<td style="text-align:right;">
4.200000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
34738
</td>
<td style="text-align:right;">
5.993491e+09
</td>
<td style="text-align:right;">
42.3480751
</td>
<td style="text-align:right;">
40.13
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01047
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
7.667066e+06
</td>
<td style="text-align:right;">
7667065.61
</td>
<td style="text-align:right;">
9.147565e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
38341
</td>
<td style="text-align:right;">
6.955631e+09
</td>
<td style="text-align:right;">
57.8428253
</td>
<td style="text-align:right;">
44.21
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01051
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.624263e+05
</td>
<td style="text-align:right;">
262426.34
</td>
<td style="text-align:right;">
1.287000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
87755
</td>
<td style="text-align:right;">
1.251095e+10
</td>
<td style="text-align:right;">
57.2701241
</td>
<td style="text-align:right;">
57.07
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01063
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
4.183933e+05
</td>
<td style="text-align:right;">
418393.30
</td>
<td style="text-align:right;">
2.392800e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
7714
</td>
<td style="text-align:right;">
1.339905e+09
</td>
<td style="text-align:right;">
18.0400891
</td>
<td style="text-align:right;">
5.98
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01065
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
3.015091e+05
</td>
<td style="text-align:right;">
301509.11
</td>
<td style="text-align:right;">
1.260400e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
14753
</td>
<td style="text-align:right;">
2.427858e+09
</td>
<td style="text-align:right;">
33.1530385
</td>
<td style="text-align:right;">
13.40
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01101
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.423541e+06
</td>
<td style="text-align:right;">
1423541.34
</td>
<td style="text-align:right;">
3.996000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
228847
</td>
<td style="text-align:right;">
4.622347e+10
</td>
<td style="text-align:right;">
86.2551702
</td>
<td style="text-align:right;">
67.92
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01119
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.275280e+04
</td>
<td style="text-align:right;">
52752.80
</td>
<td style="text-align:right;">
4.354500e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
12322
</td>
<td style="text-align:right;">
3.236461e+09
</td>
<td style="text-align:right;">
51.0658606
</td>
<td style="text-align:right;">
7.54
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4684
</td>
<td style="text-align:left;">
01123
</td>
<td style="text-align:left;">
2023-01-15
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.998020e+04
</td>
<td style="text-align:right;">
49980.20
</td>
<td style="text-align:right;">
1.200000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
41279
</td>
<td style="text-align:right;">
8.545817e+09
</td>
<td style="text-align:right;">
50.0477251
</td>
<td style="text-align:right;">
43.25
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4698
</td>
<td style="text-align:left;">
05037
</td>
<td style="text-align:left;">
2023-04-02
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.071781e+07
</td>
<td style="text-align:right;">
10704864.50
</td>
<td style="text-align:right;">
3.360577e+05
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
16824
</td>
<td style="text-align:right;">
3.357621e+09
</td>
<td style="text-align:right;">
66.4969774
</td>
<td style="text-align:right;">
33.64
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4698
</td>
<td style="text-align:left;">
05119
</td>
<td style="text-align:left;">
2023-04-02
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.806886e+05
</td>
<td style="text-align:right;">
380688.64
</td>
<td style="text-align:right;">
1.059500e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
398830
</td>
<td style="text-align:right;">
7.392333e+10
</td>
<td style="text-align:right;">
95.0684060
</td>
<td style="text-align:right;">
73.68
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4683
</td>
<td style="text-align:left;">
06085
</td>
<td style="text-align:left;">
2023-01-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.337007e+05
</td>
<td style="text-align:right;">
133700.70
</td>
<td style="text-align:right;">
1.500000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
1934625
</td>
<td style="text-align:right;">
3.815867e+11
</td>
<td style="text-align:right;">
99.8409163
</td>
<td style="text-align:right;">
68.94
</td>
<td style="text-align:left;">
SEVERE WINTER STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4709
</td>
<td style="text-align:left;">
12011
</td>
<td style="text-align:left;">
2023-04-27
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.540372e+06
</td>
<td style="text-align:right;">
1540372.01
</td>
<td style="text-align:right;">
4.170200e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
1940829
</td>
<td style="text-align:right;">
2.681728e+11
</td>
<td style="text-align:right;">
99.7454661
</td>
<td style="text-align:right;">
44.84
</td>
<td style="text-align:left;">
SEVERE STORMS, TORNADOES, AND FLOODING
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4734
</td>
<td style="text-align:left;">
12017
</td>
<td style="text-align:left;">
2023-08-31
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.434055e+06
</td>
<td style="text-align:right;">
1434055.44
</td>
<td style="text-align:right;">
8.128043e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
153476
</td>
<td style="text-align:right;">
2.146307e+10
</td>
<td style="text-align:right;">
95.8956411
</td>
<td style="text-align:right;">
7.13
</td>
<td style="text-align:left;">
HURRICANE IDALIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4734
</td>
<td style="text-align:left;">
12075
</td>
<td style="text-align:left;">
2023-08-31
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.062832e+06
</td>
<td style="text-align:right;">
1062831.95
</td>
<td style="text-align:right;">
4.746850e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
42750
</td>
<td style="text-align:right;">
4.701759e+09
</td>
<td style="text-align:right;">
83.2643971
</td>
<td style="text-align:right;">
8.40
</td>
<td style="text-align:left;">
HURRICANE IDALIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4734
</td>
<td style="text-align:left;">
12101
</td>
<td style="text-align:left;">
2023-08-31
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1.103308e+05
</td>
<td style="text-align:right;">
110330.80
</td>
<td style="text-align:right;">
1.611960e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
561566
</td>
<td style="text-align:right;">
8.980181e+10
</td>
<td style="text-align:right;">
99.0454979
</td>
<td style="text-align:right;">
13.21
</td>
<td style="text-align:left;">
HURRICANE IDALIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4734
</td>
<td style="text-align:left;">
12103
</td>
<td style="text-align:left;">
2023-08-31
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
7.846824e+04
</td>
<td style="text-align:right;">
78468.24
</td>
<td style="text-align:right;">
5.098200e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
958822
</td>
<td style="text-align:right;">
1.452981e+11
</td>
<td style="text-align:right;">
99.2045816
</td>
<td style="text-align:right;">
12.00
</td>
<td style="text-align:left;">
HURRICANE IDALIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4685
</td>
<td style="text-align:left;">
13035
</td>
<td style="text-align:left;">
2023-01-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.599814e+06
</td>
<td style="text-align:right;">
2599813.53
</td>
<td style="text-align:right;">
2.117000e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
25426
</td>
<td style="text-align:right;">
3.890726e+09
</td>
<td style="text-align:right;">
16.6083360
</td>
<td style="text-align:right;">
42.46
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4738
</td>
<td style="text-align:left;">
13075
</td>
<td style="text-align:left;">
2023-09-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.687436e+05
</td>
<td style="text-align:right;">
168743.57
</td>
<td style="text-align:right;">
8.300000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
17209
</td>
<td style="text-align:right;">
3.573371e+09
</td>
<td style="text-align:right;">
60.0381801
</td>
<td style="text-align:right;">
33.83
</td>
<td style="text-align:left;">
HURRICANE IDALIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4738
</td>
<td style="text-align:left;">
13185
</td>
<td style="text-align:left;">
2023-09-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.758775e+06
</td>
<td style="text-align:right;">
4758774.53
</td>
<td style="text-align:right;">
6.000000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
118124
</td>
<td style="text-align:right;">
2.074955e+10
</td>
<td style="text-align:right;">
81.3553929
</td>
<td style="text-align:right;">
42.20
</td>
<td style="text-align:left;">
HURRICANE IDALIA
</td>
<td style="text-align:left;">
Hurricane
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4685
</td>
<td style="text-align:left;">
13255
</td>
<td style="text-align:left;">
2023-01-16
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
6.697894e+06
</td>
<td style="text-align:right;">
6697893.83
</td>
<td style="text-align:right;">
5.685100e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
67247
</td>
<td style="text-align:right;">
1.203496e+10
</td>
<td style="text-align:right;">
44.8615972
</td>
<td style="text-align:right;">
40.20
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4685
</td>
<td style="text-align:left;">
13285
</td>
<td style="text-align:left;">
2023-01-16
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.250307e+04
</td>
<td style="text-align:right;">
12503.07
</td>
<td style="text-align:right;">
1.152000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
69343
</td>
<td style="text-align:right;">
1.439801e+10
</td>
<td style="text-align:right;">
67.3242125
</td>
<td style="text-align:right;">
46.02
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4704
</td>
<td style="text-align:left;">
18007
</td>
<td style="text-align:left;">
2023-04-15
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.208304e+04
</td>
<td style="text-align:right;">
12083.04
</td>
<td style="text-align:right;">
8.646000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
8719
</td>
<td style="text-align:right;">
2.087997e+09
</td>
<td style="text-align:right;">
11.1358575
</td>
<td style="text-align:right;">
62.35
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4704
</td>
<td style="text-align:left;">
18023
</td>
<td style="text-align:left;">
2023-04-15
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.632260e+03
</td>
<td style="text-align:right;">
6632.26
</td>
<td style="text-align:right;">
2.193614e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
33183
</td>
<td style="text-align:right;">
5.454152e+09
</td>
<td style="text-align:right;">
40.0572701
</td>
<td style="text-align:right;">
71.07
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4704
</td>
<td style="text-align:left;">
18081
</td>
<td style="text-align:left;">
2023-04-15
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1.430079e+04
</td>
<td style="text-align:right;">
13300.79
</td>
<td style="text-align:right;">
6.262000e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
161668
</td>
<td style="text-align:right;">
3.055103e+10
</td>
<td style="text-align:right;">
81.7371938
</td>
<td style="text-align:right;">
88.86
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4704
</td>
<td style="text-align:left;">
18109
</td>
<td style="text-align:left;">
2023-04-15
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
9.961140e+04
</td>
<td style="text-align:right;">
99611.40
</td>
<td style="text-align:right;">
6.060000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
71729
</td>
<td style="text-align:right;">
1.362281e+10
</td>
<td style="text-align:right;">
62.5835189
</td>
<td style="text-align:right;">
78.80
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:right;">
4702
</td>
<td style="text-align:left;">
21073
</td>
<td style="text-align:left;">
2023-04-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.150672e+05
</td>
<td style="text-align:right;">
115067.20
</td>
<td style="text-align:right;">
8.000000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
51521
</td>
<td style="text-align:right;">
1.006136e+10
</td>
<td style="text-align:right;">
62.8062361
</td>
<td style="text-align:right;">
71.55
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4702
</td>
<td style="text-align:left;">
21155
</td>
<td style="text-align:left;">
2023-04-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
6.451137e+04
</td>
<td style="text-align:right;">
64511.37
</td>
<td style="text-align:right;">
2.724600e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
19521
</td>
<td style="text-align:right;">
4.044451e+09
</td>
<td style="text-align:right;">
33.3439389
</td>
<td style="text-align:right;">
61.90
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4702
</td>
<td style="text-align:left;">
21203
</td>
<td style="text-align:left;">
2023-04-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.287000e+03
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
6.000000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
16028
</td>
<td style="text-align:right;">
2.658793e+09
</td>
<td style="text-align:right;">
17.6901050
</td>
<td style="text-align:right;">
20.66
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4702
</td>
<td style="text-align:left;">
21227
</td>
<td style="text-align:left;">
2023-04-10
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.064716e+05
</td>
<td style="text-align:right;">
206471.58
</td>
<td style="text-align:right;">
1.124000e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
134482
</td>
<td style="text-align:right;">
2.463868e+10
</td>
<td style="text-align:right;">
83.0098632
</td>
<td style="text-align:right;">
64.99
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, TORNADOES, FLOODING, LANDSLIDES, AND
MUDSLIDES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4696
</td>
<td style="text-align:left;">
23013
</td>
<td style="text-align:left;">
2023-03-22
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
8.691740e+03
</td>
<td style="text-align:right;">
8691.74
</td>
<td style="text-align:right;">
1.400000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
40597
</td>
<td style="text-align:right;">
9.304553e+09
</td>
<td style="text-align:right;">
50.4931594
</td>
<td style="text-align:right;">
97.61
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4696
</td>
<td style="text-align:left;">
23031
</td>
<td style="text-align:left;">
2023-03-22
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.426894e+04
</td>
<td style="text-align:right;">
24268.94
</td>
<td style="text-align:right;">
2.000000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
211925
</td>
<td style="text-align:right;">
4.450945e+10
</td>
<td style="text-align:right;">
83.6143812
</td>
<td style="text-align:right;">
85.84
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4697
</td>
<td style="text-align:left;">
28015
</td>
<td style="text-align:left;">
2023-03-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
5.004668e+05
</td>
<td style="text-align:right;">
500466.78
</td>
<td style="text-align:right;">
3.118400e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
9971
</td>
<td style="text-align:right;">
1.639614e+09
</td>
<td style="text-align:right;">
16.0674515
</td>
<td style="text-align:right;">
26.26
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4727
</td>
<td style="text-align:left;">
28059
</td>
<td style="text-align:left;">
2023-08-12
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7.718024e+05
</td>
<td style="text-align:right;">
771802.42
</td>
<td style="text-align:right;">
1.481796e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
143149
</td>
<td style="text-align:right;">
2.306976e+10
</td>
<td style="text-align:right;">
95.9910913
</td>
<td style="text-align:right;">
75.88
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES.
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4697
</td>
<td style="text-align:left;">
28095
</td>
<td style="text-align:left;">
2023-03-26
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
2.967720e+07
</td>
<td style="text-align:right;">
19139114.29
</td>
<td style="text-align:right;">
1.972424e+05
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
34055
</td>
<td style="text-align:right;">
5.885635e+09
</td>
<td style="text-align:right;">
63.0607700
</td>
<td style="text-align:right;">
43.76
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4697
</td>
<td style="text-align:left;">
28097
</td>
<td style="text-align:left;">
2023-03-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
3.707440e+04
</td>
<td style="text-align:right;">
37074.40
</td>
<td style="text-align:right;">
7.224000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
9775
</td>
<td style="text-align:right;">
1.587660e+09
</td>
<td style="text-align:right;">
22.2717149
</td>
<td style="text-align:right;">
27.59
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4697
</td>
<td style="text-align:left;">
28125
</td>
<td style="text-align:left;">
2023-03-26
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
1.078583e+07
</td>
<td style="text-align:right;">
10785827.85
</td>
<td style="text-align:right;">
4.687428e+05
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
3773
</td>
<td style="text-align:right;">
7.697461e+08
</td>
<td style="text-align:right;">
29.0486796
</td>
<td style="text-align:right;">
25.46
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4697
</td>
<td style="text-align:left;">
28149
</td>
<td style="text-align:left;">
2023-03-26
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.000000e+04
</td>
<td style="text-align:right;">
10000.00
</td>
<td style="text-align:right;">
2.000000e+01
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
44654
</td>
<td style="text-align:right;">
9.795010e+09
</td>
<td style="text-align:right;">
76.5192491
</td>
<td style="text-align:right;">
68.11
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4725
</td>
<td style="text-align:left;">
34041
</td>
<td style="text-align:left;">
2023-08-11
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.173197e+05
</td>
<td style="text-align:right;">
417319.66
</td>
<td style="text-align:right;">
6.346000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
109580
</td>
<td style="text-align:right;">
2.614011e+10
</td>
<td style="text-align:right;">
75.3102132
</td>
<td style="text-align:right;">
84.12
</td>
<td style="text-align:left;">
SEVERE STORM AND FLOODING
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4706
</td>
<td style="text-align:left;">
40125
</td>
<td style="text-align:left;">
2023-04-24
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4.336627e+06
</td>
<td style="text-align:right;">
4336627.23
</td>
<td style="text-align:right;">
2.172210e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
72357
</td>
<td style="text-align:right;">
8.784389e+09
</td>
<td style="text-align:right;">
85.9051861
</td>
<td style="text-align:right;">
51.85
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Tornado
</td>
<td style="text-align:right;">
6
</td>
</tr>
<tr>
<td style="text-align:right;">
4701
</td>
<td style="text-align:left;">
47015
</td>
<td style="text-align:left;">
2023-04-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.541853e+05
</td>
<td style="text-align:right;">
154185.33
</td>
<td style="text-align:right;">
1.250000e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
14490
</td>
<td style="text-align:right;">
2.125417e+09
</td>
<td style="text-align:right;">
18.8355075
</td>
<td style="text-align:right;">
26.67
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4701
</td>
<td style="text-align:left;">
47071
</td>
<td style="text-align:left;">
2023-04-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.393908e+05
</td>
<td style="text-align:right;">
239390.75
</td>
<td style="text-align:right;">
9.500000e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
26745
</td>
<td style="text-align:right;">
4.834413e+09
</td>
<td style="text-align:right;">
52.4021635
</td>
<td style="text-align:right;">
34.09
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4701
</td>
<td style="text-align:left;">
47149
</td>
<td style="text-align:left;">
2023-04-07
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.144501e+05
</td>
<td style="text-align:right;">
114450.07
</td>
<td style="text-align:right;">
6.318400e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
341253
</td>
<td style="text-align:right;">
6.433455e+10
</td>
<td style="text-align:right;">
88.2278078
</td>
<td style="text-align:right;">
70.53
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4751
</td>
<td style="text-align:left;">
47165
</td>
<td style="text-align:left;">
2023-12-13
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2.074564e+05
</td>
<td style="text-align:right;">
207456.41
</td>
<td style="text-align:right;">
1.800474e+00
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
196179
</td>
<td style="text-align:right;">
3.417330e+10
</td>
<td style="text-align:right;">
83.3598473
</td>
<td style="text-align:right;">
57.54
</td>
<td style="text-align:left;">
SEVERE STORMS AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4701
</td>
<td style="text-align:left;">
47167
</td>
<td style="text-align:left;">
2023-04-07
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
6.118566e+06
</td>
<td style="text-align:right;">
3362144.66
</td>
<td style="text-align:right;">
6.664860e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
60942
</td>
<td style="text-align:right;">
8.944088e+09
</td>
<td style="text-align:right;">
80.3372574
</td>
<td style="text-align:right;">
56.27
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4701
</td>
<td style="text-align:left;">
47181
</td>
<td style="text-align:left;">
2023-04-07
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
5.857980e+05
</td>
<td style="text-align:right;">
585797.97
</td>
<td style="text-align:right;">
1.340000e+03
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
16204
</td>
<td style="text-align:right;">
2.124535e+09
</td>
<td style="text-align:right;">
44.6706968
</td>
<td style="text-align:right;">
11.39
</td>
<td style="text-align:left;">
SEVERE STORMS, STRAIGHT-LINE WINDS, AND TORNADOES
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
4720
</td>
<td style="text-align:left;">
50023
</td>
<td style="text-align:left;">
2023-07-14
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
1.871425e+06
</td>
<td style="text-align:right;">
1871424.72
</td>
<td style="text-align:right;">
8.999176e+04
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
59777
</td>
<td style="text-align:right;">
1.290725e+10
</td>
<td style="text-align:right;">
56.7610563
</td>
<td style="text-align:right;">
90.87
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4720
</td>
<td style="text-align:left;">
50025
</td>
<td style="text-align:left;">
2023-07-14
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2.713912e+04
</td>
<td style="text-align:right;">
27139.12
</td>
<td style="text-align:right;">
1.853500e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
45853
</td>
<td style="text-align:right;">
1.231266e+10
</td>
<td style="text-align:right;">
48.6159720
</td>
<td style="text-align:right;">
69.76
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4720
</td>
<td style="text-align:left;">
50027
</td>
<td style="text-align:left;">
2023-07-14
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1.363750e+04
</td>
<td style="text-align:right;">
13637.50
</td>
<td style="text-align:right;">
6.684800e+02
</td>
<td style="text-align:right;">
2023
</td>
<td style="text-align:left;">
57706
</td>
<td style="text-align:right;">
1.478996e+10
</td>
<td style="text-align:right;">
56.9201400
</td>
<td style="text-align:right;">
67.00
</td>
<td style="text-align:left;">
SEVERE STORMS, FLOODING, LANDSLIDES, AND MUDSLIDES
</td>
<td style="text-align:left;">
Flood
</td>
<td style="text-align:right;">
1
</td>
</tr>
</tbody>
</table>

</div>

### The Data Summary

    ## [1] 319.7977

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    0.00   28.98  117.58     Inf  495.34     Inf

Minimum, Central Tendency and Quartile Points. The first quartile value
29 dollars, is close to the currently used value of **35 dollars** per
yard of Construction Debris removal.

Median Cost is at 117 dollars. The Mean value is not calculated here,
because projects with 0 values (for quantity) create an error. The
trimmed mean (which removes the outlier/errors) is \$319.

The Mean is skewed by extremely high cost projects. This is why we log
(exponentially) scale quantity and cost in most of the analysis.

### Viewing that summary:

![](/images/unnamed-chunk-19-1.png)<!-- --> Again, we see that Region 4
looks similar to others, so we can attempt to use all the data in grants
manager to fit a more robust relationship.

### The Cost & Quantity Relationship

- Here Region 4 is shown as a separate color, however, it is very
  similar to the spread of other projects.
- Note: We have removed extreme outliers from this chart already.
- This chart is **interactive** if you are viewing in your browser.

<div class="plotly html-widget html-fill-item" id="htmlwidget-25dfe2217397d5e57ac0" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-25dfe2217397d5e57ac0">{"x":{"data":[{"x":[4.1984096335377616,2.2900346113625178,2.0413926851582249,5.2442501466886622,5.1216601175420404,1.9030899869919435,5.074771948771585,5.2430722945580479,5.7822159849460446,3.6989700043360187,4.1925674533365456,1.2355284469075489,3.4890016973113775,5.8994042226163863,4.8991850050849477,6.1582259875720986,3.8829796540372992,6.0433759787151002,2.4313637641589874,3.2050418792613695,3.5151052041667898,3.5724068675580556,3.9456654994321343,6.2333936760774149,3.5147734739975087,1.8450980400142569,2.9444826721501687,6.1130413755231467,6.7864854403677892,4.3750496821292746,6.4300969955807767,3.0637085593914173,5.2695250856143598,1.3010299956639813,1.2950711714662781,2.0791812460476247,4.3639093040106847,2.2036169956331912,2.0901169107520099,4.1027629037125646,3.7320719409998668,2.255272505103306,1.5440680443502757,2.357934847000454,4.1587293762389352,3.2398898183400542,1.8450980400142569,3.370883016777606,3.8958091501691308,5.0504952949350823,3.6037612606082874,3.0926855629374908,2.5440680443502757,4.5093080578565763,3.7281150573980244,2.9858780492079968,5.3515017638802327,3.1965768448522329,3.8655623192261745,3.6868149545073168,4.3139776773448899,3.7225993750077753,5.3979330599047373,1.6654497448426819,2.9929950984313414,4.4581239446610619,3.2487087356009177,4.56059520730137,2.5717088318086878,2.4771212547196626,2.9786369483844743,2.7874604745184151,1.7781512503836436,2.5910646070264991,4.5798258811262302,3.4367587960456936,4.082507449923134,3.1283992687178066,1.9834007381805383,4.9780891730561425,2.8407332346118066,4.0483446785400696,4.5002620046610948,2.255272505103306,4.2944794532032704,3.1655410767223731,1.3010299956639813,1.954242509439325,2.5436956323092446,2.5563025007672873,3.247354508217859,4.2418760607224613,2.3617278360175931,4.8930178883115341,1.4471580313422192,1.255272505103306,4.3544284692600153,5.0044589502411947,3.7185016888672742,2.8573324964312685,1.7781512503836436,1.6011905326153335,3.4377505628203879,2.859138297294531,3.3802112417116059,3.3644952932801808,4.0387790695555381,2.6919651027673601,1.9822712330395684,1.4771212547196624,2.8853612200315122,3.1654105314399699,2.510545010206612,4.1689457569017909,4.300986564044174,3.0097482559485536,3.4505570094183291,4.6672567755966927,3.2108533653148932,4.4171061673925927,3.3070144100729419,1.6972293427597176,1.8375884382355112,4.0791479488609372,1.1814205162624751,3.7875313161272341,2.5646660642520893,1.9030899869919435,6.4771799528902045,3.4117880045438689,1.5954962218255742,2.6989700043360187,2.3096301674258988,3.3344537511509307,1.9118112227541586,2.3873898263387292,2.3010299956639813,2.8846065435331911,2.9444826721501687,2.840106094456758,3.2464985807958011,4.4813193459054528,3.4352963763370234,3.7289119178080758,3.9476297473843545,3.0025979807199086,2.5172038181418617,2.1760912590556813,3.7564877686873519,3.0591846176313711,3.4653828514484184,3.4185051298166429,2.4297522800024081,2.8522603510069531,4.1983546247369397,2.0906107078284069,4.8240028857830639,2.8350561017201161,3.5399538416563967,2.9469432706978256,2.7558748556724915,3.6532125137753435,2.7554479706597705,3.2417257394831371,2.6473829701146196,2.3222192947339191,2.4149733479708178,2.7323937598229686,4.0387790695555381,3.6991436873944838,1.4456042032735976,1.2787536009528289,2.5224442335063197,3.8205954965444904,2.8927622346158168,1.3010299956639813,3.0195316845312554,2.0791812460476247,3.3926969532596658,1.6020599913279623,5.029158048255157,5.029158048255157,3.9945397430417637,5.4889805680440604,5.4842383116077116,5.7900105637080417,3.7134905430939424,1.9461573949223723,3.0879587894607328,2.9794391044854023,4.1517543233268732,4.1445726509957801,4.8625493655096754,1.7634279935629373,3.6957442751973235,4.5925986289061314,4.3913938751356989,3.986709048064589,4.151086869007643,3.7563622110126267,2.3924859087190731,2.853819845856763,4.3939899976265382,3.584218112117405,3.0674428427763805,3.9030899869919438,2.3502480183341627,4.0116972881141422,1.954242509439325,2.0791812460476247,3.1325798476597368,3.8573324964312685,1.568201724066995,4.8375909631960896,3.1613680022349748,2.5514499979728753,4.8655301206260217,5.4485649734633181,2.2709581850920975,4.1461280356782382,3.79155030502733,5.633076829294466,1.2176951179079363,1.2193225084193366,5.0580385129555649,2.8349672019384444,1.2253092817258628,2.4668676203541096,4.5422070194326958,3.949953248133617,4.6215371780320238,4.2326658194314453,4.4717902765451916,5.8684909977946704,4.1699094419010692,4.4664969037444004,6.2266885002344523,4.2900791521022015,4.0513069108179742,5.1873723585514089,3.2819419334408249,4.7196129897309733,3.9893532476043831,4.4226896209220463,2.5974757898703773,5.7670301623264155,2.6017884724182725,4.5933890889502704,5.2991178280338591,4.9826612443139986,2.2041199826559246,5.416142697043469,2.9506082247842307,4.5648352996192942,5.3887669549130184,4.6384792730586861,3.8402592002021061,4.4654230069584191,4.135100197389721,3.2132520521963968,4.4036581640982106,2.6812412373755872,2.0606978403536118,5.2748662569248337,5.1887218566113926,6.1861142194078909,1.6020599913279623,5.4330173650934359,4.313107595194996,2.8620120512502165,2,4.1125379756093077,3.7909884750888159,1.1667260555800518,5.7549336270084162,3.287801729930226,4.9194382504422851,2.1760912590556813,4.7447497302049486,4.1787467965289578,2.710608102952619,3.5395904205339157,3.844216147843321,1.0413926851582251,1.9294189257142926,3.7084209001347128,3.1061908972634154,4.4139532291554149,1.6989700043360187,2.4189148374041607,2.0492180226701815,1.2787536009528289,2.6989700043360187,1.6989700043360187,2.1771900804896092,2.1303337684950061,2.7323937598229686,2.0791812460476247,3,4.196286748808876,1.6020599913279623,1.6020599913279623,6.1161229762050935,4.29287494299383,2.1139433523068369,3.6020599913279625,2.7941393557677738,2.2041199826559246,3.5222890074407114,3.5766868052009957,2.8145805160103188,3.4689584577652681,2.8774865280696016,1.6989700043360187,3.8289175616166857,2.5563025007672873,4.3859182101733625,5.2556101584077384,2.687582464425827,4.204119982655925,2.9188163903603797,2.909374143715874,2.6020599913279625,2.826787238816292,2.0038911662369103,4.5944811683499527,4.3156725313539823,2.7428821714372731,3.4485517392015779,5.5264138507238521,4.0251009610468138,1.1760912590556813,3.9368152311976328,1.3411601596967189,2.7967130632808965,3.782472624166286,1.146128035678238,1.3010299956639813,3.8025000677643934,4.3369018086850204,4.9542027455464295,2.2679925903655827,2.8250884183002145],"y":[6.4604701406839471,4.5986519917760278,5.2400097891921718,3.5197177660831542,5.2216908461840275,3.1601742997562785,6.4544584434010588,6.9148312682869921,5.5771627948366147,5.0635944202049918,5.1330082980230616,4.5074746852312275,5.327119669307554,7.40883160407778,5.8617082909736826,7.6418148412592224,4.8232410430396238,7.1924950374752452,3.815127947639156,4.6182886187745762,5.502232449610565,5.3465794527796877,5.3002018103724424,7.7667360311871407,4.8186260785307908,5.0909813215118893,4.0764802384571386,8.0080819873537621,7.3196145498913712,6.8884389598789264,4.1199895437319283,4.9072332527703013,6.6054554002336214,3.3154497193600747,4.3608434300824799,3.7381214843438024,7.0483153320246821,3.90848501887865,4.2572481461680107,5.7994706321883838,5.784261753054154,5.0474345337590485,3.8725798675626004,3.6118507106822602,6.6339294235130861,5.0850685209794602,4.4470630966015001,5.1845460455678261,6.6289572375519548,6.7840462679965823,6.5913353519955891,5.3685374944238422,4.4727564493172123,6.1567096319513919,5.4262259809774767,6.0838430062433657,5.862879349954067,4.9002327223981421,5.7879122033256358,6.9256904488381803,6.2285794913292323,5.8002572868192974,5.7668151862304109,5.2686636289576345,7.819872821950546,7.3766456073837192,6.739972792873048,5.3616855374517396,5.9722913411688872,3.8124059380049546,3.5720230867438456,3.7235993184736782,3.801240390075832,4.4197377208751654,5.552155689619263,5.2259699664246506,4.6353668133699344,5.5863301397155096,4.6598683965365444,5.5509925279820198,4.0470886456150916,4.5981118428941388,6.8525920521635637,3.8531108481320895,4.5609886194818108,3.6842167951388807,3.3713745322010444,4.4580396970325129,4.913401341591757,3.9628426812012423,3.5610417237160132,4.4136654905707937,3.6987293385224569,5.7465047162964922,4.1379535430124097,3.9718897371949677,4.6917426905820001,5.8559958788866977,5.5117349405366287,4.1193779761877272,3.5708183726778508,5.5182642804090385,5.3557305896504479,4.3818944857620172,4.6037293554415095,6.9048191427129249,4.1644489253755248,4.4641112752965482,4.5878016299579336,3.8752708322897389,5.11682254709637,5.7345092829903495,4.5215820928189556,6.2339214891782326,3.5999779954176843,5.0133757110388455,6.3926964679748721,5.9206693535394539,5.8091438789657852,4.0108767948608639,5.3399591794907808,3.8494433699746264,6.0326165853441553,4.1689362704543882,4.4988561811014973,5.2108333204916137,4.6834662633889961,4.6072074584292029,6.2443003023524133,4.6279213775994306,4.4844564419054294,4.1139433523068369,5.2811844799821337,4.5185092020940605,4.5210701871812304,4.1256422663922292,4.926299786932737,5.2848817146554525,6.1330310873925402,4.0791812460476251,5.6339000307857878,5.2001429080571606,3.9467916091972901,5.5281070919144648,5.7070687790680239,4.7614099830157182,4.1084970330598569,4.4589471076189442,7.606007622399213,4.2578273132517888,4.0672174568569686,5.2800909040719084,4.9007322741291341,4.8443466192559903,4.1666739844552794,4.3052651469835448,6.3930746322224365,3.9353515569460962,5.4223682209113662,4.3592395782103628,4.506587959850652,4.9439505823477052,5.5766316429606348,4.7148531966181677,4.8223160674664394,3.6064469343274004,4.8640320250727047,3.9869821146488182,5.1208756939100448,5.4745535419964293,3.6961579835661427,4.0879485038836672,4.5579203670399142,6.1194873468094526,5.5336260915984878,4.0687345078148729,5.5179962814768535,5.1419478132010834,4.7587432868327415,3.1732649589406052,4.103463333259505,4.6185710281201295,5.9873685326641795,4.6164229962656824,7.5958707300105068,4.4755304192410623,5.1349993200051491,5.3471961734382329,6.2687344067161144,6.6117877359644712,6.80526658316158,7.2085830613982269,7.0114716487615398,5.4334132621843452,7.2010939648407293,6.8634427498975201,4.784830607903209,6.8713034743951846,6.5343343000433709,6.6288562818332899,4.8487037435328721,5.9532779036067067,6.7814462300277087,7.1217127522788619,6.4328449270275794,4.5505207611034422,3.6003182396537019,5.053241023663599,4.8137360660495405,5.2233093073972423,5.6929563159940688,5.0214270619047161,4.0841861313198455,6.9538762520331794,5.3135217978417426,5.4951443249567786,7.1417126668064519,7.4619096724590035,5.1183212170820784,4.4102135498336335,5.6933860962344154,4.9428608643872387,5.4601436196700934,3.6334684555795866,6.0630321461817509,5.4147889006899996,5.5363621071927254,3.7711934242561349,7.216871984783082,5.6091569387189741,6.0430115487780958,6.479628131969557,5.8100761841840338,8.0629745055257089,6.7660842340745706,6.0534683686008908,8.0018395758823715,7.3237697600185427,6.4903584673197878,7.1195575076376212,5.8306840297962461,7.2124811471304335,5.9038494139654381,6.3308046491372156,5.5102357602011862,7.3962418692459595,6.5451776056376829,6.8615204463046675,7.7514270639279017,7.7176311914990361,5.1578300948393156,7.1778593071671768,5.4117065225153711,7.0803231189973888,6.6127636191846202,6.5754143755183518,5.6293925736097341,6.1869807207830387,5.7889238952518367,5.3827840029756402,6.1728773489598723,5.2677453693155343,4.3966980632546919,6.2200605066417811,6.8580928290117642,6.5885433277201484,3.5835070929572823,3.7493358382003681,4.053740142141546,5.0558914208236567,4.486146996806573,4.959595027236654,3.9169074835848621,4.0787365904735511,5.1574655325134131,4.144667594231179,6.6723755272699137,4.5021790369467034,5.4390081309152905,6.8175032504300628,5.041141313845162,4.9237492268586367,5.3561462759051581,5.4662913013919932,5.5475620581904002,4.057792722982879,6.6596523967791921,4.9116043025964364,3.7389564596728158,4.2011501344703666,4.1462269815057144,3.6002942568063161,4.1753900314824133,4.033241152571625,3.9608045755389045,5.9412091598756627,4.4699380086400833,4.8107109866840432,3.7304883412918475,5.0437781761636158,3.5675509688042939,4.9630698457882128,7.267053686616082,5.0461198232605273,4.9909424508445497,7.3102032430869075,4.6574064949384679,4.3474032048486633,5.4563810054009965,5.0432373954913183,5.0977767636603142,5.3666367082557818,5.3420285377073284,3.8699225199015808,6.235710951110808,4.9125798698065006,4.8114182160295558,5.5700508226671417,4.725191510863529,5.8274834283816146,6.0926832720008672,5.8732371011105711,4.7947922250895996,5.9362008674062299,5.456422594024243,6.3539062818292287,6.6589227054839863,5.0906833896700299,5.9678918125346234,7.3163438526082585,5.5805699173740617,5.1261336810491311,4.0821762131864396,3.8216615432285819,4.1553600292947133,4.9983090439837126,3.9391067265930175,4.385050807967529,5.6204688448803939,6.6371520924480896,6.2721723618559135,4.4335957613288697,4.1347347635963985],"text":["Quantity: 1.579100e+04<br />Gross_Cost:   2887155.27<br />Predicted Cost = $1,144,271<br />R4: Other_Regions","Quantity: 1.950000e+02<br />Gross_Cost:     39687.34<br />Predicted Cost = $19,646<br />R4: Other_Regions","Quantity: 1.100000e+02<br />Gross_Cost:    173784.00<br />Predicted Cost = $11,569<br />R4: Other_Regions","Quantity: 1.754891e+05<br />Gross_Cost:      3309.16<br />Predicted Cost = $10,615,278<br />R4: Other_Regions","Quantity: 1.323306e+05<br />Gross_Cost:    166606.08<br />Predicted Cost = $8,175,901<br />R4: Other_Regions","Quantity: 8.000000e+01<br />Gross_Cost:      1446.02<br />Predicted Cost = $8,617<br />R4: Other_Regions","Quantity: 1.187878e+05<br />Gross_Cost:   2847465.32<br />Predicted Cost = $7,398,847<br />R4: Other_Regions","Quantity: 1.750138e+05<br />Gross_Cost:   8219232.55<br />Predicted Cost = $10,588,681<br />R4: Other_Regions","Quantity: 6.056420e+05<br />Gross_Cost:    377713.75<br />Predicted Cost = $33,384,877<br />R4: Other_Regions","Quantity: 5.000000e+03<br />Gross_Cost:    115769.57<br />Predicted Cost = $394,955<br />R4: Other_Regions","Quantity: 1.558000e+04<br />Gross_Cost:    135833.94<br />Predicted Cost = $1,130,121<br />R4: Other_Regions","Quantity: 1.720000e+01<br />Gross_Cost:     32171.75<br />Predicted Cost = $2,079<br />R4: Other_Regions","Quantity: 3.083200e+03<br />Gross_Cost:    212382.96<br />Predicted Cost = $252,538<br />R4: Other_Regions","Quantity: 7.932393e+05<br />Gross_Cost:  25634898.61<br />Predicted Cost = $42,849,811<br />R4: Other_Regions","Quantity: 7.928390e+04<br />Gross_Cost:    727291.13<br />Predicted Cost = $5,090,332<br />R4: Other_Regions","Quantity: 1.439547e+06<br />Gross_Cost:  43834377.28<br />Predicted Cost = $74,363,366<br />R4: Other_Regions","Quantity: 7.638000e+03<br />Gross_Cost:     66564.25<br />Predicted Cost = $584,462<br />R4: Other_Regions","Quantity: 1.105035e+06<br />Gross_Cost:  15577402.35<br />Predicted Cost = $58,226,781<br />R4: Other_Regions","Quantity: 2.700000e+02<br />Gross_Cost:      6533.23<br />Predicted Cost = $26,547<br />R4: Other_Regions","Quantity: 1.603400e+03<br />Gross_Cost:     41522.99<br />Predicted Cost = $137,932<br />R4: Other_Regions","Quantity: 3.274200e+03<br />Gross_Cost:    317857.49<br />Predicted Cost = $266,976<br />R4: Other_Regions","Quantity: 3.736000e+03<br />Gross_Cost:    222115.80<br />Predicted Cost = $301,631<br />R4: Other_Regions","Quantity: 8.824000e+03<br />Gross_Cost:    199618.97<br />Predicted Cost = $667,945<br />R4: Other_Regions","Quantity: 1.711566e+06<br />Gross_Cost:  58443475.05<br />Predicted Cost = $87,274,858<br />R4: Other_Regions","Quantity: 3.271700e+03<br />Gross_Cost:     65860.66<br />Predicted Cost = $266,787<br />R4: Other_Regions","Quantity: 7.000000e+01<br />Gross_Cost:    123305.18<br />Predicted Cost = $7,616<br />R4: Other_Regions","Quantity: 8.800000e+02<br />Gross_Cost:     11925.60<br />Predicted Cost = $79,186<br />R4: Other_Regions","Quantity: 1.297303e+06<br />Gross_Cost: 101878369.88<br />Predicted Cost = $67,540,345<br />R4: Other_Regions","Quantity: 6.116253e+06<br />Gross_Cost:  20874426.37<br />Predicted Cost = $283,464,732<br />R4: Other_Regions","Quantity: 2.371645e+04<br />Gross_Cost:   7734619.61<br />Predicted Cost = $1,666,944<br />R4: Other_Regions","Quantity: 2.692136e+06<br />Gross_Cost:     13182.25<br />Predicted Cost = $132,690,466<br />R4: Other_Regions","Quantity: 1.158000e+03<br />Gross_Cost:     80766.87<br />Predicted Cost = $102,078<br />R4: Other_Regions","Quantity: 1.860052e+05<br />Gross_Cost:   4031395.44<br />Predicted Cost = $11,202,390<br />R4: Other_Regions","Quantity: 2.000000e+01<br />Gross_Cost:      2067.52<br />Predicted Cost = $2,390<br />R4: Other_Regions","Quantity: 1.972746e+01<br />Gross_Cost:     22953.21<br />Predicted Cost = $2,360<br />R4: Other_Regions","Quantity: 1.200000e+02<br />Gross_Cost:      5471.69<br />Predicted Cost = $12,538<br />R4: Other_Regions","Quantity: 2.311582e+04<br />Gross_Cost:  11176744.73<br />Predicted Cost = $1,627,856<br />R4: Other_Regions","Quantity: 1.598148e+02<br />Gross_Cost:      8100.00<br />Predicted Cost = $16,344<br />R4: Other_Regions","Quantity: 1.230600e+02<br />Gross_Cost:     18082.07<br />Predicted Cost = $12,834<br />R4: Other_Regions","Quantity: 1.266960e+04<br />Gross_Cost:    630188.73<br />Predicted Cost = $933,374<br />R4: Other_Regions","Quantity: 5.396000e+03<br />Gross_Cost:    608501.64<br />Predicted Cost = $423,805<br />R4: Other_Regions","Quantity: 1.800000e+02<br />Gross_Cost:    111541.00<br />Predicted Cost = $18,244<br />R4: Other_Regions","Quantity: 3.500000e+01<br />Gross_Cost:      7457.27<br />Predicted Cost = $4,011<br />R4: Other_Regions","Quantity: 2.280000e+02<br />Gross_Cost:      4091.20<br />Predicted Cost = $22,703<br />R4: Other_Regions","Quantity: 1.441217e+04<br />Gross_Cost:   4304566.52<br />Predicted Cost = $1,051,537<br />R4: Other_Regions","Quantity: 1.737360e+03<br />Gross_Cost:    121637.79<br />Predicted Cost = $148,559<br />R4: Other_Regions","Quantity: 7.000000e+01<br />Gross_Cost:     27993.88<br />Predicted Cost = $7,616<br />R4: Other_Regions","Quantity: 2.349000e+03<br />Gross_Cost:    152948.79<br />Predicted Cost = $196,366<br />R4: Other_Regions","Quantity: 7.867000e+03<br />Gross_Cost:   4255565.09<br />Predicted Cost = $600,653<br />R4: Other_Regions","Quantity: 1.123299e+05<br />Gross_Cost:   6081997.93<br />Predicted Cost = $7,026,000<br />R4: Other_Regions","Quantity: 4.015700e+03<br />Gross_Cost:   3902432.07<br />Predicted Cost = $322,462<br />R4: Other_Regions","Quantity: 1.237900e+03<br />Gross_Cost:    233634.78<br />Predicted Cost = $108,576<br />R4: Other_Regions","Quantity: 3.500000e+02<br />Gross_Cost:     29700.00<br />Predicted Cost = $33,749<br />R4: Other_Regions","Quantity: 3.230785e+04<br />Gross_Cost:   1434529.99<br />Predicted Cost = $2,218,758<br />R4: Other_Regions","Quantity: 5.347060e+03<br />Gross_Cost:    266824.67<br />Predicted Cost = $420,249<br />R4: Other_Regions","Quantity: 9.680060e+02<br />Gross_Cost:   1212950.30<br />Predicted Cost = $86,484<br />R4: Other_Regions","Quantity: 2.246476e+05<br />Gross_Cost:    729254.89<br />Predicted Cost = $13,339,487<br />R4: Other_Regions","Quantity: 1.572450e+03<br />Gross_Cost:     79475.40<br />Predicted Cost = $135,467<br />R4: Other_Regions","Quantity: 7.337740e+03<br />Gross_Cost:    613637.94<br />Predicted Cost = $563,177<br />R4: Other_Regions","Quantity: 4.862000e+03<br />Gross_Cost:   8427338.70<br />Predicted Cost = $384,861<br />R4: Other_Regions","Quantity: 2.060524e+04<br />Gross_Cost:   1692698.04<br />Predicted Cost = $1,463,623<br />R4: Other_Regions","Quantity: 5.279580e+03<br />Gross_Cost:    631331.25<br />Predicted Cost = $415,341<br />R4: Other_Regions","Quantity: 2.499960e+05<br />Gross_Cost:    584541.28<br />Predicted Cost = $14,726,111<br />R4: Other_Regions","Quantity: 4.628601e+01<br />Gross_Cost:    185636.61<br />Predicted Cost = $5,194<br />R4: Other_Regions","Quantity: 9.840000e+02<br />Gross_Cost:  66050000.00<br />Predicted Cost = $87,805<br />R4: Other_Regions","Quantity: 2.871600e+04<br />Gross_Cost:  23803762.44<br />Predicted Cost = $1,989,595<br />R4: Other_Regions","Quantity: 1.773000e+03<br />Gross_Cost:   5495064.48<br />Predicted Cost = $151,375<br />R4: Other_Regions","Quantity: 3.635760e+04<br />Gross_Cost:    229977.60<br />Predicted Cost = $2,474,860<br />R4: Other_Regions","Quantity: 3.730000e+02<br />Gross_Cost:    938191.17<br />Predicted Cost = $35,796<br />R4: Other_Regions","Quantity: 3.000000e+02<br />Gross_Cost:      6492.41<br />Predicted Cost = $29,264<br />R4: Other_Regions","Quantity: 9.520000e+02<br />Gross_Cost:      3732.70<br />Predicted Cost = $85,161<br />R4: Other_Regions","Quantity: 6.130000e+02<br />Gross_Cost:      5291.75<br />Predicted Cost = $56,676<br />R4: Other_Regions","Quantity: 6.000000e+01<br />Gross_Cost:      6327.62<br />Predicted Cost = $6,604<br />R4: Other_Regions","Quantity: 3.900000e+02<br />Gross_Cost:     26286.80<br />Predicted Cost = $37,302<br />R4: Other_Regions","Quantity: 3.800370e+04<br />Gross_Cost:    356578.94<br />Predicted Cost = $2,578,333<br />R4: Other_Regions","Quantity: 2.733750e+03<br />Gross_Cost:    168255.77<br />Predicted Cost = $225,945<br />R4: Other_Regions","Quantity: 1.209226e+04<br />Gross_Cost:     43188.37<br />Predicted Cost = $893,963<br />R4: Other_Regions","Quantity: 1.344000e+03<br />Gross_Cost:    385771.50<br />Predicted Cost = $117,157<br />R4: Other_Regions","Quantity: 9.625000e+01<br />Gross_Cost:     45694.97<br />Predicted Cost = $10,225<br />R4: Other_Regions","Quantity: 9.508000e+04<br />Gross_Cost:    355625.20<br />Predicted Cost = $6,021,885<br />R4: Other_Regions","Quantity: 6.930000e+02<br />Gross_Cost:     11145.22<br />Predicted Cost = $63,486<br />R4: Other_Regions","Quantity: 1.117750e+04<br />Gross_Cost:     39638.01<br />Predicted Cost = $831,225<br />R4: Other_Regions","Quantity: 3.164186e+04<br />Gross_Cost:   7121837.37<br />Predicted Cost = $2,176,418<br />R4: Other_Regions","Quantity: 1.800000e+02<br />Gross_Cost:      7130.35<br />Predicted Cost = $18,244<br />R4: Other_Regions","Quantity: 1.970060e+04<br />Gross_Cost:     36390.55<br />Predicted Cost = $1,404,085<br />R4: Other_Regions","Quantity: 1.464000e+03<br />Gross_Cost:      4833.00<br />Predicted Cost = $126,802<br />R4: Other_Regions","Quantity: 2.000000e+01<br />Gross_Cost:      2351.66<br />Predicted Cost = $2,390<br />R4: Other_Regions","Quantity: 9.000000e+01<br />Gross_Cost:     28710.43<br />Predicted Cost = $9,609<br />R4: Other_Regions","Quantity: 3.497000e+02<br />Gross_Cost:     81922.15<br />Predicted Cost = $33,722<br />R4: Other_Regions","Quantity: 3.600000e+02<br />Gross_Cost:      9180.00<br />Predicted Cost = $34,640<br />R4: Other_Regions","Quantity: 1.767480e+03<br />Gross_Cost:      3639.50<br />Predicted Cost = $150,940<br />R4: Other_Regions","Quantity: 1.745324e+04<br />Gross_Cost:     25921.82<br />Predicted Cost = $1,255,265<br />R4: Other_Regions","Quantity: 2.300000e+02<br />Gross_Cost:      4997.23<br />Predicted Cost = $22,887<br />R4: Other_Regions","Quantity: 7.816600e+04<br />Gross_Cost:    557833.66<br />Predicted Cost = $5,023,906<br />R4: Other_Regions","Quantity: 2.800000e+01<br />Gross_Cost:     13738.95<br />Predicted Cost = $3,263<br />R4: Other_Regions","Quantity: 1.800000e+01<br />Gross_Cost:      9373.24<br />Predicted Cost = $2,168<br />R4: Other_Regions","Quantity: 2.261666e+04<br />Gross_Cost:     49174.81<br />Predicted Cost = $1,595,315<br />R4: Other_Regions","Quantity: 1.010320e+05<br />Gross_Cost:    717787.48<br />Predicted Cost = $6,369,781<br />R4: Other_Regions","Quantity: 5.230000e+03<br />Gross_Cost:    324888.95<br />Predicted Cost = $411,731<br />R4: Other_Regions","Quantity: 7.200000e+02<br />Gross_Cost:     13163.70<br />Predicted Cost = $65,771<br />R4: Other_Regions","Quantity: 6.000000e+01<br />Gross_Cost:      3722.36<br />Predicted Cost = $6,604<br />R4: Other_Regions","Quantity: 3.992000e+01<br />Gross_Cost:    329810.35<br />Predicted Cost = $4,530<br />R4: Other_Regions","Quantity: 2.740000e+03<br />Gross_Cost:    226845.72<br />Predicted Cost = $226,422<br />R4: Other_Regions","Quantity: 7.230000e+02<br />Gross_Cost:     24093.20<br />Predicted Cost = $66,024<br />R4: Other_Regions","Quantity: 2.400000e+03<br />Gross_Cost:     40154.05<br />Predicted Cost = $200,307<br />R4: Other_Regions","Quantity: 2.314703e+03<br />Gross_Cost:   8031915.72<br />Predicted Cost = $193,713<br />R4: Other_Regions","Quantity: 1.093400e+04<br />Gross_Cost:     14603.23<br />Predicted Cost = $814,462<br />R4: Other_Regions","Quantity: 4.920000e+02<br />Gross_Cost:     29114.63<br />Predicted Cost = $46,245<br />R4: Other_Regions","Quantity: 9.600000e+01<br />Gross_Cost:     38708.08<br />Predicted Cost = $10,200<br />R4: Other_Regions","Quantity: 3.000000e+01<br />Gross_Cost:      7503.62<br />Predicted Cost = $3,478<br />R4: Other_Regions","Quantity: 7.680000e+02<br />Gross_Cost:    130864.71<br />Predicted Cost = $69,817<br />R4: Other_Regions","Quantity: 1.463560e+03<br />Gross_Cost:    542636.85<br />Predicted Cost = $126,767<br />R4: Other_Regions","Quantity: 3.240000e+02<br />Gross_Cost:     33233.96<br />Predicted Cost = $31,423<br />R4: Other_Regions","Quantity: 1.475522e+04<br />Gross_Cost:   1713647.49<br />Predicted Cost = $1,074,669<br />R4: Other_Regions","Quantity: 1.999800e+04<br />Gross_Cost:      3980.87<br />Predicted Cost = $1,423,681<br />R4: Other_Regions","Quantity: 1.022700e+03<br />Gross_Cost:    103127.79<br />Predicted Cost = $90,995<br />R4: Other_Regions","Quantity: 2.822000e+03<br />Gross_Cost:   2469997.24<br />Predicted Cost = $232,683<br />R4: Other_Regions","Quantity: 4.647900e+04<br />Gross_Cost:    833046.71<br />Predicted Cost = $3,106,079<br />R4: Other_Regions","Quantity: 1.625000e+03<br />Gross_Cost:    644382.71<br />Predicted Cost = $139,649<br />R4: Other_Regions","Quantity: 2.612800e+04<br />Gross_Cost:     10253.61<br />Predicted Cost = $1,823,153<br />R4: Other_Regions","Quantity: 2.027750e+03<br />Gross_Cost:    218755.60<br />Predicted Cost = $171,391<br />R4: Other_Regions","Quantity: 4.980000e+01<br />Gross_Cost:      7070.39<br />Predicted Cost = $5,558<br />R4: Other_Regions","Quantity: 6.880000e+01<br />Gross_Cost:   1077994.60<br />Predicted Cost = $7,495<br />R4: Other_Regions","Quantity: 1.199908e+04<br />Gross_Cost:     14754.90<br />Predicted Cost = $887,589<br />R4: Other_Regions","Quantity: 1.518520e+01<br />Gross_Cost:     31539.60<br />Predicted Cost = $1,853<br />R4: Other_Regions","Quantity: 6.131000e+03<br />Gross_Cost:    162492.50<br />Predicted Cost = $476,943<br />R4: Other_Regions","Quantity: 3.670000e+02<br />Gross_Cost:     48246.55<br />Predicted Cost = $35,263<br />R4: Other_Regions","Quantity: 8.000000e+01<br />Gross_Cost:     40476.92<br />Predicted Cost = $8,617<br />R4: Other_Regions","Quantity: 3.000406e+06<br />Gross_Cost:   1755093.68<br />Predicted Cost = $146,686,943<br />R4: Other_Regions","Quantity: 2.581000e+03<br />Gross_Cost:     42454.27<br />Predicted Cost = $214,242<br />R4: Other_Regions","Quantity: 3.940000e+01<br />Gross_Cost:     30511.00<br />Predicted Cost = $4,475<br />R4: Other_Regions","Quantity: 5.000000e+02<br />Gross_Cost:     13000.00<br />Predicted Cost = $46,940<br />R4: Other_Regions","Quantity: 2.040000e+02<br />Gross_Cost:    191066.47<br />Predicted Cost = $20,484<br />R4: Other_Regions","Quantity: 2.160000e+03<br />Gross_Cost:     32999.64<br />Predicted Cost = $181,706<br />R4: Other_Regions","Quantity: 8.162275e+01<br />Gross_Cost:     33194.81<br />Predicted Cost = $8,779<br />R4: Other_Regions","Quantity: 2.440000e+02<br />Gross_Cost:     13354.95<br />Predicted Cost = $24,173<br />R4: Other_Regions","Quantity: 2.000000e+02<br />Gross_Cost:     84391.71<br />Predicted Cost = $20,112<br />R4: Other_Regions","Quantity: 7.666666e+02<br />Gross_Cost:    192700.00<br />Predicted Cost = $69,705<br />R4: Other_Regions","Quantity: 8.800000e+02<br />Gross_Cost:   1358410.68<br />Predicted Cost = $79,186<br />R4: Other_Regions","Quantity: 6.920000e+02<br />Gross_Cost:     12000.00<br />Predicted Cost = $63,401<br />R4: Other_Regions","Quantity: 1.764000e+03<br />Gross_Cost:    430427.52<br />Predicted Cost = $150,665<br />R4: Other_Regions","Quantity: 3.029140e+04<br />Gross_Cost:    158541.48<br />Predicted Cost = $2,090,357<br />R4: Other_Regions","Quantity: 2.724560e+03<br />Gross_Cost:      8846.91<br />Predicted Cost = $225,242<br />R4: Other_Regions","Quantity: 5.356880e+03<br />Gross_Cost:    337370.49<br />Predicted Cost = $420,963<br />R4: Other_Regions","Quantity: 8.864000e+03<br />Gross_Cost:    509411.54<br />Predicted Cost = $670,745<br />R4: Other_Regions","Quantity: 1.006000e+03<br />Gross_Cost:     57731.12<br />Predicted Cost = $89,620<br />R4: Other_Regions","Quantity: 3.290060e+02<br />Gross_Cost:     12837.99<br />Predicted Cost = $31,872<br />R4: Other_Regions","Quantity: 1.500000e+02<br />Gross_Cost:     28770.48<br />Predicted Cost = $15,413<br />R4: Other_Regions","Quantity: 5.708050e+03<br />Gross_Cost:  40365247.75<br />Predicted Cost = $446,428<br />R4: Other_Regions","Quantity: 1.146000e+03<br />Gross_Cost:     18106.20<br />Predicted Cost = $101,099<br />R4: Other_Regions","Quantity: 2.920000e+03<br />Gross_Cost:     11673.94<br />Predicted Cost = $240,148<br />R4: Other_Regions","Quantity: 2.621230e+03<br />Gross_Cost:    190585.96<br />Predicted Cost = $217,329<br />R4: Other_Regions","Quantity: 2.690000e+02<br />Gross_Cost:     79566.87<br />Predicted Cost = $26,456<br />R4: Other_Regions","Quantity: 7.116400e+02<br />Gross_Cost:     69878.99<br />Predicted Cost = $65,064<br />R4: Other_Regions","Quantity: 1.578900e+04<br />Gross_Cost:     14678.24<br />Predicted Cost = $1,144,137<br />R4: Other_Regions","Quantity: 1.232000e+02<br />Gross_Cost:     20195.99<br />Predicted Cost = $12,847<br />R4: Other_Regions","Quantity: 6.668112e+04<br />Gross_Cost:   2472148.94<br />Predicted Cost = $4,337,132<br />R4: Other_Regions","Quantity: 6.840000e+02<br />Gross_Cost:      8616.91<br />Predicted Cost = $62,723<br />R4: Other_Regions","Quantity: 3.467000e+03<br />Gross_Cost:    264465.01<br />Predicted Cost = $281,486<br />R4: Other_Regions","Quantity: 8.850000e+02<br />Gross_Cost:     22868.60<br />Predicted Cost = $79,602<br />R4: Other_Regions","Quantity: 5.700000e+02<br />Gross_Cost:     32106.13<br />Predicted Cost = $52,989<br />R4: Other_Regions","Quantity: 4.500000e+03<br />Gross_Cost:     87892.25<br />Predicted Cost = $358,279<br />R4: Other_Regions","Quantity: 5.694400e+02<br />Gross_Cost:    377252.08<br />Predicted Cost = $52,941<br />R4: Other_Regions","Quantity: 1.744720e+03<br />Gross_Cost:     51862.47<br />Predicted Cost = $149,141<br />R4: Other_Regions","Quantity: 4.440000e+02<br />Gross_Cost:     66422.63<br />Predicted Cost = $42,056<br />R4: Other_Regions","Quantity: 2.100000e+02<br />Gross_Cost:      4040.61<br />Predicted Cost = $21,040<br />R4: Other_Regions","Quantity: 2.600000e+02<br />Gross_Cost:     73119.30<br />Predicted Cost = $25,636<br />R4: Other_Regions","Quantity: 5.400000e+02<br />Gross_Cost:      9704.70<br />Predicted Cost = $50,404<br />R4: Other_Regions","Quantity: 1.093400e+04<br />Gross_Cost:    132091.75<br />Predicted Cost = $814,462<br />R4: Other_Regions","Quantity: 5.002000e+03<br />Gross_Cost:    298231.52<br />Predicted Cost = $395,101<br />R4: Other_Regions","Quantity: 2.790000e+01<br />Gross_Cost:      4967.73<br />Predicted Cost = $3,252<br />R4: Other_Regions","Quantity: 1.900000e+01<br />Gross_Cost:     12244.71<br />Predicted Cost = $2,280<br />R4: Other_Regions","Quantity: 3.330000e+02<br />Gross_Cost:     36134.36<br />Predicted Cost = $32,230<br />R4: Other_Regions","Quantity: 6.616000e+03<br />Gross_Cost:   1316701.55<br />Predicted Cost = $511,742<br />R4: Other_Regions","Quantity: 7.812000e+02<br />Gross_Cost:    341685.14<br />Predicted Cost = $70,926<br />R4: Other_Regions","Quantity: 2.000000e+01<br />Gross_Cost:     11714.79<br />Predicted Cost = $2,390<br />R4: Other_Regions","Quantity: 1.046000e+03<br />Gross_Cost:    329606.89<br />Predicted Cost = $92,911<br />R4: Other_Regions","Quantity: 1.200000e+02<br />Gross_Cost:    138658.92<br />Predicted Cost = $12,538<br />R4: Other_Regions","Quantity: 2.470000e+03<br />Gross_Cost:     57377.72<br />Predicted Cost = $205,705<br />R4: Other_Regions","Quantity: 4.000000e+01<br />Gross_Cost:      1490.27<br />Predicted Cost = $4,538<br />R4: Other_Regions","Quantity: 1.069444e+05<br />Gross_Cost:     12690.05<br />Predicted Cost = $6,713,843<br />R4: Other_Regions","Quantity: 1.069444e+05<br />Gross_Cost:     41550.00<br />Predicted Cost = $6,713,843<br />R4: Other_Regions","Quantity: 9.875060e+03<br />Gross_Cost:    971333.87<br />Predicted Cost = $741,224<br />R4: Other_Regions","Quantity: 3.083050e+05<br />Gross_Cost:     41345.00<br />Predicted Cost = $17,877,510<br />R4: Other_Regions","Quantity: 3.049568e+05<br />Gross_Cost:  39433990.73<br />Predicted Cost = $17,697,847<br />R4: Other_Regions","Quantity: 6.166100e+05<br />Gross_Cost:     29890.31<br />Predicted Cost = $33,943,746<br />R4: Other_Regions","Quantity: 5.170000e+03<br />Gross_Cost:    136458.10<br />Predicted Cost = $407,360<br />R4: Other_Regions","Quantity: 8.834000e+01<br />Gross_Cost:    222431.44<br />Predicted Cost = $9,445<br />R4: Other_Regions","Quantity: 1.224500e+03<br />Gross_Cost:   1856668.66<br />Predicted Cost = $107,488<br />R4: Other_Regions","Quantity: 9.537600e+02<br />Gross_Cost:   4090606.80<br />Predicted Cost = $85,306<br />R4: Other_Regions","Quantity: 1.418255e+04<br />Gross_Cost:   6386553.92<br />Predicted Cost = $1,036,031<br />R4: Other_Regions","Quantity: 1.394995e+04<br />Gross_Cost:  16165273.67<br />Predicted Cost = $1,020,304<br />R4: Other_Regions","Quantity: 7.287010e+04<br />Gross_Cost:  10267664.01<br />Predicted Cost = $4,708,235<br />R4: Other_Regions","Quantity: 5.800000e+01<br />Gross_Cost:    271277.18<br />Predicted Cost = $6,400<br />R4: Other_Regions","Quantity: 4.963000e+03<br />Gross_Cost:  15888904.87<br />Predicted Cost = $392,250<br />R4: Other_Regions","Quantity: 3.913800e+04<br />Gross_Cost:   7302015.49<br />Predicted Cost = $2,649,438<br />R4: Other_Regions","Quantity: 2.462600e+04<br />Gross_Cost:     60929.92<br />Predicted Cost = $1,725,994<br />R4: Other_Regions","Quantity: 9.698600e+03<br />Gross_Cost:   7435385.23<br />Predicted Cost = $728,964<br />R4: Other_Regions","Quantity: 1.416077e+04<br />Gross_Cost:   3422427.84<br />Predicted Cost = $1,034,559<br />R4: Other_Regions","Quantity: 5.706400e+03<br />Gross_Cost:   4254575.96<br />Predicted Cost = $446,308<br />R4: Other_Regions","Quantity: 2.468800e+02<br />Gross_Cost:     70583.59<br />Predicted Cost = $24,437<br />R4: Other_Regions","Quantity: 7.142000e+02<br />Gross_Cost:    898003.24<br />Predicted Cost = $65,280<br />R4: Other_Regions","Quantity: 2.477365e+04<br />Gross_Cost:   6045694.95<br />Predicted Cost = $1,735,564<br />R4: Other_Regions","Quantity: 3.839000e+03<br />Gross_Cost:  13234658.89<br />Predicted Cost = $309,316<br />R4: Other_Regions","Quantity: 1.168000e+03<br />Gross_Cost:   2709224.08<br />Predicted Cost = $102,893<br />R4: Other_Regions","Quantity: 8.000000e+03<br />Gross_Cost:     35523.91<br />Predicted Cost = $610,040<br />R4: Other_Regions","Quantity: 2.240000e+02<br />Gross_Cost:      3983.99<br />Predicted Cost = $22,335<br />R4: Other_Regions","Quantity: 1.027300e+04<br />Gross_Cost:    113042.31<br />Predicted Cost = $768,812<br />R4: Other_Regions","Quantity: 9.000000e+01<br />Gross_Cost:     65123.25<br />Predicted Cost = $9,609<br />R4: Other_Regions","Quantity: 1.200000e+02<br />Gross_Cost:    167228.12<br />Predicted Cost = $12,538<br />R4: Other_Regions","Quantity: 1.357000e+03<br />Gross_Cost:    493124.20<br />Predicted Cost = $118,205<br />R4: Other_Regions","Quantity: 7.200000e+03<br />Gross_Cost:    105057.50<br />Predicted Cost = $553,392<br />R4: Other_Regions","Quantity: 3.700000e+01<br />Gross_Cost:     12139.09<br />Predicted Cost = $4,223<br />R4: Other_Regions","Quantity: 6.880040e+04<br />Gross_Cost:   8992413.15<br />Predicted Cost = $4,464,488<br />R4: Other_Regions","Quantity: 1.450000e+03<br />Gross_Cost:    205836.22<br />Predicted Cost = $125,680<br />R4: Other_Regions","Quantity: 3.560000e+02<br />Gross_Cost:    312711.84<br />Predicted Cost = $34,284<br />R4: Other_Regions","Quantity: 7.337196e+04<br />Gross_Cost:  13858386.42<br />Predicted Cost = $4,738,222<br />R4: Other_Regions","Quantity: 2.809086e+05<br />Gross_Cost:  28967410.41<br />Predicted Cost = $16,402,974<br />R4: Other_Regions","Quantity: 1.866200e+02<br />Gross_Cost:    131317.08<br />Predicted Cost = $18,864<br />R4: Other_Regions","Quantity: 1.400000e+04<br />Gross_Cost:     25716.60<br />Predicted Cost = $1,023,690<br />R4: Other_Regions","Quantity: 6.188000e+03<br />Gross_Cost:    493612.44<br />Predicted Cost = $481,043<br />R4: Other_Regions","Quantity: 4.296124e+05<br />Gross_Cost:     87671.99<br />Predicted Cost = $24,299,429<br />R4: Other_Regions","Quantity: 1.650802e+01<br />Gross_Cost:    288498.54<br />Predicted Cost = $2,002<br />R4: Other_Regions","Quantity: 1.657000e+01<br />Gross_Cost:      4300.00<br />Predicted Cost = $2,009<br />R4: Other_Regions","Quantity: 1.142980e+05<br />Gross_Cost:   1156197.82<br />Predicted Cost = $7,139,793<br />R4: Other_Regions","Quantity: 6.838600e+02<br />Gross_Cost:    259889.60<br />Predicted Cost = $62,711<br />R4: Other_Regions","Quantity: 1.680000e+01<br />Gross_Cost:    343844.52<br />Predicted Cost = $2,034<br />R4: Other_Regions","Quantity: 2.930000e+02<br />Gross_Cost:      5904.64<br />Predicted Cost = $28,632<br />R4: Other_Regions","Quantity: 3.485034e+04<br />Gross_Cost:  16476766.41<br />Predicted Cost = $2,379,806<br />R4: Other_Regions","Quantity: 8.911550e+03<br />Gross_Cost:    406590.23<br />Predicted Cost = $674,073<br />R4: Other_Regions","Quantity: 4.183475e+04<br />Gross_Cost:   1104107.98<br />Predicted Cost = $2,817,876<br />R4: Other_Regions","Quantity: 1.708700e+04<br />Gross_Cost:   3017366.97<br />Predicted Cost = $1,230,880<br />R4: Other_Regions","Quantity: 2.963400e+04<br />Gross_Cost:    645767.50<br />Predicted Cost = $2,048,359<br />R4: Other_Regions","Quantity: 7.387389e+05<br />Gross_Cost: 115604437.67<br />Predicted Cost = $40,119,376<br />R4: Other_Regions","Quantity: 1.478800e+04<br />Gross_Cost:   5835582.78<br />Predicted Cost = $1,076,877<br />R4: Other_Regions","Quantity: 2.927500e+04<br />Gross_Cost:   1131015.01<br />Predicted Cost = $2,025,395<br />R4: Other_Regions","Quantity: 1.685344e+06<br />Gross_Cost: 100424476.36<br />Predicted Cost = $86,037,318<br />R4: Other_Regions","Quantity: 1.950200e+04<br />Gross_Cost:  21075105.63<br />Predicted Cost = $1,390,987<br />R4: Other_Regions","Quantity: 1.125400e+04<br />Gross_Cost:   3092847.22<br />Predicted Cost = $836,486<br />R4: Other_Regions","Quantity: 1.539474e+05<br />Gross_Cost:  13169142.82<br />Predicted Cost = $9,404,147<br />R4: Other_Regions","Quantity: 1.914000e+03<br />Gross_Cost:    677148.67<br />Predicted Cost = $162,479<br />R4: Other_Regions","Quantity: 5.243400e+04<br />Gross_Cost:  16311021.01<br />Predicted Cost = $3,472,498<br />R4: Other_Regions","Quantity: 9.757830e+03<br />Gross_Cost:    801400.14<br />Predicted Cost = $733,081<br />R4: Other_Regions","Quantity: 2.646608e+04<br />Gross_Cost:   2141926.92<br />Predicted Cost = $1,844,964<br />R4: Other_Regions","Quantity: 3.958000e+02<br />Gross_Cost:    323769.37<br />Predicted Cost = $37,815<br />R4: Other_Regions","Quantity: 5.848307e+05<br />Gross_Cost:  24902438.10<br />Predicted Cost = $32,322,348<br />R4: Other_Regions","Quantity: 3.997500e+02<br />Gross_Cost:   3508953.44<br />Predicted Cost = $38,164<br />R4: Other_Regions","Quantity: 3.920930e+04<br />Gross_Cost:   7269766.24<br />Predicted Cost = $2,653,902<br />R4: Other_Regions","Quantity: 1.991214e+05<br />Gross_Cost:  56419218.21<br />Predicted Cost = $11,931,196<br />R4: Other_Regions","Quantity: 9.608625e+04<br />Gross_Cost:  52195275.17<br />Predicted Cost = $6,080,813<br />R4: Other_Regions","Quantity: 1.600000e+02<br />Gross_Cost:    143823.58<br />Predicted Cost = $16,361<br />R4: Other_Regions","Quantity: 2.607010e+05<br />Gross_Cost:  15061190.69<br />Predicted Cost = $15,308,477<br />R4: Other_Regions","Quantity: 8.925000e+02<br />Gross_Cost:    258051.58<br />Predicted Cost = $80,226<br />R4: Other_Regions","Quantity: 3.671430e+04<br />Gross_Cost:  12031592.63<br />Predicted Cost = $2,497,311<br />R4: Other_Regions","Quantity: 2.447749e+05<br />Gross_Cost:   4099808.95<br />Predicted Cost = $14,441,405<br />R4: Other_Regions","Quantity: 4.349900e+04<br />Gross_Cost:   3761961.75<br />Predicted Cost = $2,921,415<br />R4: Other_Regions","Quantity: 6.922440e+03<br />Gross_Cost:    425983.30<br />Predicted Cost = $533,629<br />R4: Other_Regions","Quantity: 2.920270e+04<br />Gross_Cost:   1538086.36<br />Predicted Cost = $2,020,767<br />R4: Other_Regions","Quantity: 1.364898e+04<br />Gross_Cost:    615069.08<br />Predicted Cost = $999,926<br />R4: Other_Regions","Quantity: 1.634000e+03<br />Gross_Cost:    241425.98<br />Predicted Cost = $140,365<br />R4: Other_Regions","Quantity: 2.533134e+04<br />Gross_Cost:   1488940.52<br />Predicted Cost = $1,771,674<br />R4: Other_Regions","Quantity: 4.800000e+02<br />Gross_Cost:    185244.52<br />Predicted Cost = $45,201<br />R4: Other_Regions","Quantity: 1.150000e+02<br />Gross_Cost:     24928.61<br />Predicted Cost = $12,054<br />R4: Other_Regions","Quantity: 1.883069e+05<br />Gross_Cost:   1659818.14<br />Predicted Cost = $11,330,558<br />R4: Other_Regions","Quantity: 1.544265e+05<br />Gross_Cost:   7212616.30<br />Predicted Cost = $9,431,217<br />R4: Other_Regions","Quantity: 1.535021e+06<br />Gross_Cost:   3877424.30<br />Predicted Cost = $78,914,290<br />R4: Other_Regions","Quantity: 4.000000e+01<br />Gross_Cost:      3832.72<br />Predicted Cost = $4,538<br />R4: Other_Regions","Quantity: 2.710300e+05<br />Gross_Cost:      5614.82<br />Predicted Cost = $15,868,690<br />R4: Other_Regions","Quantity: 2.056400e+04<br />Gross_Cost:     11317.23<br />Predicted Cost = $1,460,914<br />R4: Other_Regions","Quantity: 7.278000e+02<br />Gross_Cost:    113734.29<br />Predicted Cost = $66,430<br />R4: Other_Regions","Quantity: 1.000000e+02<br />Gross_Cost:     30630.00<br />Predicted Cost = $10,593<br />R4: Other_Regions","Quantity: 1.295800e+04<br />Gross_Cost:     91116.08<br />Predicted Cost = $953,010<br />R4: Other_Regions","Quantity: 6.180000e+03<br />Gross_Cost:      8258.62<br />Predicted Cost = $480,468<br />R4: Other_Regions","Quantity: 1.468000e+01<br />Gross_Cost:     11987.72<br />Predicted Cost = $1,796<br />R4: Other_Regions","Quantity: 5.687660e+05<br />Gross_Cost:    143702.90<br />Predicted Cost = $31,500,221<br />R4: Other_Regions","Quantity: 1.940000e+03<br />Gross_Cost:     13953.00<br />Predicted Cost = $164,519<br />R4: Other_Regions","Quantity: 8.306886e+04<br />Gross_Cost:   4703005.94<br />Predicted Cost = $5,314,720<br />R4: Other_Regions","Quantity: 1.500000e+02<br />Gross_Cost:     31781.84<br />Predicted Cost = $15,413<br />R4: Other_Regions","Quantity: 5.555840e+04<br />Gross_Cost:    274794.56<br />Predicted Cost = $3,663,477<br />R4: Other_Regions","Quantity: 1.509200e+04<br />Gross_Cost:   6569060.33<br />Predicted Cost = $1,097,339<br />R4: Other_Regions","Quantity: 5.135800e+02<br />Gross_Cost:    109936.35<br />Predicted Cost = $48,119<br />R4: Other_Regions","Quantity: 3.464100e+03<br />Gross_Cost:     83897.54<br />Predicted Cost = $281,269<br />R4: Other_Regions","Quantity: 6.985800e+03<br />Gross_Cost:    227062.95<br />Predicted Cost = $538,146<br />R4: Other_Regions","Quantity: 1.100000e+01<br />Gross_Cost:    292611.44<br />Predicted Cost = $1,375<br />R4: Other_Regions","Quantity: 8.500000e+01<br />Gross_Cost:    352827.20<br />Predicted Cost = $9,114<br />R4: Other_Regions","Quantity: 5.110000e+03<br />Gross_Cost:     11423.33<br />Predicted Cost = $402,985<br />R4: Other_Regions","Quantity: 1.277000e+03<br />Gross_Cost:   4567224.89<br />Predicted Cost = $111,745<br />R4: Other_Regions","Quantity: 2.593900e+04<br />Gross_Cost:     81583.87<br />Predicted Cost = $1,810,951<br />R4: Other_Regions","Quantity: 5.000000e+01<br />Gross_Cost:      5482.22<br />Predicted Cost = $5,579<br />R4: Other_Regions","Quantity: 2.623704e+02<br />Gross_Cost:     15890.96<br />Predicted Cost = $25,852<br />R4: Other_Regions","Quantity: 1.120000e+02<br />Gross_Cost:     14003.19<br />Predicted Cost = $11,763<br />R4: Other_Regions","Quantity: 1.900000e+01<br />Gross_Cost:      3983.77<br />Predicted Cost = $2,280<br />R4: Other_Regions","Quantity: 5.000000e+02<br />Gross_Cost:     14975.80<br />Predicted Cost = $46,940<br />R4: Other_Regions","Quantity: 5.000000e+01<br />Gross_Cost:     10795.46<br />Predicted Cost = $5,579<br />R4: Other_Regions","Quantity: 1.503800e+02<br />Gross_Cost:      9137.02<br />Predicted Cost = $15,449<br />R4: Other_Regions","Quantity: 1.350000e+02<br />Gross_Cost:    873391.90<br />Predicted Cost = $13,982<br />R4: Other_Regions","Quantity: 5.400000e+02<br />Gross_Cost:     29507.88<br />Predicted Cost = $50,404<br />R4: Other_Regions","Quantity: 1.200000e+02<br />Gross_Cost:     64671.21<br />Predicted Cost = $12,538<br />R4: Other_Regions","Quantity: 1.000000e+03<br />Gross_Cost:      5376.36<br />Predicted Cost = $89,125<br />R4: Other_Regions","Quantity: 1.571400e+04<br />Gross_Cost:    110605.87<br />Predicted Cost = $1,139,109<br />R4: Other_Regions","Quantity: 4.000000e+01<br />Gross_Cost:      3694.46<br />Predicted Cost = $4,538<br />R4: Other_Regions","Quantity: 4.000000e+01<br />Gross_Cost:     91848.03<br />Predicted Cost = $4,538<br />R4: Other_Regions","Quantity: 1.306541e+06<br />Gross_Cost:  18494972.36<br />Predicted Cost = $67,985,102<br />R4: Other_Regions","Quantity: 1.962795e+04<br />Gross_Cost:    111203.85<br />Predicted Cost = $1,399,295<br />R4: Other_Regions","Quantity: 1.300000e+02<br />Gross_Cost:     97936.02<br />Predicted Cost = $13,502<br />R4: Other_Regions","Quantity: 4.000000e+03<br />Gross_Cost:  20426936.70<br />Predicted Cost = $321,296<br />R4: Other_Regions","Quantity: 6.225000e+02<br />Gross_Cost:     45436.67<br />Predicted Cost = $57,488<br />R4: Other_Regions","Quantity: 1.600000e+02<br />Gross_Cost:     22253.75<br />Predicted Cost = $16,361<br />R4: Other_Regions","Quantity: 3.328810e+03<br />Gross_Cost:    286009.86<br />Predicted Cost = $271,092<br />R4: Other_Regions","Quantity: 3.773000e+03<br />Gross_Cost:    110468.23<br />Predicted Cost = $304,393<br />R4: Other_Regions","Quantity: 6.525000e+02<br />Gross_Cost:    125249.72<br />Predicted Cost = $60,046<br />R4: Other_Regions","Quantity: 2.944140e+03<br />Gross_Cost:    232614.46<br />Predicted Cost = $241,984<br />R4: Other_Regions","Quantity: 7.542000e+02<br />Gross_Cost:    219800.43<br />Predicted Cost = $68,655<br />R4: Other_Regions","Quantity: 5.000000e+01<br />Gross_Cost:      7411.78<br />Predicted Cost = $5,579<br />R4: Other_Regions","Quantity: 6.744000e+03<br />Gross_Cost:   1720722.95<br />Predicted Cost = $520,893<br />R4: Other_Regions","Quantity: 3.600000e+02<br />Gross_Cost:     81767.34<br />Predicted Cost = $34,640<br />R4: Other_Regions","Quantity: 2.431746e+04<br />Gross_Cost:     64776.61<br />Predicted Cost = $1,705,982<br />R4: Other_Regions","Quantity: 1.801400e+05<br />Gross_Cost:    371578.71<br />Predicted Cost = $10,875,254<br />R4: Other_Regions","Quantity: 4.870600e+02<br />Gross_Cost:     53111.86<br />Predicted Cost = $45,816<br />R4: Other_Regions","Quantity: 1.600000e+04<br />Gross_Cost:    672176.66<br />Predicted Cost = $1,158,273<br />R4: Other_Regions","Quantity: 8.295000e+02<br />Gross_Cost:   1237893.47<br />Predicted Cost = $74,973<br />R4: Other_Regions","Quantity: 8.116600e+02<br />Gross_Cost:    746856.39<br />Predicted Cost = $73,480<br />R4: Other_Regions","Quantity: 4.000000e+02<br />Gross_Cost:     62343.65<br />Predicted Cost = $38,186<br />R4: Other_Regions","Quantity: 6.711000e+02<br />Gross_Cost:    863377.78<br />Predicted Cost = $61,628<br />R4: Other_Regions","Quantity: 1.009000e+02<br />Gross_Cost:    286037.25<br />Predicted Cost = $10,681<br />R4: Other_Regions","Quantity: 3.930802e+04<br />Gross_Cost:   2258948.25<br />Predicted Cost = $2,660,082<br />R4: Other_Regions","Quantity: 2.068581e+04<br />Gross_Cost:   4559557.59<br />Predicted Cost = $1,468,916<br />R4: Other_Regions","Quantity: 5.532000e+02<br />Gross_Cost:    123220.62<br />Predicted Cost = $51,543<br />R4: Other_Regions","Quantity: 2.809000e+03<br />Gross_Cost:    928735.00<br />Predicted Cost = $231,692<br />R4: Other_Regions","Quantity: 3.360577e+05<br />Gross_Cost:  20717810.32<br />Predicted Cost = $19,361,224<br />R4: Other_Regions","Quantity: 1.059500e+04<br />Gross_Cost:    380688.64<br />Predicted Cost = $791,076<br />R4: Other_Regions","Quantity: 1.500000e+01<br />Gross_Cost:    133700.70<br />Predicted Cost = $1,832<br />R4: Other_Regions","Quantity: 8.646000e+03<br />Gross_Cost:     12083.04<br />Predicted Cost = $655,472<br />R4: Other_Regions","Quantity: 2.193614e+01<br />Gross_Cost:      6632.26<br />Predicted Cost = $2,604<br />R4: Other_Regions","Quantity: 6.262000e+02<br />Gross_Cost:     14300.79<br />Predicted Cost = $57,804<br />R4: Other_Regions","Quantity: 6.060000e+03<br />Gross_Cost:     99611.40<br />Predicted Cost = $471,832<br />R4: Other_Regions","Quantity: 1.400000e+01<br />Gross_Cost:      8691.74<br />Predicted Cost = $1,719<br />R4: Other_Regions","Quantity: 2.000000e+01<br />Gross_Cost:     24268.94<br />Predicted Cost = $2,390<br />R4: Other_Regions","Quantity: 6.346000e+03<br />Gross_Cost:    417319.66<br />Predicted Cost = $492,394<br />R4: Other_Regions","Quantity: 2.172210e+04<br />Gross_Cost:   4336627.23<br />Predicted Cost = $1,536,860<br />R4: Other_Regions","Quantity: 8.999176e+04<br />Gross_Cost:   1871424.72<br />Predicted Cost = $5,723,182<br />R4: Other_Regions","Quantity: 1.853500e+02<br />Gross_Cost:     27139.12<br />Predicted Cost = $18,745<br />R4: Other_Regions","Quantity: 6.684800e+02<br />Gross_Cost:     13637.50<br />Predicted Cost = $61,405<br />R4: Other_Regions"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(248,118,109,1)"}},"hoveron":"points","name":"Other_Regions","legendgroup":"Other_Regions","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2.7024305364455254,4.1552570676526974,2.1461280356782382,5.0550742952996881,5.6388102911519624,5.115095709153235,5.3296585812030282,5.2169432401416342,5.34476116902146,4.6230561932390266,4.7533792679960891,2.6693168805661123,4.7212548242783958,3.3424226808222062,2.6852220653346204,3.6292056571023039,4.5515301398686079,5.1145002283533332,3.9506374200705787,2.1192558892779365,6.101551030991037,5.9334293932441931,3.3703187516177455,3.1630718820038193,3.6958282732599583,4.2699138977414801,4.20744450716556,6.0945933986918552,5.8228092628357917,4.4835787393578457,4.1603112322086577,1.4393326938302626,4.295423044232602,4.9580899140341002,6.6991327474116886,1.7168377232995244,4.1076423523558283,3.8429067791404372,1.8567288903828827,2.9484129657786009,3.539803497384677,6.0269743049404001,4.4471580313422194,3.8811483271514193,2.5051499783199058,3.4750898033890065,4.6512780139981444,4.038458016269562,3.4409090820652177,5.6342373846950906,5.3124325247115989,1.9777236052888478,3.815132600842988,2.0903638794717181,3.6974211573941798,5.3010299956639813,2.9013493254156422,3.6778714633289114,3.9685296443748395,3.9640896827699628,4.4469346224414545,5.5548738217592879,5.6363013905219077,5.3758137898824598,4.9473243011870238,5.1153751634736802,5.5083661869750022,2.5237464668115646,3.1037695231936726,4.7235106556542812,5.5581228379383969,3.1085650237328344,4.5443878155045354,5.2564575963805229,2.8665000026721725,4.6286341094782966,2.3802112417116059,5.385835299070485,3.6833460758601015,4.3651771284764331,2.9489017609702137,5.9058383905918417,4.0144113213844728,2.9332847723486948,2.5269592553422457,2.0681858617461617,2.5563025007672873,3.2068258760318495,3.6356144176238723,3.9073468568040881,4.6937269489236471,4.5223398859609105,4.8645110810583923,3.6959892241501806,4.9725942677630446,3.669006784060679,3.8529676910288182,2.9950864965057331,3.5294868192458373,4.1461280356782382,3.8226910107760546,3.1388140748403659,2.4969296480732148,1.8195439355418688,2.2041199826559246,5.5874914281065235,2.3424226808222062,2.459392487759231,4.8519428931916568,3.7341595132444669,4.8246073927962358,4.673349039687956,4.7950523324442234,1.505149978319906,4.0969100130080562,5.3689070087836734,4.176305458168649,5.2629302105120859,1.7558748556724915,3.9642124729698192,4.924279286061882,1.4771212547196624,2.6696887080562082,5.2266987378384382,5.53550465751026,3.4771212547196626,3.0432089705599021,4.7422021710376834,4.1707895904463914,4.4490307604004151,4.739170297578565,3.3382572302462554,1.9095560292411753,1.608526033577194,2.3802112417116059,2.114610984232173,4.3412366232386921,3.152685756036786,1.7781512503836436,4.7699508393203791,2.8382822499146885,5.9319661147281728,4.2406989791863081,2.7118072290411912,3.7061201097027037,4.7306086846331397,2.3979400086720375,4.5375560469706144,4.4459931817876468,3.4496233969379295,5.1302082875187756,3.0847192320112975,3.424881636631067,2.1760912590556813,4.4149733479708182,5.8524066026479842,2.7176705030022621,2.1583624920952498,4.6186128354441802,3.5138752046284449,3.4774106879072515,3.7654956964868163,4.9081656145845027,3.3817286185351105,2.7109631189952759,3.2291697025391009,5.4129479940390777,5.3019930608065904,3.2593620977686291,4.7149120615989917,2.5190400386483445,5.088896590000771,4.3893433112520777,4.7878025326563645,4.8567820413923553,3.2127201544178425,4.4644746434554561,3.0899051114393981,3.356790460351716,4.0684085197781616,3.4647875196459372,4.2022157758011316,2.0856829431946151,5.3442704183205363,2.2253092817258628,5.6293247851389783,2.3299061234002103,5.767913275535089,6.0286035551931025,4.7290035198247784,3.9542425094393248,2.1072099696478683,4.0304256454923628,2.2013971243204513,3.7913137227582072,4.1218381236604849,3.729436138956145,4.0105458741106785,4.0539039708951661,1.6884198220027107,2.5576996443512146,3.5109000750153396,4.9844022725395707,3.9394568495030713,5.3305287649853836,4.9727700721181538,6.0062195350443863,4.1757262983859329,1.971832279924925,4.2158743387181401,2.5471837614500816,1.505149978319906,3.6255182289716377,3.2607866686549762,2.6232492903979003,4.0500788634833285,5.4661258704181996,3.6290016192869916,1.6232492903979006,4.9613055041429064,3.1095785469043866,2.3789064000232618,3.1005083945019623,3.6016254795539449,2.638938294887355,3.0791812460476247,4.6201568839458202,4.9099859925054599,4.676405508271646,3.2073542607973531,2.7074168686367091,2.325720858019412,1.919078092376074,1.7781512503836436,3.7547381082614368,3.0614524790871931,1.9030899869919435,2.4353027522846102,1.7781512503836436,2.0507663112330423,3.4939318217735464,4.170788418101762,5.2950002782862713,1.8587777373054493,5.6709345635958428,1.3010299956639813,4.0969100130080562,2.9777236052888476,2.8006071163924688,4.8237910311893311,3.1271047983648077],"y":[6.5688919523484,6.7716394108722255,4.7757031943756161,6.5477311605694499,7.3445386573461091,6.9052433497705339,5.8272873504085991,6.1576359584805243,6.1350241914799222,6.2323728520817738,3.7717079790447743,5.634012255665108,7.1274066768494784,3.6843102397223149,5.4882785908909248,6.8039917559697578,7.2993543007292034,7.5049343511973126,7.0695285535090653,5.048819219327604,7.3247328478917462,7.7883641680629729,6.4999708714133702,6.2302760403240462,5.6312599362807116,5.5820731514806869,6.321856041652083,7.4187206737574476,7.1397261868874287,6.5067197994730472,6.2450882732566546,4.8418875617308892,6.1375314015406781,7.1679383119219064,6.0968198267383711,4.4516855999764564,6.8503163131617013,6.8949067278677774,4.3882417381318808,5.4554396036499915,6.3800635130356254,6.774510631552082,5.4779275407634653,6.7029648301905365,5.4868414825458505,5.5466258500473611,5.0655923736866324,4.0049092083006013,6.1018341096580748,7.0316873678757341,4.8939509689556093,null,5.408061969891345,6.7960038458829892,6.8841583095593633,5.3085228174687931,5.0971056793891432,5.3442652681956631,6.0815925465111222,5.9346356262342344,5.1017455621591461,7.1243732271458526,7.323671758811356,5.5323170242586155,7.1263672813883288,7.1116678946085381,5.1730655527476479,5.4852494780786696,5.983334742965992,7.2168550591956837,7.1740335817132781,4.7081202745270012,6.4860082783237063,7.0927292385782437,4.4490877442027479,6.4349956074154271,4.8198248858187744,7.0960877420483275,4.8574176580543664,6.0543328327418271,5.4937582029790724,5.5585077408889356,5.5802259059525321,3.6781795518339857,7.0986933841193638,4.0737183503461223,4.013914455846586,5.3784632037523954,5.8803089461581717,6.3313102824322991,3.6874147989961834,5.2095205102096358,5.6279139916926129,5.460668086797356,6.4896635622279923,6.4943636857010194,4.0228125909738175,5.9431230947011402,6.1297097152924902,5.9370204604293093,5.2673117599266597,4.8715836176578113,4.0334124958544226,6.1649323829653788,4.3214378961618864,7.7837283781160549,4.0694849443545378,5.1028475290053352,5.5078910993722285,5.4916460075976641,7.1659110338185048,6.790344879994036,7.1340687895718178,6.8378461609748005,5.511593716942377,7.4638244729554009,5.5794800329940824,7.051601499408986,3.7422009916027443,6.63589982774434,7.5080322813481395,4.0538610053975894,5.9101074095677433,6.7193852745543659,7.3394714943166379,6.4021966273300004,4.9760214052070202,6.8644624854148768,6.2358745704197833,6.5806507991751744,6.5774902177615884,5.9469314832458213,5.7634342608363598,6.1012156555856665,6.1579047005518381,6.2108674629531064,4.9084850188786495,5.9701783940224304,4.776931052296149,6.3567906322360415,6.6720097297203047,5.6404151746774742,6.7145692054545654,4.9019434030825844,6.1117472813684044,6.5235652970227846,5.9039102672783468,7.1338417231769533,5.781885241586485,5.8942882758118431,6.3784725423047357,6.5102930034123423,5.8634715656455869,5.2566879262850188,7.0302823245484474,6.6406049051407736,4.4313637641589869,3.4071579717844847,5.8173003690924574,5.1393493963038273,4.8553366773119926,7.3255908035591784,6.6540666601411402,5.6262140750699574,4.183633644280671,4.796320967121261,6.9226698621468481,4.4533573330475136,4.8274305998937699,6.3594791692221202,4.628376360875186,6.8045326534790584,6.1188613965140908,5.335829670111659,6.6578216410612487,5.8894785840432675,5.716057919329355,5.5379277558050939,4.4889019581435639,5.7900115004606691,5.1662053297496389,5.3861293815776365,4.6343246860868312,6.7090353268425877,3.9854615036625809,3.8149098558834615,5.3035836380935306,7.625963020785318,8.0299593081950409,7.2018568143746524,6.6908053770551836,5.3707939116343759,6.9369717549472245,5.4379138205161857,7.1434312570163909,6.2287153466544591,6.6250935072467696,6.4589166475731927,5.8479372332444433,5.0514097416139254,5.1221994894925871,5.8643491337811442,7.4190049556494202,6.9784368371452166,7.2130027881382635,5.4423917253566163,7.7780728685019387,5.3859510523104221,4.2537851565336133,6.1624582034599982,4.9154839419404901,5.1312034805781792,5.9680405180917724,6.0876562491612907,5.3833098007071269,5.4837044051942021,7.4706724936529731,6.4867542100741593,5.2189127626485865,6.8846291796953949,5.4190074234724728,5.6215847212790875,5.4793004387413724,6.1533700836875322,4.722245515956021,4.6987979896600311,6.1876256184854963,6.1565659413043488,6.0264646013461949,5.0426967672253751,4.8946939117291466,6.4149421995876663,5.227227232959522,6.6774951284809339,6.8259382591473257,4.0970166626367739,5.0609515452712523,4.8096362649156346,3.9183973388437003,5.3148602811808203,5.6993752551589845,5.8875061359754639,7.4724229388623682,4.5690741312770164,7.0328534843378563,4,5.1880430546284932,5.379107365362084,5.0586160626340169,6.7866496306089221,5.7677478623620946],"text":["Quantity: 5.040000e+02<br />Gross_Cost:   3705885.12<br />Predicted Cost = $47,288<br />R4: R4","Quantity: 1.429740e+04<br />Gross_Cost:   5910706.72<br />Predicted Cost = $1,043,789<br />R4: R4","Quantity: 1.400000e+02<br />Gross_Cost:     59662.74<br />Predicted Cost = $14,460<br />R4: R4","Quantity: 1.135205e+05<br />Gross_Cost:   3529646.08<br />Predicted Cost = $7,094,858<br />R4: R4","Quantity: 4.353217e+05<br />Gross_Cost:  22107450.30<br />Predicted Cost = $24,597,984<br />R4: R4","Quantity: 1.303454e+05<br />Gross_Cost:   8039764.91<br />Predicted Cost = $8,062,385<br />R4: R4","Quantity: 2.136282e+05<br />Gross_Cost:    671873.25<br />Predicted Cost = $12,733,101<br />R4: R4","Quantity: 1.647947e+05<br />Gross_Cost:   1437593.03<br />Predicted Cost = $10,015,497<br />R4: R4","Quantity: 2.211878e+05<br />Gross_Cost:   1364659.15<br />Predicted Cost = $13,149,344<br />R4: R4","Quantity: 4.198133e+04<br />Gross_Cost:   1707547.73<br />Predicted Cost = $2,827,008<br />R4: R4","Quantity: 5.667340e+04<br />Gross_Cost:      5911.64<br />Predicted Cost = $3,731,435<br />R4: R4","Quantity: 4.670000e+02<br />Gross_Cost:    430538.76<br />Predicted Cost = $44,067<br />R4: R4","Quantity: 5.263260e+04<br />Gross_Cost:  13409317.59<br />Predicted Cost = $3,484,662<br />R4: R4","Quantity: 2.200000e+03<br />Gross_Cost:      4834.04<br />Predicted Cost = $184,817<br />R4: R4","Quantity: 4.844200e+02<br />Gross_Cost:    307807.07<br />Predicted Cost = $45,586<br />R4: R4","Quantity: 4.258000e+03<br />Gross_Cost:   6367834.33<br />Predicted Cost = $340,420<br />R4: R4","Quantity: 3.560657e+04<br />Gross_Cost:  19922980.08<br />Predicted Cost = $2,427,534<br />R4: R4","Quantity: 1.301668e+05<br />Gross_Cost:  31984115.95<br />Predicted Cost = $8,052,166<br />R4: R4","Quantity: 8.925600e+03<br />Gross_Cost:  11736228.42<br />Predicted Cost = $675,056<br />R4: R4","Quantity: 1.316000e+02<br />Gross_Cost:    111897.20<br />Predicted Cost = $13,656<br />R4: R4","Quantity: 1.263430e+06<br />Gross_Cost:  21121893.47<br />Predicted Cost = $65,907,480<br />R4: R4","Quantity: 8.578856e+05<br />Gross_Cost:  61427687.76<br />Predicted Cost = $46,070,422<br />R4: R4","Quantity: 2.345950e+03<br />Gross_Cost:   3162065.57<br />Predicted Cost = $196,130<br />R4: R4","Quantity: 1.455700e+03<br />Gross_Cost:   1699323.41<br />Predicted Cost = $126,137<br />R4: R4","Quantity: 4.963960e+03<br />Gross_Cost:    427818.87<br />Predicted Cost = $392,321<br />R4: R4","Quantity: 1.861718e+04<br />Gross_Cost:    382008.61<br />Predicted Cost = $1,332,510<br />R4: R4","Quantity: 1.612295e+04<br />Gross_Cost:   2098244.25<br />Predicted Cost = $1,166,504<br />R4: R4","Quantity: 1.243350e+06<br />Gross_Cost:  26225312.61<br />Predicted Cost = $64,937,998<br />R4: R4","Quantity: 6.649810e+05<br />Gross_Cost:  13795142.37<br />Predicted Cost = $36,399,764<br />R4: R4","Quantity: 3.044940e+04<br />Gross_Cost:   3211587.80<br />Predicted Cost = $2,100,440<br />R4: R4","Quantity: 1.446476e+04<br />Gross_Cost:   1758280.96<br />Predicted Cost = $1,055,086<br />R4: R4","Quantity: 2.750000e+01<br />Gross_Cost:     69484.44<br />Predicted Cost = $3,209<br />R4: R4","Quantity: 1.974345e+04<br />Gross_Cost:   1372560.20<br />Predicted Cost = $1,406,910<br />R4: R4","Quantity: 9.080085e+04<br />Gross_Cost:  14721033.87<br />Predicted Cost = $5,770,762<br />R4: R4","Quantity: 5.001874e+06<br />Gross_Cost:   1249740.45<br />Predicted Cost = $235,341,107<br />R4: R4","Quantity: 5.210000e+01<br />Gross_Cost:     28293.43<br />Predicted Cost = $5,795<br />R4: R4","Quantity: 1.281275e+04<br />Gross_Cost:   7084615.96<br />Predicted Cost = $943,125<br />R4: R4","Quantity: 6.964770e+03<br />Gross_Cost:   7850670.10<br />Predicted Cost = $536,647<br />R4: R4","Quantity: 7.190000e+01<br />Gross_Cost:     24447.91<br />Predicted Cost = $7,807<br />R4: R4","Quantity: 8.880000e+02<br />Gross_Cost:    285390.56<br />Predicted Cost = $79,851<br />R4: R4","Quantity: 3.465800e+03<br />Gross_Cost:   2399183.76<br />Predicted Cost = $281,396<br />R4: R4","Quantity: 1.064080e+06<br />Gross_Cost:   5949913.22<br />Predicted Cost = $56,227,818<br />R4: R4","Quantity: 2.800000e+04<br />Gross_Cost:    300557.48<br />Predicted Cost = $1,943,664<br />R4: R4","Quantity: 7.605860e+03<br />Gross_Cost:   5046204.31<br />Predicted Cost = $582,187<br />R4: R4","Quantity: 3.200000e+02<br />Gross_Cost:    306790.20<br />Predicted Cost = $31,064<br />R4: R4","Quantity: 2.986000e+03<br />Gross_Cost:    352067.43<br />Predicted Cost = $245,165<br />R4: R4","Quantity: 4.480000e+04<br />Gross_Cost:    116303.39<br />Predicted Cost = $3,002,148<br />R4: R4","Quantity: 1.092592e+04<br />Gross_Cost:     10113.68<br />Predicted Cost = $813,905<br />R4: R4","Quantity: 2.760000e+03<br />Gross_Cost:   1264253.34<br />Predicted Cost = $227,951<br />R4: R4","Quantity: 4.307620e+05<br />Gross_Cost:  10756905.86<br />Predicted Cost = $24,359,568<br />R4: R4","Quantity: 2.053206e+05<br />Gross_Cost:     78334.12<br />Predicted Cost = $12,274,395<br />R4: R4","Quantity: 9.500000e+01<br />Gross_Cost:         0.00<br />Predicted Cost = $10,102<br />R4: R4","Quantity: 6.533300e+03<br />Gross_Cost:    255895.10<br />Predicted Cost = $505,822<br />R4: R4","Quantity: 1.231300e+02<br />Gross_Cost:   6251782.29<br />Predicted Cost = $12,841<br />R4: R4","Quantity: 4.982200e+03<br />Gross_Cost:   7658757.34<br />Predicted Cost = $393,654<br />R4: R4","Quantity: 2.000000e+05<br />Gross_Cost:    203480.51<br />Predicted Cost = $11,979,888<br />R4: R4","Quantity: 7.968000e+02<br />Gross_Cost:    125056.33<br />Predicted Cost = $72,235<br />R4: R4","Quantity: 4.762900e+03<br />Gross_Cost:    220935.38<br />Predicted Cost = $377,599<br />R4: R4","Quantity: 9.301000e+03<br />Gross_Cost:   1206681.20<br />Predicted Cost = $701,278<br />R4: R4","Quantity: 9.206397e+03<br />Gross_Cost:    860271.68<br />Predicted Cost = $694,677<br />R4: R4","Quantity: 2.798560e+04<br />Gross_Cost:    126399.56<br />Predicted Cost = $1,942,739<br />R4: R4","Quantity: 3.588177e+05<br />Gross_Cost:  13315982.85<br />Predicted Cost = $20,571,136<br />R4: R4","Quantity: 4.328141e+05<br />Gross_Cost:  21070350.44<br />Predicted Cost = $24,466,891<br />R4: R4","Quantity: 2.375821e+05<br />Gross_Cost:    340656.77<br />Predicted Cost = $14,048,429<br />R4: R4","Quantity: 8.857768e+04<br />Gross_Cost:  13377263.49<br />Predicted Cost = $5,639,946<br />R4: R4","Quantity: 1.304293e+05<br />Gross_Cost:  12932065.47<br />Predicted Cost = $8,067,185<br />R4: R4","Quantity: 3.223786e+05<br />Gross_Cost:    148958.59<br />Predicted Cost = $18,631,110<br />R4: R4","Quantity: 3.340000e+02<br />Gross_Cost:    305667.65<br />Predicted Cost = $32,320<br />R4: R4","Quantity: 1.269900e+03<br />Gross_Cost:    962353.75<br />Predicted Cost = $111,170<br />R4: R4","Quantity: 5.290670e+04<br />Gross_Cost:  16476124.28<br />Predicted Cost = $3,501,445<br />R4: R4","Quantity: 3.615121e+05<br />Gross_Cost:  14929098.44<br />Predicted Cost = $20,713,983<br />R4: R4","Quantity: 1.284000e+03<br />Gross_Cost:     51064.64<br />Predicted Cost = $112,311<br />R4: R4","Quantity: 3.502578e+04<br />Gross_Cost:   3062021.80<br />Predicted Cost = $2,390,885<br />R4: R4","Quantity: 1.804918e+05<br />Gross_Cost:  12380244.98<br />Predicted Cost = $10,894,901<br />R4: R4","Quantity: 7.353600e+02<br />Gross_Cost:     28124.69<br />Predicted Cost = $67,068<br />R4: R4","Quantity: 4.252400e+04<br />Gross_Cost:   2722673.77<br />Predicted Cost = $2,860,794<br />R4: R4","Quantity: 2.400000e+02<br />Gross_Cost:     66042.71<br />Predicted Cost = $23,806<br />R4: R4","Quantity: 2.431282e+05<br />Gross_Cost:  12476355.53<br />Predicted Cost = $14,351,512<br />R4: R4","Quantity: 4.823320e+03<br />Gross_Cost:     72014.12<br />Predicted Cost = $382,028<br />R4: R4","Quantity: 2.318340e+04<br />Gross_Cost:   1133268.54<br />Predicted Cost = $1,632,258<br />R4: R4","Quantity: 8.890000e+02<br />Gross_Cost:    311715.36<br />Predicted Cost = $79,934<br />R4: R4","Quantity: 8.050788e+05<br />Gross_Cost:    361832.64<br />Predicted Cost = $43,441,070<br />R4: R4","Quantity: 1.033740e+04<br />Gross_Cost:    380387.21<br />Predicted Cost = $773,269<br />R4: R4","Quantity: 8.576000e+02<br />Gross_Cost:      4766.28<br />Predicted Cost = $77,319<br />R4: R4","Quantity: 3.364800e+02<br />Gross_Cost:  12551435.08<br />Predicted Cost = $32,541<br />R4: R4","Quantity: 1.170000e+02<br />Gross_Cost:     11850.00<br />Predicted Cost = $12,248<br />R4: R4","Quantity: 3.600000e+02<br />Gross_Cost:     10325.58<br />Predicted Cost = $34,640<br />R4: R4","Quantity: 1.610000e+03<br />Gross_Cost:    239035.94<br />Predicted Cost = $138,457<br />R4: R4","Quantity: 4.321300e+03<br />Gross_Cost:    759117.40<br />Predicted Cost = $345,099<br />R4: R4","Quantity: 8.078800e+03<br />Gross_Cost:   2144422.14<br />Predicted Cost = $615,596<br />R4: R4","Quantity: 4.940000e+04<br />Gross_Cost:      4868.72<br />Predicted Cost = $3,286,226<br />R4: R4","Quantity: 3.329200e+04<br />Gross_Cost:    162002.05<br />Predicted Cost = $2,281,205<br />R4: R4","Quantity: 7.320000e+04<br />Gross_Cost:    424535.48<br />Predicted Cost = $4,727,949<br />R4: R4","Quantity: 4.965800e+03<br />Gross_Cost:    288847.15<br />Predicted Cost = $392,455<br />R4: R4","Quantity: 9.388458e+04<br />Gross_Cost:   3087902.38<br />Predicted Cost = $5,951,819<br />R4: R4","Quantity: 4.666667e+03<br />Gross_Cost:   3121502.49<br />Predicted Cost = $370,537<br />R4: R4","Quantity: 7.128000e+03<br />Gross_Cost:     10539.32<br />Predicted Cost = $548,271<br />R4: R4","Quantity: 9.887500e+02<br />Gross_Cost:    877249.43<br />Predicted Cost = $88,197<br />R4: R4","Quantity: 3.384440e+03<br />Gross_Cost:   1348061.53<br />Predicted Cost = $275,280<br />R4: R4","Quantity: 1.400000e+04<br />Gross_Cost:    865008.67<br />Predicted Cost = $1,023,690<br />R4: R4","Quantity: 6.648000e+03<br />Gross_Cost:    185059.66<br />Predicted Cost = $514,031<br />R4: R4","Quantity: 1.376620e+03<br />Gross_Cost:     74401.83<br />Predicted Cost = $119,785<br />R4: R4","Quantity: 3.140000e+02<br />Gross_Cost:     10799.72<br />Predicted Cost = $30,525<br />R4: R4","Quantity: 6.600000e+01<br />Gross_Cost:   1461949.54<br />Predicted Cost = $7,212<br />R4: R4","Quantity: 1.600000e+02<br />Gross_Cost:     20962.25<br />Predicted Cost = $16,361<br />R4: R4","Quantity: 3.868044e+05<br />Gross_Cost:  60775477.28<br />Predicted Cost = $22,051,065<br />R4: R4","Quantity: 2.200000e+02<br />Gross_Cost:     11735.05<br />Predicted Cost = $21,965<br />R4: R4","Quantity: 2.880000e+02<br />Gross_Cost:    126720.69<br />Predicted Cost = $28,180<br />R4: R4","Quantity: 7.111200e+04<br />Gross_Cost:    322026.12<br />Predicted Cost = $4,603,066<br />R4: R4","Quantity: 5.422000e+03<br />Gross_Cost:    310203.01<br />Predicted Cost = $425,694<br />R4: R4","Quantity: 6.677400e+04<br />Gross_Cost:  14652476.51<br />Predicted Cost = $4,342,720<br />R4: R4","Quantity: 4.713560e+04<br />Gross_Cost:   6170848.44<br />Predicted Cost = $3,146,646<br />R4: R4","Quantity: 6.238100e+04<br />Gross_Cost:  13616603.44<br />Predicted Cost = $4,077,776<br />R4: R4","Quantity: 3.200000e+01<br />Gross_Cost:   6884084.00<br />Predicted Cost = $3,692<br />R4: R4","Quantity: 1.250000e+04<br />Gross_Cost:    324783.32<br />Predicted Cost = $921,811<br />R4: R4","Quantity: 2.338336e+05<br />Gross_Cost:  29095409.43<br />Predicted Cost = $13,843,279<br />R4: R4","Quantity: 1.500740e+04<br />Gross_Cost:    379734.48<br />Predicted Cost = $1,091,648<br />R4: R4","Quantity: 1.832020e+05<br />Gross_Cost:  11261636.35<br />Predicted Cost = $11,046,137<br />R4: R4","Quantity: 5.700000e+01<br />Gross_Cost:      5523.33<br />Predicted Cost = $6,298<br />R4: R4","Quantity: 9.209000e+03<br />Gross_Cost:   4324140.81<br />Predicted Cost = $694,859<br />R4: R4","Quantity: 8.400000e+04<br />Gross_Cost:  32213082.24<br />Predicted Cost = $5,369,803<br />R4: R4","Quantity: 3.000000e+01<br />Gross_Cost:     11320.38<br />Predicted Cost = $3,478<br />R4: R4","Quantity: 4.674000e+02<br />Gross_Cost:    813031.57<br />Predicted Cost = $44,102<br />R4: R4","Quantity: 1.685383e+05<br />Gross_Cost:   5240651.43<br />Predicted Cost = $10,225,777<br />R4: R4","Quantity: 3.431663e+05<br />Gross_Cost:  21851008.92<br />Predicted Cost = $19,739,757<br />R4: R4","Quantity: 3.000000e+03<br />Gross_Cost:   2524623.54<br />Predicted Cost = $246,228<br />R4: R4","Quantity: 1.104610e+03<br />Gross_Cost:     94628.38<br />Predicted Cost = $97,717<br />R4: R4","Quantity: 5.523345e+04<br />Gross_Cost:   7319180.97<br />Predicted Cost = $3,643,653<br />R4: R4","Quantity: 1.481800e+04<br />Gross_Cost:   1721371.35<br />Predicted Cost = $1,078,898<br />R4: R4","Quantity: 2.812100e+04<br />Gross_Cost:   3807595.45<br />Predicted Cost = $1,951,432<br />R4: R4","Quantity: 5.484920e+04<br />Gross_Cost:   3779986.23<br />Predicted Cost = $3,620,200<br />R4: R4","Quantity: 2.179000e+03<br />Gross_Cost:    884975.98<br />Predicted Cost = $183,184<br />R4: R4","Quantity: 8.120000e+01<br />Gross_Cost:    580008.37<br />Predicted Cost = $8,737<br />R4: R4","Quantity: 4.060000e+01<br />Gross_Cost:   1262454.27<br />Predicted Cost = $4,601<br />R4: R4","Quantity: 2.400000e+02<br />Gross_Cost:   1438482.89<br />Predicted Cost = $23,806<br />R4: R4","Quantity: 1.302000e+02<br />Gross_Cost:   1625052.75<br />Predicted Cost = $13,521<br />R4: R4","Quantity: 2.194000e+04<br />Gross_Cost:     81000.00<br />Predicted Cost = $1,551,115<br />R4: R4","Quantity: 1.421300e+03<br />Gross_Cost:    933637.73<br />Predicted Cost = $123,377<br />R4: R4","Quantity: 6.000000e+01<br />Gross_Cost:     59831.66<br />Predicted Cost = $6,604<br />R4: R4","Quantity: 5.887770e+04<br />Gross_Cost:   2274000.90<br />Predicted Cost = $3,865,490<br />R4: R4","Quantity: 6.891000e+02<br />Gross_Cost:   4699046.36<br />Predicted Cost = $63,155<br />R4: R4","Quantity: 8.550000e+05<br />Gross_Cost:    436933.33<br />Predicted Cost = $45,927,061<br />R4: R4","Quantity: 1.740600e+04<br />Gross_Cost:   5182856.75<br />Predicted Cost = $1,252,122<br />R4: R4","Quantity: 5.150000e+02<br />Gross_Cost:     79789.07<br />Predicted Cost = $48,242<br />R4: R4","Quantity: 5.083000e+03<br />Gross_Cost:   1293442.96<br />Predicted Cost = $401,015<br />R4: R4","Quantity: 5.377850e+04<br />Gross_Cost:   3338606.98<br />Predicted Cost = $3,554,782<br />R4: R4","Quantity: 2.500000e+02<br />Gross_Cost:    801512.44<br />Predicted Cost = $24,723<br />R4: R4","Quantity: 3.447911e+04<br />Gross_Cost:  13609486.00<br />Predicted Cost = $2,356,348<br />R4: R4","Quantity: 2.792500e+04<br />Gross_Cost:    605180.94<br />Predicted Cost = $1,938,848<br />R4: R4","Quantity: 2.815940e+03<br />Gross_Cost:    783949.84<br />Predicted Cost = $232,221<br />R4: R4","Quantity: 1.349610e+05<br />Gross_Cost:   2390410.80<br />Predicted Cost = $8,326,120<br />R4: R4","Quantity: 1.215400e+03<br />Gross_Cost:   3238120.48<br />Predicted Cost = $106,749<br />R4: R4","Quantity: 2.660000e+03<br />Gross_Cost:    730250.00<br />Predicted Cost = $220,301<br />R4: R4","Quantity: 1.500000e+02<br />Gross_Cost:    180587.60<br />Predicted Cost = $15,413<br />R4: R4","Quantity: 2.600000e+04<br />Gross_Cost:  10722161.01<br />Predicted Cost = $1,814,890<br />R4: R4","Quantity: 7.118797e+05<br />Gross_Cost:   4371242.55<br />Predicted Cost = $38,768,242<br />R4: R4","Quantity: 5.220000e+02<br />Gross_Cost:     27000.00<br />Predicted Cost = $48,848<br />R4: R4","Quantity: 1.440000e+02<br />Gross_Cost:      2553.63<br />Predicted Cost = $14,842<br />R4: R4","Quantity: 4.155400e+04<br />Gross_Cost:    656599.23<br />Predicted Cost = $2,800,379<br />R4: R4","Quantity: 3.264940e+03<br />Gross_Cost:    137831.79<br />Predicted Cost = $266,278<br />R4: R4","Quantity: 3.002000e+03<br />Gross_Cost:     71669.88<br />Predicted Cost = $246,380<br />R4: R4","Quantity: 5.827680e+03<br />Gross_Cost:  21163661.35<br />Predicted Cost = $455,076<br />R4: R4","Quantity: 8.094045e+04<br />Gross_Cost:   4508859.06<br />Predicted Cost = $5,188,636<br />R4: R4","Quantity: 2.408400e+03<br />Gross_Cost:    422877.01<br />Predicted Cost = $200,955<br />R4: R4","Quantity: 5.140000e+02<br />Gross_Cost:     15262.78<br />Predicted Cost = $48,155<br />R4: R4","Quantity: 1.695000e+03<br />Gross_Cost:     62563.49<br />Predicted Cost = $145,205<br />R4: R4","Quantity: 2.587903e+05<br />Gross_Cost:   8368928.59<br />Predicted Cost = $15,204,666<br />R4: R4","Quantity: 2.004440e+05<br />Gross_Cost:     28402.55<br />Predicted Cost = $12,004,486<br />R4: R4","Quantity: 1.817030e+03<br />Gross_Cost:     67209.49<br />Predicted Cost = $154,850<br />R4: R4","Quantity: 5.186950e+04<br />Gross_Cost:   2288121.96<br />Predicted Cost = $3,437,903<br />R4: R4","Quantity: 3.304000e+02<br />Gross_Cost:     42498.77<br />Predicted Cost = $31,997<br />R4: R4","Quantity: 1.227147e+05<br />Gross_Cost:   6375770.17<br />Predicted Cost = $7,624,816<br />R4: R4","Quantity: 2.451000e+04<br />Gross_Cost:   1314805.15<br />Predicted Cost = $1,718,472<br />R4: R4","Quantity: 6.134830e+04<br />Gross_Cost:    216685.41<br />Predicted Cost = $4,015,294<br />R4: R4","Quantity: 7.190880e+04<br />Gross_Cost:   4548012.41<br />Predicted Cost = $4,650,754<br />R4: R4","Quantity: 1.632000e+03<br />Gross_Cost:    775315.71<br />Predicted Cost = $140,206<br />R4: R4","Quantity: 2.913900e+04<br />Gross_Cost:    520065.35<br />Predicted Cost = $2,016,690<br />R4: R4","Quantity: 1.230000e+03<br />Gross_Cost:    345086.33<br />Predicted Cost = $107,935<br />R4: R4","Quantity: 2.274000e+03<br />Gross_Cost:     30824.92<br />Predicted Cost = $190,560<br />R4: R4","Quantity: 1.170600e+04<br />Gross_Cost:    616611.33<br />Predicted Cost = $867,517<br />R4: R4","Quantity: 2.916000e+03<br />Gross_Cost:    146624.09<br />Predicted Cost = $239,844<br />R4: R4","Quantity: 1.593000e+04<br />Gross_Cost:    243292.87<br />Predicted Cost = $1,153,585<br />R4: R4","Quantity: 1.218100e+02<br />Gross_Cost:     43084.86<br />Predicted Cost = $12,713<br />R4: R4","Quantity: 2.209380e+05<br />Gross_Cost:   5117234.59<br />Predicted Cost = $13,135,607<br />R4: R4","Quantity: 1.680000e+02<br />Gross_Cost:      9670.78<br />Predicted Cost = $17,116<br />R4: R4","Quantity: 4.259168e+05<br />Gross_Cost:      6529.95<br />Predicted Cost = $24,106,015<br />R4: R4","Quantity: 2.137500e+02<br />Gross_Cost:    201179.46<br />Predicted Cost = $21,388<br />R4: R4","Quantity: 5.860211e+05<br />Gross_Cost:  42263262.65<br />Predicted Cost = $32,383,201<br />R4: R4","Quantity: 1.068079e+06<br />Gross_Cost: 107141891.25<br />Predicted Cost = $56,423,275<br />R4: R4","Quantity: 5.358010e+04<br />Gross_Cost:  15916838.67<br />Predicted Cost = $3,542,650<br />R4: R4","Quantity: 9.000000e+03<br />Gross_Cost:   4906879.32<br />Predicted Cost = $680,259<br />R4: R4","Quantity: 1.280000e+02<br />Gross_Cost:    234851.81<br />Predicted Cost = $13,310<br />R4: R4","Quantity: 1.072570e+04<br />Gross_Cost:   8649116.66<br />Predicted Cost = $800,099<br />R4: R4","Quantity: 1.590000e+02<br />Gross_Cost:    274103.02<br />Predicted Cost = $16,266<br />R4: R4","Quantity: 6.184630e+03<br />Gross_Cost:  13913335.48<br />Predicted Cost = $480,801<br />R4: R4","Quantity: 1.323848e+04<br />Gross_Cost:   1693227.63<br />Predicted Cost = $972,076<br />R4: R4","Quantity: 5.363350e+03<br />Gross_Cost:   4217873.08<br />Predicted Cost = $421,433<br />R4: R4","Quantity: 1.024580e+04<br />Gross_Cost:   2876846.22<br />Predicted Cost = $766,929<br />R4: R4","Quantity: 1.132150e+04<br />Gross_Cost:    704591.23<br />Predicted Cost = $841,126<br />R4: R4","Quantity: 4.880000e+01<br />Gross_Cost:    112566.65<br />Predicted Cost = $5,455<br />R4: R4","Quantity: 3.611600e+02<br />Gross_Cost:    132495.00<br />Predicted Cost = $34,743<br />R4: R4","Quantity: 3.242650e+03<br />Gross_Cost:    731727.09<br />Predicted Cost = $264,596<br />R4: R4","Quantity: 9.647222e+04<br />Gross_Cost:  26242484.88<br />Predicted Cost = $6,103,404<br />R4: R4","Quantity: 8.698750e+03<br />Gross_Cost:   9515614.45<br />Predicted Cost = $659,170<br />R4: R4","Quantity: 2.140567e+05<br />Gross_Cost:  16330624.32<br />Predicted Cost = $12,756,722<br />R4: R4","Quantity: 9.392259e+04<br />Gross_Cost:    276943.85<br />Predicted Cost = $5,954,048<br />R4: R4","Quantity: 1.014424e+06<br />Gross_Cost:  59989172.12<br />Predicted Cost = $53,796,382<br />R4: R4","Quantity: 1.498740e+04<br />Gross_Cost:    243192.99<br />Predicted Cost = $1,090,302<br />R4: R4","Quantity: 9.372000e+01<br />Gross_Cost:     17938.46<br />Predicted Cost = $9,976<br />R4: R4","Quantity: 1.643896e+04<br />Gross_Cost:   1453644.48<br />Predicted Cost = $1,187,637<br />R4: R4","Quantity: 3.525200e+02<br />Gross_Cost:     82315.94<br />Predicted Cost = $33,974<br />R4: R4","Quantity: 3.200000e+01<br />Gross_Cost:    135270.62<br />Predicted Cost = $3,692<br />R4: R4","Quantity: 4.222000e+03<br />Gross_Cost:    929053.06<br />Predicted Cost = $337,757<br />R4: R4","Quantity: 1.823000e+03<br />Gross_Cost:   1223647.28<br />Predicted Cost = $155,320<br />R4: R4","Quantity: 4.200000e+02<br />Gross_Cost:    241718.45<br />Predicted Cost = $39,949<br />R4: R4","Quantity: 1.122222e+04<br />Gross_Cost:    304582.12<br />Predicted Cost = $834,301<br />R4: R4","Quantity: 2.925000e+05<br />Gross_Cost:  29557826.37<br />Predicted Cost = $17,028,109<br />R4: R4","Quantity: 4.256000e+03<br />Gross_Cost:   3067285.56<br />Predicted Cost = $340,272<br />R4: R4","Quantity: 4.200000e+01<br />Gross_Cost:    165543.74<br />Predicted Cost = $4,748<br />R4: R4","Quantity: 9.147565e+04<br />Gross_Cost:   7667065.61<br />Predicted Cost = $5,810,421<br />R4: R4","Quantity: 1.287000e+03<br />Gross_Cost:    262426.34<br />Predicted Cost = $112,554<br />R4: R4","Quantity: 2.392800e+02<br />Gross_Cost:    418393.30<br />Predicted Cost = $23,740<br />R4: R4","Quantity: 1.260400e+03<br />Gross_Cost:    301509.11<br />Predicted Cost = $110,400<br />R4: R4","Quantity: 3.996000e+03<br />Gross_Cost:   1423541.34<br />Predicted Cost = $320,999<br />R4: R4","Quantity: 4.354500e+02<br />Gross_Cost:     52752.80<br />Predicted Cost = $41,306<br />R4: R4","Quantity: 1.200000e+03<br />Gross_Cost:     49980.20<br />Predicted Cost = $105,498<br />R4: R4","Quantity: 4.170200e+04<br />Gross_Cost:   1540372.01<br />Predicted Cost = $2,809,604<br />R4: R4","Quantity: 8.128043e+04<br />Gross_Cost:   1434055.44<br />Predicted Cost = $5,208,792<br />R4: R4","Quantity: 4.746850e+04<br />Gross_Cost:   1062831.95<br />Predicted Cost = $3,167,197<br />R4: R4","Quantity: 1.611960e+03<br />Gross_Cost:    110330.80<br />Predicted Cost = $138,613<br />R4: R4","Quantity: 5.098200e+02<br />Gross_Cost:     78468.24<br />Predicted Cost = $47,793<br />R4: R4","Quantity: 2.117000e+02<br />Gross_Cost:   2599813.53<br />Predicted Cost = $21,198<br />R4: R4","Quantity: 8.300000e+01<br />Gross_Cost:    168743.57<br />Predicted Cost = $8,916<br />R4: R4","Quantity: 6.000000e+01<br />Gross_Cost:   4758774.53<br />Predicted Cost = $6,604<br />R4: R4","Quantity: 5.685100e+03<br />Gross_Cost:   6697893.83<br />Predicted Cost = $444,767<br />R4: R4","Quantity: 1.152000e+03<br />Gross_Cost:     12503.07<br />Predicted Cost = $101,588<br />R4: R4","Quantity: 8.000000e+01<br />Gross_Cost:    115067.20<br />Predicted Cost = $8,617<br />R4: R4","Quantity: 2.724600e+02<br />Gross_Cost:     64511.37<br />Predicted Cost = $26,770<br />R4: R4","Quantity: 6.000000e+01<br />Gross_Cost:      8287.00<br />Predicted Cost = $6,604<br />R4: R4","Quantity: 1.124000e+02<br />Gross_Cost:    206471.58<br />Predicted Cost = $11,802<br />R4: R4","Quantity: 3.118400e+03<br />Gross_Cost:    500466.78<br />Predicted Cost = $255,204<br />R4: R4","Quantity: 1.481796e+04<br />Gross_Cost:    771802.42<br />Predicted Cost = $1,078,895<br />R4: R4","Quantity: 1.972424e+05<br />Gross_Cost:  29677201.05<br />Predicted Cost = $11,827,018<br />R4: R4","Quantity: 7.224000e+01<br />Gross_Cost:     37074.40<br />Predicted Cost = $7,841<br />R4: R4","Quantity: 4.687428e+05<br />Gross_Cost:  10785827.85<br />Predicted Cost = $26,339,921<br />R4: R4","Quantity: 2.000000e+01<br />Gross_Cost:     10000.00<br />Predicted Cost = $2,390<br />R4: R4","Quantity: 1.250000e+04<br />Gross_Cost:    154185.33<br />Predicted Cost = $921,811<br />R4: R4","Quantity: 9.500000e+02<br />Gross_Cost:    239390.75<br />Predicted Cost = $84,995<br />R4: R4","Quantity: 6.318400e+02<br />Gross_Cost:    114450.07<br />Predicted Cost = $58,286<br />R4: R4","Quantity: 6.664860e+04<br />Gross_Cost:   6118565.74<br />Predicted Cost = $4,335,176<br />R4: R4","Quantity: 1.340000e+03<br />Gross_Cost:    585797.97<br />Predicted Cost = $116,835<br />R4: R4"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(0,191,196,1)"}},"hoveron":"points","name":"R4","legendgroup":"R4","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[0.75413804739774692,7.0737400781282673],"y":[2.8725776938429157,8.7182095722686483],"text":"intercept: 2.175<br />slope: 0.925","type":"scatter","mode":"lines","line":{"width":4.7244094488188981,"color":"rgba(0,255,0,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.762557077625573,"r":7.3059360730593621,"b":40.182648401826498,"l":95.707762557077658},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"title":{"text":"Construction Debris","font":{"color":"rgba(0,0,0,1)","family":"","size":17.534246575342465},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.75413804739774692,7.0737400781282673],"tickmode":"array","ticktext":["100","10,000","1,000,000"],"tickvals":[2,4,6],"categoryorder":"array","categoryarray":["100","10,000","1,000,000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"y","title":{"text":"Quantity - Cubic Yards","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[2.915034289467807,8.3081145158141805],"tickmode":"array","ticktext":["$10,000","$1,000,000","$100,000,000"],"tickvals":[4,6,8],"categoryorder":"array","categoryarray":["$10,000","$1,000,000","$100,000,000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"x","title":{"text":"Gross Cost","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498},"title":{"text":"R4","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"ee4e592095e3":{"x":{},"y":{},"text":{},"colour":{},"type":"scatter"},"ee4e6a5097cb":{"intercept":{},"slope":{}},"ee4e511dc6f4":{"intercept":{},"slope":{}}},"cur_data":"ee4e592095e3","visdat":{"ee4e592095e3":["function (y) ","x"],"ee4e6a5097cb":["function (y) ","x"],"ee4e511dc6f4":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

### Formulas Explained

- Just like for Vegetative Debris, I’ve fit a line through this data
  cloud, and the converted that to a formula that can be used to
  estimate costs.
- Slope intercept formula converted to log-log format is:
- $log10(y) = intercept + slope * log10(x)$
- Example for 10000 Cubic Yards
- $log10(10000) ~= 2.175 + .925 * log10(100)$
- $4 ~= 2.175 + .925 * 2$

(Actual answer for 100 cubic yards = 4.025, so 10^4.025 = \$10592.54
Formula for CY $10^ (log10(Cubic Yards) *.925 + 2.175)$

Note: I fit this relationship… with an eye test. An actual regression
relationship fit to this is… below. The numbers come out slightly
different, but it’s a reasonable approximation.

### Regression Fitting

``` r
cd_model <- cd_pred_MT %>% 
filter(Quantity > 10, 
       Gross_Cost < 1.25e08,
       is.na(Quantity)==FALSE,
       is.na(Gross_Cost)==FALSE) %>% 
    lm(formula = log10(1+Gross_Cost) ~ log10(1+Quantity))

summary(cd_model)
```

    ## 
    ## Call:
    ## lm(formula = log10(1 + Gross_Cost) ~ log10(1 + Quantity), data = .)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.6563 -0.5803  0.0339  0.6386  2.6086 
    ## 
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          3.56817    0.11169   31.95   <2e-16 ***
    ## log10(1 + Quantity)  0.54892    0.02945   18.64   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.8992 on 592 degrees of freedom
    ## Multiple R-squared:  0.3698, Adjusted R-squared:  0.3688 
    ## F-statistic: 347.4 on 1 and 592 DF,  p-value: < 2.2e-16

Notes: - The model is \*okay. SIgnificant fit, moderate R^2. - There is
a lot of variance in trying to fit a single straight line through the
widely disperesed cloud, so on either end, we have large residiuals (the
difference between the line and data points). - A formula derived from
this could be good **in the aggregate** but could be off by a lot for
individual predictions.

### Creating Functions to Show the Estimates

``` r
cy_eye_formula <- function(CY){
  10^ (log10(CY) *.925 + 2.175)
}

cy_reg_formula <- function(CY){
  10^(log10(CY)*0.54892  + 3.56817)
}

cy_reg_formula(100)
```

    ## [1] 46345.76

``` r
#46345.76
cy_eye_formula(100)
```

    ## [1] 10592.54

``` r
#10592.54
```

### The differences in how these two relationships predict:

![](/images/unnamed-chunk-23-1.png)<!-- -->

- If we want to choose one that **undershoots costs** - we can go with
  Blue line.
- This is what our Region 4 POCs preferred, as they would rather
  **underestimate costs** since this is being used in pre-declaration
  assessment to determine if a given county **will get declared or
  not**.

### Formula Predictions v Actuals

<div class="plotly html-widget html-fill-item" id="htmlwidget-42cdfdddbc1c5d3fb2c9" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-42cdfdddbc1c5d3fb2c9">{"x":{"data":[{"x":[2.7024305364455254,4.1984096335377616,4.1552570676526974,2.1461280356782382,5.0550742952996881,5.6388102911519624,5.115095709153235,5.3296585812030282,5.2169432401416342,5.34476116902146,4.6230561932390266,4.7533792679960891,2.6693168805661123,4.7212548242783958,3.3424226808222062,2.6852220653346204,3.6292056571023039,4.5515301398686079,5.1145002283533332,3.9506374200705787,2.1192558892779365,6.101551030991037,5.9334293932441931,3.3703187516177455,3.1630718820038193,3.6958282732599583,4.2699138977414801,4.20744450716556,6.0945933986918552,5.8228092628357917,4.4835787393578457,4.1603112322086577,1.4393326938302626,4.295423044232602,4.9580899140341002,6.6991327474116886,1.7168377232995244,4.1076423523558283,3.8429067791404372,1.8567288903828827,2.2900346113625178,2.0413926851582249,5.2442501466886622,5.1216601175420404,1.9030899869919435,2.9484129657786009,3.539803497384677,5.074771948771585,5.2430722945580479,5.7822159849460446,3.6989700043360187,4.1925674533365456,1.2355284469075489,3.4890016973113775,5.8994042226163863,4.8991850050849477,6.1582259875720986,3.8829796540372992,6.0433759787151002,2.4313637641589874,3.2050418792613695,3.5151052041667898,3.5724068675580556,3.9456654994321343,6.2333936760774149,3.5147734739975087,1.8450980400142569,2.9444826721501687,6.1130413755231467,6.7864854403677892,4.3750496821292746,6.4300969955807767,3.0637085593914173,5.2695250856143598,1.3010299956639813,1.2950711714662781,2.0791812460476247,4.3639093040106847,2.2036169956331912,2.0901169107520099,4.1027629037125646,3.7320719409998668,2.255272505103306,1.5440680443502757,2.357934847000454,4.1587293762389352,3.2398898183400542,1.8450980400142569,3.370883016777606,3.8958091501691308,5.0504952949350823,3.6037612606082874,3.0926855629374908,2.5440680443502757,4.5093080578565763,3.7281150573980244,2.9858780492079968,5.3515017638802327,3.1965768448522329,3.8655623192261745,3.6868149545073168,4.3139776773448899,3.7225993750077753,6.0269743049404001,4.4471580313422194,3.8811483271514193,2.5051499783199058,5.3979330599047373,1.6654497448426819,2.9929950984313414,4.4581239446610619,3.2487087356009177,4.56059520730137,3.4750898033890065,4.6512780139981444,4.038458016269562,3.4409090820652177,5.6342373846950906,5.3124325247115989,1.9777236052888478,3.815132600842988,2.0903638794717181,3.6974211573941798,5.3010299956639813,2.9013493254156422,3.6778714633289114,2.5717088318086878,2.4771212547196626,2.9786369483844743,2.7874604745184151,1.7781512503836436,2.5910646070264991,4.5798258811262302,3.4367587960456936,3.9685296443748395,4.082507449923134,3.1283992687178066,1.9834007381805383,4.9780891730561425,2.8407332346118066,4.0483446785400696,4.5002620046610948,3.9640896827699628,4.4469346224414545,5.5548738217592879,5.6363013905219077,5.3758137898824598,4.9473243011870238,5.1153751634736802,5.5083661869750022,2.5237464668115646,3.1037695231936726,4.7235106556542812,5.5581228379383969,3.1085650237328344,4.5443878155045354,5.2564575963805229,2.8665000026721725,4.6286341094782966,2.3802112417116059,5.385835299070485,3.6833460758601015,2.255272505103306,4.2944794532032704,3.1655410767223731,4.3651771284764331,2.9489017609702137,5.9058383905918417,4.0144113213844728,1.3010299956639813,1.954242509439325,2.5436956323092446,2.5563025007672873,3.247354508217859,4.2418760607224613,2.3617278360175931,2.9332847723486948,2.5269592553422457,4.8930178883115341,1.4471580313422192,1.255272505103306,4.3544284692600153,5.0044589502411947,3.7185016888672742,2.8573324964312685,1.7781512503836436,1.6011905326153335,3.4377505628203879,2.859138297294531,3.3802112417116059,3.3644952932801808,4.0387790695555381,2.6919651027673601,1.9822712330395684,1.4771212547196624,2.8853612200315122,3.1654105314399699,2.510545010206612,4.1689457569017909,4.300986564044174,3.0097482559485536,3.4505570094183291,4.6672567755966927,3.2108533653148932,4.4171061673925927,3.3070144100729419,1.6972293427597176,2.0681858617461617,2.5563025007672873,3.2068258760318495,3.6356144176238723,3.9073468568040881,4.6937269489236471,1.8375884382355112,4.0791479488609372,1.1814205162624751,3.7875313161272341,2.5646660642520893,1.9030899869919435,6.4771799528902045,3.4117880045438689,1.5954962218255742,2.6989700043360187,2.3096301674258988,3.3344537511509307,1.9118112227541586,2.3873898263387292,2.3010299956639813,2.8846065435331911,2.9444826721501687,2.840106094456758,3.2464985807958011,4.4813193459054528,3.4352963763370234,3.7289119178080758,3.9476297473843545,3.0025979807199086,2.5172038181418617,2.1760912590556813,3.7564877686873519,3.0591846176313711,3.4653828514484184,3.4185051298166429,2.4297522800024081,2.8522603510069531,4.1983546247369397,4.5223398859609105,4.8645110810583923,3.6959892241501806,4.9725942677630446,3.669006784060679,3.8529676910288182,2.9950864965057331,3.5294868192458373,4.1461280356782382,2.0906107078284069,4.8240028857830639,2.8350561017201161,3.5399538416563967,2.9469432706978256,2.7558748556724915,3.6532125137753435,2.7554479706597705,3.2417257394831371,3.8226910107760546,2.6473829701146196,2.3222192947339191,2.4149733479708178,2.7323937598229686,4.0387790695555381,3.1388140748403659,2.4969296480732148,1.8195439355418688,2.2041199826559246,3.6991436873944838,1.4456042032735976,1.2787536009528289,2.5224442335063197,3.8205954965444904,2.8927622346158168,1.3010299956639813,5.5874914281065235,2.3424226808222062,2.459392487759231,4.8519428931916568,3.7341595132444669,4.8246073927962358,4.673349039687956,4.7950523324442234,1.505149978319906,4.0969100130080562,3.0195316845312554,2.0791812460476247,3.3926969532596658,1.6020599913279623,5.029158048255157,5.029158048255157,5.3689070087836734,4.176305458168649,5.2629302105120859,3.9945397430417637,5.4889805680440604,5.4842383116077116,5.7900105637080417,3.7134905430939424,1.9461573949223723,3.0879587894607328,2.9794391044854023,4.1517543233268732,4.1445726509957801,4.8625493655096754,1.7634279935629373,3.6957442751973235,4.5925986289061314,4.3913938751356989,3.986709048064589,4.151086869007643,3.7563622110126267,2.3924859087190731,2.853819845856763,4.3939899976265382,3.584218112117405,3.0674428427763805,3.9030899869919438,2.3502480183341627,4.0116972881141422,1.7558748556724915,3.9642124729698192,4.924279286061882,1.4771212547196624,2.6696887080562082,5.2266987378384382,5.53550465751026,3.4771212547196626,3.0432089705599021,4.7422021710376834,4.1707895904463914,4.4490307604004151,4.739170297578565,3.3382572302462554,1.9095560292411753,1.608526033577194,2.3802112417116059,2.114610984232173,4.3412366232386921,3.152685756036786,1.954242509439325,2.0791812460476247,3.1325798476597368,1.7781512503836436,4.7699508393203791,2.8382822499146885,3.8573324964312685,1.568201724066995,4.8375909631960896,5.9319661147281728,4.2406989791863081,2.7118072290411912,3.7061201097027037,4.7306086846331397,2.3979400086720375,4.5375560469706144,4.4459931817876468,3.4496233969379295,5.1302082875187756,3.1613680022349748,2.5514499979728753,4.8655301206260217,5.4485649734633181,2.2709581850920975,4.1461280356782382,3.79155030502733,5.633076829294466,3.0847192320112975,3.424881636631067,2.1760912590556813,1.2176951179079363,1.2193225084193366,5.0580385129555649,2.8349672019384444,4.4149733479708182,5.8524066026479842,1.2253092817258628,2.7176705030022621,2.1583624920952498,4.6186128354441802,3.5138752046284449,3.4774106879072515,3.7654956964868163,4.9081656145845027,3.3817286185351105,2.7109631189952759,3.2291697025391009,5.4129479940390777,5.3019930608065904,3.2593620977686291,4.7149120615989917,2.5190400386483445,5.088896590000771,4.3893433112520777,4.7878025326563645,4.8567820413923553,2.4668676203541096,4.5422070194326958,3.949953248133617,4.6215371780320238,4.2326658194314453,4.4717902765451916,5.8684909977946704,4.1699094419010692,4.4664969037444004,6.2266885002344523,4.2900791521022015,4.0513069108179742,5.1873723585514089,3.2819419334408249,4.7196129897309733,3.9893532476043831,4.4226896209220463,2.5974757898703773,5.7670301623264155,2.6017884724182725,4.5933890889502704,5.2991178280338591,4.9826612443139986,2.2041199826559246,5.416142697043469,2.9506082247842307,4.5648352996192942,3.2127201544178425,4.4644746434554561,5.3887669549130184,4.6384792730586861,3.8402592002021061,4.4654230069584191,4.135100197389721,3.2132520521963968,4.4036581640982106,2.6812412373755872,2.0606978403536118,5.2748662569248337,5.1887218566113926,6.1861142194078909,1.6020599913279623,5.4330173650934359,4.313107595194996,2.8620120512502165,2,4.1125379756093077,3.7909884750888159,1.1667260555800518,5.7549336270084162,3.287801729930226,4.9194382504422851,3.0899051114393981,3.356790460351716,4.0684085197781616,2.1760912590556813,4.7447497302049486,4.1787467965289578,2.710608102952619,3.4647875196459372,4.2022157758011316,2.0856829431946151,5.3442704183205363,2.2253092817258628,5.6293247851389783,3.5395904205339157,3.844216147843321,1.0413926851582251,1.9294189257142926,3.7084209001347128,3.1061908972634154,4.4139532291554149,1.6989700043360187,2.3299061234002103,5.767913275535089,6.0286035551931025,4.7290035198247784,3.9542425094393248,2.1072099696478683,4.0304256454923628,2.2013971243204513,3.7913137227582072,4.1218381236604849,3.729436138956145,4.0105458741106785,4.0539039708951661,1.6884198220027107,2.5576996443512146,3.5109000750153396,4.9844022725395707,3.9394568495030713,5.3305287649853836,2.4189148374041607,2.0492180226701815,1.2787536009528289,4.9727700721181538,6.0062195350443863,4.1757262983859329,1.971832279924925,2.6989700043360187,1.6989700043360187,2.1771900804896092,2.1303337684950061,2.7323937598229686,2.0791812460476247,3,4.196286748808876,1.6020599913279623,1.6020599913279623,6.1161229762050935,4.29287494299383,2.1139433523068369,3.6020599913279625,4.2158743387181401,2.5471837614500816,1.505149978319906,3.6255182289716377,2.7941393557677738,2.2041199826559246,3.5222890074407114,3.5766868052009957,2.8145805160103188,3.4689584577652681,2.8774865280696016,1.6989700043360187,3.2607866686549762,2.6232492903979003,4.0500788634833285,5.4661258704181996,3.8289175616166857,2.5563025007672873,4.3859182101733625,5.2556101584077384,2.687582464425827,4.204119982655925,2.9188163903603797,2.909374143715874,2.6020599913279625,2.826787238816292,2.0038911662369103,4.5944811683499527,4.3156725313539823,2.7428821714372731,3.4485517392015779,3.6290016192869916,1.6232492903979006,4.9613055041429064,3.1095785469043866,2.3789064000232618,3.1005083945019623,3.6016254795539449,2.638938294887355,3.0791812460476247,5.5264138507238521,4.0251009610468138,1.1760912590556813,4.6201568839458202,4.9099859925054599,4.676405508271646,3.2073542607973531,2.7074168686367091,2.325720858019412,1.919078092376074,1.7781512503836436,3.7547381082614368,3.0614524790871931,3.9368152311976328,1.3411601596967189,2.7967130632808965,3.782472624166286,1.9030899869919435,2.4353027522846102,1.7781512503836436,2.0507663112330423,1.146128035678238,1.3010299956639813,3.4939318217735464,4.170788418101762,5.2950002782862713,1.8587777373054493,5.6709345635958428,1.3010299956639813,3.8025000677643934,4.3369018086850204,4.0969100130080562,2.9777236052888476,2.8006071163924688,4.8237910311893311,3.1271047983648077,4.9542027455464295,2.2679925903655827,2.8250884183002145],"y":[6.5688919523484,6.4604701406839471,6.7716394108722255,4.7757031943756161,6.5477311605694499,7.3445386573461091,6.9052433497705339,5.8272873504085991,6.1576359584805243,6.1350241914799222,6.2323728520817738,3.7717079790447743,5.634012255665108,7.1274066768494784,3.6843102397223149,5.4882785908909248,6.8039917559697578,7.2993543007292034,7.5049343511973126,7.0695285535090653,5.048819219327604,7.3247328478917462,7.7883641680629729,6.4999708714133702,6.2302760403240462,5.6312599362807116,5.5820731514806869,6.321856041652083,7.4187206737574476,7.1397261868874287,6.5067197994730472,6.2450882732566546,4.8418875617308892,6.1375314015406781,7.1679383119219064,6.0968198267383711,4.4516855999764564,6.8503163131617013,6.8949067278677774,4.3882417381318808,4.5986519917760278,5.2400097891921718,3.5197177660831542,5.2216908461840275,3.1601742997562785,5.4554396036499915,6.3800635130356254,6.4544584434010588,6.9148312682869921,5.5771627948366147,5.0635944202049918,5.1330082980230616,4.5074746852312275,5.327119669307554,7.40883160407778,5.8617082909736826,7.6418148412592224,4.8232410430396238,7.1924950374752452,3.815127947639156,4.6182886187745762,5.502232449610565,5.3465794527796877,5.3002018103724424,7.7667360311871407,4.8186260785307908,5.0909813215118893,4.0764802384571386,8.0080819873537621,7.3196145498913712,6.8884389598789264,4.1199895437319283,4.9072332527703013,6.6054554002336214,3.3154497193600747,4.3608434300824799,3.7381214843438024,7.0483153320246821,3.90848501887865,4.2572481461680107,5.7994706321883838,5.784261753054154,5.0474345337590485,3.8725798675626004,3.6118507106822602,6.6339294235130861,5.0850685209794602,4.4470630966015001,5.1845460455678261,6.6289572375519548,6.7840462679965823,6.5913353519955891,5.3685374944238422,4.4727564493172123,6.1567096319513919,5.4262259809774767,6.0838430062433657,5.862879349954067,4.9002327223981421,5.7879122033256358,6.9256904488381803,6.2285794913292323,5.8002572868192974,6.774510631552082,5.4779275407634653,6.7029648301905365,5.4868414825458505,5.7668151862304109,5.2686636289576345,7.819872821950546,7.3766456073837192,6.739972792873048,5.3616855374517396,5.5466258500473611,5.0655923736866324,4.0049092083006013,6.1018341096580748,7.0316873678757341,4.8939509689556093,null,5.408061969891345,6.7960038458829892,6.8841583095593633,5.3085228174687931,5.0971056793891432,5.3442652681956631,5.9722913411688872,3.8124059380049546,3.5720230867438456,3.7235993184736782,3.801240390075832,4.4197377208751654,5.552155689619263,5.2259699664246506,6.0815925465111222,4.6353668133699344,5.5863301397155096,4.6598683965365444,5.5509925279820198,4.0470886456150916,4.5981118428941388,6.8525920521635637,5.9346356262342344,5.1017455621591461,7.1243732271458526,7.323671758811356,5.5323170242586155,7.1263672813883288,7.1116678946085381,5.1730655527476479,5.4852494780786696,5.983334742965992,7.2168550591956837,7.1740335817132781,4.7081202745270012,6.4860082783237063,7.0927292385782437,4.4490877442027479,6.4349956074154271,4.8198248858187744,7.0960877420483275,4.8574176580543664,3.8531108481320895,4.5609886194818108,3.6842167951388807,6.0543328327418271,5.4937582029790724,5.5585077408889356,5.5802259059525321,3.3713745322010444,4.4580396970325129,4.913401341591757,3.9628426812012423,3.5610417237160132,4.4136654905707937,3.6987293385224569,3.6781795518339857,7.0986933841193638,5.7465047162964922,4.1379535430124097,3.9718897371949677,4.6917426905820001,5.8559958788866977,5.5117349405366287,4.1193779761877272,3.5708183726778508,5.5182642804090385,5.3557305896504479,4.3818944857620172,4.6037293554415095,6.9048191427129249,4.1644489253755248,4.4641112752965482,4.5878016299579336,3.8752708322897389,5.11682254709637,5.7345092829903495,4.5215820928189556,6.2339214891782326,3.5999779954176843,5.0133757110388455,6.3926964679748721,5.9206693535394539,5.8091438789657852,4.0108767948608639,5.3399591794907808,3.8494433699746264,4.0737183503461223,4.013914455846586,5.3784632037523954,5.8803089461581717,6.3313102824322991,3.6874147989961834,6.0326165853441553,4.1689362704543882,4.4988561811014973,5.2108333204916137,4.6834662633889961,4.6072074584292029,6.2443003023524133,4.6279213775994306,4.4844564419054294,4.1139433523068369,5.2811844799821337,4.5185092020940605,4.5210701871812304,4.1256422663922292,4.926299786932737,5.2848817146554525,6.1330310873925402,4.0791812460476251,5.6339000307857878,5.2001429080571606,3.9467916091972901,5.5281070919144648,5.7070687790680239,4.7614099830157182,4.1084970330598569,4.4589471076189442,7.606007622399213,4.2578273132517888,4.0672174568569686,5.2800909040719084,4.9007322741291341,4.8443466192559903,4.1666739844552794,5.2095205102096358,5.6279139916926129,5.460668086797356,6.4896635622279923,6.4943636857010194,4.0228125909738175,5.9431230947011402,6.1297097152924902,5.9370204604293093,4.3052651469835448,6.3930746322224365,3.9353515569460962,5.4223682209113662,4.3592395782103628,4.506587959850652,4.9439505823477052,5.5766316429606348,4.7148531966181677,5.2673117599266597,4.8223160674664394,3.6064469343274004,4.8640320250727047,3.9869821146488182,5.1208756939100448,4.8715836176578113,4.0334124958544226,6.1649323829653788,4.3214378961618864,5.4745535419964293,3.6961579835661427,4.0879485038836672,4.5579203670399142,6.1194873468094526,5.5336260915984878,4.0687345078148729,7.7837283781160549,4.0694849443545378,5.1028475290053352,5.5078910993722285,5.4916460075976641,7.1659110338185048,6.790344879994036,7.1340687895718178,6.8378461609748005,5.511593716942377,5.5179962814768535,5.1419478132010834,4.7587432868327415,3.1732649589406052,4.103463333259505,4.6185710281201295,7.4638244729554009,5.5794800329940824,7.051601499408986,5.9873685326641795,4.6164229962656824,7.5958707300105068,4.4755304192410623,5.1349993200051491,5.3471961734382329,6.2687344067161144,6.6117877359644712,6.80526658316158,7.2085830613982269,7.0114716487615398,5.4334132621843452,7.2010939648407293,6.8634427498975201,4.784830607903209,6.8713034743951846,6.5343343000433709,6.6288562818332899,4.8487037435328721,5.9532779036067067,6.7814462300277087,7.1217127522788619,6.4328449270275794,4.5505207611034422,3.6003182396537019,5.053241023663599,3.7422009916027443,6.63589982774434,7.5080322813481395,4.0538610053975894,5.9101074095677433,6.7193852745543659,7.3394714943166379,6.4021966273300004,4.9760214052070202,6.8644624854148768,6.2358745704197833,6.5806507991751744,6.5774902177615884,5.9469314832458213,5.7634342608363598,6.1012156555856665,6.1579047005518381,6.2108674629531064,4.9084850188786495,5.9701783940224304,4.8137360660495405,5.2233093073972423,5.6929563159940688,4.776931052296149,6.3567906322360415,6.6720097297203047,5.0214270619047161,4.0841861313198455,6.9538762520331794,5.6404151746774742,6.7145692054545654,4.9019434030825844,6.1117472813684044,6.5235652970227846,5.9039102672783468,7.1338417231769533,5.781885241586485,5.8942882758118431,6.3784725423047357,5.3135217978417426,5.4951443249567786,7.1417126668064519,7.4619096724590035,5.1183212170820784,4.4102135498336335,5.6933860962344154,4.9428608643872387,6.5102930034123423,5.8634715656455869,5.2566879262850188,5.4601436196700934,3.6334684555795866,6.0630321461817509,5.4147889006899996,7.0302823245484474,6.6406049051407736,5.5363621071927254,4.4313637641589869,3.4071579717844847,5.8173003690924574,5.1393493963038273,4.8553366773119926,7.3255908035591784,6.6540666601411402,5.6262140750699574,4.183633644280671,4.796320967121261,6.9226698621468481,4.4533573330475136,4.8274305998937699,6.3594791692221202,4.628376360875186,6.8045326534790584,6.1188613965140908,5.335829670111659,6.6578216410612487,3.7711934242561349,7.216871984783082,5.6091569387189741,6.0430115487780958,6.479628131969557,5.8100761841840338,8.0629745055257089,6.7660842340745706,6.0534683686008908,8.0018395758823715,7.3237697600185427,6.4903584673197878,7.1195575076376212,5.8306840297962461,7.2124811471304335,5.9038494139654381,6.3308046491372156,5.5102357602011862,7.3962418692459595,6.5451776056376829,6.8615204463046675,7.7514270639279017,7.7176311914990361,5.1578300948393156,7.1778593071671768,5.4117065225153711,7.0803231189973888,5.8894785840432675,5.716057919329355,6.6127636191846202,6.5754143755183518,5.6293925736097341,6.1869807207830387,5.7889238952518367,5.3827840029756402,6.1728773489598723,5.2677453693155343,4.3966980632546919,6.2200605066417811,6.8580928290117642,6.5885433277201484,3.5835070929572823,3.7493358382003681,4.053740142141546,5.0558914208236567,4.486146996806573,4.959595027236654,3.9169074835848621,4.0787365904735511,5.1574655325134131,4.144667594231179,6.6723755272699137,5.5379277558050939,4.4889019581435639,5.7900115004606691,4.5021790369467034,5.4390081309152905,6.8175032504300628,5.041141313845162,5.1662053297496389,5.3861293815776365,4.6343246860868312,6.7090353268425877,3.9854615036625809,3.8149098558834615,4.9237492268586367,5.3561462759051581,5.4662913013919932,5.5475620581904002,4.057792722982879,6.6596523967791921,4.9116043025964364,3.7389564596728158,5.3035836380935306,7.625963020785318,8.0299593081950409,7.2018568143746524,6.6908053770551836,5.3707939116343759,6.9369717549472245,5.4379138205161857,7.1434312570163909,6.2287153466544591,6.6250935072467696,6.4589166475731927,5.8479372332444433,5.0514097416139254,5.1221994894925871,5.8643491337811442,7.4190049556494202,6.9784368371452166,7.2130027881382635,4.2011501344703666,4.1462269815057144,3.6002942568063161,5.4423917253566163,7.7780728685019387,5.3859510523104221,4.2537851565336133,4.1753900314824133,4.033241152571625,3.9608045755389045,5.9412091598756627,4.4699380086400833,4.8107109866840432,3.7304883412918475,5.0437781761636158,3.5675509688042939,4.9630698457882128,7.267053686616082,5.0461198232605273,4.9909424508445497,7.3102032430869075,6.1624582034599982,4.9154839419404901,5.1312034805781792,5.9680405180917724,4.6574064949384679,4.3474032048486633,5.4563810054009965,5.0432373954913183,5.0977767636603142,5.3666367082557818,5.3420285377073284,3.8699225199015808,6.0876562491612907,5.3833098007071269,5.4837044051942021,7.4706724936529731,6.235710951110808,4.9125798698065006,4.8114182160295558,5.5700508226671417,4.725191510863529,5.8274834283816146,6.0926832720008672,5.8732371011105711,4.7947922250895996,5.9362008674062299,5.456422594024243,6.3539062818292287,6.6589227054839863,5.0906833896700299,5.9678918125346234,6.4867542100741593,5.2189127626485865,6.8846291796953949,5.4190074234724728,5.6215847212790875,5.4793004387413724,6.1533700836875322,4.722245515956021,4.6987979896600311,7.3163438526082585,5.5805699173740617,5.1261336810491311,6.1876256184854963,6.1565659413043488,6.0264646013461949,5.0426967672253751,4.8946939117291466,6.4149421995876663,5.227227232959522,6.6774951284809339,6.8259382591473257,4.0970166626367739,4.0821762131864396,3.8216615432285819,4.1553600292947133,4.9983090439837126,5.0609515452712523,4.8096362649156346,3.9183973388437003,5.3148602811808203,3.9391067265930175,4.385050807967529,5.6993752551589845,5.8875061359754639,7.4724229388623682,4.5690741312770164,7.0328534843378563,4,5.6204688448803939,6.6371520924480896,5.1880430546284932,5.379107365362084,5.0586160626340169,6.7866496306089221,5.7677478623620946,6.2721723618559135,4.4335957613288697,4.1347347635963985],"text":["Quantity: 5.040000e+02<br />Predicted Cost = $47,288<br />Gross_Cost:   3705885.12","Quantity: 1.579100e+04<br />Predicted Cost = $1,144,271<br />Gross_Cost:   2887155.27","Quantity: 1.429740e+04<br />Predicted Cost = $1,043,789<br />Gross_Cost:   5910706.72","Quantity: 1.400000e+02<br />Predicted Cost = $14,460<br />Gross_Cost:     59662.74","Quantity: 1.135205e+05<br />Predicted Cost = $7,094,858<br />Gross_Cost:   3529646.08","Quantity: 4.353217e+05<br />Predicted Cost = $24,597,984<br />Gross_Cost:  22107450.30","Quantity: 1.303454e+05<br />Predicted Cost = $8,062,385<br />Gross_Cost:   8039764.91","Quantity: 2.136282e+05<br />Predicted Cost = $12,733,101<br />Gross_Cost:    671873.25","Quantity: 1.647947e+05<br />Predicted Cost = $10,015,497<br />Gross_Cost:   1437593.03","Quantity: 2.211878e+05<br />Predicted Cost = $13,149,344<br />Gross_Cost:   1364659.15","Quantity: 4.198133e+04<br />Predicted Cost = $2,827,008<br />Gross_Cost:   1707547.73","Quantity: 5.667340e+04<br />Predicted Cost = $3,731,435<br />Gross_Cost:      5911.64","Quantity: 4.670000e+02<br />Predicted Cost = $44,067<br />Gross_Cost:    430538.76","Quantity: 5.263260e+04<br />Predicted Cost = $3,484,662<br />Gross_Cost:  13409317.59","Quantity: 2.200000e+03<br />Predicted Cost = $184,817<br />Gross_Cost:      4834.04","Quantity: 4.844200e+02<br />Predicted Cost = $45,586<br />Gross_Cost:    307807.07","Quantity: 4.258000e+03<br />Predicted Cost = $340,420<br />Gross_Cost:   6367834.33","Quantity: 3.560657e+04<br />Predicted Cost = $2,427,534<br />Gross_Cost:  19922980.08","Quantity: 1.301668e+05<br />Predicted Cost = $8,052,166<br />Gross_Cost:  31984115.95","Quantity: 8.925600e+03<br />Predicted Cost = $675,056<br />Gross_Cost:  11736228.42","Quantity: 1.316000e+02<br />Predicted Cost = $13,656<br />Gross_Cost:    111897.20","Quantity: 1.263430e+06<br />Predicted Cost = $65,907,480<br />Gross_Cost:  21121893.47","Quantity: 8.578856e+05<br />Predicted Cost = $46,070,422<br />Gross_Cost:  61427687.76","Quantity: 2.345950e+03<br />Predicted Cost = $196,130<br />Gross_Cost:   3162065.57","Quantity: 1.455700e+03<br />Predicted Cost = $126,137<br />Gross_Cost:   1699323.41","Quantity: 4.963960e+03<br />Predicted Cost = $392,321<br />Gross_Cost:    427818.87","Quantity: 1.861718e+04<br />Predicted Cost = $1,332,510<br />Gross_Cost:    382008.61","Quantity: 1.612295e+04<br />Predicted Cost = $1,166,504<br />Gross_Cost:   2098244.25","Quantity: 1.243350e+06<br />Predicted Cost = $64,937,998<br />Gross_Cost:  26225312.61","Quantity: 6.649810e+05<br />Predicted Cost = $36,399,764<br />Gross_Cost:  13795142.37","Quantity: 3.044940e+04<br />Predicted Cost = $2,100,440<br />Gross_Cost:   3211587.80","Quantity: 1.446476e+04<br />Predicted Cost = $1,055,086<br />Gross_Cost:   1758280.96","Quantity: 2.750000e+01<br />Predicted Cost = $3,209<br />Gross_Cost:     69484.44","Quantity: 1.974345e+04<br />Predicted Cost = $1,406,910<br />Gross_Cost:   1372560.20","Quantity: 9.080085e+04<br />Predicted Cost = $5,770,762<br />Gross_Cost:  14721033.87","Quantity: 5.001874e+06<br />Predicted Cost = $235,341,107<br />Gross_Cost:   1249740.45","Quantity: 5.210000e+01<br />Predicted Cost = $5,795<br />Gross_Cost:     28293.43","Quantity: 1.281275e+04<br />Predicted Cost = $943,125<br />Gross_Cost:   7084615.96","Quantity: 6.964770e+03<br />Predicted Cost = $536,647<br />Gross_Cost:   7850670.10","Quantity: 7.190000e+01<br />Predicted Cost = $7,807<br />Gross_Cost:     24447.91","Quantity: 1.950000e+02<br />Predicted Cost = $19,646<br />Gross_Cost:     39687.34","Quantity: 1.100000e+02<br />Predicted Cost = $11,569<br />Gross_Cost:    173784.00","Quantity: 1.754891e+05<br />Predicted Cost = $10,615,278<br />Gross_Cost:      3309.16","Quantity: 1.323306e+05<br />Predicted Cost = $8,175,901<br />Gross_Cost:    166606.08","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />Gross_Cost:      1446.02","Quantity: 8.880000e+02<br />Predicted Cost = $79,851<br />Gross_Cost:    285390.56","Quantity: 3.465800e+03<br />Predicted Cost = $281,396<br />Gross_Cost:   2399183.76","Quantity: 1.187878e+05<br />Predicted Cost = $7,398,847<br />Gross_Cost:   2847465.32","Quantity: 1.750138e+05<br />Predicted Cost = $10,588,681<br />Gross_Cost:   8219232.55","Quantity: 6.056420e+05<br />Predicted Cost = $33,384,877<br />Gross_Cost:    377713.75","Quantity: 5.000000e+03<br />Predicted Cost = $394,955<br />Gross_Cost:    115769.57","Quantity: 1.558000e+04<br />Predicted Cost = $1,130,121<br />Gross_Cost:    135833.94","Quantity: 1.720000e+01<br />Predicted Cost = $2,079<br />Gross_Cost:     32171.75","Quantity: 3.083200e+03<br />Predicted Cost = $252,538<br />Gross_Cost:    212382.96","Quantity: 7.932393e+05<br />Predicted Cost = $42,849,811<br />Gross_Cost:  25634898.61","Quantity: 7.928390e+04<br />Predicted Cost = $5,090,332<br />Gross_Cost:    727291.13","Quantity: 1.439547e+06<br />Predicted Cost = $74,363,366<br />Gross_Cost:  43834377.28","Quantity: 7.638000e+03<br />Predicted Cost = $584,462<br />Gross_Cost:     66564.25","Quantity: 1.105035e+06<br />Predicted Cost = $58,226,781<br />Gross_Cost:  15577402.35","Quantity: 2.700000e+02<br />Predicted Cost = $26,547<br />Gross_Cost:      6533.23","Quantity: 1.603400e+03<br />Predicted Cost = $137,932<br />Gross_Cost:     41522.99","Quantity: 3.274200e+03<br />Predicted Cost = $266,976<br />Gross_Cost:    317857.49","Quantity: 3.736000e+03<br />Predicted Cost = $301,631<br />Gross_Cost:    222115.80","Quantity: 8.824000e+03<br />Predicted Cost = $667,945<br />Gross_Cost:    199618.97","Quantity: 1.711566e+06<br />Predicted Cost = $87,274,858<br />Gross_Cost:  58443475.05","Quantity: 3.271700e+03<br />Predicted Cost = $266,787<br />Gross_Cost:     65860.66","Quantity: 7.000000e+01<br />Predicted Cost = $7,616<br />Gross_Cost:    123305.18","Quantity: 8.800000e+02<br />Predicted Cost = $79,186<br />Gross_Cost:     11925.60","Quantity: 1.297303e+06<br />Predicted Cost = $67,540,345<br />Gross_Cost: 101878369.88","Quantity: 6.116253e+06<br />Predicted Cost = $283,464,732<br />Gross_Cost:  20874426.37","Quantity: 2.371645e+04<br />Predicted Cost = $1,666,944<br />Gross_Cost:   7734619.61","Quantity: 2.692136e+06<br />Predicted Cost = $132,690,466<br />Gross_Cost:     13182.25","Quantity: 1.158000e+03<br />Predicted Cost = $102,078<br />Gross_Cost:     80766.87","Quantity: 1.860052e+05<br />Predicted Cost = $11,202,390<br />Gross_Cost:   4031395.44","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />Gross_Cost:      2067.52","Quantity: 1.972746e+01<br />Predicted Cost = $2,360<br />Gross_Cost:     22953.21","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />Gross_Cost:      5471.69","Quantity: 2.311582e+04<br />Predicted Cost = $1,627,856<br />Gross_Cost:  11176744.73","Quantity: 1.598148e+02<br />Predicted Cost = $16,344<br />Gross_Cost:      8100.00","Quantity: 1.230600e+02<br />Predicted Cost = $12,834<br />Gross_Cost:     18082.07","Quantity: 1.266960e+04<br />Predicted Cost = $933,374<br />Gross_Cost:    630188.73","Quantity: 5.396000e+03<br />Predicted Cost = $423,805<br />Gross_Cost:    608501.64","Quantity: 1.800000e+02<br />Predicted Cost = $18,244<br />Gross_Cost:    111541.00","Quantity: 3.500000e+01<br />Predicted Cost = $4,011<br />Gross_Cost:      7457.27","Quantity: 2.280000e+02<br />Predicted Cost = $22,703<br />Gross_Cost:      4091.20","Quantity: 1.441217e+04<br />Predicted Cost = $1,051,537<br />Gross_Cost:   4304566.52","Quantity: 1.737360e+03<br />Predicted Cost = $148,559<br />Gross_Cost:    121637.79","Quantity: 7.000000e+01<br />Predicted Cost = $7,616<br />Gross_Cost:     27993.88","Quantity: 2.349000e+03<br />Predicted Cost = $196,366<br />Gross_Cost:    152948.79","Quantity: 7.867000e+03<br />Predicted Cost = $600,653<br />Gross_Cost:   4255565.09","Quantity: 1.123299e+05<br />Predicted Cost = $7,026,000<br />Gross_Cost:   6081997.93","Quantity: 4.015700e+03<br />Predicted Cost = $322,462<br />Gross_Cost:   3902432.07","Quantity: 1.237900e+03<br />Predicted Cost = $108,576<br />Gross_Cost:    233634.78","Quantity: 3.500000e+02<br />Predicted Cost = $33,749<br />Gross_Cost:     29700.00","Quantity: 3.230785e+04<br />Predicted Cost = $2,218,758<br />Gross_Cost:   1434529.99","Quantity: 5.347060e+03<br />Predicted Cost = $420,249<br />Gross_Cost:    266824.67","Quantity: 9.680060e+02<br />Predicted Cost = $86,484<br />Gross_Cost:   1212950.30","Quantity: 2.246476e+05<br />Predicted Cost = $13,339,487<br />Gross_Cost:    729254.89","Quantity: 1.572450e+03<br />Predicted Cost = $135,467<br />Gross_Cost:     79475.40","Quantity: 7.337740e+03<br />Predicted Cost = $563,177<br />Gross_Cost:    613637.94","Quantity: 4.862000e+03<br />Predicted Cost = $384,861<br />Gross_Cost:   8427338.70","Quantity: 2.060524e+04<br />Predicted Cost = $1,463,623<br />Gross_Cost:   1692698.04","Quantity: 5.279580e+03<br />Predicted Cost = $415,341<br />Gross_Cost:    631331.25","Quantity: 1.064080e+06<br />Predicted Cost = $56,227,818<br />Gross_Cost:   5949913.22","Quantity: 2.800000e+04<br />Predicted Cost = $1,943,664<br />Gross_Cost:    300557.48","Quantity: 7.605860e+03<br />Predicted Cost = $582,187<br />Gross_Cost:   5046204.31","Quantity: 3.200000e+02<br />Predicted Cost = $31,064<br />Gross_Cost:    306790.20","Quantity: 2.499960e+05<br />Predicted Cost = $14,726,111<br />Gross_Cost:    584541.28","Quantity: 4.628601e+01<br />Predicted Cost = $5,194<br />Gross_Cost:    185636.61","Quantity: 9.840000e+02<br />Predicted Cost = $87,805<br />Gross_Cost:  66050000.00","Quantity: 2.871600e+04<br />Predicted Cost = $1,989,595<br />Gross_Cost:  23803762.44","Quantity: 1.773000e+03<br />Predicted Cost = $151,375<br />Gross_Cost:   5495064.48","Quantity: 3.635760e+04<br />Predicted Cost = $2,474,860<br />Gross_Cost:    229977.60","Quantity: 2.986000e+03<br />Predicted Cost = $245,165<br />Gross_Cost:    352067.43","Quantity: 4.480000e+04<br />Predicted Cost = $3,002,148<br />Gross_Cost:    116303.39","Quantity: 1.092592e+04<br />Predicted Cost = $813,905<br />Gross_Cost:     10113.68","Quantity: 2.760000e+03<br />Predicted Cost = $227,951<br />Gross_Cost:   1264253.34","Quantity: 4.307620e+05<br />Predicted Cost = $24,359,568<br />Gross_Cost:  10756905.86","Quantity: 2.053206e+05<br />Predicted Cost = $12,274,395<br />Gross_Cost:     78334.12","Quantity: 9.500000e+01<br />Predicted Cost = $10,102<br />Gross_Cost:         0.00","Quantity: 6.533300e+03<br />Predicted Cost = $505,822<br />Gross_Cost:    255895.10","Quantity: 1.231300e+02<br />Predicted Cost = $12,841<br />Gross_Cost:   6251782.29","Quantity: 4.982200e+03<br />Predicted Cost = $393,654<br />Gross_Cost:   7658757.34","Quantity: 2.000000e+05<br />Predicted Cost = $11,979,888<br />Gross_Cost:    203480.51","Quantity: 7.968000e+02<br />Predicted Cost = $72,235<br />Gross_Cost:    125056.33","Quantity: 4.762900e+03<br />Predicted Cost = $377,599<br />Gross_Cost:    220935.38","Quantity: 3.730000e+02<br />Predicted Cost = $35,796<br />Gross_Cost:    938191.17","Quantity: 3.000000e+02<br />Predicted Cost = $29,264<br />Gross_Cost:      6492.41","Quantity: 9.520000e+02<br />Predicted Cost = $85,161<br />Gross_Cost:      3732.70","Quantity: 6.130000e+02<br />Predicted Cost = $56,676<br />Gross_Cost:      5291.75","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />Gross_Cost:      6327.62","Quantity: 3.900000e+02<br />Predicted Cost = $37,302<br />Gross_Cost:     26286.80","Quantity: 3.800370e+04<br />Predicted Cost = $2,578,333<br />Gross_Cost:    356578.94","Quantity: 2.733750e+03<br />Predicted Cost = $225,945<br />Gross_Cost:    168255.77","Quantity: 9.301000e+03<br />Predicted Cost = $701,278<br />Gross_Cost:   1206681.20","Quantity: 1.209226e+04<br />Predicted Cost = $893,963<br />Gross_Cost:     43188.37","Quantity: 1.344000e+03<br />Predicted Cost = $117,157<br />Gross_Cost:    385771.50","Quantity: 9.625000e+01<br />Predicted Cost = $10,225<br />Gross_Cost:     45694.97","Quantity: 9.508000e+04<br />Predicted Cost = $6,021,885<br />Gross_Cost:    355625.20","Quantity: 6.930000e+02<br />Predicted Cost = $63,486<br />Gross_Cost:     11145.22","Quantity: 1.117750e+04<br />Predicted Cost = $831,225<br />Gross_Cost:     39638.01","Quantity: 3.164186e+04<br />Predicted Cost = $2,176,418<br />Gross_Cost:   7121837.37","Quantity: 9.206397e+03<br />Predicted Cost = $694,677<br />Gross_Cost:    860271.68","Quantity: 2.798560e+04<br />Predicted Cost = $1,942,739<br />Gross_Cost:    126399.56","Quantity: 3.588177e+05<br />Predicted Cost = $20,571,136<br />Gross_Cost:  13315982.85","Quantity: 4.328141e+05<br />Predicted Cost = $24,466,891<br />Gross_Cost:  21070350.44","Quantity: 2.375821e+05<br />Predicted Cost = $14,048,429<br />Gross_Cost:    340656.77","Quantity: 8.857768e+04<br />Predicted Cost = $5,639,946<br />Gross_Cost:  13377263.49","Quantity: 1.304293e+05<br />Predicted Cost = $8,067,185<br />Gross_Cost:  12932065.47","Quantity: 3.223786e+05<br />Predicted Cost = $18,631,110<br />Gross_Cost:    148958.59","Quantity: 3.340000e+02<br />Predicted Cost = $32,320<br />Gross_Cost:    305667.65","Quantity: 1.269900e+03<br />Predicted Cost = $111,170<br />Gross_Cost:    962353.75","Quantity: 5.290670e+04<br />Predicted Cost = $3,501,445<br />Gross_Cost:  16476124.28","Quantity: 3.615121e+05<br />Predicted Cost = $20,713,983<br />Gross_Cost:  14929098.44","Quantity: 1.284000e+03<br />Predicted Cost = $112,311<br />Gross_Cost:     51064.64","Quantity: 3.502578e+04<br />Predicted Cost = $2,390,885<br />Gross_Cost:   3062021.80","Quantity: 1.804918e+05<br />Predicted Cost = $10,894,901<br />Gross_Cost:  12380244.98","Quantity: 7.353600e+02<br />Predicted Cost = $67,068<br />Gross_Cost:     28124.69","Quantity: 4.252400e+04<br />Predicted Cost = $2,860,794<br />Gross_Cost:   2722673.77","Quantity: 2.400000e+02<br />Predicted Cost = $23,806<br />Gross_Cost:     66042.71","Quantity: 2.431282e+05<br />Predicted Cost = $14,351,512<br />Gross_Cost:  12476355.53","Quantity: 4.823320e+03<br />Predicted Cost = $382,028<br />Gross_Cost:     72014.12","Quantity: 1.800000e+02<br />Predicted Cost = $18,244<br />Gross_Cost:      7130.35","Quantity: 1.970060e+04<br />Predicted Cost = $1,404,085<br />Gross_Cost:     36390.55","Quantity: 1.464000e+03<br />Predicted Cost = $126,802<br />Gross_Cost:      4833.00","Quantity: 2.318340e+04<br />Predicted Cost = $1,632,258<br />Gross_Cost:   1133268.54","Quantity: 8.890000e+02<br />Predicted Cost = $79,934<br />Gross_Cost:    311715.36","Quantity: 8.050788e+05<br />Predicted Cost = $43,441,070<br />Gross_Cost:    361832.64","Quantity: 1.033740e+04<br />Predicted Cost = $773,269<br />Gross_Cost:    380387.21","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />Gross_Cost:      2351.66","Quantity: 9.000000e+01<br />Predicted Cost = $9,609<br />Gross_Cost:     28710.43","Quantity: 3.497000e+02<br />Predicted Cost = $33,722<br />Gross_Cost:     81922.15","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />Gross_Cost:      9180.00","Quantity: 1.767480e+03<br />Predicted Cost = $150,940<br />Gross_Cost:      3639.50","Quantity: 1.745324e+04<br />Predicted Cost = $1,255,265<br />Gross_Cost:     25921.82","Quantity: 2.300000e+02<br />Predicted Cost = $22,887<br />Gross_Cost:      4997.23","Quantity: 8.576000e+02<br />Predicted Cost = $77,319<br />Gross_Cost:      4766.28","Quantity: 3.364800e+02<br />Predicted Cost = $32,541<br />Gross_Cost:  12551435.08","Quantity: 7.816600e+04<br />Predicted Cost = $5,023,906<br />Gross_Cost:    557833.66","Quantity: 2.800000e+01<br />Predicted Cost = $3,263<br />Gross_Cost:     13738.95","Quantity: 1.800000e+01<br />Predicted Cost = $2,168<br />Gross_Cost:      9373.24","Quantity: 2.261666e+04<br />Predicted Cost = $1,595,315<br />Gross_Cost:     49174.81","Quantity: 1.010320e+05<br />Predicted Cost = $6,369,781<br />Gross_Cost:    717787.48","Quantity: 5.230000e+03<br />Predicted Cost = $411,731<br />Gross_Cost:    324888.95","Quantity: 7.200000e+02<br />Predicted Cost = $65,771<br />Gross_Cost:     13163.70","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />Gross_Cost:      3722.36","Quantity: 3.992000e+01<br />Predicted Cost = $4,530<br />Gross_Cost:    329810.35","Quantity: 2.740000e+03<br />Predicted Cost = $226,422<br />Gross_Cost:    226845.72","Quantity: 7.230000e+02<br />Predicted Cost = $66,024<br />Gross_Cost:     24093.20","Quantity: 2.400000e+03<br />Predicted Cost = $200,307<br />Gross_Cost:     40154.05","Quantity: 2.314703e+03<br />Predicted Cost = $193,713<br />Gross_Cost:   8031915.72","Quantity: 1.093400e+04<br />Predicted Cost = $814,462<br />Gross_Cost:     14603.23","Quantity: 4.920000e+02<br />Predicted Cost = $46,245<br />Gross_Cost:     29114.63","Quantity: 9.600000e+01<br />Predicted Cost = $10,200<br />Gross_Cost:     38708.08","Quantity: 3.000000e+01<br />Predicted Cost = $3,478<br />Gross_Cost:      7503.62","Quantity: 7.680000e+02<br />Predicted Cost = $69,817<br />Gross_Cost:    130864.71","Quantity: 1.463560e+03<br />Predicted Cost = $126,767<br />Gross_Cost:    542636.85","Quantity: 3.240000e+02<br />Predicted Cost = $31,423<br />Gross_Cost:     33233.96","Quantity: 1.475522e+04<br />Predicted Cost = $1,074,669<br />Gross_Cost:   1713647.49","Quantity: 1.999800e+04<br />Predicted Cost = $1,423,681<br />Gross_Cost:      3980.87","Quantity: 1.022700e+03<br />Predicted Cost = $90,995<br />Gross_Cost:    103127.79","Quantity: 2.822000e+03<br />Predicted Cost = $232,683<br />Gross_Cost:   2469997.24","Quantity: 4.647900e+04<br />Predicted Cost = $3,106,079<br />Gross_Cost:    833046.71","Quantity: 1.625000e+03<br />Predicted Cost = $139,649<br />Gross_Cost:    644382.71","Quantity: 2.612800e+04<br />Predicted Cost = $1,823,153<br />Gross_Cost:     10253.61","Quantity: 2.027750e+03<br />Predicted Cost = $171,391<br />Gross_Cost:    218755.60","Quantity: 4.980000e+01<br />Predicted Cost = $5,558<br />Gross_Cost:      7070.39","Quantity: 1.170000e+02<br />Predicted Cost = $12,248<br />Gross_Cost:     11850.00","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />Gross_Cost:     10325.58","Quantity: 1.610000e+03<br />Predicted Cost = $138,457<br />Gross_Cost:    239035.94","Quantity: 4.321300e+03<br />Predicted Cost = $345,099<br />Gross_Cost:    759117.40","Quantity: 8.078800e+03<br />Predicted Cost = $615,596<br />Gross_Cost:   2144422.14","Quantity: 4.940000e+04<br />Predicted Cost = $3,286,226<br />Gross_Cost:      4868.72","Quantity: 6.880000e+01<br />Predicted Cost = $7,495<br />Gross_Cost:   1077994.60","Quantity: 1.199908e+04<br />Predicted Cost = $887,589<br />Gross_Cost:     14754.90","Quantity: 1.518520e+01<br />Predicted Cost = $1,853<br />Gross_Cost:     31539.60","Quantity: 6.131000e+03<br />Predicted Cost = $476,943<br />Gross_Cost:    162492.50","Quantity: 3.670000e+02<br />Predicted Cost = $35,263<br />Gross_Cost:     48246.55","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />Gross_Cost:     40476.92","Quantity: 3.000406e+06<br />Predicted Cost = $146,686,943<br />Gross_Cost:   1755093.68","Quantity: 2.581000e+03<br />Predicted Cost = $214,242<br />Gross_Cost:     42454.27","Quantity: 3.940000e+01<br />Predicted Cost = $4,475<br />Gross_Cost:     30511.00","Quantity: 5.000000e+02<br />Predicted Cost = $46,940<br />Gross_Cost:     13000.00","Quantity: 2.040000e+02<br />Predicted Cost = $20,484<br />Gross_Cost:    191066.47","Quantity: 2.160000e+03<br />Predicted Cost = $181,706<br />Gross_Cost:     32999.64","Quantity: 8.162275e+01<br />Predicted Cost = $8,779<br />Gross_Cost:     33194.81","Quantity: 2.440000e+02<br />Predicted Cost = $24,173<br />Gross_Cost:     13354.95","Quantity: 2.000000e+02<br />Predicted Cost = $20,112<br />Gross_Cost:     84391.71","Quantity: 7.666666e+02<br />Predicted Cost = $69,705<br />Gross_Cost:    192700.00","Quantity: 8.800000e+02<br />Predicted Cost = $79,186<br />Gross_Cost:   1358410.68","Quantity: 6.920000e+02<br />Predicted Cost = $63,401<br />Gross_Cost:     12000.00","Quantity: 1.764000e+03<br />Predicted Cost = $150,665<br />Gross_Cost:    430427.52","Quantity: 3.029140e+04<br />Predicted Cost = $2,090,357<br />Gross_Cost:    158541.48","Quantity: 2.724560e+03<br />Predicted Cost = $225,242<br />Gross_Cost:      8846.91","Quantity: 5.356880e+03<br />Predicted Cost = $420,963<br />Gross_Cost:    337370.49","Quantity: 8.864000e+03<br />Predicted Cost = $670,745<br />Gross_Cost:    509411.54","Quantity: 1.006000e+03<br />Predicted Cost = $89,620<br />Gross_Cost:     57731.12","Quantity: 3.290060e+02<br />Predicted Cost = $31,872<br />Gross_Cost:     12837.99","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />Gross_Cost:     28770.48","Quantity: 5.708050e+03<br />Predicted Cost = $446,428<br />Gross_Cost:  40365247.75","Quantity: 1.146000e+03<br />Predicted Cost = $101,099<br />Gross_Cost:     18106.20","Quantity: 2.920000e+03<br />Predicted Cost = $240,148<br />Gross_Cost:     11673.94","Quantity: 2.621230e+03<br />Predicted Cost = $217,329<br />Gross_Cost:    190585.96","Quantity: 2.690000e+02<br />Predicted Cost = $26,456<br />Gross_Cost:     79566.87","Quantity: 7.116400e+02<br />Predicted Cost = $65,064<br />Gross_Cost:     69878.99","Quantity: 1.578900e+04<br />Predicted Cost = $1,144,137<br />Gross_Cost:     14678.24","Quantity: 3.329200e+04<br />Predicted Cost = $2,281,205<br />Gross_Cost:    162002.05","Quantity: 7.320000e+04<br />Predicted Cost = $4,727,949<br />Gross_Cost:    424535.48","Quantity: 4.965800e+03<br />Predicted Cost = $392,455<br />Gross_Cost:    288847.15","Quantity: 9.388458e+04<br />Predicted Cost = $5,951,819<br />Gross_Cost:   3087902.38","Quantity: 4.666667e+03<br />Predicted Cost = $370,537<br />Gross_Cost:   3121502.49","Quantity: 7.128000e+03<br />Predicted Cost = $548,271<br />Gross_Cost:     10539.32","Quantity: 9.887500e+02<br />Predicted Cost = $88,197<br />Gross_Cost:    877249.43","Quantity: 3.384440e+03<br />Predicted Cost = $275,280<br />Gross_Cost:   1348061.53","Quantity: 1.400000e+04<br />Predicted Cost = $1,023,690<br />Gross_Cost:    865008.67","Quantity: 1.232000e+02<br />Predicted Cost = $12,847<br />Gross_Cost:     20195.99","Quantity: 6.668112e+04<br />Predicted Cost = $4,337,132<br />Gross_Cost:   2472148.94","Quantity: 6.840000e+02<br />Predicted Cost = $62,723<br />Gross_Cost:      8616.91","Quantity: 3.467000e+03<br />Predicted Cost = $281,486<br />Gross_Cost:    264465.01","Quantity: 8.850000e+02<br />Predicted Cost = $79,602<br />Gross_Cost:     22868.60","Quantity: 5.700000e+02<br />Predicted Cost = $52,989<br />Gross_Cost:     32106.13","Quantity: 4.500000e+03<br />Predicted Cost = $358,279<br />Gross_Cost:     87892.25","Quantity: 5.694400e+02<br />Predicted Cost = $52,941<br />Gross_Cost:    377252.08","Quantity: 1.744720e+03<br />Predicted Cost = $149,141<br />Gross_Cost:     51862.47","Quantity: 6.648000e+03<br />Predicted Cost = $514,031<br />Gross_Cost:    185059.66","Quantity: 4.440000e+02<br />Predicted Cost = $42,056<br />Gross_Cost:     66422.63","Quantity: 2.100000e+02<br />Predicted Cost = $21,040<br />Gross_Cost:      4040.61","Quantity: 2.600000e+02<br />Predicted Cost = $25,636<br />Gross_Cost:     73119.30","Quantity: 5.400000e+02<br />Predicted Cost = $50,404<br />Gross_Cost:      9704.70","Quantity: 1.093400e+04<br />Predicted Cost = $814,462<br />Gross_Cost:    132091.75","Quantity: 1.376620e+03<br />Predicted Cost = $119,785<br />Gross_Cost:     74401.83","Quantity: 3.140000e+02<br />Predicted Cost = $30,525<br />Gross_Cost:     10799.72","Quantity: 6.600000e+01<br />Predicted Cost = $7,212<br />Gross_Cost:   1461949.54","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />Gross_Cost:     20962.25","Quantity: 5.002000e+03<br />Predicted Cost = $395,101<br />Gross_Cost:    298231.52","Quantity: 2.790000e+01<br />Predicted Cost = $3,252<br />Gross_Cost:      4967.73","Quantity: 1.900000e+01<br />Predicted Cost = $2,280<br />Gross_Cost:     12244.71","Quantity: 3.330000e+02<br />Predicted Cost = $32,230<br />Gross_Cost:     36134.36","Quantity: 6.616000e+03<br />Predicted Cost = $511,742<br />Gross_Cost:   1316701.55","Quantity: 7.812000e+02<br />Predicted Cost = $70,926<br />Gross_Cost:    341685.14","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />Gross_Cost:     11714.79","Quantity: 3.868044e+05<br />Predicted Cost = $22,051,065<br />Gross_Cost:  60775477.28","Quantity: 2.200000e+02<br />Predicted Cost = $21,965<br />Gross_Cost:     11735.05","Quantity: 2.880000e+02<br />Predicted Cost = $28,180<br />Gross_Cost:    126720.69","Quantity: 7.111200e+04<br />Predicted Cost = $4,603,066<br />Gross_Cost:    322026.12","Quantity: 5.422000e+03<br />Predicted Cost = $425,694<br />Gross_Cost:    310203.01","Quantity: 6.677400e+04<br />Predicted Cost = $4,342,720<br />Gross_Cost:  14652476.51","Quantity: 4.713560e+04<br />Predicted Cost = $3,146,646<br />Gross_Cost:   6170848.44","Quantity: 6.238100e+04<br />Predicted Cost = $4,077,776<br />Gross_Cost:  13616603.44","Quantity: 3.200000e+01<br />Predicted Cost = $3,692<br />Gross_Cost:   6884084.00","Quantity: 1.250000e+04<br />Predicted Cost = $921,811<br />Gross_Cost:    324783.32","Quantity: 1.046000e+03<br />Predicted Cost = $92,911<br />Gross_Cost:    329606.89","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />Gross_Cost:    138658.92","Quantity: 2.470000e+03<br />Predicted Cost = $205,705<br />Gross_Cost:     57377.72","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />Gross_Cost:      1490.27","Quantity: 1.069444e+05<br />Predicted Cost = $6,713,843<br />Gross_Cost:     12690.05","Quantity: 1.069444e+05<br />Predicted Cost = $6,713,843<br />Gross_Cost:     41550.00","Quantity: 2.338336e+05<br />Predicted Cost = $13,843,279<br />Gross_Cost:  29095409.43","Quantity: 1.500740e+04<br />Predicted Cost = $1,091,648<br />Gross_Cost:    379734.48","Quantity: 1.832020e+05<br />Predicted Cost = $11,046,137<br />Gross_Cost:  11261636.35","Quantity: 9.875060e+03<br />Predicted Cost = $741,224<br />Gross_Cost:    971333.87","Quantity: 3.083050e+05<br />Predicted Cost = $17,877,510<br />Gross_Cost:     41345.00","Quantity: 3.049568e+05<br />Predicted Cost = $17,697,847<br />Gross_Cost:  39433990.73","Quantity: 6.166100e+05<br />Predicted Cost = $33,943,746<br />Gross_Cost:     29890.31","Quantity: 5.170000e+03<br />Predicted Cost = $407,360<br />Gross_Cost:    136458.10","Quantity: 8.834000e+01<br />Predicted Cost = $9,445<br />Gross_Cost:    222431.44","Quantity: 1.224500e+03<br />Predicted Cost = $107,488<br />Gross_Cost:   1856668.66","Quantity: 9.537600e+02<br />Predicted Cost = $85,306<br />Gross_Cost:   4090606.80","Quantity: 1.418255e+04<br />Predicted Cost = $1,036,031<br />Gross_Cost:   6386553.92","Quantity: 1.394995e+04<br />Predicted Cost = $1,020,304<br />Gross_Cost:  16165273.67","Quantity: 7.287010e+04<br />Predicted Cost = $4,708,235<br />Gross_Cost:  10267664.01","Quantity: 5.800000e+01<br />Predicted Cost = $6,400<br />Gross_Cost:    271277.18","Quantity: 4.963000e+03<br />Predicted Cost = $392,250<br />Gross_Cost:  15888904.87","Quantity: 3.913800e+04<br />Predicted Cost = $2,649,438<br />Gross_Cost:   7302015.49","Quantity: 2.462600e+04<br />Predicted Cost = $1,725,994<br />Gross_Cost:     60929.92","Quantity: 9.698600e+03<br />Predicted Cost = $728,964<br />Gross_Cost:   7435385.23","Quantity: 1.416077e+04<br />Predicted Cost = $1,034,559<br />Gross_Cost:   3422427.84","Quantity: 5.706400e+03<br />Predicted Cost = $446,308<br />Gross_Cost:   4254575.96","Quantity: 2.468800e+02<br />Predicted Cost = $24,437<br />Gross_Cost:     70583.59","Quantity: 7.142000e+02<br />Predicted Cost = $65,280<br />Gross_Cost:    898003.24","Quantity: 2.477365e+04<br />Predicted Cost = $1,735,564<br />Gross_Cost:   6045694.95","Quantity: 3.839000e+03<br />Predicted Cost = $309,316<br />Gross_Cost:  13234658.89","Quantity: 1.168000e+03<br />Predicted Cost = $102,893<br />Gross_Cost:   2709224.08","Quantity: 8.000000e+03<br />Predicted Cost = $610,040<br />Gross_Cost:     35523.91","Quantity: 2.240000e+02<br />Predicted Cost = $22,335<br />Gross_Cost:      3983.99","Quantity: 1.027300e+04<br />Predicted Cost = $768,812<br />Gross_Cost:    113042.31","Quantity: 5.700000e+01<br />Predicted Cost = $6,298<br />Gross_Cost:      5523.33","Quantity: 9.209000e+03<br />Predicted Cost = $694,859<br />Gross_Cost:   4324140.81","Quantity: 8.400000e+04<br />Predicted Cost = $5,369,803<br />Gross_Cost:  32213082.24","Quantity: 3.000000e+01<br />Predicted Cost = $3,478<br />Gross_Cost:     11320.38","Quantity: 4.674000e+02<br />Predicted Cost = $44,102<br />Gross_Cost:    813031.57","Quantity: 1.685383e+05<br />Predicted Cost = $10,225,777<br />Gross_Cost:   5240651.43","Quantity: 3.431663e+05<br />Predicted Cost = $19,739,757<br />Gross_Cost:  21851008.92","Quantity: 3.000000e+03<br />Predicted Cost = $246,228<br />Gross_Cost:   2524623.54","Quantity: 1.104610e+03<br />Predicted Cost = $97,717<br />Gross_Cost:     94628.38","Quantity: 5.523345e+04<br />Predicted Cost = $3,643,653<br />Gross_Cost:   7319180.97","Quantity: 1.481800e+04<br />Predicted Cost = $1,078,898<br />Gross_Cost:   1721371.35","Quantity: 2.812100e+04<br />Predicted Cost = $1,951,432<br />Gross_Cost:   3807595.45","Quantity: 5.484920e+04<br />Predicted Cost = $3,620,200<br />Gross_Cost:   3779986.23","Quantity: 2.179000e+03<br />Predicted Cost = $183,184<br />Gross_Cost:    884975.98","Quantity: 8.120000e+01<br />Predicted Cost = $8,737<br />Gross_Cost:    580008.37","Quantity: 4.060000e+01<br />Predicted Cost = $4,601<br />Gross_Cost:   1262454.27","Quantity: 2.400000e+02<br />Predicted Cost = $23,806<br />Gross_Cost:   1438482.89","Quantity: 1.302000e+02<br />Predicted Cost = $13,521<br />Gross_Cost:   1625052.75","Quantity: 2.194000e+04<br />Predicted Cost = $1,551,115<br />Gross_Cost:     81000.00","Quantity: 1.421300e+03<br />Predicted Cost = $123,377<br />Gross_Cost:    933637.73","Quantity: 9.000000e+01<br />Predicted Cost = $9,609<br />Gross_Cost:     65123.25","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />Gross_Cost:    167228.12","Quantity: 1.357000e+03<br />Predicted Cost = $118,205<br />Gross_Cost:    493124.20","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />Gross_Cost:     59831.66","Quantity: 5.887770e+04<br />Predicted Cost = $3,865,490<br />Gross_Cost:   2274000.90","Quantity: 6.891000e+02<br />Predicted Cost = $63,155<br />Gross_Cost:   4699046.36","Quantity: 7.200000e+03<br />Predicted Cost = $553,392<br />Gross_Cost:    105057.50","Quantity: 3.700000e+01<br />Predicted Cost = $4,223<br />Gross_Cost:     12139.09","Quantity: 6.880040e+04<br />Predicted Cost = $4,464,488<br />Gross_Cost:   8992413.15","Quantity: 8.550000e+05<br />Predicted Cost = $45,927,061<br />Gross_Cost:    436933.33","Quantity: 1.740600e+04<br />Predicted Cost = $1,252,122<br />Gross_Cost:   5182856.75","Quantity: 5.150000e+02<br />Predicted Cost = $48,242<br />Gross_Cost:     79789.07","Quantity: 5.083000e+03<br />Predicted Cost = $401,015<br />Gross_Cost:   1293442.96","Quantity: 5.377850e+04<br />Predicted Cost = $3,554,782<br />Gross_Cost:   3338606.98","Quantity: 2.500000e+02<br />Predicted Cost = $24,723<br />Gross_Cost:    801512.44","Quantity: 3.447911e+04<br />Predicted Cost = $2,356,348<br />Gross_Cost:  13609486.00","Quantity: 2.792500e+04<br />Predicted Cost = $1,938,848<br />Gross_Cost:    605180.94","Quantity: 2.815940e+03<br />Predicted Cost = $232,221<br />Gross_Cost:    783949.84","Quantity: 1.349610e+05<br />Predicted Cost = $8,326,120<br />Gross_Cost:   2390410.80","Quantity: 1.450000e+03<br />Predicted Cost = $125,680<br />Gross_Cost:    205836.22","Quantity: 3.560000e+02<br />Predicted Cost = $34,284<br />Gross_Cost:    312711.84","Quantity: 7.337196e+04<br />Predicted Cost = $4,738,222<br />Gross_Cost:  13858386.42","Quantity: 2.809086e+05<br />Predicted Cost = $16,402,974<br />Gross_Cost:  28967410.41","Quantity: 1.866200e+02<br />Predicted Cost = $18,864<br />Gross_Cost:    131317.08","Quantity: 1.400000e+04<br />Predicted Cost = $1,023,690<br />Gross_Cost:     25716.60","Quantity: 6.188000e+03<br />Predicted Cost = $481,043<br />Gross_Cost:    493612.44","Quantity: 4.296124e+05<br />Predicted Cost = $24,299,429<br />Gross_Cost:     87671.99","Quantity: 1.215400e+03<br />Predicted Cost = $106,749<br />Gross_Cost:   3238120.48","Quantity: 2.660000e+03<br />Predicted Cost = $220,301<br />Gross_Cost:    730250.00","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />Gross_Cost:    180587.60","Quantity: 1.650802e+01<br />Predicted Cost = $2,002<br />Gross_Cost:    288498.54","Quantity: 1.657000e+01<br />Predicted Cost = $2,009<br />Gross_Cost:      4300.00","Quantity: 1.142980e+05<br />Predicted Cost = $7,139,793<br />Gross_Cost:   1156197.82","Quantity: 6.838600e+02<br />Predicted Cost = $62,711<br />Gross_Cost:    259889.60","Quantity: 2.600000e+04<br />Predicted Cost = $1,814,890<br />Gross_Cost:  10722161.01","Quantity: 7.118797e+05<br />Predicted Cost = $38,768,242<br />Gross_Cost:   4371242.55","Quantity: 1.680000e+01<br />Predicted Cost = $2,034<br />Gross_Cost:    343844.52","Quantity: 5.220000e+02<br />Predicted Cost = $48,848<br />Gross_Cost:     27000.00","Quantity: 1.440000e+02<br />Predicted Cost = $14,842<br />Gross_Cost:      2553.63","Quantity: 4.155400e+04<br />Predicted Cost = $2,800,379<br />Gross_Cost:    656599.23","Quantity: 3.264940e+03<br />Predicted Cost = $266,278<br />Gross_Cost:    137831.79","Quantity: 3.002000e+03<br />Predicted Cost = $246,380<br />Gross_Cost:     71669.88","Quantity: 5.827680e+03<br />Predicted Cost = $455,076<br />Gross_Cost:  21163661.35","Quantity: 8.094045e+04<br />Predicted Cost = $5,188,636<br />Gross_Cost:   4508859.06","Quantity: 2.408400e+03<br />Predicted Cost = $200,955<br />Gross_Cost:    422877.01","Quantity: 5.140000e+02<br />Predicted Cost = $48,155<br />Gross_Cost:     15262.78","Quantity: 1.695000e+03<br />Predicted Cost = $145,205<br />Gross_Cost:     62563.49","Quantity: 2.587903e+05<br />Predicted Cost = $15,204,666<br />Gross_Cost:   8368928.59","Quantity: 2.004440e+05<br />Predicted Cost = $12,004,486<br />Gross_Cost:     28402.55","Quantity: 1.817030e+03<br />Predicted Cost = $154,850<br />Gross_Cost:     67209.49","Quantity: 5.186950e+04<br />Predicted Cost = $3,437,903<br />Gross_Cost:   2288121.96","Quantity: 3.304000e+02<br />Predicted Cost = $31,997<br />Gross_Cost:     42498.77","Quantity: 1.227147e+05<br />Predicted Cost = $7,624,816<br />Gross_Cost:   6375770.17","Quantity: 2.451000e+04<br />Predicted Cost = $1,718,472<br />Gross_Cost:   1314805.15","Quantity: 6.134830e+04<br />Predicted Cost = $4,015,294<br />Gross_Cost:    216685.41","Quantity: 7.190880e+04<br />Predicted Cost = $4,650,754<br />Gross_Cost:   4548012.41","Quantity: 2.930000e+02<br />Predicted Cost = $28,632<br />Gross_Cost:      5904.64","Quantity: 3.485034e+04<br />Predicted Cost = $2,379,806<br />Gross_Cost:  16476766.41","Quantity: 8.911550e+03<br />Predicted Cost = $674,073<br />Gross_Cost:    406590.23","Quantity: 4.183475e+04<br />Predicted Cost = $2,817,876<br />Gross_Cost:   1104107.98","Quantity: 1.708700e+04<br />Predicted Cost = $1,230,880<br />Gross_Cost:   3017366.97","Quantity: 2.963400e+04<br />Predicted Cost = $2,048,359<br />Gross_Cost:    645767.50","Quantity: 7.387389e+05<br />Predicted Cost = $40,119,376<br />Gross_Cost: 115604437.67","Quantity: 1.478800e+04<br />Predicted Cost = $1,076,877<br />Gross_Cost:   5835582.78","Quantity: 2.927500e+04<br />Predicted Cost = $2,025,395<br />Gross_Cost:   1131015.01","Quantity: 1.685344e+06<br />Predicted Cost = $86,037,318<br />Gross_Cost: 100424476.36","Quantity: 1.950200e+04<br />Predicted Cost = $1,390,987<br />Gross_Cost:  21075105.63","Quantity: 1.125400e+04<br />Predicted Cost = $836,486<br />Gross_Cost:   3092847.22","Quantity: 1.539474e+05<br />Predicted Cost = $9,404,147<br />Gross_Cost:  13169142.82","Quantity: 1.914000e+03<br />Predicted Cost = $162,479<br />Gross_Cost:    677148.67","Quantity: 5.243400e+04<br />Predicted Cost = $3,472,498<br />Gross_Cost:  16311021.01","Quantity: 9.757830e+03<br />Predicted Cost = $733,081<br />Gross_Cost:    801400.14","Quantity: 2.646608e+04<br />Predicted Cost = $1,844,964<br />Gross_Cost:   2141926.92","Quantity: 3.958000e+02<br />Predicted Cost = $37,815<br />Gross_Cost:    323769.37","Quantity: 5.848307e+05<br />Predicted Cost = $32,322,348<br />Gross_Cost:  24902438.10","Quantity: 3.997500e+02<br />Predicted Cost = $38,164<br />Gross_Cost:   3508953.44","Quantity: 3.920930e+04<br />Predicted Cost = $2,653,902<br />Gross_Cost:   7269766.24","Quantity: 1.991214e+05<br />Predicted Cost = $11,931,196<br />Gross_Cost:  56419218.21","Quantity: 9.608625e+04<br />Predicted Cost = $6,080,813<br />Gross_Cost:  52195275.17","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />Gross_Cost:    143823.58","Quantity: 2.607010e+05<br />Predicted Cost = $15,308,477<br />Gross_Cost:  15061190.69","Quantity: 8.925000e+02<br />Predicted Cost = $80,226<br />Gross_Cost:    258051.58","Quantity: 3.671430e+04<br />Predicted Cost = $2,497,311<br />Gross_Cost:  12031592.63","Quantity: 1.632000e+03<br />Predicted Cost = $140,206<br />Gross_Cost:    775315.71","Quantity: 2.913900e+04<br />Predicted Cost = $2,016,690<br />Gross_Cost:    520065.35","Quantity: 2.447749e+05<br />Predicted Cost = $14,441,405<br />Gross_Cost:   4099808.95","Quantity: 4.349900e+04<br />Predicted Cost = $2,921,415<br />Gross_Cost:   3761961.75","Quantity: 6.922440e+03<br />Predicted Cost = $533,629<br />Gross_Cost:    425983.30","Quantity: 2.920270e+04<br />Predicted Cost = $2,020,767<br />Gross_Cost:   1538086.36","Quantity: 1.364898e+04<br />Predicted Cost = $999,926<br />Gross_Cost:    615069.08","Quantity: 1.634000e+03<br />Predicted Cost = $140,365<br />Gross_Cost:    241425.98","Quantity: 2.533134e+04<br />Predicted Cost = $1,771,674<br />Gross_Cost:   1488940.52","Quantity: 4.800000e+02<br />Predicted Cost = $45,201<br />Gross_Cost:    185244.52","Quantity: 1.150000e+02<br />Predicted Cost = $12,054<br />Gross_Cost:     24928.61","Quantity: 1.883069e+05<br />Predicted Cost = $11,330,558<br />Gross_Cost:   1659818.14","Quantity: 1.544265e+05<br />Predicted Cost = $9,431,217<br />Gross_Cost:   7212616.30","Quantity: 1.535021e+06<br />Predicted Cost = $78,914,290<br />Gross_Cost:   3877424.30","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />Gross_Cost:      3832.72","Quantity: 2.710300e+05<br />Predicted Cost = $15,868,690<br />Gross_Cost:      5614.82","Quantity: 2.056400e+04<br />Predicted Cost = $1,460,914<br />Gross_Cost:     11317.23","Quantity: 7.278000e+02<br />Predicted Cost = $66,430<br />Gross_Cost:    113734.29","Quantity: 1.000000e+02<br />Predicted Cost = $10,593<br />Gross_Cost:     30630.00","Quantity: 1.295800e+04<br />Predicted Cost = $953,010<br />Gross_Cost:     91116.08","Quantity: 6.180000e+03<br />Predicted Cost = $480,468<br />Gross_Cost:      8258.62","Quantity: 1.468000e+01<br />Predicted Cost = $1,796<br />Gross_Cost:     11987.72","Quantity: 5.687660e+05<br />Predicted Cost = $31,500,221<br />Gross_Cost:    143702.90","Quantity: 1.940000e+03<br />Predicted Cost = $164,519<br />Gross_Cost:     13953.00","Quantity: 8.306886e+04<br />Predicted Cost = $5,314,720<br />Gross_Cost:   4703005.94","Quantity: 1.230000e+03<br />Predicted Cost = $107,935<br />Gross_Cost:    345086.33","Quantity: 2.274000e+03<br />Predicted Cost = $190,560<br />Gross_Cost:     30824.92","Quantity: 1.170600e+04<br />Predicted Cost = $867,517<br />Gross_Cost:    616611.33","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />Gross_Cost:     31781.84","Quantity: 5.555840e+04<br />Predicted Cost = $3,663,477<br />Gross_Cost:    274794.56","Quantity: 1.509200e+04<br />Predicted Cost = $1,097,339<br />Gross_Cost:   6569060.33","Quantity: 5.135800e+02<br />Predicted Cost = $48,119<br />Gross_Cost:    109936.35","Quantity: 2.916000e+03<br />Predicted Cost = $239,844<br />Gross_Cost:    146624.09","Quantity: 1.593000e+04<br />Predicted Cost = $1,153,585<br />Gross_Cost:    243292.87","Quantity: 1.218100e+02<br />Predicted Cost = $12,713<br />Gross_Cost:     43084.86","Quantity: 2.209380e+05<br />Predicted Cost = $13,135,607<br />Gross_Cost:   5117234.59","Quantity: 1.680000e+02<br />Predicted Cost = $17,116<br />Gross_Cost:      9670.78","Quantity: 4.259168e+05<br />Predicted Cost = $24,106,015<br />Gross_Cost:      6529.95","Quantity: 3.464100e+03<br />Predicted Cost = $281,269<br />Gross_Cost:     83897.54","Quantity: 6.985800e+03<br />Predicted Cost = $538,146<br />Gross_Cost:    227062.95","Quantity: 1.100000e+01<br />Predicted Cost = $1,375<br />Gross_Cost:    292611.44","Quantity: 8.500000e+01<br />Predicted Cost = $9,114<br />Gross_Cost:    352827.20","Quantity: 5.110000e+03<br />Predicted Cost = $402,985<br />Gross_Cost:     11423.33","Quantity: 1.277000e+03<br />Predicted Cost = $111,745<br />Gross_Cost:   4567224.89","Quantity: 2.593900e+04<br />Predicted Cost = $1,810,951<br />Gross_Cost:     81583.87","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />Gross_Cost:      5482.22","Quantity: 2.137500e+02<br />Predicted Cost = $21,388<br />Gross_Cost:    201179.46","Quantity: 5.860211e+05<br />Predicted Cost = $32,383,201<br />Gross_Cost:  42263262.65","Quantity: 1.068079e+06<br />Predicted Cost = $56,423,275<br />Gross_Cost: 107141891.25","Quantity: 5.358010e+04<br />Predicted Cost = $3,542,650<br />Gross_Cost:  15916838.67","Quantity: 9.000000e+03<br />Predicted Cost = $680,259<br />Gross_Cost:   4906879.32","Quantity: 1.280000e+02<br />Predicted Cost = $13,310<br />Gross_Cost:    234851.81","Quantity: 1.072570e+04<br />Predicted Cost = $800,099<br />Gross_Cost:   8649116.66","Quantity: 1.590000e+02<br />Predicted Cost = $16,266<br />Gross_Cost:    274103.02","Quantity: 6.184630e+03<br />Predicted Cost = $480,801<br />Gross_Cost:  13913335.48","Quantity: 1.323848e+04<br />Predicted Cost = $972,076<br />Gross_Cost:   1693227.63","Quantity: 5.363350e+03<br />Predicted Cost = $421,433<br />Gross_Cost:   4217873.08","Quantity: 1.024580e+04<br />Predicted Cost = $766,929<br />Gross_Cost:   2876846.22","Quantity: 1.132150e+04<br />Predicted Cost = $841,126<br />Gross_Cost:    704591.23","Quantity: 4.880000e+01<br />Predicted Cost = $5,455<br />Gross_Cost:    112566.65","Quantity: 3.611600e+02<br />Predicted Cost = $34,743<br />Gross_Cost:    132495.00","Quantity: 3.242650e+03<br />Predicted Cost = $264,596<br />Gross_Cost:    731727.09","Quantity: 9.647222e+04<br />Predicted Cost = $6,103,404<br />Gross_Cost:  26242484.88","Quantity: 8.698750e+03<br />Predicted Cost = $659,170<br />Gross_Cost:   9515614.45","Quantity: 2.140567e+05<br />Predicted Cost = $12,756,722<br />Gross_Cost:  16330624.32","Quantity: 2.623704e+02<br />Predicted Cost = $25,852<br />Gross_Cost:     15890.96","Quantity: 1.120000e+02<br />Predicted Cost = $11,763<br />Gross_Cost:     14003.19","Quantity: 1.900000e+01<br />Predicted Cost = $2,280<br />Gross_Cost:      3983.77","Quantity: 9.392259e+04<br />Predicted Cost = $5,954,048<br />Gross_Cost:    276943.85","Quantity: 1.014424e+06<br />Predicted Cost = $53,796,382<br />Gross_Cost:  59989172.12","Quantity: 1.498740e+04<br />Predicted Cost = $1,090,302<br />Gross_Cost:    243192.99","Quantity: 9.372000e+01<br />Predicted Cost = $9,976<br />Gross_Cost:     17938.46","Quantity: 5.000000e+02<br />Predicted Cost = $46,940<br />Gross_Cost:     14975.80","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />Gross_Cost:     10795.46","Quantity: 1.503800e+02<br />Predicted Cost = $15,449<br />Gross_Cost:      9137.02","Quantity: 1.350000e+02<br />Predicted Cost = $13,982<br />Gross_Cost:    873391.90","Quantity: 5.400000e+02<br />Predicted Cost = $50,404<br />Gross_Cost:     29507.88","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />Gross_Cost:     64671.21","Quantity: 1.000000e+03<br />Predicted Cost = $89,125<br />Gross_Cost:      5376.36","Quantity: 1.571400e+04<br />Predicted Cost = $1,139,109<br />Gross_Cost:    110605.87","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />Gross_Cost:      3694.46","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />Gross_Cost:     91848.03","Quantity: 1.306541e+06<br />Predicted Cost = $67,985,102<br />Gross_Cost:  18494972.36","Quantity: 1.962795e+04<br />Predicted Cost = $1,399,295<br />Gross_Cost:    111203.85","Quantity: 1.300000e+02<br />Predicted Cost = $13,502<br />Gross_Cost:     97936.02","Quantity: 4.000000e+03<br />Predicted Cost = $321,296<br />Gross_Cost:  20426936.70","Quantity: 1.643896e+04<br />Predicted Cost = $1,187,637<br />Gross_Cost:   1453644.48","Quantity: 3.525200e+02<br />Predicted Cost = $33,974<br />Gross_Cost:     82315.94","Quantity: 3.200000e+01<br />Predicted Cost = $3,692<br />Gross_Cost:    135270.62","Quantity: 4.222000e+03<br />Predicted Cost = $337,757<br />Gross_Cost:    929053.06","Quantity: 6.225000e+02<br />Predicted Cost = $57,488<br />Gross_Cost:     45436.67","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />Gross_Cost:     22253.75","Quantity: 3.328810e+03<br />Predicted Cost = $271,092<br />Gross_Cost:    286009.86","Quantity: 3.773000e+03<br />Predicted Cost = $304,393<br />Gross_Cost:    110468.23","Quantity: 6.525000e+02<br />Predicted Cost = $60,046<br />Gross_Cost:    125249.72","Quantity: 2.944140e+03<br />Predicted Cost = $241,984<br />Gross_Cost:    232614.46","Quantity: 7.542000e+02<br />Predicted Cost = $68,655<br />Gross_Cost:    219800.43","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />Gross_Cost:      7411.78","Quantity: 1.823000e+03<br />Predicted Cost = $155,320<br />Gross_Cost:   1223647.28","Quantity: 4.200000e+02<br />Predicted Cost = $39,949<br />Gross_Cost:    241718.45","Quantity: 1.122222e+04<br />Predicted Cost = $834,301<br />Gross_Cost:    304582.12","Quantity: 2.925000e+05<br />Predicted Cost = $17,028,109<br />Gross_Cost:  29557826.37","Quantity: 6.744000e+03<br />Predicted Cost = $520,893<br />Gross_Cost:   1720722.95","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />Gross_Cost:     81767.34","Quantity: 2.431746e+04<br />Predicted Cost = $1,705,982<br />Gross_Cost:     64776.61","Quantity: 1.801400e+05<br />Predicted Cost = $10,875,254<br />Gross_Cost:    371578.71","Quantity: 4.870600e+02<br />Predicted Cost = $45,816<br />Gross_Cost:     53111.86","Quantity: 1.600000e+04<br />Predicted Cost = $1,158,273<br />Gross_Cost:    672176.66","Quantity: 8.295000e+02<br />Predicted Cost = $74,973<br />Gross_Cost:   1237893.47","Quantity: 8.116600e+02<br />Predicted Cost = $73,480<br />Gross_Cost:    746856.39","Quantity: 4.000000e+02<br />Predicted Cost = $38,186<br />Gross_Cost:     62343.65","Quantity: 6.711000e+02<br />Predicted Cost = $61,628<br />Gross_Cost:    863377.78","Quantity: 1.009000e+02<br />Predicted Cost = $10,681<br />Gross_Cost:    286037.25","Quantity: 3.930802e+04<br />Predicted Cost = $2,660,082<br />Gross_Cost:   2258948.25","Quantity: 2.068581e+04<br />Predicted Cost = $1,468,916<br />Gross_Cost:   4559557.59","Quantity: 5.532000e+02<br />Predicted Cost = $51,543<br />Gross_Cost:    123220.62","Quantity: 2.809000e+03<br />Predicted Cost = $231,692<br />Gross_Cost:    928735.00","Quantity: 4.256000e+03<br />Predicted Cost = $340,272<br />Gross_Cost:   3067285.56","Quantity: 4.200000e+01<br />Predicted Cost = $4,748<br />Gross_Cost:    165543.74","Quantity: 9.147565e+04<br />Predicted Cost = $5,810,421<br />Gross_Cost:   7667065.61","Quantity: 1.287000e+03<br />Predicted Cost = $112,554<br />Gross_Cost:    262426.34","Quantity: 2.392800e+02<br />Predicted Cost = $23,740<br />Gross_Cost:    418393.30","Quantity: 1.260400e+03<br />Predicted Cost = $110,400<br />Gross_Cost:    301509.11","Quantity: 3.996000e+03<br />Predicted Cost = $320,999<br />Gross_Cost:   1423541.34","Quantity: 4.354500e+02<br />Predicted Cost = $41,306<br />Gross_Cost:     52752.80","Quantity: 1.200000e+03<br />Predicted Cost = $105,498<br />Gross_Cost:     49980.20","Quantity: 3.360577e+05<br />Predicted Cost = $19,361,224<br />Gross_Cost:  20717810.32","Quantity: 1.059500e+04<br />Predicted Cost = $791,076<br />Gross_Cost:    380688.64","Quantity: 1.500000e+01<br />Predicted Cost = $1,832<br />Gross_Cost:    133700.70","Quantity: 4.170200e+04<br />Predicted Cost = $2,809,604<br />Gross_Cost:   1540372.01","Quantity: 8.128043e+04<br />Predicted Cost = $5,208,792<br />Gross_Cost:   1434055.44","Quantity: 4.746850e+04<br />Predicted Cost = $3,167,197<br />Gross_Cost:   1062831.95","Quantity: 1.611960e+03<br />Predicted Cost = $138,613<br />Gross_Cost:    110330.80","Quantity: 5.098200e+02<br />Predicted Cost = $47,793<br />Gross_Cost:     78468.24","Quantity: 2.117000e+02<br />Predicted Cost = $21,198<br />Gross_Cost:   2599813.53","Quantity: 8.300000e+01<br />Predicted Cost = $8,916<br />Gross_Cost:    168743.57","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />Gross_Cost:   4758774.53","Quantity: 5.685100e+03<br />Predicted Cost = $444,767<br />Gross_Cost:   6697893.83","Quantity: 1.152000e+03<br />Predicted Cost = $101,588<br />Gross_Cost:     12503.07","Quantity: 8.646000e+03<br />Predicted Cost = $655,472<br />Gross_Cost:     12083.04","Quantity: 2.193614e+01<br />Predicted Cost = $2,604<br />Gross_Cost:      6632.26","Quantity: 6.262000e+02<br />Predicted Cost = $57,804<br />Gross_Cost:     14300.79","Quantity: 6.060000e+03<br />Predicted Cost = $471,832<br />Gross_Cost:     99611.40","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />Gross_Cost:    115067.20","Quantity: 2.724600e+02<br />Predicted Cost = $26,770<br />Gross_Cost:     64511.37","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />Gross_Cost:      8287.00","Quantity: 1.124000e+02<br />Predicted Cost = $11,802<br />Gross_Cost:    206471.58","Quantity: 1.400000e+01<br />Predicted Cost = $1,719<br />Gross_Cost:      8691.74","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />Gross_Cost:     24268.94","Quantity: 3.118400e+03<br />Predicted Cost = $255,204<br />Gross_Cost:    500466.78","Quantity: 1.481796e+04<br />Predicted Cost = $1,078,895<br />Gross_Cost:    771802.42","Quantity: 1.972424e+05<br />Predicted Cost = $11,827,018<br />Gross_Cost:  29677201.05","Quantity: 7.224000e+01<br />Predicted Cost = $7,841<br />Gross_Cost:     37074.40","Quantity: 4.687428e+05<br />Predicted Cost = $26,339,921<br />Gross_Cost:  10785827.85","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />Gross_Cost:     10000.00","Quantity: 6.346000e+03<br />Predicted Cost = $492,394<br />Gross_Cost:    417319.66","Quantity: 2.172210e+04<br />Predicted Cost = $1,536,860<br />Gross_Cost:   4336627.23","Quantity: 1.250000e+04<br />Predicted Cost = $921,811<br />Gross_Cost:    154185.33","Quantity: 9.500000e+02<br />Predicted Cost = $84,995<br />Gross_Cost:    239390.75","Quantity: 6.318400e+02<br />Predicted Cost = $58,286<br />Gross_Cost:    114450.07","Quantity: 6.664860e+04<br />Predicted Cost = $4,335,176<br />Gross_Cost:   6118565.74","Quantity: 1.340000e+03<br />Predicted Cost = $116,835<br />Gross_Cost:    585797.97","Quantity: 8.999176e+04<br />Predicted Cost = $5,723,182<br />Gross_Cost:   1871424.72","Quantity: 1.853500e+02<br />Predicted Cost = $18,745<br />Gross_Cost:     27139.12","Quantity: 6.684800e+02<br />Predicted Cost = $61,405<br />Gross_Cost:     13637.50"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,0,0,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(0,0,0,1)"}},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2.7024305364455254,4.1984096335377616,4.1552570676526974,2.1461280356782382,5.0550742952996881,5.6388102911519624,5.115095709153235,5.3296585812030282,5.2169432401416342,5.34476116902146,4.6230561932390266,4.7533792679960891,2.6693168805661123,4.7212548242783958,3.3424226808222062,2.6852220653346204,3.6292056571023039,4.5515301398686079,5.1145002283533332,3.9506374200705787,2.1192558892779365,6.101551030991037,5.9334293932441931,3.3703187516177455,3.1630718820038193,3.6958282732599583,4.2699138977414801,4.20744450716556,6.0945933986918552,5.8228092628357917,4.4835787393578457,4.1603112322086577,1.4393326938302626,4.295423044232602,4.9580899140341002,6.6991327474116886,1.7168377232995244,4.1076423523558283,3.8429067791404372,1.8567288903828827,2.2900346113625178,2.0413926851582249,5.2442501466886622,5.1216601175420404,1.9030899869919435,2.9484129657786009,3.539803497384677,5.074771948771585,5.2430722945580479,5.7822159849460446,3.6989700043360187,4.1925674533365456,1.2355284469075489,3.4890016973113775,5.8994042226163863,4.8991850050849477,6.1582259875720986,3.8829796540372992,6.0433759787151002,2.4313637641589874,3.2050418792613695,3.5151052041667898,3.5724068675580556,3.9456654994321343,6.2333936760774149,3.5147734739975087,1.8450980400142569,2.9444826721501687,6.1130413755231467,6.7864854403677892,4.3750496821292746,6.4300969955807767,3.0637085593914173,5.2695250856143598,1.3010299956639813,1.2950711714662781,2.0791812460476247,4.3639093040106847,2.2036169956331912,2.0901169107520099,4.1027629037125646,3.7320719409998668,2.255272505103306,1.5440680443502757,2.357934847000454,4.1587293762389352,3.2398898183400542,1.8450980400142569,3.370883016777606,3.8958091501691308,5.0504952949350823,3.6037612606082874,3.0926855629374908,2.5440680443502757,4.5093080578565763,3.7281150573980244,2.9858780492079968,5.3515017638802327,3.1965768448522329,3.8655623192261745,3.6868149545073168,4.3139776773448899,3.7225993750077753,6.0269743049404001,4.4471580313422194,3.8811483271514193,2.5051499783199058,5.3979330599047373,1.6654497448426819,2.9929950984313414,4.4581239446610619,3.2487087356009177,4.56059520730137,3.4750898033890065,4.6512780139981444,4.038458016269562,3.4409090820652177,5.6342373846950906,5.3124325247115989,1.9777236052888478,3.815132600842988,2.0903638794717181,3.6974211573941798,5.3010299956639813,2.9013493254156422,3.6778714633289114,2.5717088318086878,2.4771212547196626,2.9786369483844743,2.7874604745184151,1.7781512503836436,2.5910646070264991,4.5798258811262302,3.4367587960456936,3.9685296443748395,4.082507449923134,3.1283992687178066,1.9834007381805383,4.9780891730561425,2.8407332346118066,4.0483446785400696,4.5002620046610948,3.9640896827699628,4.4469346224414545,5.5548738217592879,5.6363013905219077,5.3758137898824598,4.9473243011870238,5.1153751634736802,5.5083661869750022,2.5237464668115646,3.1037695231936726,4.7235106556542812,5.5581228379383969,3.1085650237328344,4.5443878155045354,5.2564575963805229,2.8665000026721725,4.6286341094782966,2.3802112417116059,5.385835299070485,3.6833460758601015,2.255272505103306,4.2944794532032704,3.1655410767223731,4.3651771284764331,2.9489017609702137,5.9058383905918417,4.0144113213844728,1.3010299956639813,1.954242509439325,2.5436956323092446,2.5563025007672873,3.247354508217859,4.2418760607224613,2.3617278360175931,2.9332847723486948,2.5269592553422457,4.8930178883115341,1.4471580313422192,1.255272505103306,4.3544284692600153,5.0044589502411947,3.7185016888672742,2.8573324964312685,1.7781512503836436,1.6011905326153335,3.4377505628203879,2.859138297294531,3.3802112417116059,3.3644952932801808,4.0387790695555381,2.6919651027673601,1.9822712330395684,1.4771212547196624,2.8853612200315122,3.1654105314399699,2.510545010206612,4.1689457569017909,4.300986564044174,3.0097482559485536,3.4505570094183291,4.6672567755966927,3.2108533653148932,4.4171061673925927,3.3070144100729419,1.6972293427597176,2.0681858617461617,2.5563025007672873,3.2068258760318495,3.6356144176238723,3.9073468568040881,4.6937269489236471,1.8375884382355112,4.0791479488609372,1.1814205162624751,3.7875313161272341,2.5646660642520893,1.9030899869919435,6.4771799528902045,3.4117880045438689,1.5954962218255742,2.6989700043360187,2.3096301674258988,3.3344537511509307,1.9118112227541586,2.3873898263387292,2.3010299956639813,2.8846065435331911,2.9444826721501687,2.840106094456758,3.2464985807958011,4.4813193459054528,3.4352963763370234,3.7289119178080758,3.9476297473843545,3.0025979807199086,2.5172038181418617,2.1760912590556813,3.7564877686873519,3.0591846176313711,3.4653828514484184,3.4185051298166429,2.4297522800024081,2.8522603510069531,4.1983546247369397,4.5223398859609105,4.8645110810583923,3.6959892241501806,4.9725942677630446,3.669006784060679,3.8529676910288182,2.9950864965057331,3.5294868192458373,4.1461280356782382,2.0906107078284069,4.8240028857830639,2.8350561017201161,3.5399538416563967,2.9469432706978256,2.7558748556724915,3.6532125137753435,2.7554479706597705,3.2417257394831371,3.8226910107760546,2.6473829701146196,2.3222192947339191,2.4149733479708178,2.7323937598229686,4.0387790695555381,3.1388140748403659,2.4969296480732148,1.8195439355418688,2.2041199826559246,3.6991436873944838,1.4456042032735976,1.2787536009528289,2.5224442335063197,3.8205954965444904,2.8927622346158168,1.3010299956639813,5.5874914281065235,2.3424226808222062,2.459392487759231,4.8519428931916568,3.7341595132444669,4.8246073927962358,4.673349039687956,4.7950523324442234,1.505149978319906,4.0969100130080562,3.0195316845312554,2.0791812460476247,3.3926969532596658,1.6020599913279623,5.029158048255157,5.029158048255157,5.3689070087836734,4.176305458168649,5.2629302105120859,3.9945397430417637,5.4889805680440604,5.4842383116077116,5.7900105637080417,3.7134905430939424,1.9461573949223723,3.0879587894607328,2.9794391044854023,4.1517543233268732,4.1445726509957801,4.8625493655096754,1.7634279935629373,3.6957442751973235,4.5925986289061314,4.3913938751356989,3.986709048064589,4.151086869007643,3.7563622110126267,2.3924859087190731,2.853819845856763,4.3939899976265382,3.584218112117405,3.0674428427763805,3.9030899869919438,2.3502480183341627,4.0116972881141422,1.7558748556724915,3.9642124729698192,4.924279286061882,1.4771212547196624,2.6696887080562082,5.2266987378384382,5.53550465751026,3.4771212547196626,3.0432089705599021,4.7422021710376834,4.1707895904463914,4.4490307604004151,4.739170297578565,3.3382572302462554,1.9095560292411753,1.608526033577194,2.3802112417116059,2.114610984232173,4.3412366232386921,3.152685756036786,1.954242509439325,2.0791812460476247,3.1325798476597368,1.7781512503836436,4.7699508393203791,2.8382822499146885,3.8573324964312685,1.568201724066995,4.8375909631960896,5.9319661147281728,4.2406989791863081,2.7118072290411912,3.7061201097027037,4.7306086846331397,2.3979400086720375,4.5375560469706144,4.4459931817876468,3.4496233969379295,5.1302082875187756,3.1613680022349748,2.5514499979728753,4.8655301206260217,5.4485649734633181,2.2709581850920975,4.1461280356782382,3.79155030502733,5.633076829294466,3.0847192320112975,3.424881636631067,2.1760912590556813,1.2176951179079363,1.2193225084193366,5.0580385129555649,2.8349672019384444,4.4149733479708182,5.8524066026479842,1.2253092817258628,2.7176705030022621,2.1583624920952498,4.6186128354441802,3.5138752046284449,3.4774106879072515,3.7654956964868163,4.9081656145845027,3.3817286185351105,2.7109631189952759,3.2291697025391009,5.4129479940390777,5.3019930608065904,3.2593620977686291,4.7149120615989917,2.5190400386483445,5.088896590000771,4.3893433112520777,4.7878025326563645,4.8567820413923553,2.4668676203541096,4.5422070194326958,3.949953248133617,4.6215371780320238,4.2326658194314453,4.4717902765451916,5.8684909977946704,4.1699094419010692,4.4664969037444004,6.2266885002344523,4.2900791521022015,4.0513069108179742,5.1873723585514089,3.2819419334408249,4.7196129897309733,3.9893532476043831,4.4226896209220463,2.5974757898703773,5.7670301623264155,2.6017884724182725,4.5933890889502704,5.2991178280338591,4.9826612443139986,2.2041199826559246,5.416142697043469,2.9506082247842307,4.5648352996192942,3.2127201544178425,4.4644746434554561,5.3887669549130184,4.6384792730586861,3.8402592002021061,4.4654230069584191,4.135100197389721,3.2132520521963968,4.4036581640982106,2.6812412373755872,2.0606978403536118,5.2748662569248337,5.1887218566113926,6.1861142194078909,1.6020599913279623,5.4330173650934359,4.313107595194996,2.8620120512502165,2,4.1125379756093077,3.7909884750888159,1.1667260555800518,5.7549336270084162,3.287801729930226,4.9194382504422851,3.0899051114393981,3.356790460351716,4.0684085197781616,2.1760912590556813,4.7447497302049486,4.1787467965289578,2.710608102952619,3.4647875196459372,4.2022157758011316,2.0856829431946151,5.3442704183205363,2.2253092817258628,5.6293247851389783,3.5395904205339157,3.844216147843321,1.0413926851582251,1.9294189257142926,3.7084209001347128,3.1061908972634154,4.4139532291554149,1.6989700043360187,2.3299061234002103,5.767913275535089,6.0286035551931025,4.7290035198247784,3.9542425094393248,2.1072099696478683,4.0304256454923628,2.2013971243204513,3.7913137227582072,4.1218381236604849,3.729436138956145,4.0105458741106785,4.0539039708951661,1.6884198220027107,2.5576996443512146,3.5109000750153396,4.9844022725395707,3.9394568495030713,5.3305287649853836,2.4189148374041607,2.0492180226701815,1.2787536009528289,4.9727700721181538,6.0062195350443863,4.1757262983859329,1.971832279924925,2.6989700043360187,1.6989700043360187,2.1771900804896092,2.1303337684950061,2.7323937598229686,2.0791812460476247,3,4.196286748808876,1.6020599913279623,1.6020599913279623,6.1161229762050935,4.29287494299383,2.1139433523068369,3.6020599913279625,4.2158743387181401,2.5471837614500816,1.505149978319906,3.6255182289716377,2.7941393557677738,2.2041199826559246,3.5222890074407114,3.5766868052009957,2.8145805160103188,3.4689584577652681,2.8774865280696016,1.6989700043360187,3.2607866686549762,2.6232492903979003,4.0500788634833285,5.4661258704181996,3.8289175616166857,2.5563025007672873,4.3859182101733625,5.2556101584077384,2.687582464425827,4.204119982655925,2.9188163903603797,2.909374143715874,2.6020599913279625,2.826787238816292,2.0038911662369103,4.5944811683499527,4.3156725313539823,2.7428821714372731,3.4485517392015779,3.6290016192869916,1.6232492903979006,4.9613055041429064,3.1095785469043866,2.3789064000232618,3.1005083945019623,3.6016254795539449,2.638938294887355,3.0791812460476247,5.5264138507238521,4.0251009610468138,1.1760912590556813,4.6201568839458202,4.9099859925054599,4.676405508271646,3.2073542607973531,2.7074168686367091,2.325720858019412,1.919078092376074,1.7781512503836436,3.7547381082614368,3.0614524790871931,3.9368152311976328,1.3411601596967189,2.7967130632808965,3.782472624166286,1.9030899869919435,2.4353027522846102,1.7781512503836436,2.0507663112330423,1.146128035678238,1.3010299956639813,3.4939318217735464,4.170788418101762,5.2950002782862713,1.8587777373054493,5.6709345635958428,1.3010299956639813,3.8025000677643934,4.3369018086850204,4.0969100130080562,2.9777236052888476,2.8006071163924688,4.8237910311893311,3.1271047983648077,4.9542027455464295,2.2679925903655827,2.8250884183002145],"y":[5.0515881700656777,5.8727610160415473,5.8490737095759187,4.7462226013444981,6.3430013821759044,6.6634257450191345,6.3759483366683938,6.4937261883939659,6.4318544833785456,6.5020163008992595,6.1058580055927667,6.1773949477884127,5.0334114220803503,6.1597611981428972,5.402892657956925,5.0421420961034791,5.5603135692965964,6.0665959243766761,6.3756214653477112,5.736753892625142,4.731471942742445,6.9174333919315991,6.8251480625396024,5.418205369138013,5.3044434174695363,5.5968840557578563,5.9120111367482533,5.8777204388733191,6.9136142084099328,6.7644264605558222,6.0292960416083083,5.8518480415839758,4.3582485022973074,5.9260136174401596,6.2897647156115983,7.2454579477092231,4.5105765630735748,5.8229370400551606,5.6776183892057688,4.5873656225089716,4.8252157988691131,4.6887312727370523,6.4468437905203402,6.3795516717211767,4.6128141556596169,5.1866128451751896,5.5112389357843963,6.3538138181196979,6.4461972439288031,6.7421439984565819,5.5986086147801277,5.8695541264854967,4.2463762750764911,5.4833528116881611,6.8064709658785869,6.2574306329912286,6.9485434090980753,5.6996151916941535,6.8854999422362919,4.9027941974221516,5.3274815883641509,5.4976815486712338,5.529135577739968,5.7340247059482863,6.989804456672414,5.4974994553467127,4.5809812161246253,5.1844554283966708,6.9237406718521655,7.2934075879266871,5.9697222715144012,7.0977788428141997,5.2499009024211363,6.4607177099954338,4.2823313852198721,4.2790604674412691,4.7094741695804618,5.9636070951575446,4.7777794412429708,4.7154769746499934,5.8202586131059011,5.6167789298536466,4.8061341835013067,4.4157398309047533,4.8624875962154892,5.8509797292050756,5.3466103190832222,4.5809812161246253,5.4185151055695631,5.7066575587108392,6.3404878772957645,5.5463466311731011,5.2658069592076471,4.9646598309047532,6.0434193791186317,5.6146069173069231,5.2071781787712537,6.5057163482291376,5.3228349616762873,5.690054468269631,5.591936464828156,5.9361986266481566,5.6115792489292673,6.8764967354678834,6.0093039865643707,5.6986099397399563,4.9432969260993627,6.5312034152429082,4.4823686739390443,5.2110848694309322,6.0153233957033496,5.3514511991460552,6.0715719211918682,5.4757162948762934,6.1213495274438614,5.7849603742906872,5.4569538133272388,6.6609155852068289,6.4842704614646909,4.6537820414151536,5.6623725872547332,4.715612540719615,5.5977584217168133,6.4780113852198724,5.1607786717071544,5.5870272036505053,4.9798324119564246,4.9279113991407169,5.2032033937072057,5.0982628036726485,4.5442327843605899,4.9904571840889851,6.0821280226678098,5.4546756383254014,5.7465752923902365,5.8091399894118059,5.2854109265845786,4.6568983332020606,6.3007427088739778,5.1275052871431122,5.7903873609442149,6.0384538195985673,5.7441381086660872,6.0091813529505629,6.6173513382401081,6.6620485592852852,6.5190617055422795,6.2838552554075804,6.3761017347339717,6.5918223673543181,4.953504910562204,5.2718911666714705,6.160999469101748,6.6191347882011442,5.274523512827427,6.0626753596867493,6.4535447038051963,5.1416491814668088,6.1089198353748264,4.8747155548003347,6.5245627123657703,5.5900323279611266,4.8061341835013067,5.9254956614523389,5.3057988078344449,5.9643030293632835,5.1868811546317692,6.8100028093636737,5.7717606625343647,4.2823313852198721,4.6408927982814339,4.9644554064871897,4.9713755687211787,5.3507078366509466,5.8966206072517728,4.8645696437467771,5.178308677237645,4.9552684744424651,6.2540453792519664,4.3625439865643711,4.2572141835013069,5.9584028753462075,6.3152176069663959,5.6093299470530233,5.1366169539410516,4.5442327843605899,4.4470955071632083,5.4552200389433674,5.1376081941509142,5.4236355548003345,5.4150087563873566,5.7851366068604255,5.0458434842110593,4.6562783252400797,4.378991399140717,5.1520024808996974,5.3057271489180282,4.9462583670026135,5.8565877048785309,5.929067544735128,5.2202810126552794,5.4622497536099086,6.1301205892605362,5.3306716292886511,5.9928079174051412,5.383456349977239,4.4998131308276639,4.703438583229703,4.9713755687211787,5.3284608598714023,5.563831466122096,5.7129908366368998,6.1446505968031682,4.5768590455162368,5.8072958920887459,4.2166753497867973,5.6472216900485606,4.9759664959892564,4.6128141556596169,7.1236236197404903,5.44096867145422,4.4439697860844944,5.049688614780127,4.8359721915034246,5.3985183530817684,4.6176014163942121,4.8786560234738552,4.831251385219872,5.1515882238762387,5.1844554283966708,5.1271610373692038,5.3502380009704309,6.0280558153544206,5.4538728868989184,5.6150443299232089,5.735102920934219,5.2163560835767715,4.9499135198544302,4.7626700139208449,5.6301812659878614,5.2474176203102116,5.4703879548170651,5.444655835858951,4.9019096215389215,5.1338327518747366,5.8727308206106006,6.0505728102016629,6.2383974226145718,5.5969724049205167,6.2977264454604907,5.5821612039065878,5.6831410249595384,5.2122328796619266,5.5055759048204251,5.8440626013444987,4.7157480297411691,6.2161616640640389,5.1243889953562061,5.5113214627620293,5.1858061001514502,5.0809248257757442,5.5734914130615607,5.080690500054561,5.3476180929170836,5.6665215496351919,5.0213714599553168,4.8428826152653421,4.8937971701681411,5.0680355826420236,5.7851366068604255,5.2911278219613731,4.9387846224203491,4.5669540570976421,4.7780555408794898,5.5987039528845797,4.3616910592609432,4.2701034266350266,4.9527900886562888,5.6653712799632014,5.1560650458253141,4.2823313852198721,6.6352557947162332,4.8539726579569251,4.9181797243807965,6.231498492930764,5.6179248400101525,6.2164934900537094,6.1334647548655123,6.2002701263252824,4.3943769260993628,5.8170458443403819,5.2256513322728964,4.7094741695804618,5.4304892115832955,4.447572770439745,6.32877543584822,6.32877543584822,6.5152704352615336,5.8606275920979343,6.4570976511542941,5.7608527557504843,6.5811812134107459,6.578578094007705,6.7464225986306179,5.6065792289151268,4.6364547172207882,5.263212338710785,5.2036437132341264,5.8471509831605868,5.8432088195846035,6.2373205977155708,4.5361508942265676,5.5968379475413146,6.0891392393791532,5.9786939259394876,5.7565543306636133,5.8467846041356752,5.6301123448690511,4.8814533650140737,5.1346887897876936,5.9801189894971589,5.5356190061034862,5.2519507252568101,5.7106541556596175,4.858268142223988,5.7702708753916152,4.5320048257757435,5.7442055106625922,6.2712053857050876,4.378991399140717,5.0336155256262138,6.4372094711742758,6.6067192166005313,5.4768313991407167,5.238648268119741,6.1712596157260045,5.8575998219878329,6.0103319649989952,6.1695953597468254,5.4006061588267738,4.6163634955710657,4.4511221103511929,4.8747155548003347,4.7289222614647244,5.9511616072281823,5.2987422652037122,4.6408927982814339,4.7094741695804618,5.2877057299773824,4.5442327843605899,6.1864914147197423,5.1261598926231704,5.6855369539410514,4.4289872903748551,6.2236204315175971,6.8243448396965878,5.8959744836549479,5.0567352241652905,5.602533450618008,6.1648957191688227,4.884447229560255,6.0589252653031096,6.0086645773468748,5.4617372750471684,6.3842439331848055,5.3035081237868216,4.9687119328872704,6.2389567938140349,6.5589962852334844,4.8147443669607544,5.8440626013444987,5.6494277934356019,6.6602785331363172,5.2614340808356417,5.4481560279795254,4.7626700139208449,4.2365872041220243,4.2374805113215421,6.3446285005315683,5.1243401964880508,5.9916371701681417,6.7806730323255309,4.2407667709249601,5.0599536925080013,4.7529383391609246,6.1034189576320195,5.497006377324646,5.4769902748060479,5.6351258977155432,6.2623602691577247,5.4244684732862929,5.0562718752788864,5.3407258331177632,6.5394454128879307,6.4785400309379533,5.3572990427071554,6.1562795288529184,4.9509214580148493,6.361567116183223,5.97756833041249,6.1962905662257306,6.2341547981610912,4.9222829741647773,6.0614782771069944,5.7363783369655046,6.1050241877653377,5.891564921602308,6.0228251186011867,6.7895020785094502,5.8571166908483345,6.0199194804033755,6.9861238515486956,5.9230802481719405,5.7920133894862023,6.4156224350560391,5.3696935661043375,6.1588599623231257,5.7580057846749977,5.9958727867165287,4.9939764105756472,6.7338081967042154,4.9963437282798377,6.0895731387065819,6.4769617581643457,6.3032524102288399,4.7780555408794898,6.5411990492611007,5.1878178667485599,6.0738993926670233,5.3316963471630423,6.0188094212855692,6.5261719568908543,6.1143240425673735,5.6761650801749397,6.0193299969796152,5.8380092003511654,5.3319883164916462,5.9854260394367893,5.0399569400202076,4.6993282585269043,6.4636495857511793,6.4163632015311247,6.9638518173173791,4.447572770439745,6.550461892047089,5.9357210211544373,5.1391856551722688,4.66601,5.8256243455714607,5.6491193937457531,4.2086092664290016,6.7271681665374601,5.3729101255932994,6.2685480444327784,5.2642807137713143,5.4107794194962633,5.8014008046766286,4.7626700139208449,6.1726580219041001,5.8619676915506753,5.0560769998727508,5.4700611652840472,5.8748502836527567,4.7130430811783874,6.5017469180245087,4.7896867709249609,6.6582189610584877,5.5111219736394768,5.6783371278741557,4.1398112727370524,4.627266636703089,5.6037964005019463,5.2732203073258335,5.99107720654799,4.5007686147801271,4.8471020692568434,6.7342929552067208,6.8773910635165976,6.1640146121022177,5.7387327982814345,4.7248596965391076,5.780551245323668,4.7765609094819821,5.6492979286964342,5.8307293828397135,5.6153320853958064,5.7696388412168336,5.7934389677037741,4.4949774086937282,4.9721424887772683,5.4953732691774198,6.3042080954424211,5.7306166538292258,6.4942038496757766,4.8959607325478913,4.693026757004116,4.2701034266350266,6.2978229479870969,6.8651040271765638,5.8603096797100065,4.6505481750963895,5.049688614780127,4.5007686147801271,4.7632731789823559,4.7375528122022788,5.0680355826420236,4.7094741695804618,5.2149299999999998,5.8715957221561679,4.447572770439745,4.447572770439745,6.9254322240984996,5.9246149137081723,4.7285557849482682,5.5454127704397447,5.882347742009161,4.9663701103351787,4.3943769260993628,5.5582894662471105,5.1019289751680459,4.7780555408794898,5.5016248819643554,5.5314849211109305,5.1131495368483844,5.4723506766365109,5.1476799049879656,4.5007686147801271,5.3580810181580896,5.008124000485215,5.7913392897432683,6.5686358127899585,5.6699394279226309,4.9713755687211787,5.975688223928362,6.4530795281531752,5.0434377663726249,5.8758955408794904,5.1703666929966197,5.1651836549685175,4.9964927704397448,5.1198500511310385,4.6681459389707651,6.0901726029306555,5.9371289659108282,5.0737928815453479,5.4611490206825302,5.5602015688590152,4.4592040004852151,6.2915298173341236,5.2750798559667551,4.8739993011007687,5.2701010679100166,5.5451742582367514,5.0167360088295663,5.2583941695804617,6.6017290909393367,5.7776284195378169,4.2137500139208441,6.1042665167355388,6.2633595110060973,6.1351425116004714,5.3287509008368827,5.0543252675320618,4.844804693384015,4.621590346467074,4.5442327843605899,5.6292208423868679,5.2486624948205414,5.7291666167090041,4.3043596348607229,5.1033417346961496,5.644444872857358,4.6128141556596169,4.9049563867840682,4.5442327843605899,4.6938766435620414,4.1973026013444983,4.2823313852198721,5.4860590556079352,5.8575991784644188,6.4747015527568994,4.588490275561707,6.6810594006490298,4.2823313852198721,5.6554383371972303,5.948782140823381,5.8170458443403819,5.2027020414151544,5.1054792583301536,6.2160453728404477,5.2847003659184102,6.2876309710853455,4.8131164927034753,5.1189175345733533],"text":["Quantity: 5.040000e+02<br />Predicted Cost = $47,288<br />reg_preds:   112612.91","Quantity: 1.579100e+04<br />Predicted Cost = $1,144,271<br />reg_preds:   746038.11","Quantity: 1.429740e+04<br />Predicted Cost = $1,043,789<br />reg_preds:   706437.44","Quantity: 1.400000e+02<br />Predicted Cost = $14,460<br />reg_preds:    55747.14","Quantity: 1.135205e+05<br />Predicted Cost = $7,094,858<br />reg_preds:  2202933.47","Quantity: 4.353217e+05<br />Predicted Cost = $24,597,984<br />reg_preds:  4607079.91","Quantity: 1.303454e+05<br />Predicted Cost = $8,062,385<br />reg_preds:  2376557.56","Quantity: 2.136282e+05<br />Predicted Cost = $12,733,101<br />reg_preds:  3116923.82","Quantity: 1.647947e+05<br />Predicted Cost = $10,015,497<br />reg_preds:  2703052.52","Quantity: 2.211878e+05<br />Predicted Cost = $13,149,344<br />reg_preds:  3176993.31","Quantity: 4.198133e+04<br />Predicted Cost = $2,827,008<br />reg_preds:  1276021.54","Quantity: 5.667340e+04<br />Predicted Cost = $3,731,435<br />reg_preds:  1504509.55","Quantity: 4.670000e+02<br />Predicted Cost = $44,067<br />reg_preds:   107996.93","Quantity: 5.263260e+04<br />Predicted Cost = $3,484,662<br />reg_preds:  1444645.20","Quantity: 2.200000e+03<br />Predicted Cost = $184,817<br />reg_preds:   252867.29","Quantity: 4.844200e+02<br />Predicted Cost = $45,586<br />reg_preds:   110189.98","Quantity: 4.258000e+03<br />Predicted Cost = $340,420<br />reg_preds:   363340.30","Quantity: 3.560657e+04<br />Predicted Cost = $2,427,534<br />reg_preds:  1165724.50","Quantity: 1.301668e+05<br />Predicted Cost = $8,052,166<br />reg_preds:  2374769.52","Quantity: 8.925600e+03<br />Predicted Cost = $675,056<br />reg_preds:   545448.68","Quantity: 1.316000e+02<br />Predicted Cost = $13,656<br />reg_preds:    53885.50","Quantity: 1.263430e+06<br />Predicted Cost = $65,907,480<br />reg_preds:  8268626.82","Quantity: 8.578856e+05<br />Predicted Cost = $46,070,422<br />reg_preds:  6685718.13","Quantity: 2.345950e+03<br />Predicted Cost = $196,130<br />reg_preds:   261942.14","Quantity: 1.455700e+03<br />Predicted Cost = $126,137<br />reg_preds:   201578.13","Quantity: 4.963960e+03<br />Predicted Cost = $392,321<br />reg_preds:   395261.08","Quantity: 1.861718e+04<br />Predicted Cost = $1,332,510<br />reg_preds:   816603.31","Quantity: 1.612295e+04<br />Predicted Cost = $1,166,504<br />reg_preds:   754606.32","Quantity: 1.243350e+06<br />Predicted Cost = $64,937,998<br />reg_preds:  8196231.35","Quantity: 6.649810e+05<br />Predicted Cost = $36,399,764<br />reg_preds:  5813349.86","Quantity: 3.044940e+04<br />Predicted Cost = $2,100,440<br />reg_preds:  1069783.86","Quantity: 1.446476e+04<br />Predicted Cost = $1,055,086<br />reg_preds:   710964.71","Quantity: 2.750000e+01<br />Predicted Cost = $3,209<br />reg_preds:    22816.47","Quantity: 1.974345e+04<br />Predicted Cost = $1,406,910<br />reg_preds:   843361.20","Quantity: 9.080085e+04<br />Predicted Cost = $5,770,762<br />reg_preds:  1948788.53","Quantity: 5.001874e+06<br />Predicted Cost = $235,341,107<br />reg_preds: 17597782.58","Quantity: 5.210000e+01<br />Predicted Cost = $5,795<br />reg_preds:    32402.35","Quantity: 1.281275e+04<br />Predicted Cost = $943,125<br />reg_preds:   665176.72","Quantity: 6.964770e+03<br />Predicted Cost = $536,647<br />reg_preds:   476012.53","Quantity: 7.190000e+01<br />Predicted Cost = $7,807<br />reg_preds:    38669.24","Quantity: 1.950000e+02<br />Predicted Cost = $19,646<br />reg_preds:    66867.61","Quantity: 1.100000e+02<br />Predicted Cost = $11,569<br />reg_preds:    48835.01","Quantity: 1.754891e+05<br />Predicted Cost = $10,615,278<br />reg_preds:  2797974.75","Quantity: 1.323306e+05<br />Predicted Cost = $8,175,901<br />reg_preds:  2396357.85","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />reg_preds:    41002.86","Quantity: 8.880000e+02<br />Predicted Cost = $79,851<br />reg_preds:   153678.41","Quantity: 3.465800e+03<br />Predicted Cost = $281,396<br />reg_preds:   324518.11","Quantity: 1.187878e+05<br />Predicted Cost = $7,398,847<br />reg_preds:  2258467.36","Quantity: 1.750138e+05<br />Predicted Cost = $10,588,681<br />reg_preds:  2793812.42","Quantity: 6.056420e+05<br />Predicted Cost = $33,384,877<br />reg_preds:  5522605.21","Quantity: 5.000000e+03<br />Predicted Cost = $394,955<br />reg_preds:   396833.76","Quantity: 1.558000e+04<br />Predicted Cost = $1,130,121<br />reg_preds:   740549.56","Quantity: 1.720000e+01<br />Predicted Cost = $2,079<br />reg_preds:    17635.03","Quantity: 3.083200e+03<br />Predicted Cost = $252,538<br />reg_preds:   304335.64","Quantity: 7.932393e+05<br />Predicted Cost = $42,849,811<br />reg_preds:  6404289.65","Quantity: 7.928390e+04<br />Predicted Cost = $5,090,332<br />reg_preds:  1808966.95","Quantity: 1.439547e+06<br />Predicted Cost = $74,363,366<br />reg_preds:  8882667.57","Quantity: 7.638000e+03<br />Predicted Cost = $584,462<br />reg_preds:   500743.35","Quantity: 1.105035e+06<br />Predicted Cost = $58,226,781<br />reg_preds:  7682453.53","Quantity: 2.700000e+02<br />Predicted Cost = $26,547<br />reg_preds:    79945.53","Quantity: 1.603400e+03<br />Predicted Cost = $137,932<br />reg_preds:   212560.02","Quantity: 3.274200e+03<br />Predicted Cost = $266,976<br />reg_preds:   314544.10","Quantity: 3.736000e+03<br />Predicted Cost = $301,631<br />reg_preds:   338170.39","Quantity: 8.824000e+03<br />Predicted Cost = $667,945<br />reg_preds:   542031.72","Quantity: 1.711566e+06<br />Predicted Cost = $87,274,858<br />reg_preds:  9767973.14","Quantity: 3.271700e+03<br />Predicted Cost = $266,787<br />reg_preds:   314412.25","Quantity: 7.000000e+01<br />Predicted Cost = $7,616<br />reg_preds:    38104.93","Quantity: 8.800000e+02<br />Predicted Cost = $79,186<br />reg_preds:   152916.88","Quantity: 1.297303e+06<br />Predicted Cost = $67,540,345<br />reg_preds:  8389588.73","Quantity: 6.116253e+06<br />Predicted Cost = $283,464,732<br />reg_preds: 19652037.67","Quantity: 2.371645e+04<br />Predicted Cost = $1,666,944<br />reg_preds:   932657.68","Quantity: 2.692136e+06<br />Predicted Cost = $132,690,466<br />reg_preds: 12525031.96","Quantity: 1.158000e+03<br />Predicted Cost = $102,078<br />reg_preds:   177787.37","Quantity: 1.860052e+05<br />Predicted Cost = $11,202,390<br />reg_preds:  2888801.56","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />reg_preds:    19157.17","Quantity: 1.972746e+01<br />Predicted Cost = $2,360<br />reg_preds:    19013.43","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />reg_preds:    51224.08","Quantity: 2.311582e+04<br />Predicted Cost = $1,627,856<br />reg_preds:   919617.22","Quantity: 1.598148e+02<br />Predicted Cost = $16,344<br />reg_preds:    59948.65","Quantity: 1.230600e+02<br />Predicted Cost = $12,834<br />reg_preds:    51937.01","Quantity: 1.266960e+04<br />Predicted Cost = $933,374<br />reg_preds:   661086.99","Quantity: 5.396000e+03<br />Predicted Cost = $423,805<br />reg_preds:   413788.99","Quantity: 1.800000e+02<br />Predicted Cost = $18,244<br />reg_preds:    63993.25","Quantity: 3.500000e+01<br />Predicted Cost = $4,011<br />reg_preds:    26045.93","Quantity: 2.280000e+02<br />Predicted Cost = $22,703<br />reg_preds:    72859.74","Quantity: 1.441217e+04<br />Predicted Cost = $1,051,537<br />reg_preds:   709544.65","Quantity: 1.737360e+03<br />Predicted Cost = $148,559<br />reg_preds:   222131.59","Quantity: 7.000000e+01<br />Predicted Cost = $7,616<br />reg_preds:    38104.93","Quantity: 2.349000e+03<br />Predicted Cost = $196,366<br />reg_preds:   262129.02","Quantity: 7.867000e+03<br />Predicted Cost = $600,653<br />reg_preds:   508929.42","Quantity: 1.123299e+05<br />Predicted Cost = $7,026,000<br />reg_preds:  2190220.69","Quantity: 4.015700e+03<br />Predicted Cost = $322,462<br />reg_preds:   351841.15","Quantity: 1.237900e+03<br />Predicted Cost = $108,576<br />reg_preds:   184419.55","Quantity: 3.500000e+02<br />Predicted Cost = $33,749<br />reg_preds:    92184.91","Quantity: 3.230785e+04<br />Predicted Cost = $2,218,758<br />reg_preds:  1105145.30","Quantity: 5.347060e+03<br />Predicted Cost = $420,249<br />reg_preds:   411724.70","Quantity: 9.680060e+02<br />Predicted Cost = $86,484<br />reg_preds:   161130.66","Quantity: 2.246476e+05<br />Predicted Cost = $13,339,487<br />reg_preds:  3204175.89","Quantity: 1.572450e+03<br />Predicted Cost = $135,467<br />reg_preds:   210297.91","Quantity: 7.337740e+03<br />Predicted Cost = $563,177<br />reg_preds:   489840.25","Quantity: 4.862000e+03<br />Predicted Cost = $384,861<br />reg_preds:   390783.72","Quantity: 2.060524e+04<br />Predicted Cost = $1,463,623<br />reg_preds:   863373.33","Quantity: 5.279580e+03<br />Predicted Cost = $415,341<br />reg_preds:   408864.35","Quantity: 1.064080e+06<br />Predicted Cost = $56,227,818<br />reg_preds:  7524830.74","Quantity: 2.800000e+04<br />Predicted Cost = $1,943,664<br />reg_preds:  1021654.35","Quantity: 7.605860e+03<br />Predicted Cost = $582,187<br />reg_preds:   499585.63","Quantity: 3.200000e+02<br />Predicted Cost = $31,064<br />reg_preds:    87760.06","Quantity: 2.499960e+05<br />Predicted Cost = $14,726,111<br />reg_preds:  3397843.84","Quantity: 4.628601e+01<br />Predicted Cost = $5,194<br />reg_preds:    30364.68","Quantity: 9.840000e+02<br />Predicted Cost = $87,805<br />reg_preds:   162586.65","Quantity: 2.871600e+04<br />Predicted Cost = $1,989,595<br />reg_preds:  1035913.27","Quantity: 1.773000e+03<br />Predicted Cost = $151,375<br />reg_preds:   224621.44","Quantity: 3.635760e+04<br />Predicted Cost = $2,474,860<br />reg_preds:  1179157.78","Quantity: 2.986000e+03<br />Predicted Cost = $245,165<br />reg_preds:   299031.06","Quantity: 4.480000e+04<br />Predicted Cost = $3,002,148<br />reg_preds:  1322359.46","Quantity: 1.092592e+04<br />Predicted Cost = $813,905<br />reg_preds:   609481.28","Quantity: 2.760000e+03<br />Predicted Cost = $227,951<br />reg_preds:   286387.34","Quantity: 4.307620e+05<br />Predicted Cost = $24,359,568<br />reg_preds:  4580528.45","Quantity: 2.053206e+05<br />Predicted Cost = $12,274,395<br />reg_preds:  3049793.69","Quantity: 9.500000e+01<br />Predicted Cost = $10,102<br />reg_preds:    45059.05","Quantity: 6.533300e+03<br />Predicted Cost = $505,822<br />reg_preds:   459592.13","Quantity: 1.231300e+02<br />Predicted Cost = $12,841<br />reg_preds:    51953.23","Quantity: 4.982200e+03<br />Predicted Cost = $393,654<br />reg_preds:   396057.66","Quantity: 2.000000e+05<br />Predicted Cost = $11,979,888<br />reg_preds:  3006155.11","Quantity: 7.968000e+02<br />Predicted Cost = $72,235<br />reg_preds:   144803.37","Quantity: 4.762900e+03<br />Predicted Cost = $377,599<br />reg_preds:   386391.18","Quantity: 3.730000e+02<br />Predicted Cost = $35,796<br />reg_preds:    95462.41","Quantity: 3.000000e+02<br />Predicted Cost = $29,264<br />reg_preds:    84705.46","Quantity: 9.520000e+02<br />Predicted Cost = $85,161<br />reg_preds:   159662.67","Quantity: 6.130000e+02<br />Predicted Cost = $56,676<br />reg_preds:   125389.97","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />reg_preds:    35013.28","Quantity: 3.900000e+02<br />Predicted Cost = $37,302<br />reg_preds:    97826.65","Quantity: 3.800370e+04<br />Predicted Cost = $2,578,333<br />reg_preds:  1208169.93","Quantity: 2.733750e+03<br />Predicted Cost = $225,945<br />reg_preds:   284888.97","Quantity: 9.301000e+03<br />Predicted Cost = $701,278<br />reg_preds:   557924.32","Quantity: 1.209226e+04<br />Predicted Cost = $893,963<br />reg_preds:   644376.94","Quantity: 1.344000e+03<br />Predicted Cost = $117,157<br />reg_preds:   192934.96","Quantity: 9.625000e+01<br />Predicted Cost = $10,225<br />reg_preds:    45383.54","Quantity: 9.508000e+04<br />Predicted Cost = $6,021,885<br />reg_preds:  1998677.43","Quantity: 6.930000e+02<br />Predicted Cost = $63,486<br />reg_preds:   134123.63","Quantity: 1.117750e+04<br />Predicted Cost = $831,225<br />reg_preds:   617145.21","Quantity: 3.164186e+04<br />Predicted Cost = $2,176,418<br />reg_preds:  1092581.44","Quantity: 9.206397e+03<br />Predicted Cost = $694,677<br />reg_preds:   554802.12","Quantity: 2.798560e+04<br />Predicted Cost = $1,942,739<br />reg_preds:  1021365.90","Quantity: 3.588177e+05<br />Predicted Cost = $20,571,136<br />reg_preds:  4143347.30","Quantity: 4.328141e+05<br />Predicted Cost = $24,466,891<br />reg_preds:  4592493.60","Quantity: 2.375821e+05<br />Predicted Cost = $14,048,429<br />reg_preds:  3304164.84","Quantity: 8.857768e+04<br />Predicted Cost = $5,639,946<br />reg_preds:  1922450.89","Quantity: 1.304293e+05<br />Predicted Cost = $8,067,185<br />reg_preds:  2377397.13","Quantity: 3.223786e+05<br />Predicted Cost = $18,631,110<br />reg_preds:  3906810.69","Quantity: 3.340000e+02<br />Predicted Cost = $32,320<br />reg_preds:    89847.28","Quantity: 1.269900e+03<br />Predicted Cost = $111,170<br />reg_preds:   187021.34","Quantity: 5.290670e+04<br />Predicted Cost = $3,501,445<br />reg_preds:  1448770.08","Quantity: 3.615121e+05<br />Predicted Cost = $20,713,983<br />reg_preds:  4160397.13","Quantity: 1.284000e+03<br />Predicted Cost = $112,311<br />reg_preds:   188158.36","Quantity: 3.502578e+04<br />Predicted Cost = $2,390,885<br />reg_preds:  1155248.36","Quantity: 1.804918e+05<br />Predicted Cost = $10,894,901<br />reg_preds:  2841480.66","Quantity: 7.353600e+02<br />Predicted Cost = $67,068<br />reg_preds:   138563.61","Quantity: 4.252400e+04<br />Predicted Cost = $2,860,794<br />reg_preds:  1285049.44","Quantity: 2.400000e+02<br />Predicted Cost = $23,806<br />reg_preds:    74940.32","Quantity: 2.431282e+05<br />Predicted Cost = $14,351,512<br />reg_preds:  3346283.35","Quantity: 4.823320e+03<br />Predicted Cost = $382,028<br />reg_preds:   389074.11","Quantity: 1.800000e+02<br />Predicted Cost = $18,244<br />reg_preds:    63993.25","Quantity: 1.970060e+04<br />Predicted Cost = $1,404,085<br />reg_preds:   842355.98","Quantity: 1.464000e+03<br />Predicted Cost = $126,802<br />reg_preds:   202208.22","Quantity: 2.318340e+04<br />Predicted Cost = $1,632,258<br />reg_preds:   921092.04","Quantity: 8.890000e+02<br />Predicted Cost = $79,934<br />reg_preds:   153773.38","Quantity: 8.050788e+05<br />Predicted Cost = $43,441,070<br />reg_preds:  6456584.06","Quantity: 1.033740e+04<br />Predicted Cost = $773,269<br />reg_preds:   591235.72","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />reg_preds:    19157.17","Quantity: 9.000000e+01<br />Predicted Cost = $9,609<br />reg_preds:    43741.41","Quantity: 3.497000e+02<br />Predicted Cost = $33,722<br />reg_preds:    92141.53","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />reg_preds:    93621.49","Quantity: 1.767480e+03<br />Predicted Cost = $150,940<br />reg_preds:   224237.29","Quantity: 1.745324e+04<br />Predicted Cost = $1,255,265<br />reg_preds:   788171.28","Quantity: 2.300000e+02<br />Predicted Cost = $22,887<br />reg_preds:    73209.87","Quantity: 8.576000e+02<br />Predicted Cost = $77,319<br />reg_preds:   150767.83","Quantity: 3.364800e+02<br />Predicted Cost = $32,541<br />reg_preds:    90212.86","Quantity: 7.816600e+04<br />Predicted Cost = $5,023,906<br />reg_preds:  1794921.17","Quantity: 2.800000e+01<br />Predicted Cost = $3,263<br />reg_preds:    23043.26","Quantity: 1.800000e+01<br />Predicted Cost = $2,168<br />reg_preds:    18080.66","Quantity: 2.261666e+04<br />Predicted Cost = $1,595,315<br />reg_preds:   908663.06","Quantity: 1.010320e+05<br />Predicted Cost = $6,369,781<br />reg_preds:  2066415.29","Quantity: 5.230000e+03<br />Predicted Cost = $411,731<br />reg_preds:   406752.23","Quantity: 7.200000e+02<br />Predicted Cost = $65,771<br />reg_preds:   136967.32","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />reg_preds:    35013.28","Quantity: 3.992000e+01<br />Predicted Cost = $4,530<br />reg_preds:    27995.97","Quantity: 2.740000e+03<br />Predicted Cost = $226,422<br />reg_preds:   285246.31","Quantity: 7.230000e+02<br />Predicted Cost = $66,024<br />reg_preds:   137280.29","Quantity: 2.400000e+03<br />Predicted Cost = $200,307<br />reg_preds:   265237.88","Quantity: 2.314703e+03<br />Predicted Cost = $193,713<br />reg_preds:   260021.20","Quantity: 1.093400e+04<br />Predicted Cost = $814,462<br />reg_preds:   609728.66","Quantity: 4.920000e+02<br />Predicted Cost = $46,245<br />reg_preds:   111133.11","Quantity: 9.600000e+01<br />Predicted Cost = $10,200<br />reg_preds:    45318.79","Quantity: 3.000000e+01<br />Predicted Cost = $3,478<br />reg_preds:    23932.68","Quantity: 7.680000e+02<br />Predicted Cost = $69,817<br />reg_preds:   141906.56","Quantity: 1.463560e+03<br />Predicted Cost = $126,767<br />reg_preds:   202174.86","Quantity: 3.240000e+02<br />Predicted Cost = $31,423<br />reg_preds:    88360.54","Quantity: 1.475522e+04<br />Predicted Cost = $1,074,669<br />reg_preds:   718766.30","Quantity: 1.999800e+04<br />Predicted Cost = $1,423,681<br />reg_preds:   849312.56","Quantity: 1.022700e+03<br />Predicted Cost = $90,995<br />reg_preds:   166066.11","Quantity: 2.822000e+03<br />Predicted Cost = $232,683<br />reg_preds:   289901.03","Quantity: 4.647900e+04<br />Predicted Cost = $3,106,079<br />reg_preds:  1349337.50","Quantity: 1.625000e+03<br />Predicted Cost = $139,649<br />reg_preds:   214127.10","Quantity: 2.612800e+04<br />Predicted Cost = $1,823,153<br />reg_preds:   983575.99","Quantity: 2.027750e+03<br />Predicted Cost = $171,391<br />reg_preds:   241800.03","Quantity: 4.980000e+01<br />Predicted Cost = $5,558<br />reg_preds:    31609.17","Quantity: 1.170000e+02<br />Predicted Cost = $12,248<br />reg_preds:    50517.12","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />reg_preds:    93621.49","Quantity: 1.610000e+03<br />Predicted Cost = $138,457<br />reg_preds:   213039.86","Quantity: 4.321300e+03<br />Predicted Cost = $345,099<br />reg_preds:   366295.40","Quantity: 8.078800e+03<br />Predicted Cost = $615,596<br />reg_preds:   516405.47","Quantity: 4.940000e+04<br />Predicted Cost = $3,286,226<br />reg_preds:  1395245.39","Quantity: 6.880000e+01<br />Predicted Cost = $7,495<br />reg_preds:    37744.97","Quantity: 1.199908e+04<br />Predicted Cost = $887,589<br />reg_preds:   641646.59","Quantity: 1.518520e+01<br />Predicted Cost = $1,853<br />reg_preds:    16469.31","Quantity: 6.131000e+03<br />Predicted Cost = $476,943<br />reg_preds:   443835.15","Quantity: 3.670000e+02<br />Predicted Cost = $35,263<br />reg_preds:    94616.42","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />reg_preds:    41002.86","Quantity: 3.000406e+06<br />Predicted Cost = $146,686,943<br />reg_preds: 13293018.82","Quantity: 2.581000e+03<br />Predicted Cost = $214,242<br />reg_preds:   276037.87","Quantity: 3.940000e+01<br />Predicted Cost = $4,475<br />reg_preds:    27795.20","Quantity: 5.000000e+02<br />Predicted Cost = $46,940<br />reg_preds:   112121.43","Quantity: 2.040000e+02<br />Predicted Cost = $20,484<br />reg_preds:    68544.43","Quantity: 2.160000e+03<br />Predicted Cost = $181,706<br />reg_preds:   250333.14","Quantity: 8.162275e+01<br />Predicted Cost = $8,779<br />reg_preds:    41457.34","Quantity: 2.440000e+02<br />Predicted Cost = $24,173<br />reg_preds:    75623.37","Quantity: 2.000000e+02<br />Predicted Cost = $20,112<br />reg_preds:    67803.39","Quantity: 7.666666e+02<br />Predicted Cost = $69,705<br />reg_preds:   141771.27","Quantity: 8.800000e+02<br />Predicted Cost = $79,186<br />reg_preds:   152916.88","Quantity: 6.920000e+02<br />Predicted Cost = $63,401<br />reg_preds:   134017.35","Quantity: 1.764000e+03<br />Predicted Cost = $150,665<br />reg_preds:   223994.83","Quantity: 3.029140e+04<br />Predicted Cost = $2,090,357<br />reg_preds:  1066733.21","Quantity: 2.724560e+03<br />Predicted Cost = $225,242<br />reg_preds:   284362.87","Quantity: 5.356880e+03<br />Predicted Cost = $420,963<br />reg_preds:   412139.59","Quantity: 8.864000e+03<br />Predicted Cost = $670,745<br />reg_preds:   543379.09","Quantity: 1.006000e+03<br />Predicted Cost = $89,620<br />reg_preds:   164572.05","Quantity: 3.290060e+02<br />Predicted Cost = $31,872<br />reg_preds:    89107.35","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />reg_preds:    57898.86","Quantity: 5.708050e+03<br />Predicted Cost = $446,428<br />reg_preds:   426757.60","Quantity: 1.146000e+03<br />Predicted Cost = $101,099<br />reg_preds:   176773.69","Quantity: 2.920000e+03<br />Predicted Cost = $240,148<br />reg_preds:   295384.67","Quantity: 2.621230e+03<br />Predicted Cost = $217,329<br />reg_preds:   278391.41","Quantity: 2.690000e+02<br />Predicted Cost = $26,456<br />reg_preds:    79782.86","Quantity: 7.116400e+02<br />Predicted Cost = $65,064<br />reg_preds:   136092.05","Quantity: 1.578900e+04<br />Predicted Cost = $1,144,137<br />reg_preds:   745986.25","Quantity: 3.329200e+04<br />Predicted Cost = $2,281,205<br />reg_preds:  1123499.31","Quantity: 7.320000e+04<br />Predicted Cost = $4,727,949<br />reg_preds:  1731400.04","Quantity: 4.965800e+03<br />Predicted Cost = $392,455<br />reg_preds:   395341.50","Quantity: 9.388458e+04<br />Predicted Cost = $5,951,819<br />reg_preds:  1984844.30","Quantity: 4.666667e+03<br />Predicted Cost = $370,537<br />reg_preds:   382086.07","Quantity: 7.128000e+03<br />Predicted Cost = $548,271<br />reg_preds:   482104.32","Quantity: 9.887500e+02<br />Predicted Cost = $88,197<br />reg_preds:   163016.99","Quantity: 3.384440e+03<br />Predicted Cost = $275,280<br />reg_preds:   320313.99","Quantity: 1.400000e+04<br />Predicted Cost = $1,023,690<br />reg_preds:   698333.06","Quantity: 1.232000e+02<br />Predicted Cost = $12,847<br />reg_preds:    51969.44","Quantity: 6.668112e+04<br />Predicted Cost = $4,337,132<br />reg_preds:  1644983.95","Quantity: 6.840000e+02<br />Predicted Cost = $62,723<br />reg_preds:   133164.66","Quantity: 3.467000e+03<br />Predicted Cost = $281,486<br />reg_preds:   324579.78","Quantity: 8.850000e+02<br />Predicted Cost = $79,602<br />reg_preds:   153393.20","Quantity: 5.700000e+02<br />Predicted Cost = $52,989<br />reg_preds:   120482.74","Quantity: 4.500000e+03<br />Predicted Cost = $358,279<br />reg_preds:   374534.14","Quantity: 5.694400e+02<br />Predicted Cost = $52,941<br />reg_preds:   120417.75","Quantity: 1.744720e+03<br />Predicted Cost = $149,141<br />reg_preds:   222647.64","Quantity: 6.648000e+03<br />Predicted Cost = $514,031<br />reg_preds:   464003.81","Quantity: 4.440000e+02<br />Predicted Cost = $42,056<br />reg_preds:   105044.05","Quantity: 2.100000e+02<br />Predicted Cost = $21,040<br />reg_preds:    69643.82","Quantity: 2.600000e+02<br />Predicted Cost = $25,636<br />reg_preds:    78306.38","Quantity: 5.400000e+02<br />Predicted Cost = $50,404<br />reg_preds:   116959.52","Quantity: 1.093400e+04<br />Predicted Cost = $814,462<br />reg_preds:   609728.66","Quantity: 1.376620e+03<br />Predicted Cost = $119,785<br />reg_preds:   195491.47","Quantity: 3.140000e+02<br />Predicted Cost = $30,525<br />reg_preds:    86852.96","Quantity: 6.600000e+01<br />Predicted Cost = $7,212<br />reg_preds:    36893.86","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />reg_preds:    59986.78","Quantity: 5.002000e+03<br />Predicted Cost = $395,101<br />reg_preds:   396920.89","Quantity: 2.790000e+01<br />Predicted Cost = $3,252<br />reg_preds:    22998.05","Quantity: 1.900000e+01<br />Predicted Cost = $2,280<br />reg_preds:    18625.31","Quantity: 3.330000e+02<br />Predicted Cost = $32,230<br />reg_preds:    89699.51","Quantity: 6.616000e+03<br />Predicted Cost = $511,742<br />reg_preds:   462776.48","Quantity: 7.812000e+02<br />Predicted Cost = $70,926<br />reg_preds:   143240.24","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />reg_preds:    19157.17","Quantity: 3.868044e+05<br />Predicted Cost = $22,051,065<br />reg_preds:  4317733.12","Quantity: 2.200000e+02<br />Predicted Cost = $21,965<br />reg_preds:    71445.13","Quantity: 2.880000e+02<br />Predicted Cost = $28,180<br />reg_preds:    82828.49","Quantity: 7.111200e+04<br />Predicted Cost = $4,603,066<br />reg_preds:  1704113.41","Quantity: 5.422000e+03<br />Predicted Cost = $425,694<br />reg_preds:   414882.24","Quantity: 6.677400e+04<br />Predicted Cost = $4,342,720<br />reg_preds:  1646241.29","Quantity: 4.713560e+04<br />Predicted Cost = $3,146,646<br />reg_preds:  1359767.81","Quantity: 6.238100e+04<br />Predicted Cost = $4,077,776<br />reg_preds:  1585879.28","Quantity: 3.200000e+01<br />Predicted Cost = $3,692<br />reg_preds:    24795.73","Quantity: 1.250000e+04<br />Predicted Cost = $921,811<br />reg_preds:   656214.53","Quantity: 1.046000e+03<br />Predicted Cost = $92,911<br />reg_preds:   168132.37","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />reg_preds:    51224.08","Quantity: 2.470000e+03<br />Predicted Cost = $205,705<br />reg_preds:   269456.84","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />reg_preds:    28026.75","Quantity: 1.069444e+05<br />Predicted Cost = $6,713,843<br />reg_preds:  2131942.25","Quantity: 1.069444e+05<br />Predicted Cost = $6,713,843<br />reg_preds:  2131942.25","Quantity: 2.338336e+05<br />Predicted Cost = $13,843,279<br />reg_preds:  3275445.93","Quantity: 1.500740e+04<br />Predicted Cost = $1,091,648<br />reg_preds:   725483.59","Quantity: 1.832020e+05<br />Predicted Cost = $11,046,137<br />reg_preds:  2864822.05","Quantity: 9.875060e+03<br />Predicted Cost = $741,224<br />reg_preds:   576570.95","Quantity: 3.083050e+05<br />Predicted Cost = $17,877,510<br />reg_preds:  3812248.60","Quantity: 3.049568e+05<br />Predicted Cost = $17,697,847<br />reg_preds:  3789466.69","Quantity: 6.166100e+05<br />Predicted Cost = $33,943,746<br />reg_preds:  5577281.93","Quantity: 5.170000e+03<br />Predicted Cost = $407,360<br />reg_preds:   404184.10","Quantity: 8.834000e+01<br />Predicted Cost = $9,445<br />reg_preds:    43296.69","Quantity: 1.224500e+03<br />Predicted Cost = $107,488<br />reg_preds:   183321.05","Quantity: 9.537600e+02<br />Predicted Cost = $85,306<br />reg_preds:   159824.63","Quantity: 1.418255e+04<br />Predicted Cost = $1,036,031<br />reg_preds:   703316.79","Quantity: 1.394995e+04<br />Predicted Cost = $1,020,304<br />reg_preds:   696961.55","Quantity: 7.287010e+04<br />Predicted Cost = $4,708,235<br />reg_preds:  1727112.38","Quantity: 5.800000e+01<br />Predicted Cost = $6,400<br />reg_preds:    34367.73","Quantity: 4.963000e+03<br />Predicted Cost = $392,250<br />reg_preds:   395219.12","Quantity: 3.913800e+04<br />Predicted Cost = $2,649,438<br />reg_preds:  1227832.82","Quantity: 2.462600e+04<br />Predicted Cost = $1,725,994<br />reg_preds:   952124.91","Quantity: 9.698600e+03<br />Predicted Cost = $728,964<br />reg_preds:   570892.49","Quantity: 1.416077e+04<br />Predicted Cost = $1,034,559<br />reg_preds:   702723.71","Quantity: 5.706400e+03<br />Predicted Cost = $446,308<br />reg_preds:   426689.88","Quantity: 2.468800e+02<br />Predicted Cost = $24,437<br />reg_preds:    76112.04","Quantity: 7.142000e+02<br />Predicted Cost = $65,280<br />reg_preds:   136360.56","Quantity: 2.477365e+04<br />Predicted Cost = $1,735,564<br />reg_preds:   955254.27","Quantity: 3.839000e+03<br />Predicted Cost = $309,316<br />reg_preds:   343256.69","Quantity: 1.168000e+03<br />Predicted Cost = $102,893<br />reg_preds:   178628.49","Quantity: 8.000000e+03<br />Predicted Cost = $610,040<br />reg_preds:   513634.46","Quantity: 2.240000e+02<br />Predicted Cost = $22,335<br />reg_preds:    72155.28","Quantity: 1.027300e+04<br />Predicted Cost = $768,812<br />reg_preds:   589211.04","Quantity: 5.700000e+01<br />Predicted Cost = $6,298<br />reg_preds:    34041.20","Quantity: 9.209000e+03<br />Predicted Cost = $694,859<br />reg_preds:   554888.23","Quantity: 8.400000e+04<br />Predicted Cost = $5,369,803<br />reg_preds:  1867262.54","Quantity: 3.000000e+01<br />Predicted Cost = $3,478<br />reg_preds:    23932.68","Quantity: 4.674000e+02<br />Predicted Cost = $44,102<br />reg_preds:   108047.70","Quantity: 1.685383e+05<br />Predicted Cost = $10,225,777<br />reg_preds:  2736588.33","Quantity: 3.431663e+05<br />Predicted Cost = $19,739,757<br />reg_preds:  4043144.07","Quantity: 3.000000e+03<br />Predicted Cost = $246,228<br />reg_preds:   299799.84","Quantity: 1.104610e+03<br />Predicted Cost = $97,717<br />reg_preds:   173240.04","Quantity: 5.523345e+04<br />Predicted Cost = $3,643,653<br />reg_preds:  1483404.58","Quantity: 1.481800e+04<br />Predicted Cost = $1,078,898<br />reg_preds:   720443.33","Quantity: 2.812100e+04<br />Predicted Cost = $1,951,432<br />reg_preds:  1024075.47","Quantity: 5.484920e+04<br />Predicted Cost = $3,620,200<br />reg_preds:  1477730.92","Quantity: 2.179000e+03<br />Predicted Cost = $183,184<br />reg_preds:   251539.48","Quantity: 8.120000e+01<br />Predicted Cost = $8,737<br />reg_preds:    41339.34","Quantity: 4.060000e+01<br />Predicted Cost = $4,601<br />reg_preds:    28256.74","Quantity: 2.400000e+02<br />Predicted Cost = $23,806<br />reg_preds:    74940.32","Quantity: 1.302000e+02<br />Predicted Cost = $13,521<br />reg_preds:    53570.08","Quantity: 2.194000e+04<br />Predicted Cost = $1,551,115<br />reg_preds:   893637.96","Quantity: 1.421300e+03<br />Predicted Cost = $123,377<br />reg_preds:   198949.23","Quantity: 9.000000e+01<br />Predicted Cost = $9,609<br />reg_preds:    43741.41","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />reg_preds:    51224.08","Quantity: 1.357000e+03<br />Predicted Cost = $118,205<br />reg_preds:   193957.12","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />reg_preds:    35013.28","Quantity: 5.887770e+04<br />Predicted Cost = $3,865,490<br />reg_preds:  1536354.42","Quantity: 6.891000e+02<br />Predicted Cost = $63,155<br />reg_preds:   133708.77","Quantity: 7.200000e+03<br />Predicted Cost = $553,392<br />reg_preds:   484771.36","Quantity: 3.700000e+01<br />Predicted Cost = $4,223<br />reg_preds:    26852.66","Quantity: 6.880040e+04<br />Predicted Cost = $4,464,488<br />reg_preds:  1673479.63","Quantity: 8.550000e+05<br />Predicted Cost = $45,927,061<br />reg_preds:  6673364.39","Quantity: 1.740600e+04<br />Predicted Cost = $1,252,122<br />reg_preds:   786999.55","Quantity: 5.150000e+02<br />Predicted Cost = $48,242<br />reg_preds:   113955.48","Quantity: 5.083000e+03<br />Predicted Cost = $401,015<br />reg_preds:   400436.31","Quantity: 5.377850e+04<br />Predicted Cost = $3,554,782<br />reg_preds:  1461826.13","Quantity: 2.500000e+02<br />Predicted Cost = $24,723<br />reg_preds:    76638.54","Quantity: 3.447911e+04<br />Predicted Cost = $2,356,348<br />reg_preds:  1145315.84","Quantity: 2.792500e+04<br />Predicted Cost = $1,938,848<br />reg_preds:  1020151.28","Quantity: 2.815940e+03<br />Predicted Cost = $232,221<br />reg_preds:   289559.14","Quantity: 1.349610e+05<br />Predicted Cost = $8,326,120<br />reg_preds:  2422389.26","Quantity: 1.450000e+03<br />Predicted Cost = $125,680<br />reg_preds:   201144.48","Quantity: 3.560000e+02<br />Predicted Cost = $34,284<br />reg_preds:    93049.05","Quantity: 7.337196e+04<br />Predicted Cost = $4,738,222<br />reg_preds:  1733631.52","Quantity: 2.809086e+05<br />Predicted Cost = $16,402,974<br />reg_preds:  3622399.00","Quantity: 1.866200e+02<br />Predicted Cost = $18,864<br />reg_preds:    65274.62","Quantity: 1.400000e+04<br />Predicted Cost = $1,023,690<br />reg_preds:   698333.06","Quantity: 6.188000e+03<br />Predicted Cost = $481,043<br />reg_preds:   446095.45","Quantity: 4.296124e+05<br />Predicted Cost = $24,299,429<br />reg_preds:  4573814.35","Quantity: 1.215400e+03<br />Predicted Cost = $106,749<br />reg_preds:   182571.96","Quantity: 2.660000e+03<br />Predicted Cost = $220,301<br />reg_preds:   280644.17","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />reg_preds:    57898.86","Quantity: 1.650802e+01<br />Predicted Cost = $2,002<br />reg_preds:    17241.98","Quantity: 1.657000e+01<br />Predicted Cost = $2,009<br />reg_preds:    17277.48","Quantity: 1.142980e+05<br />Predicted Cost = $7,139,793<br />reg_preds:  2211202.42","Quantity: 6.838600e+02<br />Predicted Cost = $62,711<br />reg_preds:   133149.70","Quantity: 2.600000e+04<br />Predicted Cost = $1,814,890<br />reg_preds:   980928.09","Quantity: 7.118797e+05<br />Predicted Cost = $38,768,242<br />reg_preds:  6034941.05","Quantity: 1.680000e+01<br />Predicted Cost = $2,034<br />reg_preds:    17408.72","Quantity: 5.220000e+02<br />Predicted Cost = $48,848<br />reg_preds:   114803.12","Quantity: 1.440000e+02<br />Predicted Cost = $14,842<br />reg_preds:    56615.89","Quantity: 4.155400e+04<br />Predicted Cost = $2,800,379<br />reg_preds:  1268875.34","Quantity: 3.264940e+03<br />Predicted Cost = $266,278<br />reg_preds:   314055.48","Quantity: 3.002000e+03<br />Predicted Cost = $246,380<br />reg_preds:   299909.54","Quantity: 5.827680e+03<br />Predicted Cost = $455,076<br />reg_preds:   431644.19","Quantity: 8.094045e+04<br />Predicted Cost = $5,188,636<br />reg_preds:  1829617.35","Quantity: 2.408400e+03<br />Predicted Cost = $200,955<br />reg_preds:   265747.06","Quantity: 5.140000e+02<br />Predicted Cost = $48,155<br />reg_preds:   113833.97","Quantity: 1.695000e+03<br />Predicted Cost = $145,205<br />reg_preds:   219142.11","Quantity: 2.587903e+05<br />Predicted Cost = $15,204,666<br />reg_preds:  3462943.56","Quantity: 2.004440e+05<br />Predicted Cost = $12,004,486<br />reg_preds:  3009816.58","Quantity: 1.817030e+03<br />Predicted Cost = $154,850<br />reg_preds:   227666.45","Quantity: 5.186950e+04<br />Predicted Cost = $3,437,903<br />reg_preds:  1433110.01","Quantity: 3.304000e+02<br />Predicted Cost = $31,997<br />reg_preds:    89314.39","Quantity: 1.227147e+05<br />Predicted Cost = $7,624,816<br />reg_preds:  2299148.99","Quantity: 2.451000e+04<br />Predicted Cost = $1,718,472<br />reg_preds:   949660.40","Quantity: 6.134830e+04<br />Predicted Cost = $4,015,294<br />reg_preds:  1571413.81","Quantity: 7.190880e+04<br />Predicted Cost = $4,650,754<br />reg_preds:  1714568.33","Quantity: 2.930000e+02<br />Predicted Cost = $28,632<br />reg_preds:    83614.77","Quantity: 3.485034e+04<br />Predicted Cost = $2,379,806<br />reg_preds:  1152068.43","Quantity: 8.911550e+03<br />Predicted Cost = $674,073<br />reg_preds:   544977.20","Quantity: 4.183475e+04<br />Predicted Cost = $2,817,876<br />reg_preds:  1273574.01","Quantity: 1.708700e+04<br />Predicted Cost = $1,230,880<br />reg_preds:   779049.26","Quantity: 2.963400e+04<br />Predicted Cost = $2,048,359<br />reg_preds:  1053962.40","Quantity: 7.387389e+05<br />Predicted Cost = $40,119,376<br />reg_preds:  6158884.77","Quantity: 1.478800e+04<br />Predicted Cost = $1,076,877<br />reg_preds:   719642.31","Quantity: 2.927500e+04<br />Predicted Cost = $2,025,395<br />reg_preds:  1046934.43","Quantity: 1.685344e+06<br />Predicted Cost = $86,037,318<br />reg_preds:  9685540.28","Quantity: 1.950200e+04<br />Predicted Cost = $1,390,987<br />reg_preds:   837684.05","Quantity: 1.125400e+04<br />Predicted Cost = $836,486<br />reg_preds:   619460.17","Quantity: 1.539474e+05<br />Predicted Cost = $9,404,147<br />reg_preds:  2603888.81","Quantity: 1.914000e+03<br />Predicted Cost = $162,479<br />reg_preds:   234257.53","Quantity: 5.243400e+04<br />Predicted Cost = $3,472,498<br />reg_preds:  1441650.42","Quantity: 9.757830e+03<br />Predicted Cost = $733,081<br />reg_preds:   572803.66","Quantity: 2.646608e+04<br />Predicted Cost = $1,844,964<br />reg_preds:   990541.75","Quantity: 3.958000e+02<br />Predicted Cost = $37,815<br />reg_preds:    98622.59","Quantity: 5.848307e+05<br />Predicted Cost = $32,322,348<br />reg_preds:  5417615.72","Quantity: 3.997500e+02<br />Predicted Cost = $38,164<br />reg_preds:    99161.65","Quantity: 3.920930e+04<br />Predicted Cost = $2,653,902<br />reg_preds:  1229060.15","Quantity: 1.991214e+05<br />Predicted Cost = $11,931,196<br />reg_preds:  2998898.44","Quantity: 9.608625e+04<br />Predicted Cost = $6,080,813<br />reg_preds:  2010260.83","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />reg_preds:    59986.78","Quantity: 2.607010e+05<br />Predicted Cost = $15,308,477<br />reg_preds:  3476954.83","Quantity: 8.925000e+02<br />Predicted Cost = $80,226<br />reg_preds:   154105.40","Quantity: 3.671430e+04<br />Predicted Cost = $2,497,311<br />reg_preds:  1185494.09","Quantity: 1.632000e+03<br />Predicted Cost = $140,206<br />reg_preds:   214632.93","Quantity: 2.913900e+04<br />Predicted Cost = $2,016,690<br />reg_preds:  1044261.87","Quantity: 2.447749e+05<br />Predicted Cost = $14,441,405<br />reg_preds:  3358705.74","Quantity: 4.349900e+04<br />Predicted Cost = $2,921,415<br />reg_preds:  1301140.04","Quantity: 6.922440e+03<br />Predicted Cost = $533,629<br />reg_preds:   474422.28","Quantity: 2.920270e+04<br />Predicted Cost = $2,020,767<br />reg_preds:  1045514.35","Quantity: 1.364898e+04<br />Predicted Cost = $999,926<br />reg_preds:   688666.89","Quantity: 1.634000e+03<br />Predicted Cost = $140,365<br />reg_preds:   214777.27","Quantity: 2.533134e+04<br />Predicted Cost = $1,771,674<br />reg_preds:   966999.03","Quantity: 4.800000e+02<br />Predicted Cost = $45,201<br />reg_preds:   109636.95","Quantity: 1.150000e+02<br />Predicted Cost = $12,054<br />reg_preds:    50041.26","Quantity: 1.883069e+05<br />Predicted Cost = $11,330,558<br />reg_preds:  2908369.53","Quantity: 1.544265e+05<br />Predicted Cost = $9,431,217<br />reg_preds:  2608333.99","Quantity: 1.535021e+06<br />Predicted Cost = $78,914,290<br />reg_preds:  9201355.65","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />reg_preds:    28026.75","Quantity: 2.710300e+05<br />Predicted Cost = $15,868,690<br />reg_preds:  3551909.50","Quantity: 2.056400e+04<br />Predicted Cost = $1,460,914<br />reg_preds:   862424.37","Quantity: 7.278000e+02<br />Predicted Cost = $66,430<br />reg_preds:   137779.83","Quantity: 1.000000e+02<br />Predicted Cost = $10,593<br />reg_preds:    46345.76","Quantity: 1.295800e+04<br />Predicted Cost = $953,010<br />reg_preds:   669305.43","Quantity: 6.180000e+03<br />Predicted Cost = $480,468<br />reg_preds:   445778.78","Quantity: 1.468000e+01<br />Predicted Cost = $1,796<br />reg_preds:    16166.25","Quantity: 5.687660e+05<br />Predicted Cost = $31,500,221<br />reg_preds:  5335414.52","Quantity: 1.940000e+03<br />Predicted Cost = $164,519<br />reg_preds:   235998.98","Quantity: 8.306886e+04<br />Predicted Cost = $5,314,720<br />reg_preds:  1855872.11","Quantity: 1.230000e+03<br />Predicted Cost = $107,935<br />reg_preds:   183772.58","Quantity: 2.274000e+03<br />Predicted Cost = $190,560<br />reg_preds:   257501.30","Quantity: 1.170600e+04<br />Predicted Cost = $867,517<br />reg_preds:   632995.77","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />reg_preds:    57898.86","Quantity: 5.555840e+04<br />Predicted Cost = $3,663,477<br />reg_preds:  1488188.77","Quantity: 1.509200e+04<br />Predicted Cost = $1,097,339<br />reg_preds:   727725.66","Quantity: 5.135800e+02<br />Predicted Cost = $48,119<br />reg_preds:   113782.90","Quantity: 2.916000e+03<br />Predicted Cost = $239,844<br />reg_preds:   295162.49","Quantity: 1.593000e+04<br />Predicted Cost = $1,153,585<br />reg_preds:   749635.74","Quantity: 1.218100e+02<br />Predicted Cost = $12,713<br />reg_preds:    51646.76","Quantity: 2.209380e+05<br />Predicted Cost = $13,135,607<br />reg_preds:  3175023.31","Quantity: 1.680000e+02<br />Predicted Cost = $17,116<br />reg_preds:    61615.05","Quantity: 4.259168e+05<br />Predicted Cost = $24,106,015<br />reg_preds:  4552175.12","Quantity: 3.464100e+03<br />Predicted Cost = $281,269<br />reg_preds:   324430.72","Quantity: 6.985800e+03<br />Predicted Cost = $538,146<br />reg_preds:   476800.97","Quantity: 1.100000e+01<br />Predicted Cost = $1,375<br />reg_preds:    13797.85","Quantity: 8.500000e+01<br />Predicted Cost = $9,114<br />reg_preds:    42390.31","Quantity: 5.110000e+03<br />Predicted Cost = $402,985<br />reg_preds:   401602.49","Quantity: 1.277000e+03<br />Predicted Cost = $111,745<br />reg_preds:   187594.59","Quantity: 2.593900e+04<br />Predicted Cost = $1,810,951<br />reg_preds:   979664.13","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />reg_preds:    31678.79","Quantity: 2.137500e+02<br />Predicted Cost = $21,388<br />reg_preds:    70323.76","Quantity: 5.860211e+05<br />Predicted Cost = $32,383,201<br />reg_preds:  5423666.23","Quantity: 1.068079e+06<br />Predicted Cost = $56,423,275<br />reg_preds:  7540342.34","Quantity: 5.358010e+04<br />Predicted Cost = $3,542,650<br />reg_preds:  1458863.34","Quantity: 9.000000e+03<br />Predicted Cost = $680,259<br />reg_preds:   547939.74","Quantity: 1.280000e+02<br />Predicted Cost = $13,310<br />reg_preds:    53071.30","Quantity: 1.072570e+04<br />Predicted Cost = $800,099<br />reg_preds:   603324.89","Quantity: 1.590000e+02<br />Predicted Cost = $16,266<br />reg_preds:    59780.69","Quantity: 6.184630e+03<br />Predicted Cost = $480,801<br />reg_preds:   445962.08","Quantity: 1.323848e+04<br />Predicted Cost = $972,076<br />reg_preds:   677219.39","Quantity: 5.363350e+03<br />Predicted Cost = $421,433<br />reg_preds:   412412.75","Quantity: 1.024580e+04<br />Predicted Cost = $766,929<br />reg_preds:   588354.18","Quantity: 1.132150e+04<br />Predicted Cost = $841,126<br />reg_preds:   621496.90","Quantity: 4.880000e+01<br />Predicted Cost = $5,455<br />reg_preds:    31259.17","Quantity: 3.611600e+02<br />Predicted Cost = $34,743<br />reg_preds:    93786.97","Quantity: 3.242650e+03<br />Predicted Cost = $264,596<br />reg_preds:   312876.73","Quantity: 9.647222e+04<br />Predicted Cost = $6,103,404<br />reg_preds:  2014689.37","Quantity: 8.698750e+03<br />Predicted Cost = $659,170<br />reg_preds:   537794.87","Quantity: 2.140567e+05<br />Predicted Cost = $12,756,722<br />reg_preds:  3120353.88","Quantity: 2.623704e+02<br />Predicted Cost = $25,852<br />reg_preds:    78697.46","Quantity: 1.120000e+02<br />Predicted Cost = $11,763<br />reg_preds:    49320.42","Quantity: 1.900000e+01<br />Predicted Cost = $2,280<br />reg_preds:    18625.31","Quantity: 9.392259e+04<br />Predicted Cost = $5,954,048<br />reg_preds:  1985285.40","Quantity: 1.014424e+06<br />Predicted Cost = $53,796,382<br />reg_preds:  7330000.89","Quantity: 1.498740e+04<br />Predicted Cost = $1,090,302<br />reg_preds:   724952.71","Quantity: 9.372000e+01<br />Predicted Cost = $9,976<br />reg_preds:    44724.78","Quantity: 5.000000e+02<br />Predicted Cost = $46,940<br />reg_preds:   112121.43","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />reg_preds:    31678.79","Quantity: 1.503800e+02<br />Predicted Cost = $15,449<br />reg_preds:    57979.33","Quantity: 1.350000e+02<br />Predicted Cost = $13,982<br />reg_preds:    54645.30","Quantity: 5.400000e+02<br />Predicted Cost = $50,404<br />reg_preds:   116959.52","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />reg_preds:    51224.08","Quantity: 1.000000e+03<br />Predicted Cost = $89,125<br />reg_preds:   164032.54","Quantity: 1.571400e+04<br />Predicted Cost = $1,139,109<br />reg_preds:   744039.04","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />reg_preds:    28026.75","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />reg_preds:    28026.75","Quantity: 1.306541e+06<br />Predicted Cost = $67,985,102<br />reg_preds:  8422329.42","Quantity: 1.962795e+04<br />Predicted Cost = $1,399,295<br />reg_preds:   840649.41","Quantity: 1.300000e+02<br />Predicted Cost = $13,502<br />reg_preds:    53524.89","Quantity: 4.000000e+03<br />Predicted Cost = $321,296<br />reg_preds:   351085.40","Quantity: 1.643896e+04<br />Predicted Cost = $1,187,637<br />reg_preds:   762689.46","Quantity: 3.525200e+02<br />Predicted Cost = $33,974<br />reg_preds:    92548.65","Quantity: 3.200000e+01<br />Predicted Cost = $3,692<br />reg_preds:    24795.73","Quantity: 4.222000e+03<br />Predicted Cost = $337,757<br />reg_preds:   361650.83","Quantity: 6.225000e+02<br />Predicted Cost = $57,488<br />reg_preds:   126452.95","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />reg_preds:    59986.78","Quantity: 3.328810e+03<br />Predicted Cost = $271,092<br />reg_preds:   317413.13","Quantity: 3.773000e+03<br />Predicted Cost = $304,393<br />reg_preds:   340004.70","Quantity: 6.525000e+02<br />Predicted Cost = $60,046<br />reg_preds:   129762.60","Quantity: 2.944140e+03<br />Predicted Cost = $241,984<br />reg_preds:   296722.63","Quantity: 7.542000e+02<br />Predicted Cost = $68,655<br />reg_preds:   140501.16","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />reg_preds:    31678.79","Quantity: 1.823000e+03<br />Predicted Cost = $155,320<br />reg_preds:   228076.75","Quantity: 4.200000e+02<br />Predicted Cost = $39,949<br />reg_preds:   101888.23","Quantity: 1.122222e+04<br />Predicted Cost = $834,301<br />reg_preds:   618499.41","Quantity: 2.925000e+05<br />Predicted Cost = $17,028,109<br />reg_preds:  3703700.10","Quantity: 6.744000e+03<br />Predicted Cost = $520,893<br />reg_preds:   467669.91","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />reg_preds:    93621.49","Quantity: 2.431746e+04<br />Predicted Cost = $1,705,982<br />reg_preds:   945558.11","Quantity: 1.801400e+05<br />Predicted Cost = $10,875,254<br />reg_preds:  2838438.76","Quantity: 4.870600e+02<br />Predicted Cost = $45,816<br />reg_preds:   110519.21","Quantity: 1.600000e+04<br />Predicted Cost = $1,158,273<br />reg_preds:   751442.13","Quantity: 8.295000e+02<br />Predicted Cost = $74,973<br />reg_preds:   148035.78","Quantity: 8.116600e+02<br />Predicted Cost = $73,480<br />reg_preds:   146279.56","Quantity: 4.000000e+02<br />Predicted Cost = $38,186<br />reg_preds:    99195.68","Quantity: 6.711000e+02<br />Predicted Cost = $61,628<br />reg_preds:   131780.17","Quantity: 1.009000e+02<br />Predicted Cost = $10,681<br />reg_preds:    46574.26","Quantity: 3.930802e+04<br />Predicted Cost = $2,660,082<br />reg_preds:  1230757.82","Quantity: 2.068581e+04<br />Predicted Cost = $1,468,916<br />reg_preds:   865224.81","Quantity: 5.532000e+02<br />Predicted Cost = $51,543<br />reg_preds:   118520.34","Quantity: 2.809000e+03<br />Predicted Cost = $231,692<br />reg_preds:   289167.19","Quantity: 4.256000e+03<br />Predicted Cost = $340,272<br />reg_preds:   363246.61","Quantity: 4.200000e+01<br />Predicted Cost = $4,748<br />reg_preds:    28787.50","Quantity: 9.147565e+04<br />Predicted Cost = $5,810,421<br />reg_preds:  1956725.11","Quantity: 1.287000e+03<br />Predicted Cost = $112,554<br />reg_preds:   188399.55","Quantity: 2.392800e+02<br />Predicted Cost = $23,740<br />reg_preds:    74816.83","Quantity: 1.260400e+03<br />Predicted Cost = $110,400<br />reg_preds:   186252.05","Quantity: 3.996000e+03<br />Predicted Cost = $320,999<br />reg_preds:   350892.64","Quantity: 4.354500e+02<br />Predicted Cost = $41,306<br />reg_preds:   103928.82","Quantity: 1.200000e+03<br />Predicted Cost = $105,498<br />reg_preds:   181298.48","Quantity: 3.360577e+05<br />Predicted Cost = $19,361,224<br />reg_preds:  3996953.46","Quantity: 1.059500e+04<br />Predicted Cost = $791,076<br />reg_preds:   599278.12","Quantity: 1.500000e+01<br />Predicted Cost = $1,832<br />reg_preds:    16358.75","Quantity: 4.170200e+04<br />Predicted Cost = $2,809,604<br />reg_preds:  1271354.07","Quantity: 8.128043e+04<br />Predicted Cost = $5,208,792<br />reg_preds:  1833831.85","Quantity: 4.746850e+04<br />Predicted Cost = $3,167,197<br />reg_preds:  1365030.99","Quantity: 1.611960e+03<br />Predicted Cost = $138,613<br />reg_preds:   213182.18","Quantity: 5.098200e+02<br />Predicted Cost = $47,793<br />reg_preds:   113324.88","Quantity: 2.117000e+02<br />Predicted Cost = $21,198<br />reg_preds:    69952.73","Quantity: 8.300000e+01<br />Predicted Cost = $8,916<br />reg_preds:    41839.87","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />reg_preds:    35013.28","Quantity: 5.685100e+03<br />Predicted Cost = $444,767<br />reg_preds:   425814.89","Quantity: 1.152000e+03<br />Predicted Cost = $101,588<br />reg_preds:   177281.12","Quantity: 8.646000e+03<br />Predicted Cost = $655,472<br />reg_preds:   536002.25","Quantity: 2.193614e+01<br />Predicted Cost = $2,604<br />reg_preds:    20153.92","Quantity: 6.262000e+02<br />Predicted Cost = $57,804<br />reg_preds:   126864.97","Quantity: 6.060000e+03<br />Predicted Cost = $471,832<br />reg_preds:   441006.38","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />reg_preds:    41002.86","Quantity: 2.724600e+02<br />Predicted Cost = $26,770<br />reg_preds:    80344.54","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />reg_preds:    35013.28","Quantity: 1.124000e+02<br />Predicted Cost = $11,802<br />reg_preds:    49417.03","Quantity: 1.400000e+01<br />Predicted Cost = $1,719<br />reg_preds:    15750.80","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />reg_preds:    19157.17","Quantity: 3.118400e+03<br />Predicted Cost = $255,204<br />reg_preds:   306237.98","Quantity: 1.481796e+04<br />Predicted Cost = $1,078,895<br />reg_preds:   720442.26","Quantity: 1.972424e+05<br />Predicted Cost = $11,827,018<br />reg_preds:  2983331.77","Quantity: 7.224000e+01<br />Predicted Cost = $7,841<br />reg_preds:    38769.51","Quantity: 4.687428e+05<br />Predicted Cost = $26,339,921<br />reg_preds:  4797990.69","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />reg_preds:    19157.17","Quantity: 6.346000e+03<br />Predicted Cost = $492,394<br />reg_preds:   452312.24","Quantity: 2.172210e+04<br />Predicted Cost = $1,536,860<br />reg_preds:   888755.17","Quantity: 1.250000e+04<br />Predicted Cost = $921,811<br />reg_preds:   656214.53","Quantity: 9.500000e+02<br />Predicted Cost = $84,995<br />reg_preds:   159478.46","Quantity: 6.318400e+02<br />Predicted Cost = $58,286<br />reg_preds:   127490.92","Quantity: 6.664860e+04<br />Predicted Cost = $4,335,176<br />reg_preds:  1644543.53","Quantity: 1.340000e+03<br />Predicted Cost = $116,835<br />reg_preds:   192619.55","Quantity: 8.999176e+04<br />Predicted Cost = $5,723,182<br />reg_preds:  1939237.37","Quantity: 1.853500e+02<br />Predicted Cost = $18,745<br />reg_preds:    65030.41","Quantity: 6.684800e+02<br />Predicted Cost = $61,405<br />reg_preds:   131497.51"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,0,255,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(0,0,255,1)"}},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2.7024305364455254,4.1984096335377616,4.1552570676526974,2.1461280356782382,5.0550742952996881,5.6388102911519624,5.115095709153235,5.3296585812030282,5.2169432401416342,5.34476116902146,4.6230561932390266,4.7533792679960891,2.6693168805661123,4.7212548242783958,3.3424226808222062,2.6852220653346204,3.6292056571023039,4.5515301398686079,5.1145002283533332,3.9506374200705787,2.1192558892779365,6.101551030991037,5.9334293932441931,3.3703187516177455,3.1630718820038193,3.6958282732599583,4.2699138977414801,4.20744450716556,6.0945933986918552,5.8228092628357917,4.4835787393578457,4.1603112322086577,1.4393326938302626,4.295423044232602,4.9580899140341002,6.6991327474116886,1.7168377232995244,4.1076423523558283,3.8429067791404372,1.8567288903828827,2.2900346113625178,2.0413926851582249,5.2442501466886622,5.1216601175420404,1.9030899869919435,2.9484129657786009,3.539803497384677,5.074771948771585,5.2430722945580479,5.7822159849460446,3.6989700043360187,4.1925674533365456,1.2355284469075489,3.4890016973113775,5.8994042226163863,4.8991850050849477,6.1582259875720986,3.8829796540372992,6.0433759787151002,2.4313637641589874,3.2050418792613695,3.5151052041667898,3.5724068675580556,3.9456654994321343,6.2333936760774149,3.5147734739975087,1.8450980400142569,2.9444826721501687,6.1130413755231467,6.7864854403677892,4.3750496821292746,6.4300969955807767,3.0637085593914173,5.2695250856143598,1.3010299956639813,1.2950711714662781,2.0791812460476247,4.3639093040106847,2.2036169956331912,2.0901169107520099,4.1027629037125646,3.7320719409998668,2.255272505103306,1.5440680443502757,2.357934847000454,4.1587293762389352,3.2398898183400542,1.8450980400142569,3.370883016777606,3.8958091501691308,5.0504952949350823,3.6037612606082874,3.0926855629374908,2.5440680443502757,4.5093080578565763,3.7281150573980244,2.9858780492079968,5.3515017638802327,3.1965768448522329,3.8655623192261745,3.6868149545073168,4.3139776773448899,3.7225993750077753,6.0269743049404001,4.4471580313422194,3.8811483271514193,2.5051499783199058,5.3979330599047373,1.6654497448426819,2.9929950984313414,4.4581239446610619,3.2487087356009177,4.56059520730137,3.4750898033890065,4.6512780139981444,4.038458016269562,3.4409090820652177,5.6342373846950906,5.3124325247115989,1.9777236052888478,3.815132600842988,2.0903638794717181,3.6974211573941798,5.3010299956639813,2.9013493254156422,3.6778714633289114,2.5717088318086878,2.4771212547196626,2.9786369483844743,2.7874604745184151,1.7781512503836436,2.5910646070264991,4.5798258811262302,3.4367587960456936,3.9685296443748395,4.082507449923134,3.1283992687178066,1.9834007381805383,4.9780891730561425,2.8407332346118066,4.0483446785400696,4.5002620046610948,3.9640896827699628,4.4469346224414545,5.5548738217592879,5.6363013905219077,5.3758137898824598,4.9473243011870238,5.1153751634736802,5.5083661869750022,2.5237464668115646,3.1037695231936726,4.7235106556542812,5.5581228379383969,3.1085650237328344,4.5443878155045354,5.2564575963805229,2.8665000026721725,4.6286341094782966,2.3802112417116059,5.385835299070485,3.6833460758601015,2.255272505103306,4.2944794532032704,3.1655410767223731,4.3651771284764331,2.9489017609702137,5.9058383905918417,4.0144113213844728,1.3010299956639813,1.954242509439325,2.5436956323092446,2.5563025007672873,3.247354508217859,4.2418760607224613,2.3617278360175931,2.9332847723486948,2.5269592553422457,4.8930178883115341,1.4471580313422192,1.255272505103306,4.3544284692600153,5.0044589502411947,3.7185016888672742,2.8573324964312685,1.7781512503836436,1.6011905326153335,3.4377505628203879,2.859138297294531,3.3802112417116059,3.3644952932801808,4.0387790695555381,2.6919651027673601,1.9822712330395684,1.4771212547196624,2.8853612200315122,3.1654105314399699,2.510545010206612,4.1689457569017909,4.300986564044174,3.0097482559485536,3.4505570094183291,4.6672567755966927,3.2108533653148932,4.4171061673925927,3.3070144100729419,1.6972293427597176,2.0681858617461617,2.5563025007672873,3.2068258760318495,3.6356144176238723,3.9073468568040881,4.6937269489236471,1.8375884382355112,4.0791479488609372,1.1814205162624751,3.7875313161272341,2.5646660642520893,1.9030899869919435,6.4771799528902045,3.4117880045438689,1.5954962218255742,2.6989700043360187,2.3096301674258988,3.3344537511509307,1.9118112227541586,2.3873898263387292,2.3010299956639813,2.8846065435331911,2.9444826721501687,2.840106094456758,3.2464985807958011,4.4813193459054528,3.4352963763370234,3.7289119178080758,3.9476297473843545,3.0025979807199086,2.5172038181418617,2.1760912590556813,3.7564877686873519,3.0591846176313711,3.4653828514484184,3.4185051298166429,2.4297522800024081,2.8522603510069531,4.1983546247369397,4.5223398859609105,4.8645110810583923,3.6959892241501806,4.9725942677630446,3.669006784060679,3.8529676910288182,2.9950864965057331,3.5294868192458373,4.1461280356782382,2.0906107078284069,4.8240028857830639,2.8350561017201161,3.5399538416563967,2.9469432706978256,2.7558748556724915,3.6532125137753435,2.7554479706597705,3.2417257394831371,3.8226910107760546,2.6473829701146196,2.3222192947339191,2.4149733479708178,2.7323937598229686,4.0387790695555381,3.1388140748403659,2.4969296480732148,1.8195439355418688,2.2041199826559246,3.6991436873944838,1.4456042032735976,1.2787536009528289,2.5224442335063197,3.8205954965444904,2.8927622346158168,1.3010299956639813,5.5874914281065235,2.3424226808222062,2.459392487759231,4.8519428931916568,3.7341595132444669,4.8246073927962358,4.673349039687956,4.7950523324442234,1.505149978319906,4.0969100130080562,3.0195316845312554,2.0791812460476247,3.3926969532596658,1.6020599913279623,5.029158048255157,5.029158048255157,5.3689070087836734,4.176305458168649,5.2629302105120859,3.9945397430417637,5.4889805680440604,5.4842383116077116,5.7900105637080417,3.7134905430939424,1.9461573949223723,3.0879587894607328,2.9794391044854023,4.1517543233268732,4.1445726509957801,4.8625493655096754,1.7634279935629373,3.6957442751973235,4.5925986289061314,4.3913938751356989,3.986709048064589,4.151086869007643,3.7563622110126267,2.3924859087190731,2.853819845856763,4.3939899976265382,3.584218112117405,3.0674428427763805,3.9030899869919438,2.3502480183341627,4.0116972881141422,1.7558748556724915,3.9642124729698192,4.924279286061882,1.4771212547196624,2.6696887080562082,5.2266987378384382,5.53550465751026,3.4771212547196626,3.0432089705599021,4.7422021710376834,4.1707895904463914,4.4490307604004151,4.739170297578565,3.3382572302462554,1.9095560292411753,1.608526033577194,2.3802112417116059,2.114610984232173,4.3412366232386921,3.152685756036786,1.954242509439325,2.0791812460476247,3.1325798476597368,1.7781512503836436,4.7699508393203791,2.8382822499146885,3.8573324964312685,1.568201724066995,4.8375909631960896,5.9319661147281728,4.2406989791863081,2.7118072290411912,3.7061201097027037,4.7306086846331397,2.3979400086720375,4.5375560469706144,4.4459931817876468,3.4496233969379295,5.1302082875187756,3.1613680022349748,2.5514499979728753,4.8655301206260217,5.4485649734633181,2.2709581850920975,4.1461280356782382,3.79155030502733,5.633076829294466,3.0847192320112975,3.424881636631067,2.1760912590556813,1.2176951179079363,1.2193225084193366,5.0580385129555649,2.8349672019384444,4.4149733479708182,5.8524066026479842,1.2253092817258628,2.7176705030022621,2.1583624920952498,4.6186128354441802,3.5138752046284449,3.4774106879072515,3.7654956964868163,4.9081656145845027,3.3817286185351105,2.7109631189952759,3.2291697025391009,5.4129479940390777,5.3019930608065904,3.2593620977686291,4.7149120615989917,2.5190400386483445,5.088896590000771,4.3893433112520777,4.7878025326563645,4.8567820413923553,2.4668676203541096,4.5422070194326958,3.949953248133617,4.6215371780320238,4.2326658194314453,4.4717902765451916,5.8684909977946704,4.1699094419010692,4.4664969037444004,6.2266885002344523,4.2900791521022015,4.0513069108179742,5.1873723585514089,3.2819419334408249,4.7196129897309733,3.9893532476043831,4.4226896209220463,2.5974757898703773,5.7670301623264155,2.6017884724182725,4.5933890889502704,5.2991178280338591,4.9826612443139986,2.2041199826559246,5.416142697043469,2.9506082247842307,4.5648352996192942,3.2127201544178425,4.4644746434554561,5.3887669549130184,4.6384792730586861,3.8402592002021061,4.4654230069584191,4.135100197389721,3.2132520521963968,4.4036581640982106,2.6812412373755872,2.0606978403536118,5.2748662569248337,5.1887218566113926,6.1861142194078909,1.6020599913279623,5.4330173650934359,4.313107595194996,2.8620120512502165,2,4.1125379756093077,3.7909884750888159,1.1667260555800518,5.7549336270084162,3.287801729930226,4.9194382504422851,3.0899051114393981,3.356790460351716,4.0684085197781616,2.1760912590556813,4.7447497302049486,4.1787467965289578,2.710608102952619,3.4647875196459372,4.2022157758011316,2.0856829431946151,5.3442704183205363,2.2253092817258628,5.6293247851389783,3.5395904205339157,3.844216147843321,1.0413926851582251,1.9294189257142926,3.7084209001347128,3.1061908972634154,4.4139532291554149,1.6989700043360187,2.3299061234002103,5.767913275535089,6.0286035551931025,4.7290035198247784,3.9542425094393248,2.1072099696478683,4.0304256454923628,2.2013971243204513,3.7913137227582072,4.1218381236604849,3.729436138956145,4.0105458741106785,4.0539039708951661,1.6884198220027107,2.5576996443512146,3.5109000750153396,4.9844022725395707,3.9394568495030713,5.3305287649853836,2.4189148374041607,2.0492180226701815,1.2787536009528289,4.9727700721181538,6.0062195350443863,4.1757262983859329,1.971832279924925,2.6989700043360187,1.6989700043360187,2.1771900804896092,2.1303337684950061,2.7323937598229686,2.0791812460476247,3,4.196286748808876,1.6020599913279623,1.6020599913279623,6.1161229762050935,4.29287494299383,2.1139433523068369,3.6020599913279625,4.2158743387181401,2.5471837614500816,1.505149978319906,3.6255182289716377,2.7941393557677738,2.2041199826559246,3.5222890074407114,3.5766868052009957,2.8145805160103188,3.4689584577652681,2.8774865280696016,1.6989700043360187,3.2607866686549762,2.6232492903979003,4.0500788634833285,5.4661258704181996,3.8289175616166857,2.5563025007672873,4.3859182101733625,5.2556101584077384,2.687582464425827,4.204119982655925,2.9188163903603797,2.909374143715874,2.6020599913279625,2.826787238816292,2.0038911662369103,4.5944811683499527,4.3156725313539823,2.7428821714372731,3.4485517392015779,3.6290016192869916,1.6232492903979006,4.9613055041429064,3.1095785469043866,2.3789064000232618,3.1005083945019623,3.6016254795539449,2.638938294887355,3.0791812460476247,5.5264138507238521,4.0251009610468138,1.1760912590556813,4.6201568839458202,4.9099859925054599,4.676405508271646,3.2073542607973531,2.7074168686367091,2.325720858019412,1.919078092376074,1.7781512503836436,3.7547381082614368,3.0614524790871931,3.9368152311976328,1.3411601596967189,2.7967130632808965,3.782472624166286,1.9030899869919435,2.4353027522846102,1.7781512503836436,2.0507663112330423,1.146128035678238,1.3010299956639813,3.4939318217735464,4.170788418101762,5.2950002782862713,1.8587777373054493,5.6709345635958428,1.3010299956639813,3.8025000677643934,4.3369018086850204,4.0969100130080562,2.9777236052888476,2.8006071163924688,4.8237910311893311,3.1271047983648077,4.9542027455464295,2.2679925903655827,2.8250884183002145],"y":[4.6747482462121113,6.0585289110224299,6.0186127875787445,4.1601684330023705,6.8509437231522119,7.3908995193155649,6.9064635309667421,7.1049341876128009,7.0006724971310117,7.1189040813448505,6.4513269787461001,6.5718758228963825,4.6441181145236534,6.5421607124575161,5.2667409797605407,4.6588304104345237,5.5320152328196315,6.3851653793784626,6.9059127112268328,5.8293396135652848,4.1353116975820914,7.8189347036667094,7.6634221887508787,5.2925448452464146,5.1008414908535329,5.5936411527654615,6.1246703554108688,6.0668861691281428,7.8124988937899662,7.5610985681231071,6.3223103339060076,6.0232878897930089,3.5063827417929927,6.1482663159151567,6.7612331704815425,8.3716977913558122,3.76307489405206,5.9745691759291413,5.7296887707049038,3.8924742236041663,4.2932820155103286,4.0632882337713578,7.0259313856870129,6.9125356087263876,3.9353582379675478,4.9022819933452055,5.4493182350808258,6.8691640526137165,7.0248418724661947,7.5235497860750913,5.596547254010817,6.053124894336305,3.3178638133894829,5.4023265700130239,7.6319489059201571,6.7067461297035766,7.8713590385041909,5.7667561799845011,7.7651227803114677,4.4240114818470637,5.1396637383167665,5.4264723138542807,5.4794763524912016,5.8247405869747242,7.9408891503716088,5.4261654634476955,3.8817156870131875,4.898646471738906,7.8295632723589108,8.4524990323402047,6.2219209559695789,8.1228397209122178,5.0089304174370604,7.0493107041932825,3.3784527459891827,3.3729408336063074,4.0982426525940525,6.2116161062098829,4.2133457209607013,4.108358142445609,5.9700556859341223,5.6271665454248767,4.2611270672205581,3.6032629410240049,4.3560897334754198,6.0218246730210154,5.1718980819645495,3.8817156870131875,5.2930667905192852,5.7786234639064453,6.846708147814951,5.5084791660626653,5.0357341457171785,4.5282629410240052,6.3461099535173329,5.6235064280931724,4.9369371955173964,7.125139131589215,5.1318335814883156,5.7506451452842118,5.5853038329192675,6.1654293515440237,5.6184044218821922,7.7499512320698702,6.2886211789915532,5.7650622026150629,4.4922637299459129,7.1680880804118816,3.7155410139794807,4.9435204660489909,6.2987646488114821,5.1800555804308495,6.3935505667537669,5.3894580681348305,6.4774321629482836,5.9105736650493448,5.3578409009103263,7.386669580842959,7.0890000853582293,4.0043943348921847,5.703997655779764,4.1085865885113391,5.5951145705896161,7.0784527459891828,4.8587481260094689,5.5770311035792428,4.5538306694230357,4.4663371606156872,4.9302391772556389,4.7534009389295342,3.8197899066048704,4.5717347614995116,6.4113389400417633,5.3540018863422665,5.8458899210467266,5.9513193911788989,5.0687693235639717,4.0096456828169975,6.7797324850769316,4.8026782420159204,5.9197188276495645,6.337742354311513,5.8417829565622155,6.2884145257583457,7.3132582851273416,7.388578786232765,7.1476277556412757,6.7512749785979969,6.9067220262131546,7.270238722951877,4.5094654818006976,5.0459868089541473,6.5442473564802102,7.3162636250930175,5.0504226469528719,6.3785587293416954,7.037223276651984,4.8265125024717594,6.4564865512674245,4.3766953985832355,7.1568976516401985,5.5820951201705942,4.2611270672205581,6.1473934942130253,5.1031254959681949,6.2127888438407011,4.902734128897448,7.6379005112974534,5.8883304722806376,3.3784527459891827,3.9826743212313755,4.5279184598860507,4.5395798132097411,5.1788029201015195,6.0987353561682767,4.3595982483162734,4.8882884144225427,4.5124373111915776,6.7010415466881694,3.5136211789915528,3.3361270672205579,6.202846334065514,6.8041245289731052,5.6146140622022287,4.8180325591989233,3.8197899066048704,3.6561012426691835,5.3549192706088586,4.8197029249974417,5.3016953985832354,5.2871581462841668,5.9108706393388726,4.6650677200598079,4.0086008905616008,3.5413371606156874,4.8439591285291481,5.1030047415819721,4.4972541344411159,6.0312748251341564,6.1534125717408603,4.9590171367524114,5.3667652337119538,6.4922125174269407,5.1450393629162763,6.2608232048381485,5.2339883293174712,3.7449371420527386,4.0880719221151995,4.5395798132097411,5.1413139353294603,5.5379433363020816,5.7892958425437815,6.5166974277543739,3.874769305367848,5.9482118526963674,3.2678139775427892,5.6784664674176915,4.5473161094331829,3.9353582379675478,8.1663914564234403,5.3309039042030788,3.6508340051886563,4.6715472540108172,4.3114079048689558,5.259369719814611,3.9434253810475965,4.383335589363325,4.3034527459891825,4.8432610527682023,4.898646471738906,4.8020981373725009,5.1780111872361161,6.3202203949625435,5.3526491481117464,5.6242435239724706,5.8265575163305279,4.9524031321659159,4.5034135317812218,4.1878844146265051,5.6497511860358003,5.0047457713090182,5.3804791375897869,5.3371172450803943,4.422520859002228,4.8133408246814318,6.0584780278816694,6.3581643945138424,6.6746727499790133,5.5937900323389176,6.7746496976808164,5.5688312752561284,5.7389951142016571,4.9454550092678033,5.4397753078023996,6.0101684330023701,4.1088149047412763,6.6372026693493344,4.7974268940911076,5.4494573035321672,4.9009225253954884,4.7241842414970545,5.5542215752421926,4.7237893728602875,5.1735963090219013,5.7109891849678505,4.6238292473560225,4.3230528476288752,4.408850346873006,4.7024642278362458,5.9108706393388726,5.0784030192273386,4.4846599244677234,3.8580781403762288,4.2138109839567299,5.596707910839898,3.512183888028078,3.3578470808813665,4.5082609159933451,5.7090508343036532,4.8508050670196301,3.3784527459891827,7.3434295709985342,4.3417409797605409,4.4499380511772886,6.6630471762022827,5.6290975497511315,6.6377618383365178,6.4978478617113593,6.6104234075109067,3.5672637299459131,5.9646417620324517,4.968066808191411,4.0982426525940525,5.313244681765191,3.6569054919783648,6.8269711946360205,6.8269711946360205,7.1412389831248984,6.0380825488060008,7.0432104447236794,5.8699492623136313,7.2523070254407562,7.2479204382371334,7.5307597714299384,5.6099787523618971,3.9751955903031941,5.0313618802511773,4.9309811716489964,6.0153727490773576,6.008729702171097,6.6728581630964499,3.8061708940457168,5.5935634545575237,6.4231537317381715,6.2370393345005217,5.8627058694597451,6.0147553538320704,5.6496350451866792,4.3880494655651425,4.8147833574175056,6.2394407478045482,5.4904017537085998,5.0123846295681513,5.7853582379675483,4.3489794169591001,5.8858199915055813,3.7991842414970547,5.8418965374970826,6.7299583396072409,3.5413371606156874,4.6444620549519922,7.009696332500555,7.2953418081969907,5.3913371606156879,4.9899682977679092,6.5615370082098572,6.0329803711629122,6.2903534533703844,6.5587325252601723,5.2628879379777862,3.941339327048087,3.6628865810589044,4.3766953985832355,4.1310151604147602,6.1906438764957903,5.0912343243340272,3.9826743212313755,4.0982426525940525,5.0726363590852568,3.8197899066048704,6.5872045263713508,4.8004110811710863,5.7430325591989231,3.6255865947619705,6.6497716409563825,7.6620686561235596,6.0976465557473354,4.6834216868631025,5.6031611014750009,6.550813033285654,4.393094508021635,6.3722393434478182,6.2875436931535731,5.3659016421675849,6.9204426659548677,5.0992654020673518,4.5350912481249095,6.6756153615790703,7.2149226004535691,4.2756363212101895,6.0101684330023701,5.6821840321502801,7.3855960670973815,5.0283652896104503,5.3430155138837367,4.1878844146265051,3.3013679840648411,3.3028733202878864,6.853685624483898,4.7973446617930611,6.2588503468730066,7.5884761074493854,3.3084110855964228,4.6888452152770927,4.1714853051881065,6.4472168727858667,5.4253345642813109,5.391604886314207,5.6580835192503045,6.7150531934906654,5.3030989721449773,4.6826408850706303,5.1619819748486684,7.1819768944861471,7.0793435812460963,5.1899099404359816,6.5362936569790673,4.5051120357497183,6.8822293457507131,6.2351425629081723,6.603717342707137,6.667523388287929,4.4568525488275519,6.3765414929752433,5.828706754523596,6.4499218896796222,6.0902158829740873,6.3114060058043018,7.6033541729600707,6.0321662337584891,6.3065096359635708,7.9346868627168687,6.1433232156945365,5.9224588925066257,6.9733194316600535,5.2107962884327623,6.5406420155011507,5.8651517540340539,6.2659878993528926,4.577665105630099,7.5095029001519347,4.5816543369869018,6.4238849072790005,7.0766839909313202,6.7839616509904488,4.2138109839567299,7.1849319947652086,4.9043126079254131,6.3974726521478473,5.1467661428365048,6.3046390451962973,7.1596094332945421,6.4655933275792847,5.727239760186948,6.305516281436538,5.999967682585492,5.1472581482816668,6.2483838017908448,4.6551481445724185,4.0811455023270913,7.0542512876554708,6.974567717365538,7.897155652952299,3.6569054919783648,7.2005410627114284,6.1646245255553715,4.8223611474064505,4.0250000000000004,5.979097627438609,5.6816643394571553,3.2542216014115475,7.4983136049827852,5.2162166001854589,6.7254803816591133,5.0331622280814425,5.2800311758253375,5.9382778807948,4.1878844146265051,6.5638935004395771,6.0403407867892858,4.682312495231173,5.3799284556724913,6.0620495926160469,4.1042567224550188,7.1184501369464961,4.2334110855964227,7.3821254262535554,5.4491211389938723,5.7308999367550717,3.138288233771358,3.9597125062857206,5.6052893326246096,5.0482265799686594,6.2579067369687591,3.7465472540108173,4.3301631641451941,7.510319779869957,7.7514582885536196,6.5493282558379198,5.8326743212313747,4.1241692219242783,5.9031437220804355,4.2112923399964171,5.681965193551342,5.987700264385948,5.6247284285344339,5.8847549335523777,5.9248611730780283,3.7367883353525073,4.5408721710248736,5.4225825693891885,6.7855721020991027,5.8189975857903411,7.1057391076114795,4.4124962245988488,4.0705266709699179,3.3578470808813665,6.7748123167092924,7.7307530699160578,6.037546826006988,3.9989448589305558,4.6715472540108172,3.7465472540108173,4.1889008244528885,4.1455587358578807,4.7024642278362458,4.0982426525940525,4.9500000000000002,6.0565652426482099,3.6569054919783648,3.6569054919783648,7.8324137529897113,6.1459093222692927,4.1303976008838239,5.5069054919783653,6.0746837633142796,4.5311449793413257,3.5672637299459131,5.5286043617987648,4.7595789040851901,4.2138109839567299,5.4331173318826576,5.4834352948109206,4.7784869773095444,5.3837865734328734,4.8366750384643815,3.7465472540108173,5.1912276685058529,4.6015055936180573,5.9213229487220786,7.2311664301368346,5.7167487444954341,4.5395798132097411,6.2319743444103599,7.0364393965271583,4.6610137795938904,6.0638109839567305,4.8749051610833511,4.8661710829371838,4.5819054919783646,4.78977819590507,4.0285993287691415,6.4248950807237062,6.1669970915024335,4.7121660085794774,5.3649103587614597,5.5318264978404672,3.676505593618058,6.7642075913321884,5.0513601558865577,4.3754884200215169,5.0429702649143149,5.5065035685873989,4.6160179227708031,5.0232426525940532,7.2869328119195629,5.8982183889683029,3.2628844146265052,6.4486451176498836,6.7167370430675506,6.5006750951512728,5.141802691237551,4.6793606034889557,4.3262917936679557,3.9501472354478686,3.8197899066048704,5.648132750141829,5.0068435431556537,5.81655408885781,3.4155731477194649,4.7619595835348294,5.6737871773538142,3.9353582379675478,4.4276550458632649,3.8197899066048704,4.0719588378905645,3.2351684330023698,3.3784527459891827,5.4068869351405304,6.0329792867441299,7.0728752574148013,3.8943694070075408,7.4206144713261546,3.3784527459891827,5.6923125626820639,6.186634173033644,5.9646417620324517,4.9293943348921836,4.7655615826630342,6.6370067038501315,5.0675719384874469,6.7576375396304469,4.2728931460881636,4.7882067869276987],"text":["Quantity: 5.040000e+02<br />Predicted Cost = $47,288<br />eye_preds: 4.728771e+04","Quantity: 1.579100e+04<br />Predicted Cost = $1,144,271<br />eye_preds: 1.144271e+06","Quantity: 1.429740e+04<br />Predicted Cost = $1,043,789<br />eye_preds: 1.043789e+06","Quantity: 1.400000e+02<br />Predicted Cost = $14,460<br />eye_preds: 1.446000e+04","Quantity: 1.135205e+05<br />Predicted Cost = $7,094,858<br />eye_preds: 7.094858e+06","Quantity: 4.353217e+05<br />Predicted Cost = $24,597,984<br />eye_preds: 2.459798e+07","Quantity: 1.303454e+05<br />Predicted Cost = $8,062,385<br />eye_preds: 8.062385e+06","Quantity: 2.136282e+05<br />Predicted Cost = $12,733,101<br />eye_preds: 1.273310e+07","Quantity: 1.647947e+05<br />Predicted Cost = $10,015,497<br />eye_preds: 1.001550e+07","Quantity: 2.211878e+05<br />Predicted Cost = $13,149,344<br />eye_preds: 1.314934e+07","Quantity: 4.198133e+04<br />Predicted Cost = $2,827,008<br />eye_preds: 2.827008e+06","Quantity: 5.667340e+04<br />Predicted Cost = $3,731,435<br />eye_preds: 3.731435e+06","Quantity: 4.670000e+02<br />Predicted Cost = $44,067<br />eye_preds: 4.406747e+04","Quantity: 5.263260e+04<br />Predicted Cost = $3,484,662<br />eye_preds: 3.484662e+06","Quantity: 2.200000e+03<br />Predicted Cost = $184,817<br />eye_preds: 1.848166e+05","Quantity: 4.844200e+02<br />Predicted Cost = $45,586<br />eye_preds: 4.558589e+04","Quantity: 4.258000e+03<br />Predicted Cost = $340,420<br />eye_preds: 3.404201e+05","Quantity: 3.560657e+04<br />Predicted Cost = $2,427,534<br />eye_preds: 2.427534e+06","Quantity: 1.301668e+05<br />Predicted Cost = $8,052,166<br />eye_preds: 8.052166e+06","Quantity: 8.925600e+03<br />Predicted Cost = $675,056<br />eye_preds: 6.750557e+05","Quantity: 1.316000e+02<br />Predicted Cost = $13,656<br />eye_preds: 1.365563e+04","Quantity: 1.263430e+06<br />Predicted Cost = $65,907,480<br />eye_preds: 6.590748e+07","Quantity: 8.578856e+05<br />Predicted Cost = $46,070,422<br />eye_preds: 4.607042e+07","Quantity: 2.345950e+03<br />Predicted Cost = $196,130<br />eye_preds: 1.961304e+05","Quantity: 1.455700e+03<br />Predicted Cost = $126,137<br />eye_preds: 1.261367e+05","Quantity: 4.963960e+03<br />Predicted Cost = $392,321<br />eye_preds: 3.923206e+05","Quantity: 1.861718e+04<br />Predicted Cost = $1,332,510<br />eye_preds: 1.332510e+06","Quantity: 1.612295e+04<br />Predicted Cost = $1,166,504<br />eye_preds: 1.166504e+06","Quantity: 1.243350e+06<br />Predicted Cost = $64,937,998<br />eye_preds: 6.493800e+07","Quantity: 6.649810e+05<br />Predicted Cost = $36,399,764<br />eye_preds: 3.639976e+07","Quantity: 3.044940e+04<br />Predicted Cost = $2,100,440<br />eye_preds: 2.100440e+06","Quantity: 1.446476e+04<br />Predicted Cost = $1,055,086<br />eye_preds: 1.055086e+06","Quantity: 2.750000e+01<br />Predicted Cost = $3,209<br />eye_preds: 3.209096e+03","Quantity: 1.974345e+04<br />Predicted Cost = $1,406,910<br />eye_preds: 1.406910e+06","Quantity: 9.080085e+04<br />Predicted Cost = $5,770,762<br />eye_preds: 5.770762e+06","Quantity: 5.001874e+06<br />Predicted Cost = $235,341,107<br />eye_preds: 2.353411e+08","Quantity: 5.210000e+01<br />Predicted Cost = $5,795<br />eye_preds: 5.795286e+03","Quantity: 1.281275e+04<br />Predicted Cost = $943,125<br />eye_preds: 9.431248e+05","Quantity: 6.964770e+03<br />Predicted Cost = $536,647<br />eye_preds: 5.366471e+05","Quantity: 7.190000e+01<br />Predicted Cost = $7,807<br />eye_preds: 7.806821e+03","Quantity: 1.950000e+02<br />Predicted Cost = $19,646<br />eye_preds: 1.964636e+04","Quantity: 1.100000e+02<br />Predicted Cost = $11,569<br />eye_preds: 1.156880e+04","Quantity: 1.754891e+05<br />Predicted Cost = $10,615,278<br />eye_preds: 1.061528e+07","Quantity: 1.323306e+05<br />Predicted Cost = $8,175,901<br />eye_preds: 8.175901e+06","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />eye_preds: 8.617043e+03","Quantity: 8.880000e+02<br />Predicted Cost = $79,851<br />eye_preds: 7.985130e+04","Quantity: 3.465800e+03<br />Predicted Cost = $281,396<br />eye_preds: 2.813962e+05","Quantity: 1.187878e+05<br />Predicted Cost = $7,398,847<br />eye_preds: 7.398847e+06","Quantity: 1.750138e+05<br />Predicted Cost = $10,588,681<br />eye_preds: 1.058868e+07","Quantity: 6.056420e+05<br />Predicted Cost = $33,384,877<br />eye_preds: 3.338488e+07","Quantity: 5.000000e+03<br />Predicted Cost = $394,955<br />eye_preds: 3.949547e+05","Quantity: 1.558000e+04<br />Predicted Cost = $1,130,121<br />eye_preds: 1.130121e+06","Quantity: 1.720000e+01<br />Predicted Cost = $2,079<br />eye_preds: 2.079045e+03","Quantity: 3.083200e+03<br />Predicted Cost = $252,538<br />eye_preds: 2.525379e+05","Quantity: 7.932393e+05<br />Predicted Cost = $42,849,811<br />eye_preds: 4.284981e+07","Quantity: 7.928390e+04<br />Predicted Cost = $5,090,332<br />eye_preds: 5.090332e+06","Quantity: 1.439547e+06<br />Predicted Cost = $74,363,366<br />eye_preds: 7.436337e+07","Quantity: 7.638000e+03<br />Predicted Cost = $584,462<br />eye_preds: 5.844619e+05","Quantity: 1.105035e+06<br />Predicted Cost = $58,226,781<br />eye_preds: 5.822678e+07","Quantity: 2.700000e+02<br />Predicted Cost = $26,547<br />eye_preds: 2.654676e+04","Quantity: 1.603400e+03<br />Predicted Cost = $137,932<br />eye_preds: 1.379316e+05","Quantity: 3.274200e+03<br />Predicted Cost = $266,976<br />eye_preds: 2.669761e+05","Quantity: 3.736000e+03<br />Predicted Cost = $301,631<br />eye_preds: 3.016313e+05","Quantity: 8.824000e+03<br />Predicted Cost = $667,945<br />eye_preds: 6.679448e+05","Quantity: 1.711566e+06<br />Predicted Cost = $87,274,858<br />eye_preds: 8.727486e+07","Quantity: 3.271700e+03<br />Predicted Cost = $266,787<br />eye_preds: 2.667875e+05","Quantity: 7.000000e+01<br />Predicted Cost = $7,616<br />eye_preds: 7.615803e+03","Quantity: 8.800000e+02<br />Predicted Cost = $79,186<br />eye_preds: 7.918565e+04","Quantity: 1.297303e+06<br />Predicted Cost = $67,540,345<br />eye_preds: 6.754034e+07","Quantity: 6.116253e+06<br />Predicted Cost = $283,464,732<br />eye_preds: 2.834647e+08","Quantity: 2.371645e+04<br />Predicted Cost = $1,666,944<br />eye_preds: 1.666944e+06","Quantity: 2.692136e+06<br />Predicted Cost = $132,690,466<br />eye_preds: 1.326905e+08","Quantity: 1.158000e+03<br />Predicted Cost = $102,078<br />eye_preds: 1.020776e+05","Quantity: 1.860052e+05<br />Predicted Cost = $11,202,390<br />eye_preds: 1.120239e+07","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />eye_preds: 2.390302e+03","Quantity: 1.972746e+01<br />Predicted Cost = $2,360<br />eye_preds: 2.360157e+03","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />eye_preds: 1.253842e+04","Quantity: 2.311582e+04<br />Predicted Cost = $1,627,856<br />eye_preds: 1.627856e+06","Quantity: 1.598148e+02<br />Predicted Cost = $16,344<br />eye_preds: 1.634352e+04","Quantity: 1.230600e+02<br />Predicted Cost = $12,834<br />eye_preds: 1.283388e+04","Quantity: 1.266960e+04<br />Predicted Cost = $933,374<br />eye_preds: 9.333740e+05","Quantity: 5.396000e+03<br />Predicted Cost = $423,805<br />eye_preds: 4.238055e+05","Quantity: 1.800000e+02<br />Predicted Cost = $18,244<br />eye_preds: 1.824429e+04","Quantity: 3.500000e+01<br />Predicted Cost = $4,011<br />eye_preds: 4.011095e+03","Quantity: 2.280000e+02<br />Predicted Cost = $22,703<br />eye_preds: 2.270334e+04","Quantity: 1.441217e+04<br />Predicted Cost = $1,051,537<br />eye_preds: 1.051537e+06","Quantity: 1.737360e+03<br />Predicted Cost = $148,559<br />eye_preds: 1.485587e+05","Quantity: 7.000000e+01<br />Predicted Cost = $7,616<br />eye_preds: 7.615803e+03","Quantity: 2.349000e+03<br />Predicted Cost = $196,366<br />eye_preds: 1.963662e+05","Quantity: 7.867000e+03<br />Predicted Cost = $600,653<br />eye_preds: 6.006527e+05","Quantity: 1.123299e+05<br />Predicted Cost = $7,026,000<br />eye_preds: 7.026000e+06","Quantity: 4.015700e+03<br />Predicted Cost = $322,462<br />eye_preds: 3.224625e+05","Quantity: 1.237900e+03<br />Predicted Cost = $108,576<br />eye_preds: 1.085761e+05","Quantity: 3.500000e+02<br />Predicted Cost = $33,749<br />eye_preds: 3.374916e+04","Quantity: 3.230785e+04<br />Predicted Cost = $2,218,758<br />eye_preds: 2.218758e+06","Quantity: 5.347060e+03<br />Predicted Cost = $420,249<br />eye_preds: 4.202487e+05","Quantity: 9.680060e+02<br />Predicted Cost = $86,484<br />eye_preds: 8.648428e+04","Quantity: 2.246476e+05<br />Predicted Cost = $13,339,487<br />eye_preds: 1.333949e+07","Quantity: 1.572450e+03<br />Predicted Cost = $135,467<br />eye_preds: 1.354670e+05","Quantity: 7.337740e+03<br />Predicted Cost = $563,177<br />eye_preds: 5.631773e+05","Quantity: 4.862000e+03<br />Predicted Cost = $384,861<br />eye_preds: 3.848609e+05","Quantity: 2.060524e+04<br />Predicted Cost = $1,463,623<br />eye_preds: 1.463623e+06","Quantity: 5.279580e+03<br />Predicted Cost = $415,341<br />eye_preds: 4.153406e+05","Quantity: 1.064080e+06<br />Predicted Cost = $56,227,818<br />eye_preds: 5.622782e+07","Quantity: 2.800000e+04<br />Predicted Cost = $1,943,664<br />eye_preds: 1.943664e+06","Quantity: 7.605860e+03<br />Predicted Cost = $582,187<br />eye_preds: 5.821866e+05","Quantity: 3.200000e+02<br />Predicted Cost = $31,064<br />eye_preds: 3.106445e+04","Quantity: 2.499960e+05<br />Predicted Cost = $14,726,111<br />eye_preds: 1.472611e+07","Quantity: 4.628601e+01<br />Predicted Cost = $5,194<br />eye_preds: 5.194467e+03","Quantity: 9.840000e+02<br />Predicted Cost = $87,805<br />eye_preds: 8.780525e+04","Quantity: 2.871600e+04<br />Predicted Cost = $1,989,595<br />eye_preds: 1.989595e+06","Quantity: 1.773000e+03<br />Predicted Cost = $151,375<br />eye_preds: 1.513755e+05","Quantity: 3.635760e+04<br />Predicted Cost = $2,474,860<br />eye_preds: 2.474860e+06","Quantity: 2.986000e+03<br />Predicted Cost = $245,165<br />eye_preds: 2.451648e+05","Quantity: 4.480000e+04<br />Predicted Cost = $3,002,148<br />eye_preds: 3.002148e+06","Quantity: 1.092592e+04<br />Predicted Cost = $813,905<br />eye_preds: 8.139049e+05","Quantity: 2.760000e+03<br />Predicted Cost = $227,951<br />eye_preds: 2.279507e+05","Quantity: 4.307620e+05<br />Predicted Cost = $24,359,568<br />eye_preds: 2.435957e+07","Quantity: 2.053206e+05<br />Predicted Cost = $12,274,395<br />eye_preds: 1.227439e+07","Quantity: 9.500000e+01<br />Predicted Cost = $10,102<br />eye_preds: 1.010170e+04","Quantity: 6.533300e+03<br />Predicted Cost = $505,822<br />eye_preds: 5.058219e+05","Quantity: 1.231300e+02<br />Predicted Cost = $12,841<br />eye_preds: 1.284064e+04","Quantity: 4.982200e+03<br />Predicted Cost = $393,654<br />eye_preds: 3.936539e+05","Quantity: 2.000000e+05<br />Predicted Cost = $11,979,888<br />eye_preds: 1.197989e+07","Quantity: 7.968000e+02<br />Predicted Cost = $72,235<br />eye_preds: 7.223507e+04","Quantity: 4.762900e+03<br />Predicted Cost = $377,599<br />eye_preds: 3.775992e+05","Quantity: 3.730000e+02<br />Predicted Cost = $35,796<br />eye_preds: 3.579568e+04","Quantity: 3.000000e+02<br />Predicted Cost = $29,264<br />eye_preds: 2.926423e+04","Quantity: 9.520000e+02<br />Predicted Cost = $85,161<br />eye_preds: 8.516069e+04","Quantity: 6.130000e+02<br />Predicted Cost = $56,676<br />eye_preds: 5.667623e+04","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />eye_preds: 6.603739e+03","Quantity: 3.900000e+02<br />Predicted Cost = $37,302<br />eye_preds: 3.730223e+04","Quantity: 3.800370e+04<br />Predicted Cost = $2,578,333<br />eye_preds: 2.578333e+06","Quantity: 2.733750e+03<br />Predicted Cost = $225,945<br />eye_preds: 2.259446e+05","Quantity: 9.301000e+03<br />Predicted Cost = $701,278<br />eye_preds: 7.012775e+05","Quantity: 1.209226e+04<br />Predicted Cost = $893,963<br />eye_preds: 8.939627e+05","Quantity: 1.344000e+03<br />Predicted Cost = $117,157<br />eye_preds: 1.171573e+05","Quantity: 9.625000e+01<br />Predicted Cost = $10,225<br />eye_preds: 1.022458e+04","Quantity: 9.508000e+04<br />Predicted Cost = $6,021,885<br />eye_preds: 6.021885e+06","Quantity: 6.930000e+02<br />Predicted Cost = $63,486<br />eye_preds: 6.348604e+04","Quantity: 1.117750e+04<br />Predicted Cost = $831,225<br />eye_preds: 8.312254e+05","Quantity: 3.164186e+04<br />Predicted Cost = $2,176,418<br />eye_preds: 2.176418e+06","Quantity: 9.206397e+03<br />Predicted Cost = $694,677<br />eye_preds: 6.946771e+05","Quantity: 2.798560e+04<br />Predicted Cost = $1,942,739<br />eye_preds: 1.942739e+06","Quantity: 3.588177e+05<br />Predicted Cost = $20,571,136<br />eye_preds: 2.057114e+07","Quantity: 4.328141e+05<br />Predicted Cost = $24,466,891<br />eye_preds: 2.446689e+07","Quantity: 2.375821e+05<br />Predicted Cost = $14,048,429<br />eye_preds: 1.404843e+07","Quantity: 8.857768e+04<br />Predicted Cost = $5,639,946<br />eye_preds: 5.639946e+06","Quantity: 1.304293e+05<br />Predicted Cost = $8,067,185<br />eye_preds: 8.067185e+06","Quantity: 3.223786e+05<br />Predicted Cost = $18,631,110<br />eye_preds: 1.863111e+07","Quantity: 3.340000e+02<br />Predicted Cost = $32,320<br />eye_preds: 3.231956e+04","Quantity: 1.269900e+03<br />Predicted Cost = $111,170<br />eye_preds: 1.111698e+05","Quantity: 5.290670e+04<br />Predicted Cost = $3,501,445<br />eye_preds: 3.501445e+06","Quantity: 3.615121e+05<br />Predicted Cost = $20,713,983<br />eye_preds: 2.071398e+07","Quantity: 1.284000e+03<br />Predicted Cost = $112,311<br />eye_preds: 1.123111e+05","Quantity: 3.502578e+04<br />Predicted Cost = $2,390,885<br />eye_preds: 2.390885e+06","Quantity: 1.804918e+05<br />Predicted Cost = $10,894,901<br />eye_preds: 1.089490e+07","Quantity: 7.353600e+02<br />Predicted Cost = $67,068<br />eye_preds: 6.706756e+04","Quantity: 4.252400e+04<br />Predicted Cost = $2,860,794<br />eye_preds: 2.860794e+06","Quantity: 2.400000e+02<br />Predicted Cost = $23,806<br />eye_preds: 2.380649e+04","Quantity: 2.431282e+05<br />Predicted Cost = $14,351,512<br />eye_preds: 1.435151e+07","Quantity: 4.823320e+03<br />Predicted Cost = $382,028<br />eye_preds: 3.820279e+05","Quantity: 1.800000e+02<br />Predicted Cost = $18,244<br />eye_preds: 1.824429e+04","Quantity: 1.970060e+04<br />Predicted Cost = $1,404,085<br />eye_preds: 1.404085e+06","Quantity: 1.464000e+03<br />Predicted Cost = $126,802<br />eye_preds: 1.268018e+05","Quantity: 2.318340e+04<br />Predicted Cost = $1,632,258<br />eye_preds: 1.632258e+06","Quantity: 8.890000e+02<br />Predicted Cost = $79,934<br />eye_preds: 7.993448e+04","Quantity: 8.050788e+05<br />Predicted Cost = $43,441,070<br />eye_preds: 4.344107e+07","Quantity: 1.033740e+04<br />Predicted Cost = $773,269<br />eye_preds: 7.732688e+05","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />eye_preds: 2.390302e+03","Quantity: 9.000000e+01<br />Predicted Cost = $9,609<br />eye_preds: 9.608914e+03","Quantity: 3.497000e+02<br />Predicted Cost = $33,722<br />eye_preds: 3.372240e+04","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />eye_preds: 3.464015e+04","Quantity: 1.767480e+03<br />Predicted Cost = $150,940<br />eye_preds: 1.509395e+05","Quantity: 1.745324e+04<br />Predicted Cost = $1,255,265<br />eye_preds: 1.255265e+06","Quantity: 2.300000e+02<br />Predicted Cost = $22,887<br />eye_preds: 2.288749e+04","Quantity: 8.576000e+02<br />Predicted Cost = $77,319<br />eye_preds: 7.731939e+04","Quantity: 3.364800e+02<br />Predicted Cost = $32,541<br />eye_preds: 3.254148e+04","Quantity: 7.816600e+04<br />Predicted Cost = $5,023,906<br />eye_preds: 5.023906e+06","Quantity: 2.800000e+01<br />Predicted Cost = $3,263<br />eye_preds: 3.263031e+03","Quantity: 1.800000e+01<br />Predicted Cost = $2,168<br />eye_preds: 2.168338e+03","Quantity: 2.261666e+04<br />Predicted Cost = $1,595,315<br />eye_preds: 1.595315e+06","Quantity: 1.010320e+05<br />Predicted Cost = $6,369,781<br />eye_preds: 6.369781e+06","Quantity: 5.230000e+03<br />Predicted Cost = $411,731<br />eye_preds: 4.117315e+05","Quantity: 7.200000e+02<br />Predicted Cost = $65,771<br />eye_preds: 6.577071e+04","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />eye_preds: 6.603739e+03","Quantity: 3.992000e+01<br />Predicted Cost = $4,530<br />eye_preds: 4.530032e+03","Quantity: 2.740000e+03<br />Predicted Cost = $226,422<br />eye_preds: 2.264223e+05","Quantity: 7.230000e+02<br />Predicted Cost = $66,024<br />eye_preds: 6.602417e+04","Quantity: 2.400000e+03<br />Predicted Cost = $200,307<br />eye_preds: 2.003067e+05","Quantity: 2.314703e+03<br />Predicted Cost = $193,713<br />eye_preds: 1.937127e+05","Quantity: 1.093400e+04<br />Predicted Cost = $814,462<br />eye_preds: 8.144616e+05","Quantity: 4.920000e+02<br />Predicted Cost = $46,245<br />eye_preds: 4.624531e+04","Quantity: 9.600000e+01<br />Predicted Cost = $10,200<br />eye_preds: 1.020002e+04","Quantity: 3.000000e+01<br />Predicted Cost = $3,478<br />eye_preds: 3.478061e+03","Quantity: 7.680000e+02<br />Predicted Cost = $69,817<br />eye_preds: 6.981667e+04","Quantity: 1.463560e+03<br />Predicted Cost = $126,767<br />eye_preds: 1.267666e+05","Quantity: 3.240000e+02<br />Predicted Cost = $31,423<br />eye_preds: 3.142347e+04","Quantity: 1.475522e+04<br />Predicted Cost = $1,074,669<br />eye_preds: 1.074669e+06","Quantity: 1.999800e+04<br />Predicted Cost = $1,423,681<br />eye_preds: 1.423681e+06","Quantity: 1.022700e+03<br />Predicted Cost = $90,995<br />eye_preds: 9.099492e+04","Quantity: 2.822000e+03<br />Predicted Cost = $232,683<br />eye_preds: 2.326833e+05","Quantity: 4.647900e+04<br />Predicted Cost = $3,106,079<br />eye_preds: 3.106079e+06","Quantity: 1.625000e+03<br />Predicted Cost = $139,649<br />eye_preds: 1.396495e+05","Quantity: 2.612800e+04<br />Predicted Cost = $1,823,153<br />eye_preds: 1.823153e+06","Quantity: 2.027750e+03<br />Predicted Cost = $171,391<br />eye_preds: 1.713911e+05","Quantity: 4.980000e+01<br />Predicted Cost = $5,558<br />eye_preds: 5.558238e+03","Quantity: 1.170000e+02<br />Predicted Cost = $12,248<br />eye_preds: 1.224819e+04","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />eye_preds: 3.464015e+04","Quantity: 1.610000e+03<br />Predicted Cost = $138,457<br />eye_preds: 1.384567e+05","Quantity: 4.321300e+03<br />Predicted Cost = $345,099<br />eye_preds: 3.450987e+05","Quantity: 8.078800e+03<br />Predicted Cost = $615,596<br />eye_preds: 6.155961e+05","Quantity: 4.940000e+04<br />Predicted Cost = $3,286,226<br />eye_preds: 3.286226e+06","Quantity: 6.880000e+01<br />Predicted Cost = $7,495<br />eye_preds: 7.494960e+03","Quantity: 1.199908e+04<br />Predicted Cost = $887,589<br />eye_preds: 8.875889e+05","Quantity: 1.518520e+01<br />Predicted Cost = $1,853<br />eye_preds: 1.852738e+03","Quantity: 6.131000e+03<br />Predicted Cost = $476,943<br />eye_preds: 4.769430e+05","Quantity: 3.670000e+02<br />Predicted Cost = $35,263<br />eye_preds: 3.526274e+04","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />eye_preds: 8.617043e+03","Quantity: 3.000406e+06<br />Predicted Cost = $146,686,943<br />eye_preds: 1.466869e+08","Quantity: 2.581000e+03<br />Predicted Cost = $214,242<br />eye_preds: 2.142416e+05","Quantity: 3.940000e+01<br />Predicted Cost = $4,475<br />eye_preds: 4.475422e+03","Quantity: 5.000000e+02<br />Predicted Cost = $46,940<br />eye_preds: 4.694045e+04","Quantity: 2.040000e+02<br />Predicted Cost = $20,484<br />eye_preds: 2.048368e+04","Quantity: 2.160000e+03<br />Predicted Cost = $181,706<br />eye_preds: 1.817062e+05","Quantity: 8.162275e+01<br />Predicted Cost = $8,779<br />eye_preds: 8.778602e+03","Quantity: 2.440000e+02<br />Predicted Cost = $24,173<br />eye_preds: 2.417328e+04","Quantity: 2.000000e+02<br />Predicted Cost = $20,112<br />eye_preds: 2.011188e+04","Quantity: 7.666666e+02<br />Predicted Cost = $69,705<br />eye_preds: 6.970454e+04","Quantity: 8.800000e+02<br />Predicted Cost = $79,186<br />eye_preds: 7.918565e+04","Quantity: 6.920000e+02<br />Predicted Cost = $63,401<br />eye_preds: 6.340130e+04","Quantity: 1.764000e+03<br />Predicted Cost = $150,665<br />eye_preds: 1.506646e+05","Quantity: 3.029140e+04<br />Predicted Cost = $2,090,357<br />eye_preds: 2.090357e+06","Quantity: 2.724560e+03<br />Predicted Cost = $225,242<br />eye_preds: 2.252419e+05","Quantity: 5.356880e+03<br />Predicted Cost = $420,963<br />eye_preds: 4.209626e+05","Quantity: 8.864000e+03<br />Predicted Cost = $670,745<br />eye_preds: 6.707451e+05","Quantity: 1.006000e+03<br />Predicted Cost = $89,620<br />eye_preds: 8.961963e+04","Quantity: 3.290060e+02<br />Predicted Cost = $31,872<br />eye_preds: 3.187231e+04","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />eye_preds: 1.541290e+04","Quantity: 5.708050e+03<br />Predicted Cost = $446,428<br />eye_preds: 4.464278e+05","Quantity: 1.146000e+03<br />Predicted Cost = $101,099<br />eye_preds: 1.010987e+05","Quantity: 2.920000e+03<br />Predicted Cost = $240,148<br />eye_preds: 2.401481e+05","Quantity: 2.621230e+03<br />Predicted Cost = $217,329<br />eye_preds: 2.173288e+05","Quantity: 2.690000e+02<br />Predicted Cost = $26,456<br />eye_preds: 2.645580e+04","Quantity: 7.116400e+02<br />Predicted Cost = $65,064<br />eye_preds: 6.506401e+04","Quantity: 1.578900e+04<br />Predicted Cost = $1,144,137<br />eye_preds: 1.144137e+06","Quantity: 3.329200e+04<br />Predicted Cost = $2,281,205<br />eye_preds: 2.281205e+06","Quantity: 7.320000e+04<br />Predicted Cost = $4,727,949<br />eye_preds: 4.727949e+06","Quantity: 4.965800e+03<br />Predicted Cost = $392,455<br />eye_preds: 3.924551e+05","Quantity: 9.388458e+04<br />Predicted Cost = $5,951,819<br />eye_preds: 5.951819e+06","Quantity: 4.666667e+03<br />Predicted Cost = $370,537<br />eye_preds: 3.705367e+05","Quantity: 7.128000e+03<br />Predicted Cost = $548,271<br />eye_preds: 5.482708e+05","Quantity: 9.887500e+02<br />Predicted Cost = $88,197<br />eye_preds: 8.819724e+04","Quantity: 3.384440e+03<br />Predicted Cost = $275,280<br />eye_preds: 2.752804e+05","Quantity: 1.400000e+04<br />Predicted Cost = $1,023,690<br />eye_preds: 1.023690e+06","Quantity: 1.232000e+02<br />Predicted Cost = $12,847<br />eye_preds: 1.284739e+04","Quantity: 6.668112e+04<br />Predicted Cost = $4,337,132<br />eye_preds: 4.337132e+06","Quantity: 6.840000e+02<br />Predicted Cost = $62,723<br />eye_preds: 6.272301e+04","Quantity: 3.467000e+03<br />Predicted Cost = $281,486<br />eye_preds: 2.814863e+05","Quantity: 8.850000e+02<br />Predicted Cost = $79,602<br />eye_preds: 7.960173e+04","Quantity: 5.700000e+02<br />Predicted Cost = $52,989<br />eye_preds: 5.298882e+04","Quantity: 4.500000e+03<br />Predicted Cost = $358,279<br />eye_preds: 3.582792e+05","Quantity: 5.694400e+02<br />Predicted Cost = $52,941<br />eye_preds: 5.294066e+04","Quantity: 1.744720e+03<br />Predicted Cost = $149,141<br />eye_preds: 1.491407e+05","Quantity: 6.648000e+03<br />Predicted Cost = $514,031<br />eye_preds: 5.140309e+05","Quantity: 4.440000e+02<br />Predicted Cost = $42,056<br />eye_preds: 4.205612e+04","Quantity: 2.100000e+02<br />Predicted Cost = $21,040<br />eye_preds: 2.104034e+04","Quantity: 2.600000e+02<br />Predicted Cost = $25,636<br />eye_preds: 2.563600e+04","Quantity: 5.400000e+02<br />Predicted Cost = $50,404<br />eye_preds: 5.040391e+04","Quantity: 1.093400e+04<br />Predicted Cost = $814,462<br />eye_preds: 8.144616e+05","Quantity: 1.376620e+03<br />Predicted Cost = $119,785<br />eye_preds: 1.197852e+05","Quantity: 3.140000e+02<br />Predicted Cost = $30,525<br />eye_preds: 3.052530e+04","Quantity: 6.600000e+01<br />Predicted Cost = $7,212<br />eye_preds: 7.212372e+03","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />eye_preds: 1.636104e+04","Quantity: 5.002000e+03<br />Predicted Cost = $395,101<br />eye_preds: 3.951008e+05","Quantity: 2.790000e+01<br />Predicted Cost = $3,252<br />eye_preds: 3.252250e+03","Quantity: 1.900000e+01<br />Predicted Cost = $2,280<br />eye_preds: 2.279539e+03","Quantity: 3.330000e+02<br />Predicted Cost = $32,230<br />eye_preds: 3.223005e+04","Quantity: 6.616000e+03<br />Predicted Cost = $511,742<br />eye_preds: 5.117417e+05","Quantity: 7.812000e+02<br />Predicted Cost = $70,926<br />eye_preds: 7.092593e+04","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />eye_preds: 2.390302e+03","Quantity: 3.868044e+05<br />Predicted Cost = $22,051,065<br />eye_preds: 2.205107e+07","Quantity: 2.200000e+02<br />Predicted Cost = $21,965<br />eye_preds: 2.196549e+04","Quantity: 2.880000e+02<br />Predicted Cost = $28,180<br />eye_preds: 2.817981e+04","Quantity: 7.111200e+04<br />Predicted Cost = $4,603,066<br />eye_preds: 4.603066e+06","Quantity: 5.422000e+03<br />Predicted Cost = $425,694<br />eye_preds: 4.256940e+05","Quantity: 6.677400e+04<br />Predicted Cost = $4,342,720<br />eye_preds: 4.342720e+06","Quantity: 4.713560e+04<br />Predicted Cost = $3,146,646<br />eye_preds: 3.146646e+06","Quantity: 6.238100e+04<br />Predicted Cost = $4,077,776<br />eye_preds: 4.077776e+06","Quantity: 3.200000e+01<br />Predicted Cost = $3,692<br />eye_preds: 3.692017e+03","Quantity: 1.250000e+04<br />Predicted Cost = $921,811<br />eye_preds: 9.218107e+05","Quantity: 1.046000e+03<br />Predicted Cost = $92,911<br />eye_preds: 9.291093e+04","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />eye_preds: 1.253842e+04","Quantity: 2.470000e+03<br />Predicted Cost = $205,705<br />eye_preds: 2.057049e+05","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />eye_preds: 4.538428e+03","Quantity: 1.069444e+05<br />Predicted Cost = $6,713,843<br />eye_preds: 6.713843e+06","Quantity: 1.069444e+05<br />Predicted Cost = $6,713,843<br />eye_preds: 6.713843e+06","Quantity: 2.338336e+05<br />Predicted Cost = $13,843,279<br />eye_preds: 1.384328e+07","Quantity: 1.500740e+04<br />Predicted Cost = $1,091,648<br />eye_preds: 1.091648e+06","Quantity: 1.832020e+05<br />Predicted Cost = $11,046,137<br />eye_preds: 1.104614e+07","Quantity: 9.875060e+03<br />Predicted Cost = $741,224<br />eye_preds: 7.412236e+05","Quantity: 3.083050e+05<br />Predicted Cost = $17,877,510<br />eye_preds: 1.787751e+07","Quantity: 3.049568e+05<br />Predicted Cost = $17,697,847<br />eye_preds: 1.769785e+07","Quantity: 6.166100e+05<br />Predicted Cost = $33,943,746<br />eye_preds: 3.394375e+07","Quantity: 5.170000e+03<br />Predicted Cost = $407,360<br />eye_preds: 4.073603e+05","Quantity: 8.834000e+01<br />Predicted Cost = $9,445<br />eye_preds: 9.444861e+03","Quantity: 1.224500e+03<br />Predicted Cost = $107,488<br />eye_preds: 1.074885e+05","Quantity: 9.537600e+02<br />Predicted Cost = $85,306<br />eye_preds: 8.530631e+04","Quantity: 1.418255e+04<br />Predicted Cost = $1,036,031<br />eye_preds: 1.036031e+06","Quantity: 1.394995e+04<br />Predicted Cost = $1,020,304<br />eye_preds: 1.020304e+06","Quantity: 7.287010e+04<br />Predicted Cost = $4,708,235<br />eye_preds: 4.708235e+06","Quantity: 5.800000e+01<br />Predicted Cost = $6,400<br />eye_preds: 6.399866e+03","Quantity: 4.963000e+03<br />Predicted Cost = $392,250<br />eye_preds: 3.922505e+05","Quantity: 3.913800e+04<br />Predicted Cost = $2,649,438<br />eye_preds: 2.649438e+06","Quantity: 2.462600e+04<br />Predicted Cost = $1,725,994<br />eye_preds: 1.725994e+06","Quantity: 9.698600e+03<br />Predicted Cost = $728,964<br />eye_preds: 7.289636e+05","Quantity: 1.416077e+04<br />Predicted Cost = $1,034,559<br />eye_preds: 1.034559e+06","Quantity: 5.706400e+03<br />Predicted Cost = $446,308<br />eye_preds: 4.463084e+05","Quantity: 2.468800e+02<br />Predicted Cost = $24,437<br />eye_preds: 2.443709e+04","Quantity: 7.142000e+02<br />Predicted Cost = $65,280<br />eye_preds: 6.528048e+04","Quantity: 2.477365e+04<br />Predicted Cost = $1,735,564<br />eye_preds: 1.735564e+06","Quantity: 3.839000e+03<br />Predicted Cost = $309,316<br />eye_preds: 3.093156e+05","Quantity: 1.168000e+03<br />Predicted Cost = $102,893<br />eye_preds: 1.028927e+05","Quantity: 8.000000e+03<br />Predicted Cost = $610,040<br />eye_preds: 6.100399e+05","Quantity: 2.240000e+02<br />Predicted Cost = $22,335<br />eye_preds: 2.233466e+04","Quantity: 1.027300e+04<br />Predicted Cost = $768,812<br />eye_preds: 7.688117e+05","Quantity: 5.700000e+01<br />Predicted Cost = $6,298<br />eye_preds: 6.297733e+03","Quantity: 9.209000e+03<br />Predicted Cost = $694,859<br />eye_preds: 6.948588e+05","Quantity: 8.400000e+04<br />Predicted Cost = $5,369,803<br />eye_preds: 5.369803e+06","Quantity: 3.000000e+01<br />Predicted Cost = $3,478<br />eye_preds: 3.478061e+03","Quantity: 4.674000e+02<br />Predicted Cost = $44,102<br />eye_preds: 4.410238e+04","Quantity: 1.685383e+05<br />Predicted Cost = $10,225,777<br />eye_preds: 1.022578e+07","Quantity: 3.431663e+05<br />Predicted Cost = $19,739,757<br />eye_preds: 1.973976e+07","Quantity: 3.000000e+03<br />Predicted Cost = $246,228<br />eye_preds: 2.462278e+05","Quantity: 1.104610e+03<br />Predicted Cost = $97,717<br />eye_preds: 9.771659e+04","Quantity: 5.523345e+04<br />Predicted Cost = $3,643,653<br />eye_preds: 3.643653e+06","Quantity: 1.481800e+04<br />Predicted Cost = $1,078,898<br />eye_preds: 1.078898e+06","Quantity: 2.812100e+04<br />Predicted Cost = $1,951,432<br />eye_preds: 1.951432e+06","Quantity: 5.484920e+04<br />Predicted Cost = $3,620,200<br />eye_preds: 3.620200e+06","Quantity: 2.179000e+03<br />Predicted Cost = $183,184<br />eye_preds: 1.831842e+05","Quantity: 8.120000e+01<br />Predicted Cost = $8,737<br />eye_preds: 8.736537e+03","Quantity: 4.060000e+01<br />Predicted Cost = $4,601<br />eye_preds: 4.601364e+03","Quantity: 2.400000e+02<br />Predicted Cost = $23,806<br />eye_preds: 2.380649e+04","Quantity: 1.302000e+02<br />Predicted Cost = $13,521<br />eye_preds: 1.352120e+04","Quantity: 2.194000e+04<br />Predicted Cost = $1,551,115<br />eye_preds: 1.551115e+06","Quantity: 1.421300e+03<br />Predicted Cost = $123,377<br />eye_preds: 1.233770e+05","Quantity: 9.000000e+01<br />Predicted Cost = $9,609<br />eye_preds: 9.608914e+03","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />eye_preds: 1.253842e+04","Quantity: 1.357000e+03<br />Predicted Cost = $118,205<br />eye_preds: 1.182051e+05","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />eye_preds: 6.603739e+03","Quantity: 5.887770e+04<br />Predicted Cost = $3,865,490<br />eye_preds: 3.865490e+06","Quantity: 6.891000e+02<br />Predicted Cost = $63,155<br />eye_preds: 6.315549e+04","Quantity: 7.200000e+03<br />Predicted Cost = $553,392<br />eye_preds: 5.533916e+05","Quantity: 3.700000e+01<br />Predicted Cost = $4,223<br />eye_preds: 4.222665e+03","Quantity: 6.880040e+04<br />Predicted Cost = $4,464,488<br />eye_preds: 4.464488e+06","Quantity: 8.550000e+05<br />Predicted Cost = $45,927,061<br />eye_preds: 4.592706e+07","Quantity: 1.740600e+04<br />Predicted Cost = $1,252,122<br />eye_preds: 1.252122e+06","Quantity: 5.150000e+02<br />Predicted Cost = $48,242<br />eye_preds: 4.824160e+04","Quantity: 5.083000e+03<br />Predicted Cost = $401,015<br />eye_preds: 4.010154e+05","Quantity: 5.377850e+04<br />Predicted Cost = $3,554,782<br />eye_preds: 3.554782e+06","Quantity: 2.500000e+02<br />Predicted Cost = $24,723<br />eye_preds: 2.472262e+04","Quantity: 3.447911e+04<br />Predicted Cost = $2,356,348<br />eye_preds: 2.356348e+06","Quantity: 2.792500e+04<br />Predicted Cost = $1,938,848<br />eye_preds: 1.938848e+06","Quantity: 2.815940e+03<br />Predicted Cost = $232,221<br />eye_preds: 2.322211e+05","Quantity: 1.349610e+05<br />Predicted Cost = $8,326,120<br />eye_preds: 8.326120e+06","Quantity: 1.450000e+03<br />Predicted Cost = $125,680<br />eye_preds: 1.256798e+05","Quantity: 3.560000e+02<br />Predicted Cost = $34,284<br />eye_preds: 3.428398e+04","Quantity: 7.337196e+04<br />Predicted Cost = $4,738,222<br />eye_preds: 4.738222e+06","Quantity: 2.809086e+05<br />Predicted Cost = $16,402,974<br />eye_preds: 1.640297e+07","Quantity: 1.866200e+02<br />Predicted Cost = $18,864<br />eye_preds: 1.886411e+04","Quantity: 1.400000e+04<br />Predicted Cost = $1,023,690<br />eye_preds: 1.023690e+06","Quantity: 6.188000e+03<br />Predicted Cost = $481,043<br />eye_preds: 4.810431e+05","Quantity: 4.296124e+05<br />Predicted Cost = $24,299,429<br />eye_preds: 2.429943e+07","Quantity: 1.215400e+03<br />Predicted Cost = $106,749<br />eye_preds: 1.067494e+05","Quantity: 2.660000e+03<br />Predicted Cost = $220,301<br />eye_preds: 2.203005e+05","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />eye_preds: 1.541290e+04","Quantity: 1.650802e+01<br />Predicted Cost = $2,002<br />eye_preds: 2.001557e+03","Quantity: 1.657000e+01<br />Predicted Cost = $2,009<br />eye_preds: 2.008507e+03","Quantity: 1.142980e+05<br />Predicted Cost = $7,139,793<br />eye_preds: 7.139793e+06","Quantity: 6.838600e+02<br />Predicted Cost = $62,711<br />eye_preds: 6.271114e+04","Quantity: 2.600000e+04<br />Predicted Cost = $1,814,890<br />eye_preds: 1.814890e+06","Quantity: 7.118797e+05<br />Predicted Cost = $38,768,242<br />eye_preds: 3.876824e+07","Quantity: 1.680000e+01<br />Predicted Cost = $2,034<br />eye_preds: 2.034282e+03","Quantity: 5.220000e+02<br />Predicted Cost = $48,848<br />eye_preds: 4.884782e+04","Quantity: 1.440000e+02<br />Predicted Cost = $14,842<br />eye_preds: 1.484176e+04","Quantity: 4.155400e+04<br />Predicted Cost = $2,800,379<br />eye_preds: 2.800379e+06","Quantity: 3.264940e+03<br />Predicted Cost = $266,278<br />eye_preds: 2.662776e+05","Quantity: 3.002000e+03<br />Predicted Cost = $246,380<br />eye_preds: 2.463797e+05","Quantity: 5.827680e+03<br />Predicted Cost = $455,076<br />eye_preds: 4.550756e+05","Quantity: 8.094045e+04<br />Predicted Cost = $5,188,636<br />eye_preds: 5.188636e+06","Quantity: 2.408400e+03<br />Predicted Cost = $200,955<br />eye_preds: 2.009551e+05","Quantity: 5.140000e+02<br />Predicted Cost = $48,155<br />eye_preds: 4.815494e+04","Quantity: 1.695000e+03<br />Predicted Cost = $145,205<br />eye_preds: 1.452051e+05","Quantity: 2.587903e+05<br />Predicted Cost = $15,204,666<br />eye_preds: 1.520467e+07","Quantity: 2.004440e+05<br />Predicted Cost = $12,004,486<br />eye_preds: 1.200449e+07","Quantity: 1.817030e+03<br />Predicted Cost = $154,850<br />eye_preds: 1.548495e+05","Quantity: 5.186950e+04<br />Predicted Cost = $3,437,903<br />eye_preds: 3.437903e+06","Quantity: 3.304000e+02<br />Predicted Cost = $31,997<br />eye_preds: 3.199720e+04","Quantity: 1.227147e+05<br />Predicted Cost = $7,624,816<br />eye_preds: 7.624816e+06","Quantity: 2.451000e+04<br />Predicted Cost = $1,718,472<br />eye_preds: 1.718472e+06","Quantity: 6.134830e+04<br />Predicted Cost = $4,015,294<br />eye_preds: 4.015294e+06","Quantity: 7.190880e+04<br />Predicted Cost = $4,650,754<br />eye_preds: 4.650754e+06","Quantity: 2.930000e+02<br />Predicted Cost = $28,632<br />eye_preds: 2.863206e+04","Quantity: 3.485034e+04<br />Predicted Cost = $2,379,806<br />eye_preds: 2.379806e+06","Quantity: 8.911550e+03<br />Predicted Cost = $674,073<br />eye_preds: 6.740727e+05","Quantity: 4.183475e+04<br />Predicted Cost = $2,817,876<br />eye_preds: 2.817876e+06","Quantity: 1.708700e+04<br />Predicted Cost = $1,230,880<br />eye_preds: 1.230880e+06","Quantity: 2.963400e+04<br />Predicted Cost = $2,048,359<br />eye_preds: 2.048359e+06","Quantity: 7.387389e+05<br />Predicted Cost = $40,119,376<br />eye_preds: 4.011938e+07","Quantity: 1.478800e+04<br />Predicted Cost = $1,076,877<br />eye_preds: 1.076877e+06","Quantity: 2.927500e+04<br />Predicted Cost = $2,025,395<br />eye_preds: 2.025395e+06","Quantity: 1.685344e+06<br />Predicted Cost = $86,037,318<br />eye_preds: 8.603732e+07","Quantity: 1.950200e+04<br />Predicted Cost = $1,390,987<br />eye_preds: 1.390987e+06","Quantity: 1.125400e+04<br />Predicted Cost = $836,486<br />eye_preds: 8.364864e+05","Quantity: 1.539474e+05<br />Predicted Cost = $9,404,147<br />eye_preds: 9.404147e+06","Quantity: 1.914000e+03<br />Predicted Cost = $162,479<br />eye_preds: 1.624786e+05","Quantity: 5.243400e+04<br />Predicted Cost = $3,472,498<br />eye_preds: 3.472498e+06","Quantity: 9.757830e+03<br />Predicted Cost = $733,081<br />eye_preds: 7.330806e+05","Quantity: 2.646608e+04<br />Predicted Cost = $1,844,964<br />eye_preds: 1.844964e+06","Quantity: 3.958000e+02<br />Predicted Cost = $37,815<br />eye_preds: 3.781509e+04","Quantity: 5.848307e+05<br />Predicted Cost = $32,322,348<br />eye_preds: 3.232235e+07","Quantity: 3.997500e+02<br />Predicted Cost = $38,164<br />eye_preds: 3.816404e+04","Quantity: 3.920930e+04<br />Predicted Cost = $2,653,902<br />eye_preds: 2.653902e+06","Quantity: 1.991214e+05<br />Predicted Cost = $11,931,196<br />eye_preds: 1.193120e+07","Quantity: 9.608625e+04<br />Predicted Cost = $6,080,813<br />eye_preds: 6.080813e+06","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />eye_preds: 1.636104e+04","Quantity: 2.607010e+05<br />Predicted Cost = $15,308,477<br />eye_preds: 1.530848e+07","Quantity: 8.925000e+02<br />Predicted Cost = $80,226<br />eye_preds: 8.022553e+04","Quantity: 3.671430e+04<br />Predicted Cost = $2,497,311<br />eye_preds: 2.497311e+06","Quantity: 1.632000e+03<br />Predicted Cost = $140,206<br />eye_preds: 1.402059e+05","Quantity: 2.913900e+04<br />Predicted Cost = $2,016,690<br />eye_preds: 2.016690e+06","Quantity: 2.447749e+05<br />Predicted Cost = $14,441,405<br />eye_preds: 1.444140e+07","Quantity: 4.349900e+04<br />Predicted Cost = $2,921,415<br />eye_preds: 2.921415e+06","Quantity: 6.922440e+03<br />Predicted Cost = $533,629<br />eye_preds: 5.336294e+05","Quantity: 2.920270e+04<br />Predicted Cost = $2,020,767<br />eye_preds: 2.020767e+06","Quantity: 1.364898e+04<br />Predicted Cost = $999,926<br />eye_preds: 9.999256e+05","Quantity: 1.634000e+03<br />Predicted Cost = $140,365<br />eye_preds: 1.403648e+05","Quantity: 2.533134e+04<br />Predicted Cost = $1,771,674<br />eye_preds: 1.771674e+06","Quantity: 4.800000e+02<br />Predicted Cost = $45,201<br />eye_preds: 4.520101e+04","Quantity: 1.150000e+02<br />Predicted Cost = $12,054<br />eye_preds: 1.205440e+04","Quantity: 1.883069e+05<br />Predicted Cost = $11,330,558<br />eye_preds: 1.133056e+07","Quantity: 1.544265e+05<br />Predicted Cost = $9,431,217<br />eye_preds: 9.431217e+06","Quantity: 1.535021e+06<br />Predicted Cost = $78,914,290<br />eye_preds: 7.891429e+07","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />eye_preds: 4.538428e+03","Quantity: 2.710300e+05<br />Predicted Cost = $15,868,690<br />eye_preds: 1.586869e+07","Quantity: 2.056400e+04<br />Predicted Cost = $1,460,914<br />eye_preds: 1.460914e+06","Quantity: 7.278000e+02<br />Predicted Cost = $66,430<br />eye_preds: 6.642953e+04","Quantity: 1.000000e+02<br />Predicted Cost = $10,593<br />eye_preds: 1.059254e+04","Quantity: 1.295800e+04<br />Predicted Cost = $953,010<br />eye_preds: 9.530104e+05","Quantity: 6.180000e+03<br />Predicted Cost = $480,468<br />eye_preds: 4.804679e+05","Quantity: 1.468000e+01<br />Predicted Cost = $1,796<br />eye_preds: 1.795650e+03","Quantity: 5.687660e+05<br />Predicted Cost = $31,500,221<br />eye_preds: 3.150022e+07","Quantity: 1.940000e+03<br />Predicted Cost = $164,519<br />eye_preds: 1.645192e+05","Quantity: 8.306886e+04<br />Predicted Cost = $5,314,720<br />eye_preds: 5.314720e+06","Quantity: 1.230000e+03<br />Predicted Cost = $107,935<br />eye_preds: 1.079350e+05","Quantity: 2.274000e+03<br />Predicted Cost = $190,560<br />eye_preds: 1.905598e+05","Quantity: 1.170600e+04<br />Predicted Cost = $867,517<br />eye_preds: 8.675168e+05","Quantity: 1.500000e+02<br />Predicted Cost = $15,413<br />eye_preds: 1.541290e+04","Quantity: 5.555840e+04<br />Predicted Cost = $3,663,477<br />eye_preds: 3.663477e+06","Quantity: 1.509200e+04<br />Predicted Cost = $1,097,339<br />eye_preds: 1.097339e+06","Quantity: 5.135800e+02<br />Predicted Cost = $48,119<br />eye_preds: 4.811855e+04","Quantity: 2.916000e+03<br />Predicted Cost = $239,844<br />eye_preds: 2.398438e+05","Quantity: 1.593000e+04<br />Predicted Cost = $1,153,585<br />eye_preds: 1.153585e+06","Quantity: 1.218100e+02<br />Predicted Cost = $12,713<br />eye_preds: 1.271325e+04","Quantity: 2.209380e+05<br />Predicted Cost = $13,135,607<br />eye_preds: 1.313561e+07","Quantity: 1.680000e+02<br />Predicted Cost = $17,116<br />eye_preds: 1.711635e+04","Quantity: 4.259168e+05<br />Predicted Cost = $24,106,015<br />eye_preds: 2.410602e+07","Quantity: 3.464100e+03<br />Predicted Cost = $281,269<br />eye_preds: 2.812685e+05","Quantity: 6.985800e+03<br />Predicted Cost = $538,146<br />eye_preds: 5.381458e+05","Quantity: 1.100000e+01<br />Predicted Cost = $1,375<br />eye_preds: 1.374954e+03","Quantity: 8.500000e+01<br />Predicted Cost = $9,114<br />eye_preds: 9.114073e+03","Quantity: 5.110000e+03<br />Predicted Cost = $402,985<br />eye_preds: 4.029854e+05","Quantity: 1.277000e+03<br />Predicted Cost = $111,745<br />eye_preds: 1.117446e+05","Quantity: 2.593900e+04<br />Predicted Cost = $1,810,951<br />eye_preds: 1.810951e+06","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />eye_preds: 5.578883e+03","Quantity: 2.137500e+02<br />Predicted Cost = $21,388<br />eye_preds: 2.138765e+04","Quantity: 5.860211e+05<br />Predicted Cost = $32,383,201<br />eye_preds: 3.238320e+07","Quantity: 1.068079e+06<br />Predicted Cost = $56,423,275<br />eye_preds: 5.642327e+07","Quantity: 5.358010e+04<br />Predicted Cost = $3,542,650<br />eye_preds: 3.542650e+06","Quantity: 9.000000e+03<br />Predicted Cost = $680,259<br />eye_preds: 6.802590e+05","Quantity: 1.280000e+02<br />Predicted Cost = $13,310<br />eye_preds: 1.330973e+04","Quantity: 1.072570e+04<br />Predicted Cost = $800,099<br />eye_preds: 8.000990e+05","Quantity: 1.590000e+02<br />Predicted Cost = $16,266<br />eye_preds: 1.626643e+04","Quantity: 6.184630e+03<br />Predicted Cost = $480,801<br />eye_preds: 4.808008e+05","Quantity: 1.323848e+04<br />Predicted Cost = $972,076<br />eye_preds: 9.720761e+05","Quantity: 5.363350e+03<br />Predicted Cost = $421,433<br />eye_preds: 4.214329e+05","Quantity: 1.024580e+04<br />Predicted Cost = $766,929<br />eye_preds: 7.669286e+05","Quantity: 1.132150e+04<br />Predicted Cost = $841,126<br />eye_preds: 8.411262e+05","Quantity: 4.880000e+01<br />Predicted Cost = $5,455<br />eye_preds: 5.454919e+03","Quantity: 3.611600e+02<br />Predicted Cost = $34,743<br />eye_preds: 3.474339e+04","Quantity: 3.242650e+03<br />Predicted Cost = $264,596<br />eye_preds: 2.645956e+05","Quantity: 9.647222e+04<br />Predicted Cost = $6,103,404<br />eye_preds: 6.103404e+06","Quantity: 8.698750e+03<br />Predicted Cost = $659,170<br />eye_preds: 6.591702e+05","Quantity: 2.140567e+05<br />Predicted Cost = $12,756,722<br />eye_preds: 1.275672e+07","Quantity: 2.623704e+02<br />Predicted Cost = $25,852<br />eye_preds: 2.585212e+04","Quantity: 1.120000e+02<br />Predicted Cost = $11,763<br />eye_preds: 1.176323e+04","Quantity: 1.900000e+01<br />Predicted Cost = $2,280<br />eye_preds: 2.279539e+03","Quantity: 9.392259e+04<br />Predicted Cost = $5,954,048<br />eye_preds: 5.954048e+06","Quantity: 1.014424e+06<br />Predicted Cost = $53,796,382<br />eye_preds: 5.379638e+07","Quantity: 1.498740e+04<br />Predicted Cost = $1,090,302<br />eye_preds: 1.090302e+06","Quantity: 9.372000e+01<br />Predicted Cost = $9,976<br />eye_preds: 9.975734e+03","Quantity: 5.000000e+02<br />Predicted Cost = $46,940<br />eye_preds: 4.694045e+04","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />eye_preds: 5.578883e+03","Quantity: 1.503800e+02<br />Predicted Cost = $15,449<br />eye_preds: 1.544902e+04","Quantity: 1.350000e+02<br />Predicted Cost = $13,982<br />eye_preds: 1.398166e+04","Quantity: 5.400000e+02<br />Predicted Cost = $50,404<br />eye_preds: 5.040391e+04","Quantity: 1.200000e+02<br />Predicted Cost = $12,538<br />eye_preds: 1.253842e+04","Quantity: 1.000000e+03<br />Predicted Cost = $89,125<br />eye_preds: 8.912509e+04","Quantity: 1.571400e+04<br />Predicted Cost = $1,139,109<br />eye_preds: 1.139109e+06","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />eye_preds: 4.538428e+03","Quantity: 4.000000e+01<br />Predicted Cost = $4,538<br />eye_preds: 4.538428e+03","Quantity: 1.306541e+06<br />Predicted Cost = $67,985,102<br />eye_preds: 6.798510e+07","Quantity: 1.962795e+04<br />Predicted Cost = $1,399,295<br />eye_preds: 1.399295e+06","Quantity: 1.300000e+02<br />Predicted Cost = $13,502<br />eye_preds: 1.350198e+04","Quantity: 4.000000e+03<br />Predicted Cost = $321,296<br />eye_preds: 3.212961e+05","Quantity: 1.643896e+04<br />Predicted Cost = $1,187,637<br />eye_preds: 1.187637e+06","Quantity: 3.525200e+02<br />Predicted Cost = $33,974<br />eye_preds: 3.397387e+04","Quantity: 3.200000e+01<br />Predicted Cost = $3,692<br />eye_preds: 3.692017e+03","Quantity: 4.222000e+03<br />Predicted Cost = $337,757<br />eye_preds: 3.377570e+05","Quantity: 6.225000e+02<br />Predicted Cost = $57,488<br />eye_preds: 5.748823e+04","Quantity: 1.600000e+02<br />Predicted Cost = $16,361<br />eye_preds: 1.636104e+04","Quantity: 3.328810e+03<br />Predicted Cost = $271,092<br />eye_preds: 2.710924e+05","Quantity: 3.773000e+03<br />Predicted Cost = $304,393<br />eye_preds: 3.043934e+05","Quantity: 6.525000e+02<br />Predicted Cost = $60,046<br />eye_preds: 6.004640e+04","Quantity: 2.944140e+03<br />Predicted Cost = $241,984<br />eye_preds: 2.419840e+05","Quantity: 7.542000e+02<br />Predicted Cost = $68,655<br />eye_preds: 6.865545e+04","Quantity: 5.000000e+01<br />Predicted Cost = $5,579<br />eye_preds: 5.578883e+03","Quantity: 1.823000e+03<br />Predicted Cost = $155,320<br />eye_preds: 1.553201e+05","Quantity: 4.200000e+02<br />Predicted Cost = $39,949<br />eye_preds: 3.994897e+04","Quantity: 1.122222e+04<br />Predicted Cost = $834,301<br />eye_preds: 8.343014e+05","Quantity: 2.925000e+05<br />Predicted Cost = $17,028,109<br />eye_preds: 1.702811e+07","Quantity: 6.744000e+03<br />Predicted Cost = $520,893<br />eye_preds: 5.208933e+05","Quantity: 3.600000e+02<br />Predicted Cost = $34,640<br />eye_preds: 3.464015e+04","Quantity: 2.431746e+04<br />Predicted Cost = $1,705,982<br />eye_preds: 1.705982e+06","Quantity: 1.801400e+05<br />Predicted Cost = $10,875,254<br />eye_preds: 1.087525e+07","Quantity: 4.870600e+02<br />Predicted Cost = $45,816<br />eye_preds: 4.581564e+04","Quantity: 1.600000e+04<br />Predicted Cost = $1,158,273<br />eye_preds: 1.158273e+06","Quantity: 8.295000e+02<br />Predicted Cost = $74,973<br />eye_preds: 7.497305e+04","Quantity: 8.116600e+02<br />Predicted Cost = $73,480<br />eye_preds: 7.348033e+04","Quantity: 4.000000e+02<br />Predicted Cost = $38,186<br />eye_preds: 3.818612e+04","Quantity: 6.711000e+02<br />Predicted Cost = $61,628<br />eye_preds: 6.162802e+04","Quantity: 1.009000e+02<br />Predicted Cost = $10,681<br />eye_preds: 1.068069e+04","Quantity: 3.930802e+04<br />Predicted Cost = $2,660,082<br />eye_preds: 2.660082e+06","Quantity: 2.068581e+04<br />Predicted Cost = $1,468,916<br />eye_preds: 1.468916e+06","Quantity: 5.532000e+02<br />Predicted Cost = $51,543<br />eye_preds: 5.154256e+04","Quantity: 2.809000e+03<br />Predicted Cost = $231,692<br />eye_preds: 2.316916e+05","Quantity: 4.256000e+03<br />Predicted Cost = $340,272<br />eye_preds: 3.402722e+05","Quantity: 4.200000e+01<br />Predicted Cost = $4,748<br />eye_preds: 4.747944e+03","Quantity: 9.147565e+04<br />Predicted Cost = $5,810,421<br />eye_preds: 5.810421e+06","Quantity: 1.287000e+03<br />Predicted Cost = $112,554<br />eye_preds: 1.125538e+05","Quantity: 2.392800e+02<br />Predicted Cost = $23,740<br />eye_preds: 2.374042e+04","Quantity: 1.260400e+03<br />Predicted Cost = $110,400<br />eye_preds: 1.104003e+05","Quantity: 3.996000e+03<br />Predicted Cost = $320,999<br />eye_preds: 3.209989e+05","Quantity: 4.354500e+02<br />Predicted Cost = $41,306<br />eye_preds: 4.130645e+04","Quantity: 1.200000e+03<br />Predicted Cost = $105,498<br />eye_preds: 1.054976e+05","Quantity: 3.360577e+05<br />Predicted Cost = $19,361,224<br />eye_preds: 1.936122e+07","Quantity: 1.059500e+04<br />Predicted Cost = $791,076<br />eye_preds: 7.910763e+05","Quantity: 1.500000e+01<br />Predicted Cost = $1,832<br />eye_preds: 1.831827e+03","Quantity: 4.170200e+04<br />Predicted Cost = $2,809,604<br />eye_preds: 2.809604e+06","Quantity: 8.128043e+04<br />Predicted Cost = $5,208,792<br />eye_preds: 5.208792e+06","Quantity: 4.746850e+04<br />Predicted Cost = $3,167,197<br />eye_preds: 3.167197e+06","Quantity: 1.611960e+03<br />Predicted Cost = $138,613<br />eye_preds: 1.386126e+05","Quantity: 5.098200e+02<br />Predicted Cost = $47,793<br />eye_preds: 4.779259e+04","Quantity: 2.117000e+02<br />Predicted Cost = $21,198<br />eye_preds: 2.119785e+04","Quantity: 8.300000e+01<br />Predicted Cost = $8,916<br />eye_preds: 8.915531e+03","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />eye_preds: 6.603739e+03","Quantity: 5.685100e+03<br />Predicted Cost = $444,767<br />eye_preds: 4.447672e+05","Quantity: 1.152000e+03<br />Predicted Cost = $101,588<br />eye_preds: 1.015883e+05","Quantity: 8.646000e+03<br />Predicted Cost = $655,472<br />eye_preds: 6.554719e+05","Quantity: 2.193614e+01<br />Predicted Cost = $2,604<br />eye_preds: 2.603593e+03","Quantity: 6.262000e+02<br />Predicted Cost = $57,804<br />eye_preds: 5.780423e+04","Quantity: 6.060000e+03<br />Predicted Cost = $471,832<br />eye_preds: 4.718318e+05","Quantity: 8.000000e+01<br />Predicted Cost = $8,617<br />eye_preds: 8.617043e+03","Quantity: 2.724600e+02<br />Predicted Cost = $26,770<br />eye_preds: 2.677041e+04","Quantity: 6.000000e+01<br />Predicted Cost = $6,604<br />eye_preds: 6.603739e+03","Quantity: 1.124000e+02<br />Predicted Cost = $11,802<br />eye_preds: 1.180209e+04","Quantity: 1.400000e+01<br />Predicted Cost = $1,719<br />eye_preds: 1.718575e+03","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />eye_preds: 2.390302e+03","Quantity: 3.118400e+03<br />Predicted Cost = $255,204<br />eye_preds: 2.552037e+05","Quantity: 1.481796e+04<br />Predicted Cost = $1,078,895<br />eye_preds: 1.078895e+06","Quantity: 1.972424e+05<br />Predicted Cost = $11,827,018<br />eye_preds: 1.182702e+07","Quantity: 7.224000e+01<br />Predicted Cost = $7,841<br />eye_preds: 7.840963e+03","Quantity: 4.687428e+05<br />Predicted Cost = $26,339,921<br />eye_preds: 2.633992e+07","Quantity: 2.000000e+01<br />Predicted Cost = $2,390<br />eye_preds: 2.390302e+03","Quantity: 6.346000e+03<br />Predicted Cost = $492,394<br />eye_preds: 4.923938e+05","Quantity: 2.172210e+04<br />Predicted Cost = $1,536,860<br />eye_preds: 1.536860e+06","Quantity: 1.250000e+04<br />Predicted Cost = $921,811<br />eye_preds: 9.218107e+05","Quantity: 9.500000e+02<br />Predicted Cost = $84,995<br />eye_preds: 8.499519e+04","Quantity: 6.318400e+02<br />Predicted Cost = $58,286<br />eye_preds: 5.828564e+04","Quantity: 6.664860e+04<br />Predicted Cost = $4,335,176<br />eye_preds: 4.335176e+06","Quantity: 1.340000e+03<br />Predicted Cost = $116,835<br />eye_preds: 1.168347e+05","Quantity: 8.999176e+04<br />Predicted Cost = $5,723,182<br />eye_preds: 5.723182e+06","Quantity: 1.853500e+02<br />Predicted Cost = $18,745<br />eye_preds: 1.874533e+04","Quantity: 6.684800e+02<br />Predicted Cost = $61,405<br />eye_preds: 6.140543e+04"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(255,0,0,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(255,0,0,1)"}},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.762557077625573,"r":7.3059360730593621,"b":40.182648401826498,"l":95.707762557077658},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"title":{"text":"Actuals against Formula Predicted Values","font":{"color":"rgba(0,0,0,1)","family":"","size":17.534246575342465},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.75413804739774692,7.0737400781282673],"tickmode":"array","ticktext":["100","10,000","1,000,000"],"tickvals":[2,4,6],"categoryorder":"array","categoryarray":["100","10,000","1,000,000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"y","title":{"text":"Quantity","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[2.8725776938429157,8.7182095722686466],"tickmode":"array","ticktext":["$10,000","$1,000,000","$100,000,000"],"tickvals":[4,6,8],"categoryorder":"array","categoryarray":["$10,000","$1,000,000","$100,000,000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"x","title":{"text":"Gross_Cost","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"ee4e3b07fc03":{"x":{},"text":{},"y":{},"type":"scatter"},"ee4e932473c":{"x":{},"text":{},"y":{}},"ee4e43e2b8db":{"x":{},"text":{},"y":{}}},"cur_data":"ee4e3b07fc03","visdat":{"ee4e3b07fc03":["function (y) ","x"],"ee4e932473c":["function (y) ","x"],"ee4e43e2b8db":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

#### Notes:

- The blue line is the ‘Regression Predictions’ (reg_preds)‘, red line
  is the ’Eye Prediction’ (eye_pred)
- The chart is interactive.
- They are slightly differently balanced, in my opinion, the eye
  prediction formula should be preferred under 500-8000 Cubic Yards of
  quantity, then the regression predictor satisfies Region 4’s approach
  better at higher numbers of Cubic Yardage to be removed.
- The formula promoted in the **BLUF** section uses the **regression
  formula**.

------------------------------------------------------------------------

## Appendix

------------------------------------------------------------------------

------------------------------------------------------------------------

### Assessing Historials as Predictors in Complex Model

``` r
# load(file="historicals.rda")
# #glimpse(hist)
# names(hist) <- gsub(names(hist), pattern = " ", replacement = "_")
# hist[,3:51] <- lapply(hist[,3:51],FUN = as.numeric)
# 
# veg_pred.m.hist <- left_join(veg_pred.m, hist, by="FIPS")
# veg_pred.m.hist$year <- factor(veg_pred.m.hist$year, ordered=T)
# veg_pred.m.hist$USPS <- factor(veg_pred.m.hist$USPS, ordered=F)
# 
# cor.test(veg_pred.m.hist$Gross_Cost, veg_pred.m.hist$`Historical_Vegetative_75th_Quantile_Gross_Cost.75%`)
# #0.24, fairly weak...
# 
# #library(lme4)
# veg_multi_mod <- 
#   veg_pred.m.hist %>% filter(
#     Quantity > 10,
#     Gross_Cost > 500,
#     Gross_Cost < 1e07) %>% 
#   lme4::lmer(formula = log10(Gross_Cost) ~ log10(Quantity) +
#              `Historical_Vegetative_75th_Quantile_Gross_Cost.75%` 
#              + year
#              + (1|USPS))
# 
# summary(veg_multi_mod)
# 
# 
# #veg_lm_mod_state <- 
# #  veg_pred.m.hist %>% filter(
# #    Quantity > 10,
# #    Gross_Cost > 500,
# #    Gross_Cost < 1e07) %>% 
# #  lm(formula = log10(Gross_Cost) ~ log10(Quantity) + USPS)
# 
# #summary(veg_lm_mod_state)  # state not a useful variable. 
```

Successful modeling, but not really adding much of significance. Would
need to evalauate a larger field of predictors, particularly those that
don’t come from GM (cross contamination data, since the 75th for costs
are some composite from the predictand anyway…)

``` r
# piecewiseSEM::rsquared(veg_multi_mod) 
# piecewiseSEM::rsquared(veg_model)
```

\#LOWER RSQUARED than the simple model, no reason to do a complex
process for what we may *think* (but can’t really prove) are different,
nested structures influencing this process. States/Counties are more
alike than different… is the simple conclusion.

The idea here would be to have **separate intercepts** for each state
(or if we could get down to it… each **county**) as R4 would like
multipliers or factors for **states and counties** where the number is
higher… lower etc…

At least in this model, the differences aren’t too meaningful.

------------------------------------------------------------------------

------------------------------------------------------------------------

### Additional Data Views and Visualization.

#### Project Based Aggregation and Exploration

``` r
# load("input_tables/Vegetative_Debris_CY_Debris_Data_By_Applicant_Project_Id.rda", verbose=T)
# glimpse(CY_Debris_Data_By_Applicant_Project_Id)
# 
# veg_stat <- CY_Debris_Data_By_Applicant_Project_Id %>% 
#   group_by(State, County, Applicant_Project_Id) %>% summarise(
#     .groups = "keep",
#     Gross_Cost = max(Gross_Cost, na.rm=T),
#     Quantity = sum(Quantity, na.rm=T)
#   ) %>% 
#   ungroup()
# glimpse(veg_stat)
```

CTA: Previous work made this look like an exponential or power law
relationship, attempting to fix exponential/poly models and see how
close they fit the line, could use that as a **product** for the R4
group. (There is still a large error term..)

Look at fitting various exponential relationships
