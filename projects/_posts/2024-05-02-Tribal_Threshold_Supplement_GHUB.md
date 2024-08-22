---
title: "100k Tribal Threshold Contextualization"
author: "Colin T. Annand"
date: "2024-05-02"
always_allow_html: true
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../projects/_posts") })
description: "Follow-up on an extended project @FEMA."
layout: post
categories:
  - R Markdown
  - Jekyll    
---

![](/../images/fema_logo_circle.png)<!-- -->

# \_\_\_\_\_

# BLUF

## Changes to the *threshold policy* would likely mean more Tribes applying directly, and data **indicates low chance of substantial increases in overall declared disasters**.

- There are 37 Declarations involving tribes (non-COVID, non-EM) under
  the National Delivery Model (2017-2023).
- 23 are *independent* Tribal Declarations, where a tribal nation is
  applicant and recipient.
- 14 are State Declarations, where a tribal nation is recipient, but not
  applicant.
- These are in Regions 4, 6, 7, 8, 9, 10.
- Regions *typically* see 0 to 1 Tribal declarations in a year, with R6,
  R8 and R9 having higher trends (0 to 2 or 4 declarations at peak).
- There are few Tribal PDAs between the 100k-250k range, meaning a few
  more tribes may apply independently.
- Limited data, but no evidence of turndowns for tribes related to a
  change in the 100-250k range.

## Historic Tables and Estimating Future Years

The following tables show historic, estimated, and estimated +
substantive increase in trends of *direct* Tribal Declarations. Data
details and methods are summarized in side notes, with full details in
the section below. [^1]

### 1. Independent Tribal Declarations by Region and Year

Placeholder (you should not see this)

| Year  |  R4 |  R6 |  R7 |  R8 |  R9 | R10 | Total |
|:------|----:|----:|----:|----:|----:|----:|------:|
| 2017  |   1 |   1 |   0 |   0 |   2 |   0 |     4 |
| 2018  |   0 |   0 |   0 |   0 |   2 |   1 |     3 |
| 2019  |   0 |   1 |   2 |   1 |   4 |   0 |     8 |
| 2020  |   0 |   0 |   1 |   0 |   0 |   0 |     1 |
| 2021  |   0 |   0 |   0 |   0 |   0 |   1 |     1 |
| 2022  |   1 |   0 |   0 |   0 |   2 |   0 |     3 |
| 2023  |   0 |   0 |   0 |   2 |   1 |   0 |     3 |
| Total |   2 |   2 |   3 |   3 |  11 |   2 |    23 |

## Prediction:

If tribal application and declaration activity over the last several
years remains moderately similar over the next five years, and
declarations are made in consideration of a \$100k threshold, we may
expect:

- **~30** tribal declarations within the next 5 years.
- Regions responding to **0-1** additional disasters per year, beyond
  their historic trends.

Placeholder (you should not see this)

### 2. Projected 5 Years - Sample

| Year  |  R4 |  R6 |  R7 |  R8 |  R9 | R10 | Total |
|:------|----:|----:|----:|----:|----:|----:|------:|
| 2025  |   0 |   0 |   0 |   1 |   3 |  NA |     4 |
| 2026  |   0 |   0 |   2 |   0 |  NA |   0 |     2 |
| 2027  |   1 |   2 |  NA |   0 |   4 |   1 |     8 |
| 2028  |  NA |   0 |   1 |   0 |   1 |   1 |     3 |
| 2029  |   0 |   1 |   3 |   1 |   8 |  NA |    13 |
| Total |   1 |   3 |   6 |   2 |  16 |   2 |    30 |

If FEMA Response activity significantly exceeds our expectation based on
current data, such as a 30-40% increase in declarations, we may expect:

- **~50** declarations over the next 5 years.
- Regions responding to **1-2 additional** disasters per year, beyond
  their historic trend.

Placeholder (you should not see this)

### 3. Projected 5 Years - Sample + Planning Factor

| Year  |  R4 |  R6 |  R7 |  R8 |  R9 | R10 | Total |
|:------|----:|----:|----:|----:|----:|----:|------:|
| 2025  |   1 |   1 |   1 |   1 |   4 |  NA |     8 |
| 2026  |   1 |   1 |   3 |   1 |  NA |   1 |     7 |
| 2027  |   1 |   3 |  NA |   1 |   5 |   1 |    11 |
| 2028  |  NA |   1 |   1 |   1 |   1 |   1 |     5 |
| 2029  |   1 |   1 |   4 |   1 |  11 |  NA |    18 |
| Total |   4 |   7 |   9 |   5 |  21 |   3 |    49 |

Both of the scenarios described should be considered along with Regional
and Adminstrative planning staff expertise. Our data comprises 7 years
of events, but the number of Tribal Events is still quite small. The
following sections add context to this idea above, considering when
Tribes may have been able to apply independently, and when data on event
impact (PDA) implies possible events for declaration under our
prescribed ‘future’ scenarios.

## \_\_\_\_\_\_\_\_\_

## Context

Existing Tribal Pilot Guidance requires minimum damage threshold of
\$250K for tribal declarations. [^2] This figure also represents an
assumed minimum administrative cost for any declaration. Changing to the
\$100k threshold is an equitable decision that allows FEMA to service
Tribes proportionally to the response historically given to states. [^3]

Most Tribes are quite small compared to county and state equivalents.
The current policy creates a *filtering effect* on our data. Impacts to
small tribes and geographies (e.g. a hypothetical \$60,000 of flooding
damage) may affect 70-90% of a community, but not come close to either
the \$100k or \$250k thresholds. [^4] Changing the declaration impact
threshold doesn’t change the **frequency of impacts** to tribes,
however, the “unknown data” (tribes that may have been impacted, but not
assessed, and never working with FEMA) is not something our analysis can
directly account for. However, we can say that such impacts, if they
represent an increase in events, would be quite small in terms of cost,
and are likely occur slightly more often than the typical declared
disasters we see for tribes. In a scientific best guess, this might be
**1-2 small tribal declarations per Region, per year**, instead of the
typical 0-1 declarations that Regions currently see. The following
analysis attempts to justify this perspective with available FEMA Data.

## \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## **Analytic Focus**

- Examine distribution of tribal nations across Regions/States and
  distribution of decs (tribal or otherwise) across Regions/States
- Examine tribal turndowns between \$100k and \$250k historically
- Examine events that impacted tribes at a level below \$100k that they
  may not have even requested historically.
- Examine events where a tribe came in as a part of a State dec. but may
  in the future choose to pursue their own instead.

### Data

- **COVID Events** are excluded from counts, tables and analyses.
- PDA Data 2012-2022
- Applicant Level Data: 2017-2023 (Grants Manager)

Placeholder (you should not see this)

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:350px; overflow-x: scroll; width:800px; ">

<table class="table table-striped" style="font-size: 14px; color: black; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Disaster_Number
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
State_Code
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Declaration_Date
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Title
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Region
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Tribal_Declaration
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Recipient_is_Tribal
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Name
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_is_Tribal
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
County
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
County_FiveDigit_FIPS
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Subrecipient_Status
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Type
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Applicant_Eligibility
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
RPA_Entry_Date
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
RPA_Approved_Date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Effective_Cost_Share
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
RPA_Notification_Date
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Number_Of_PWs
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Project_Amount
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Number_Of_PWs_Obligated
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Federal_Share_Obligated
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Number_Of_PWs_Closed
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
4302
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
2017-02-14
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
06000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2017-03-14 21:20:22
</td>
<td style="text-align:left;">
2017-04-17 16:22:48
</td>
<td style="text-align:right;">
91.25000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
3707167.57
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
3345510.33
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4312
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
2017-05-02
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
06000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2017-06-21 18:37:13
</td>
<td style="text-align:left;">
2017-06-22 18:15:08
</td>
<td style="text-align:right;">
90.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
299927.52
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
269934.77
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
2017-09-27
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
12000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2017-10-24 09:47:57
</td>
<td style="text-align:left;">
2017-10-26 12:16:07
</td>
<td style="text-align:right;">
91.61789
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
47
</td>
<td style="text-align:right;">
12029.13
</td>
<td style="text-align:right;">
43
</td>
<td style="text-align:right;">
10613.94
</td>
<td style="text-align:right;">
47
</td>
</tr>
<tr>
<td style="text-align:right;">
4352
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
2017-12-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
35000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2018-01-17 20:44:56
</td>
<td style="text-align:left;">
2018-01-17 21:18:43
</td>
<td style="text-align:right;">
90.24242
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
106
</td>
<td style="text-align:right;">
16019463.41
</td>
<td style="text-align:right;">
106
</td>
<td style="text-align:right;">
14430450.53
</td>
<td style="text-align:right;">
106
</td>
</tr>
<tr>
<td style="text-align:right;">
4384
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
2018-08-17
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
53000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2018-10-25 21:08:30
</td>
<td style="text-align:left;">
2018-10-25 21:29:18
</td>
<td style="text-align:right;">
77.50000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
613372.22
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
464404.17
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:right;">
4389
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
2018-08-31
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
04000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2018-10-17 17:21:32
</td>
<td style="text-align:left;">
2018-11-09 22:16:19
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
687414.97
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
515561.24
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4409
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
2018-11-30
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
04000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-01-22 23:51:46
</td>
<td style="text-align:left;">
2019-01-29 22:00:21
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
1809119.93
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
1356839.93
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:right;">
4409
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
2018-11-30
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
04000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Special District Government
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-02-27 14:04:32
</td>
<td style="text-align:left;">
2019-05-02 16:51:22
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
3871112.13
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
2903334.11
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4422
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
2019-03-26
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
06000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-04-15 23:32:22
</td>
<td style="text-align:left;">
2019-04-18 23:05:43
</td>
<td style="text-align:right;">
90.90909
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
596378.24
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
540891.07
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4423
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
2019-03-28
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
06000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-05-01 16:43:54
</td>
<td style="text-align:left;">
2019-10-30 14:44:01
</td>
<td style="text-align:right;">
87.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2319678.85
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
2087710.97
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4425
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
2019-04-08
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
06000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-05-01 17:07:38
</td>
<td style="text-align:left;">
2019-08-29 22:58:34
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
663568.93
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
497676.70
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4430
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
2019-04-29
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tama
</td>
<td style="text-align:left;">
19171
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-08-15 19:14:02
</td>
<td style="text-align:left;">
2019-12-12 21:53:12
</td>
<td style="text-align:right;">
100.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
52562.17
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
52562.17
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4430
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
2019-04-29
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
19000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-05-14 19:56:17
</td>
<td style="text-align:left;">
2019-06-18 18:27:34
</td>
<td style="text-align:right;">
77.27273
</td>
<td style="text-align:left;">
2019-08-15 19:51:42
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
740911.69
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
555683.78
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4436
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
2019-05-21
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
04000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-06-18 16:44:22
</td>
<td style="text-align:left;">
2019-07-19 20:37:53
</td>
<td style="text-align:right;">
76.92308
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
738255.83
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
555139.68
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4446
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
2019-06-17
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
31000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-12-16 20:10:19
</td>
<td style="text-align:left;">
2019-12-16 20:47:01
</td>
<td style="text-align:right;">
90.33333
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1215036.25
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1088251.32
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-07-16 16:35:09
</td>
<td style="text-align:left;">
2019-07-18 17:56:26
</td>
<td style="text-align:right;">
91.66667
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
66394.00
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
54305.65
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-07-22 15:34:48
</td>
<td style="text-align:left;">
2019-10-04 15:50:54
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
125503.19
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
94127.39
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-07-22 17:50:51
</td>
<td style="text-align:left;">
2019-12-06 18:09:53
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
240893.51
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
180670.14
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-07-22 18:14:49
</td>
<td style="text-align:left;">
2019-10-10 20:55:07
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
275172.56
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
206379.43
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-07-22 18:28:49
</td>
<td style="text-align:left;">
2019-12-06 18:10:42
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
738136.21
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
553602.16
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2019-06-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2019-07-22 18:36:43
</td>
<td style="text-align:left;">
2019-12-04 00:38:34
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
120448.33
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
90336.25
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4456
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
2019-08-07
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
40000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2020-06-09 13:15:49
</td>
<td style="text-align:left;">
2020-06-09 13:43:07
</td>
<td style="text-align:right;">
78.57143
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
8708489.25
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
6543056.06
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:right;">
4561
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
2020-09-10
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
19000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2020-12-22 23:04:21
</td>
<td style="text-align:left;">
2020-12-22 23:09:37
</td>
<td style="text-align:right;">
90.66667
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
1345621.54
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
1219862.52
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4631
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
2021-12-21
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
53000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2022-05-18 18:02:45
</td>
<td style="text-align:left;">
2022-06-14 21:44:31
</td>
<td style="text-align:right;">
91.25000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2689900.09
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
2441910.08
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4668
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
2022-09-02
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
04000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2022-09-14 21:02:53
</td>
<td style="text-align:left;">
2022-09-14 21:09:24
</td>
<td style="text-align:right;">
78.57143
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1071802.89
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
815764.95
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4675
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
2022-09-30
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
12000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2022-10-13 18:27:24
</td>
<td style="text-align:left;">
2023-02-14 11:28:50
</td>
<td style="text-align:right;">
96.42857
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1362959.16
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
1360994.43
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4681
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
2022-12-30
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
04000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2023-02-17 00:12:41
</td>
<td style="text-align:left;">
2023-02-17 00:23:14
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
226715.29
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4687
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2023-02-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2023-05-10 15:50:01
</td>
<td style="text-align:left;">
2023-05-10 17:51:39
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1064230.64
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
798172.99
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4688
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
2023-02-20
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
46000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2023-06-07 20:44:55
</td>
<td style="text-align:left;">
2023-06-07 21:12:03
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
23656.15
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
17742.11
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
4692
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
2023-03-08
</td>
<td style="text-align:left;">
Severe Storm
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Albany
</td>
<td style="text-align:left;">
Tribal
</td>
<td style="text-align:left;">
Statewide
</td>
<td style="text-align:left;">
06000
</td>
<td style="text-align:left;">
Tribal Recipient
</td>
<td style="text-align:left;">
Indian/Native American Tribal Government (Federally Recognized)
</td>
<td style="text-align:left;">
ELIGIBLE
</td>
<td style="text-align:left;">
2023-06-09 17:11:36
</td>
<td style="text-align:left;">
2023-06-09 17:21:28
</td>
<td style="text-align:right;">
75.00000
</td>
<td style="text-align:left;">
NA
</td>
<td style="text-align:right;">
11
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0.00
</td>
<td style="text-align:right;">
0
</td>
</tr>
</tbody>
</table>

</div>

# \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

# Distribution of Tribal Declarations

The following table shows the number of *direct* Tribal Declarations,
per Region, per Year. Establishing baseline expectations of for typical
Region response to tribes.

### Independent Tribal Declarations by Region and Year

Placeholder (you should not see this)

| Year  |  R4 |  R6 |  R7 |  R8 |  R9 | R10 | Total |
|:------|----:|----:|----:|----:|----:|----:|------:|
| 2017  |   1 |   1 |   0 |   0 |   2 |   0 |     4 |
| 2018  |   0 |   0 |   0 |   0 |   2 |   1 |     3 |
| 2019  |   0 |   1 |   2 |   1 |   4 |   0 |     8 |
| 2020  |   0 |   0 |   1 |   0 |   0 |   0 |     1 |
| 2021  |   0 |   0 |   0 |   0 |   0 |   1 |     1 |
| 2022  |   1 |   0 |   0 |   0 |   2 |   0 |     3 |
| 2023  |   0 |   0 |   0 |   2 |   1 |   0 |     3 |
| Total |   2 |   2 |   3 |   3 |  11 |   2 |    23 |

- Not all regions have Tribal Declarations in a year.
- Select Regions may respond to 1-2 Tribal Declarations in a year.
- In a *4 year period*, the per year probability of a region seeing a
  declaration would be around 25%.
- If halving the threshold (250k -\> 100k) doubles this probability to
  50%, it’s a coinflip chance that (select) Regions will respond to a
  one Tribal Declaration in a year.
- The most frequent value is ‘0’, for example Region 4 had **no
  independent Tribal independent Decs for 4 years, 2018-2021**, but
  there were State-Decs for hurricanes in this region that included
  tribes.

## Gauging the Future

Taking this Region by Year breakdown, we create a sample set of
potential disaster years (labelled 2025:2029), using the following
method and assumptions. - Sample 25% of all Tribal Declarations -
Aggregate to Region - If a Region doesn’t appear in the sample it
*wasn’t impacted by a disaster*.

``` r
# #sampling routine
years = 2025:2029  #year set
years_projected = data.frame(Year = 0, "R4"=0,"R6"=0,"R7"=0,"R8"=0,"R9"=0,"R10"=0)

no_tot <- historic_drs2[1:7,1:7]   #Decs by Region/Year - stripped of 'totals'
no_tot[no_tot==0]<- NA

set.seed(6147) #set to reproduce same tables...
for(year in years){
  yt <- no_tot %>%
    pivot_longer(cols=2:7, names_to = "Region", values_to = "DR_Count") %>%
    slice_sample(prop=.25, replace = T) %>% #sample 25% of the distribution...
    group_by(Region) %>%
    summarise(DR_Count = sum(DR_Count, na.rm = T)) %>%
    mutate(Year = year) %>%
    pivot_wider(id_cols = Year, names_from = Region, values_from = DR_Count, values_fill = 0)

  years_projected <- bind_rows(years_projected, yt) #concatenate table
}

years_projected <- years_projected[2:nrow(years_projected),] %>% mutate(Year = as.character(Year))
```

This gives us the following *probabilistic sample* table:

Placeholder (you should not see this)

### Projected 5 Years - Sample

| Year  |  R4 |  R6 |  R7 |  R8 |  R9 | R10 | Total |
|:------|----:|----:|----:|----:|----:|----:|------:|
| 2025  |   0 |   0 |   0 |   1 |   3 |  NA |     4 |
| 2026  |   0 |   0 |   2 |   0 |  NA |   0 |     2 |
| 2027  |   1 |   2 |  NA |   0 |   4 |   1 |     8 |
| 2028  |  NA |   0 |   1 |   0 |   1 |   1 |     3 |
| 2029  |   0 |   1 |   3 |   1 |   8 |  NA |    13 |
| Total |   1 |   3 |   6 |   2 |  16 |   2 |    30 |

The sampling method which generates the ‘projected future’ years uses
*random sampling* which is a good way to think about potential disaster
impacts. We can’t say for sure a Cat 3 Hurricane will impact Florida in
2024, but we are very sure that in a 4-5 year span, one of those years
would contain a hurricane. Each year itself could be considered a
different scenario to plan for. One year may contain no significant
weather events, or no tribal declarations even with weather events
occurring.

The sampling method can yield different results. In this specific case,
the total predicted is 43 events across 5 years. The particular high
year is 2029, where R8 experiences 4 and R9 sees 8 declarations.

Note: When **NA** occurs in a year, the sampling routine didn’t select a
given Region (for Tribal related impacts) meaning “no weather impacts
occurred”. Therefore the difference between 0 and NA in a sampleyear
is: - 0 indicates “no declarations” would be made in the projected
year. - NA indicates “no weather impacts/events” would occur in the
projected year, (therefore no declarations).

\###\_\_\_\_\_\_

We then create a **Planning factor scenario** based off this sample
table, making assumptions that lowering the threshold will increase
declarations. The method is: - Replace any 0 values with 1 (meaning when
a Region *did* experience an impact, the lower threshold made it
possible to delcare a disaster) - Multiply remaining values by 1.35 (a
30-40% increase in disaster declarations)

Placeholder (you should not see this)

### Projected 5 Years - Sample + Planning Factor

| Year  |  R4 |  R6 |  R7 |  R8 |  R9 | R10 | Total |
|:------|----:|----:|----:|----:|----:|----:|------:|
| 2025  |   1 |   1 |   1 |   1 |   4 |  NA |     8 |
| 2026  |   1 |   1 |   3 |   1 |  NA |   1 |     7 |
| 2027  |   1 |   3 |  NA |   1 |   5 |   1 |    11 |
| 2028  |  NA |   1 |   1 |   1 |   1 |   1 |     5 |
| 2029  |   1 |   1 |   4 |   1 |  11 |  NA |    18 |
| Total |   4 |   7 |   9 |   5 |  21 |   3 |    49 |

Sampling with replacement was chosen as the number of total Tribal
Events is quite small. Because of higher share of samples, Regions like
6 or 9 come out with high numbers of declarations, however, their rate
of *new declarations* (changes from 0 to 1) are much lower. This can be
interpreted as Regions with a history of response and coordination with
Tribes may see small to moderate increases, but not *novel* work. Region
4, which historically had many years with no declarations, might
actually experience a **larger change** (qualitative, and quantitative
change) in necessitated response if the lowered threshold changes the
way their applicants work with FEMA.

This sampling method can’t account for large changes in impacts.
Regional and HQ expertise is more relevant here, but I noted R4 above
because of the prevalance of Hurricanes and the number of Tribes within
the hurricane belt (e.g. Moccosukee, Choctaw, Muscogee,m Seminole in
Florida, Mississippi). While anecdotal, if a number of these tribes
switch to independent declaration as opposed to state declaration, the
number of DRs could go up quite a bit (Thus the assumption of changing
0-1 in the *Planning Factor* scenario).

The following sections provide expanded analysis that could be applied
to the envisioned scenarios.

## \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Which Tribes may have applied independently?

- The table below shows the Tribes with *Disaster Total Amounts* above
  \$100k (and below \$1M).
- **Declaration Type** indicates whether the instance was a State-DR or
  Tribal (independent) DR.
- Assessing *specific impacts to Tribes* in a State-DR, there were only
  4 instances in the last 7 years where a tribe may have applied
  independently under the 100k, but not the 250k threshold.

Placeholder (you should not see this)

### State and Tribal Declarations near the Threshold

| Year |   DR | Declaration_Type | Total_Amount | App.or.Recip | Name                                        |
|-----:|-----:|:-----------------|:-------------|:-------------|:--------------------------------------------|
| 2017 | 4312 | Tribal           | \$299,928    | Tribal       | Resighini Rancheria (indian Reservation)    |
| 2022 | 4681 | Tribal           | \$226,715    | Tribal       | Havasupai Indian Reservation                |
| 2019 | 4458 | State-DR         | \$214,168    | Tribal       | Coushatta Indian Reservation                |
| 2017 | 4298 | State-DR         | \$201,490    | Tribal       | Lake Traverse (sisseton) Indian Reservation |
| 2019 | 4467 | State-DR         | \$139,929    | Tribal       | Cheyenne River Indian Reservation           |
| 2021 | 4587 | State-DR         | \$120,164    | Tribal       | Chickasaw Nation                            |

## Similar Distributions

One way of thinking about *how many* Declarations we may additionally
see, is by looking at the distribution of declaration data. We would
expect the distribution to look approximately symmetrical, or normal
(after a log transformation). Where we see gaps is an indication of
potential impacts that could have occured (or may occur) but are not
currently declared because of the threshold policy.

Here is the distribution plot of current DRs in Grants Manager, data
from **seven years of Declarations**.

![](/images/unnamed-chunk-23-1.png)<!-- -->

And here is a second plot with **added** bars to fill in the missing gap
that *should* be there for Tribal specific events.

![](/images/unnamed-chunk-24-1.png)<!-- -->

The additions to this plot represent a sample of 4 to 12 small scale
impacts, ranging from \$22,500 to \$130,000, that could occur over a 1-7
year time period. Additional years would also likely add Declarations in
the rest of the Tribal distribution (the current range of
\$200,000-\$1M+) Some caveats: - Assumes the \$250k threshold is what
‘prevents’ smaller impacts from being registered as Declarations, as
opposed to state (or tribal) resources dealing with this impacts without
FEMA. - Assumes ‘similar underlying’ character of geographic entities,
between Tribes and States, with tribes being a ‘scaled down’ equivalent
to States. [^5]

To try and validate this idea that there **are impacts below the 250k
threshold** that *could have been* declared but were not, we now turn to
Preliminary Damage Assessment data.

# \_\_\_\_\_\_\_\_\_

# PDA Data

The Preliminary Damage Assessment data provides a look at how often
tribes approached the **250K Threshold** and how often they might cross
the new **100k Threshold**. - *Data Range 2012-2022*

## Tribal PDA Amounts

Placeholder (you should not see this)

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:500px; overflow-x: scroll; width:800px; ">

<table class="table table-striped" style="color: black; width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
Year
</th>
<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">
DR
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
State
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
County_Name
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Total_Cost_Estimate
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Declaration_Status
</th>
<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">
Is_Tribe
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4104
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also NM and UT)
</td>
<td style="text-align:left;">
\$10,446,468
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4352
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Acoma
</td>
<td style="text-align:left;">
\$5,600,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4561
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
Sac and Fox Tribe of the Mississippi in Iowa
</td>
<td style="text-align:left;">
\$4,320,600
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
4243
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Colville Indian Reservation
</td>
<td style="text-align:left;">
\$4,191,319
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4151
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santa Clara Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$3,969,920
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Hollywood Indian Reservation
</td>
<td style="text-align:left;">
\$3,667,797
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4591
</td>
<td style="text-align:left;">
AL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also FL)
</td>
<td style="text-align:left;">
\$3,500,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4103
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Eastern Band of Cherokee Indians of North Carolina
</td>
<td style="text-align:left;">
\$3,161,875
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
4409
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Tohono O’odham Reservation and Trust Lands
</td>
<td style="text-align:left;">
\$2,522,957
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4303
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Pyramid Lake Indian Reservation
</td>
<td style="text-align:left;">
\$2,397,346
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4148
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Cochiti Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$2,110,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4448
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Oglala Sioux Tribe of the Pine Ridge Reservation
</td>
<td style="text-align:left;">
\$1,808,022
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4425
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Soboba Indian Reservation
</td>
<td style="text-align:left;">
\$1,480,010
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4202
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Moapa River Indian Reservation
</td>
<td style="text-align:left;">
\$1,397,460
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
4206
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Soboba Indian Reservation
</td>
<td style="text-align:left;">
\$1,350,696
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4148
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Sandia Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$1,028,354
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4142
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Karuk Reservation
</td>
<td style="text-align:left;">
\$1,021,557
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4069
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Fond du Lac Indian Reservation
</td>
<td style="text-align:left;">
\$930,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4312
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Resighini Rancheria (Indian Reservation)
</td>
<td style="text-align:left;">
\$833,072
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4436
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also NM and UT)
</td>
<td style="text-align:left;">
\$802,689
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4456
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Creek (OTSA)
</td>
<td style="text-align:left;">
\$788,507
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4302
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Hoopa Valley Indian Reservation
</td>
<td style="text-align:left;">
\$765,987
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4463
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Cheyenne River Indian Reservation
</td>
<td style="text-align:left;">
\$683,848
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4422
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
La Jolla Indian Reservation
</td>
<td style="text-align:left;">
\$611,725
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4423
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Cahuilla Indian Reservation
</td>
<td style="text-align:left;">
\$584,033
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4430
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
Sac and Fox Tribe of the Mississippi in Iowa
</td>
<td style="text-align:left;">
\$540,580
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
4233
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Crow Creek Indian Reservation
</td>
<td style="text-align:left;">
\$441,750
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
4233
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lower Brule Indian Reservation
</td>
<td style="text-align:left;">
\$428,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
4389
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Havasupai Indian Reservation
</td>
<td style="text-align:left;">
\$419,760
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4146
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Eastern Band of Cherokee Indians of North Carolina
</td>
<td style="text-align:left;">
\$362,606
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
4384
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Colville Indian Reservation
</td>
<td style="text-align:left;">
\$356,438
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4197
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Santa Clara
</td>
<td style="text-align:left;">
\$325,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lake Traverse (Sisseton) Indian Reservation
</td>
<td style="text-align:left;">
\$265,963
</td>
<td style="text-align:left;">
Turndown
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4079
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Mescalero Tribe
</td>
<td style="text-align:left;">
\$180,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Upper Sioux Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$147,979
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4076
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Red Cliff Indian Reservation
</td>
<td style="text-align:left;">
\$125,900
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4155
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Oglala Sioux Tribe of the Pine Ridge Reservation
</td>
<td style="text-align:left;">
\$113,660
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4463
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Rosebud Indian Reservation
</td>
<td style="text-align:left;">
\$76,296
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Prairie Island Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$75,316
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4323
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Turtle Mountain Indian Reservation
</td>
<td style="text-align:left;">
\$73,095
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4182
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Prairie Island Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$65,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4467
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Cheyenne River Indian Reservation
</td>
<td style="text-align:left;">
\$60,202
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
4390
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
White Earth Indian Reservation
</td>
<td style="text-align:left;">
\$58,481
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4188
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Colville Indian Reservation
</td>
<td style="text-align:left;">
\$54,000
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4443
</td>
<td style="text-align:left;">
ID
</td>
<td style="text-align:left;">
Nez Perce Indian Reservation
</td>
<td style="text-align:left;">
\$38,440
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Red Lake Indian Reservation
</td>
<td style="text-align:left;">
\$35,200
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4190
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Standing Rock Sioux Tribe of North & South Dakota
</td>
<td style="text-align:left;">
\$24,099
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4467
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lower Brule Indian Reservation
</td>
<td style="text-align:left;">
\$10,845
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2011
</td>
<td style="text-align:right;">
4047
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Cochiti Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2011
</td>
<td style="text-align:right;">
4047
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Acoma
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2011
</td>
<td style="text-align:right;">
4047
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santa Clara Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4069
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Grand Portage Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4069
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Mille Lacs Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4079
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santa Clara Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4081
</td>
<td style="text-align:left;">
MS
</td>
<td style="text-align:left;">
Mississippi Choctaw Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4083
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Colville Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
4087
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Mashantucket Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
CO
</td>
<td style="text-align:left;">
Southern Ute Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
CO
</td>
<td style="text-align:left;">
Ute Mountain Indian Reservation (Also NM)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Big Cypress Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Brighton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Florida Tribe of Eastern Creek (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Fort Pierce Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Hollywood Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Immokalee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Miccosukee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also AL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Tampa Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Apache Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Chitimacha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Clifton Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Coushatta Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Jena Band of Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Tunica-Biloxi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
United Houma Nation (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Pawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Turndown
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4106
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Mashantucket Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4115
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Oglala Sioux Tribe of the Pine Ridge Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4118
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Spirit Lake Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4123
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Standing Rock Sioux Tribe of North & South Dakota
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4125
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Oglala Sioux Tribe of the Pine Ridge Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4127
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Fort Belknap Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4127
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Fort Peck Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4127
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Rocky Boy’s Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4128
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Spirit Lake Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4128
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Turtle Mountain Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4147
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santa Clara Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4148
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also AZ and UT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4148
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
San Felipe Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4148
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santo Domingo Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4152
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Isleta Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4152
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also AZ and UT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4152
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Sandia Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
4155
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Cheyenne River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
San Carlos Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Turndown
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Omaha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4168
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Sauk-Suiattle Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4168
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Stillaguamish Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4168
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Tulalip Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4182
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Bois Forte (Nett Lake) Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4182
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Red Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4184
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
Sac and Fox Tribe of the Mississippi in Iowa
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4186
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Standing Rock Sioux Tribe of North & South Dakota
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4197
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Acoma
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
4198
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Fort Belknap Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
4246
</td>
<td style="text-align:left;">
ID
</td>
<td style="text-align:left;">
Coeur d’Alene Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Mashantucket Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Paucatuck Eastern Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Kalispel Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Lummi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Nooksack Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Quileute Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Quinault Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Spokane Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Yakama Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2016
</td>
<td style="text-align:right;">
4276
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Bad River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2016
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Hopi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2016
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Los Coyotes Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Turndown
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4298
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Cheyenne River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4298
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lake Traverse (Sisseton) Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4298
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Oglala Sioux Tribe of the Pine Ridge Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4303
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Reno-Sparks Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4303
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Washoe Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4308
</td>
<td style="text-align:left;">
CA
</td>
<td style="text-align:left;">
Tule River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4315
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Pawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Big Cypress Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Brighton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Fort Pierce Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Immokalee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4341
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Tampa Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
4346
</td>
<td style="text-align:left;">
SC
</td>
<td style="text-align:left;">
Catawba Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Big Cypress Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Brighton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Florida Tribe of Eastern Creek (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Fort Pierce Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Hollywood Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Immokalee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Miccosukee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also AL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Tampa Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
GA
</td>
<td style="text-align:left;">
Tama Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
ID
</td>
<td style="text-align:left;">
Coeur d’Alene Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
ID
</td>
<td style="text-align:left;">
Kootenai Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
ID
</td>
<td style="text-align:left;">
Nez Perce Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Omaha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2017
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Winnebago Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
4390
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Leech Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
4390
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Red Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2018
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
SC
</td>
<td style="text-align:left;">
Catawba Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Omaha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Ponca (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Sac and Fox Indian Reservation (Also KS)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Santee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4420
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Winnebago Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4438
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Pawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4440
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Cheyenne River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4440
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lake Traverse (Sisseton) Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4440
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Rosebud Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4442
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
White Earth Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4446
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Ponca (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4446
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Ponca TDSA
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4459
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Menominee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4459
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
St. Croix Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4469
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Flandreau Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
4469
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Yankton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2019
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
MS
</td>
<td style="text-align:left;">
Mississippi Choctaw Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4480
</td>
<td style="text-align:left;">
NY
</td>
<td style="text-align:left;">
Cayuga Nation (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4480
</td>
<td style="text-align:left;">
NY
</td>
<td style="text-align:left;">
Oneida (East) Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4483
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
Omaha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4483
</td>
<td style="text-align:left;">
IA
</td>
<td style="text-align:left;">
Sac and Fox Tribe of the Mississippi in Iowa
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Apache Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Chitimacha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Clifton Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Coushatta Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Jena Band of Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Tunica-Biloxi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4484
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
United Houma Nation (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4485
</td>
<td style="text-align:left;">
TX
</td>
<td style="text-align:left;">
Alabama and Coushatta Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4485
</td>
<td style="text-align:left;">
TX
</td>
<td style="text-align:left;">
Kickapoo Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4485
</td>
<td style="text-align:left;">
TX
</td>
<td style="text-align:left;">
Ysleta del Sur Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4486
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Florida Tribe of Eastern Creek (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4486
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Miccosukee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4486
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also AL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4487
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Coharie (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4487
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Eastern Band of Cherokee Indians of North Carolina
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4487
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Haliwa-Saponi (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4487
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Lumbee (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4487
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Meherrin (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4487
</td>
<td style="text-align:left;">
NC
</td>
<td style="text-align:left;">
Waccamaw Siouan (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4488
</td>
<td style="text-align:left;">
NJ
</td>
<td style="text-align:left;">
Ramapough (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4488
</td>
<td style="text-align:left;">
NJ
</td>
<td style="text-align:left;">
Rankokus Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4492
</td>
<td style="text-align:left;">
SC
</td>
<td style="text-align:left;">
Catawba Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Bay Mills Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Grand Traverse Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Hannahville Community Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Huron Potawatomi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Isabella Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
L’Anse Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Lac Vieux Desert Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Little Traverse Bay Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Ontonagon Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Pokagon Potawatomi Band (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4494
</td>
<td style="text-align:left;">
MI
</td>
<td style="text-align:left;">
Sault Ste. Marie Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4498
</td>
<td style="text-align:left;">
CO
</td>
<td style="text-align:left;">
Southern Ute Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4498
</td>
<td style="text-align:left;">
CO
</td>
<td style="text-align:left;">
Ute Mountain Indian Reservation (Also NM)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4500
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Golden Hill Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4500
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Mashantucket Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4500
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Paucatuck Eastern Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4500
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Schaghticoke Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4501
</td>
<td style="text-align:left;">
GA
</td>
<td style="text-align:left;">
Tama Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4503
</td>
<td style="text-align:left;">
AL
</td>
<td style="text-align:left;">
MOWA Choctaw (SAIR)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4503
</td>
<td style="text-align:left;">
AL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also FL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4504
</td>
<td style="text-align:left;">
KS
</td>
<td style="text-align:left;">
Delaware-Muncie (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4504
</td>
<td style="text-align:left;">
KS
</td>
<td style="text-align:left;">
Sac and Fox Indian Reservation (Also NE)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4505
</td>
<td style="text-align:left;">
RI
</td>
<td style="text-align:left;">
Grand Ronde Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4505
</td>
<td style="text-align:left;">
RI
</td>
<td style="text-align:left;">
Narragansett Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Blackfeet Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Crow Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Flathead Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Fort Belknap Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Fort Peck Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Northern Cheyenne Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4508
</td>
<td style="text-align:left;">
MT
</td>
<td style="text-align:left;">
Rocky Boy’s Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4509
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Fort Berthold Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4509
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Lake Traverse Sisseton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4509
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Spirit Lake Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4509
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Standing Rock Sioux Tribe of North & South Dakota
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4509
</td>
<td style="text-align:left;">
ND
</td>
<td style="text-align:left;">
Turtle Mountain Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4512
</td>
<td style="text-align:left;">
VA
</td>
<td style="text-align:left;">
Chickahominy (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4512
</td>
<td style="text-align:left;">
VA
</td>
<td style="text-align:left;">
Eastern Chickahominy (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4512
</td>
<td style="text-align:left;">
VA
</td>
<td style="text-align:left;">
Mattaponi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4512
</td>
<td style="text-align:left;">
VA
</td>
<td style="text-align:left;">
Pamunkey Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4515
</td>
<td style="text-align:left;">
IN
</td>
<td style="text-align:left;">
Pokagon Potawatomi Band (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4518
</td>
<td style="text-align:left;">
AR
</td>
<td style="text-align:left;">
Ozark Test Tribe
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4519
</td>
<td style="text-align:left;">
OR
</td>
<td style="text-align:left;">
Umatilla Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Bad River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Ho-Chunk Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Lac Courte Oreilles Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Lac du Flambeau Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Menominee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Oneida Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Red Cliff Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Sokaogon Chippewa Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
St. Croix Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4520
</td>
<td style="text-align:left;">
WI
</td>
<td style="text-align:left;">
Stockbridge Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Iowa Indian Reservation (Also KS)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Omaha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Pine Ridge Indian Reservation Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Ponca (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Ponca TDSA
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Sac and Fox Indian Reservation (Also KS)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Santee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4521
</td>
<td style="text-align:left;">
NE
</td>
<td style="text-align:left;">
Winnebago Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4522
</td>
<td style="text-align:left;">
ME
</td>
<td style="text-align:left;">
Aroostook Band of Micmac (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4522
</td>
<td style="text-align:left;">
ME
</td>
<td style="text-align:left;">
Houlton Maliseet Indian Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4522
</td>
<td style="text-align:left;">
ME
</td>
<td style="text-align:left;">
Indian Stream (Township of)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4522
</td>
<td style="text-align:left;">
ME
</td>
<td style="text-align:left;">
Passamaquoddy Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4522
</td>
<td style="text-align:left;">
ME
</td>
<td style="text-align:left;">
T3 Indian Purchase
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4522
</td>
<td style="text-align:left;">
ME
</td>
<td style="text-align:left;">
T4 Indian Purchase
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Battle Mountain Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Campbell Ranch (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Carson Colony (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Dresslerville Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Duck Valley (Western Shoshone) Indian Reservation (A
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Duckwater Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Elko Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Ely Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Fallon Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Fallon Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Fort McDermitt Indian Reservation (Also OR)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Fort Mojave Indian Reservation (Also AZ)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Goshute Indian Reservation (Also UT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Las Vegas Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Lovelock Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Moapa River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Pyramid Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Reno-Sparks Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
South Fork Reservation and Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Summit Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Walker River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Washoe Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Wells Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Winnemucca Colony (Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Yerington Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4523
</td>
<td style="text-align:left;">
NV
</td>
<td style="text-align:left;">
Yomba Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Cocopah Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Colorado River Indian Reservation (Also CA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Fort McDowell Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Fort Mojave Indian Reservation (Also CA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Fort Yuma (Quechan) Indian Reservation (Also CA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Gila River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Havasupai Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Hopi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Hualapai Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Kaibab Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Maricopa Indian Reservation (Ak Chin)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also NM and UT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Pascua Yaqui Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Salt River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
San Carlos Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Tohono O’odham Reservation and Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Tonto Apache Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
White Mountain Apache Tribe
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Yavapai Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Yavapai-Apache Nation Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4524
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Zuni Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Goshute Indian Reservation (Also NV)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also AZ and NM)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Northwestern Shoshoni Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Paiute of Utah Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Skull Valley Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Uintah and Ouray Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4525
</td>
<td style="text-align:left;">
UT
</td>
<td style="text-align:left;">
Ute Mountain Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Cheyenne River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Crow Creek Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Flandreau Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lake Traverse (Sisseton) Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Lower Brule Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Northern Cheyenne Reservation and Trust Lands (&MT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Oglala Sioux Tribe of the Pine Ridge Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Rosebud Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Standing Rock Sioux Tribe of North & South Dakota
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Turtle Mountain Reservation and Trust Lands (& ND)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4527
</td>
<td style="text-align:left;">
SD
</td>
<td style="text-align:left;">
Yankton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4528
</td>
<td style="text-align:left;">
MS
</td>
<td style="text-align:left;">
Mississippi Choctaw Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Alamo Navajo Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Canoncito Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Cochiti Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Isleta Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Jemez Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Jicarilla Apache Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Laguna Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Mescalero Tribe
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Nambe Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also AZ and UT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Acoma
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Picuris
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Pojoaque
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of San Felipe
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Santa Ana
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Santa Clara
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Pueblo of Taos
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Ramah Navajo Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
San Felipe Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
San Ildefonso Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
San Juan Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Sandia Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santa Ana Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santa Clara Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Santo Domingo Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Taos Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Tesuque Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Ute Mountain Indian Reservation (Also CO)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Zia Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4529
</td>
<td style="text-align:left;">
NM
</td>
<td style="text-align:left;">
Zuni Pueblo (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Absentee Shawnee-Citizens Band of Potawatomi (TJSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Caddo-Wichita-Delaware (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Cherokee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Cheyenne-Arapaho (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Chickasaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Choctaw
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Choctaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Citizen Potawatomi Nation-Absentee Shawnee Tribe (OT
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Creek (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Creek-Seminole Joint Area (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Eastern Shawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Eastern Shawnee-Wyandotte (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Iowa (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Iowa-Sac and Fox Joint Area (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Kaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Kickapoo (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Kiowa-Comanche-Apache-Fort Sill Apache (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Miami (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Modoc (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Osage Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Otoe-Missouria (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Ottowa (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Pawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Peoria (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Ponca (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Quapaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Sac and Fox (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Seminole (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Seneca-Cayuga (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Tonkawa (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4530
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Wyandotte (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Bois Forte (Nett Lake) Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Deer Creek Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Fond du Lac Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Grand Portage Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Ho-Chunk Reservation and Trust Lands (Also WI)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Leech Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Lower Sioux Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Mille Lacs Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Minnesota Chippewa Indian Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Prairie Island Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Red Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Sandy Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Shakopee Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Upper Sioux Community (Indian Reservation)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
Vermillion Lake Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4531
</td>
<td style="text-align:left;">
MN
</td>
<td style="text-align:left;">
White Earth Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4533
</td>
<td style="text-align:left;">
AK
</td>
<td style="text-align:left;">
Tetlin (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4535
</td>
<td style="text-align:left;">
WY
</td>
<td style="text-align:left;">
Wind River Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Big Cypress Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Brighton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Fort Pierce Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Hollywood Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Immokalee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Seminole Indian Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4545
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Tampa Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4563
</td>
<td style="text-align:left;">
AL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also FL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4580
</td>
<td style="text-align:left;">
CT
</td>
<td style="text-align:left;">
Mashantucket Pequot Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4582
</td>
<td style="text-align:left;">
AZ
</td>
<td style="text-align:left;">
Navajo Nation Reservation (Also NM and UT)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4584
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Colville Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
4584
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Yakama Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
AL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also FL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
MA
</td>
<td style="text-align:left;">
Hassanamisco Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
MA
</td>
<td style="text-align:left;">
Mashpee Wampanoag Tribe
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2020
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OR
</td>
<td style="text-align:left;">
Klamath TDSA
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
4593
</td>
<td style="text-align:left;">
WA
</td>
<td style="text-align:left;">
Puyallup Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
4598
</td>
<td style="text-align:left;">
MS
</td>
<td style="text-align:left;">
Mississippi Choctaw Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
4599
</td>
<td style="text-align:left;">
OR
</td>
<td style="text-align:left;">
Grand Ronde Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Big Cypress Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Brighton Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Florida Tribe of Eastern Creek (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Fort Pierce Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Hollywood Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Immokalee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Miccosukee Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Poarch Creek Reservation and Trust Lands (also AL)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Seminole Indian Trust Lands
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
FL
</td>
<td style="text-align:left;">
Tampa Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Apache Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Chitimacha Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Clifton Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Coushatta Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Jena Band of Choctaw (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
Tunica-Biloxi Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
LA
</td>
<td style="text-align:left;">
United Houma Nation (TDSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Absentee Shawnee-Citizens Band of Potawatomi (TJSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Caddo-Wichita-Delaware (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Cherokee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Cheyenne-Arapaho (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Chickasaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Choctaw
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Choctaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Citizen Potawatomi Nation-Absentee Shawnee Tribe (OT
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Creek (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Creek-Seminole Joint Area (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Eastern Shawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Eastern Shawnee-Wyandotte (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Iowa (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Iowa-Sac and Fox Joint Area (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Kaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Kickapoo (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Kiowa-Comanche-Apache-Fort Sill Apache (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Miami (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Modoc (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Osage Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Otoe-Missouria (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Ottowa (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Pawnee (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Peoria (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Ponca (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Quapaw (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Sac and Fox (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Seminole (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Seneca-Cayuga (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Tonkawa (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OK
</td>
<td style="text-align:left;">
Wyandotte (OTSA)
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
<tr>
<td style="text-align:right;">
2021
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:left;">
OR
</td>
<td style="text-align:left;">
Siletz Indian Reservation
</td>
<td style="text-align:left;">
\$0
</td>
<td style="text-align:left;">
Declared
</td>
<td style="text-align:left;">
Tribe
</td>
</tr>
</tbody>
</table>

</div>

## Turndown Status

Placeholder (you should not see this)

| Year |  DR | State | County_Name                                 | Total_Cost_Estimate | Declaration_Status | Is_Tribe |
|-----:|----:|:------|:--------------------------------------------|:--------------------|:-------------------|:---------|
| 2017 |  NA | SD    | Lake Traverse (Sisseton) Indian Reservation | \$265,963           | Turndown           | Tribe    |
| 2012 |  NA | OK    | Pawnee (OTSA)                               | \$0                 | Turndown           | Tribe    |
| 2013 |  NA | AZ    | San Carlos Indian Reservation               | \$0                 | Turndown           | Tribe    |
| 2016 |  NA | CA    | Los Coyotes Indian Reservation              | \$0                 | Turndown           | Tribe    |

## Tribes Near Threshold

Placeholder (you should not see this)

| Year |   DR | State | County_Name                                      | Total_Cost_Estimate | Declaration_Status | Is_Tribe |
|-----:|-----:|:------|:-------------------------------------------------|:--------------------|:-------------------|:---------|
| 2012 | 4079 | NM    | Mescalero Tribe                                  | \$180,000           | Declared           | Tribe    |
| 2019 | 4442 | MN    | Upper Sioux Community (Indian Reservation)       | \$147,979           | Declared           | Tribe    |
| 2012 | 4076 | WI    | Red Cliff Indian Reservation                     | \$125,900           | Declared           | Tribe    |
| 2013 | 4155 | SD    | Oglala Sioux Tribe of the Pine Ridge Reservation | \$113,660           | Declared           | Tribe    |
| 2019 | 4463 | SD    | Rosebud Indian Reservation                       | \$76,296            | Declared           | Tribe    |
| 2019 | 4442 | MN    | Prairie Island Community (Indian Reservation)    | \$75,316            | Declared           | Tribe    |
| 2017 | 4323 | ND    | Turtle Mountain Indian Reservation               | \$73,095            | Declared           | Tribe    |
| 2014 | 4182 | MN    | Prairie Island Community (Indian Reservation)    | \$65,000            | Declared           | Tribe    |
| 2019 | 4467 | SD    | Cheyenne River Indian Reservation                | \$60,202            | Declared           | Tribe    |
| 2018 | 4390 | MN    | White Earth Indian Reservation                   | \$58,481            | Declared           | Tribe    |
| 2014 | 4188 | WA    | Colville Indian Reservation                      | \$54,000            | Declared           | Tribe    |

- There are 4 instances of Tribal PDAs that are **above (or near)
  \$100k** that were declared.
- These represent specific Tribes that could have applied directly,
  instead of under a State.

## \_\_\_\_\_\_\_\_

## Appendix

## Current Tribal PDAs

- Shows active Tribal PDAs in the FACT Portal.
- Three instances: 1 Tribal Specific Declaration, 2 Sub-State
  Declarations
- One state level cost estimate is above the \$100k Threshold (NY),
  tribe name unlisted.

<table class="table table-striped" style="color: black; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
pda_datetime
</th>
<th style="text-align:left;">
ST
</th>
<th style="text-align:left;">
Cnty
</th>
<th style="text-align:left;">
Work_Cat
</th>
<th style="text-align:left;">
Estimated_Cost
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
1/25/2023
</td>
<td style="text-align:left;">
New York
</td>
<td style="text-align:left;">
Suffolk County
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
\$166,414
</td>
</tr>
<tr>
<td style="text-align:left;">
5/31/2023
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:left;">
Valley County
</td>
<td style="text-align:left;">
C
</td>
<td style="text-align:left;">
\$6,251
</td>
</tr>
<tr>
<td style="text-align:left;">
5/31/2023
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:left;">
Valley County
</td>
<td style="text-align:left;">
C
</td>
<td style="text-align:left;">
\$41,000
</td>
</tr>
<tr>
<td style="text-align:left;">
5/31/2023
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:left;">
Roosevelt County
</td>
<td style="text-align:left;">
C
</td>
<td style="text-align:left;">
\$27,500
</td>
</tr>
<tr>
<td style="text-align:left;">
5/31/2023
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:left;">
Roosevelt County
</td>
<td style="text-align:left;">
C
</td>
<td style="text-align:left;">
\$70,000
</td>
</tr>
<tr>
<td style="text-align:left;">
7/31/2023
</td>
<td style="text-align:left;">
Montana
</td>
<td style="text-align:left;">
Golden Valley County
</td>
<td style="text-align:left;">
B
</td>
<td style="text-align:left;">
\$3,500
</td>
</tr>
<tr>
<td style="text-align:left;">
11/14/2023
</td>
<td style="text-align:left;">
New York
</td>
<td style="text-align:left;">
Westchester County
</td>
<td style="text-align:left;">
A
</td>
<td style="text-align:left;">
\$2,593
</td>
</tr>
</tbody>
</table>

## \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## Previous Threshold Analysis, Key Figures

<div class="plotly html-widget html-fill-item" id="htmlwidget-c1b6e5526685f99639ff" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-c1b6e5526685f99639ff">{"x":{"data":[{"x":[0.8160701347514987,0.84497203249484298,0.89694307148456565,1.2631660386919976,1.0080769795924425,0.61917129438370466,0.70834118463098994,0.85571067929267874,0.72730237934738395,1.0737606432288884,0.73000122681260105,1.1063741313293576,1.3187912032008171,0.82405774053186176,1.2777964901179075,1.3777930341660976,1.2230998042970898,1.3410296911373734,1.2395150208845735,1.3346649764105678,0.6242892669513822,1.1126001413911581,1.2437802212312818,1.0220352120697498,1.174245491810143,0.64547456689178939,0.67294001299887896,0.71438215393573046,1.1170673174783587,1.0291475085541606,1.1440627155825496,0.73296117205172773,1.247263447381556,0.74465367961674933,0.61558770854026079,0.84202119503170247,1.2085701556876303,0.72518412563949819,0.73736510351300244,0.62174861151725058,0.65691416580229989,0.98178203999996183,0.67522842902690172,0.65709238946437831,0.65829134583473203,1.2747027570381761,1.0933301636949182,0.99474819768220191,1.0542719215154648,0.72190115991979842,0.61400564648210998,1.0024916801601649,1.3747613724321126,0.9157143726944923,0.64353626649826756,0.82766177263110874,1.2243281828239561,1.0925789354369044,0.72818675562739377,0.95641066674143072,1.1310300262644888,1.3856341758742929,0.68231884445995084,0.99122447036206718,0.96101350691169496,0.67418735846877098,0.99128118287771938,0.80187093112617736,1.1930858572944998,0.8717782294377685,1.2662821231409906,1.1652944896370172,0.82854767292737963,1.3125223329290749,1.0350417548790574,0.7317318700253963,0.79996003620326517,1.250131829828024,0.99509739428758626,1.1177085431292653,1.2666015235707164,1.1405058439821005,1.218790138885379,1.1769446637481451,0.98478269148617981,1.256394086778164,0.80311269238591199,0.9888324089348316,0.76880026925355194,1.0520370127633214,0.93645224776118996,1.1046634059399367,1.2025090070441364,0.72808219399303198,1.235223113745451,0.68698963336646557,1.3108144229277969,0.8391608271747828,1.3308640174567699,0.624330448359251,0.96844111233949659,1.1909582799300551,1.2569643056020141,1.0482053717598319,0.73773036170750861,1.2320267833769321,0.91928334236145015,1.3343370728194714,0.85504430904984474,0.94865454453974962,1.0616490446031093,1.2089145956560969,0.96905508823692799,0.74276906400918963,0.98693653587251906,1.3979020033031704,1.0525649048388004,0.6390706732869148,1.3657581025734544,0.70171532463282338,1.0874914079904556,1.1362683752551674,1.125523173250258,1.1623462880030275,1.2600955398753286,1.2676275996491313,1.378736930899322,0.66722837686538694,1.0521710705012084,1.0916436005383729,1.2758156152442099,0.91618371102958918,0.93561312835663557,0.66571187805384402,0.91109068542718885,0.97760848551988599,1.1907675962895155,0.82019171882420783,0.72547635678201916,0.73898842893540861,0.8608897440135479,0.63675736356526613,1.2209685310721397,0.88047985136508933,1.1115535646677017,1.0308176903054118,1.0270692944526671,0.98727148398756981,0.82217431049793954,0.78108176048845057,0.8681006377562881,1.3507240861654282,0.89926707725971933,1.1542147399857641,0.62352493144571786,0.78569740988314152,0.70219061337411404,0.72634863499552016,1.1883575100451709,0.70103975702077148,1.1527615122497081,0.649007778801024,1.2529945280402899,1.1569797148928047,0.66571829058229914,0.75498325750231743,0.60103479549288741,1.0648306882008911,1.0279165927320719,0.93921616096049543,1.2555002611130477,1.0474763501435518,0.88288498129695658,0.92559723388403659,1.0755773697048425,0.60033874046057467,1.1292989391833543,0.89783753845840697,1.105749166943133,0.88566626720130448,0.88110691104084249,1.1256152173504232,1.0132670320570469,1.0094905821606517,0.89105256460607052,1.1462465634569525,1.2625313922762871,1.0662586238235234,1.3297909308224916,0.92655508499592543,1.0261603279039264,0.71503591965883961,0.62522839847952127,1.3451166419312357,0.85118354316800837,1.0288522716611623,1.17923871781677,0.63587144147604702,0.84592075143009426,1.2246946441009641,0.9831481222063303,1.383490405790508,0.74001762028783558,1.1761656641960143,0.62650920618325467,1.3120336456224324,0.69896379094570871,0.91295702252537014,0.98929426725953817,0.65442097205668692,0.70937664881348605,1.3501442549750209,0.66364352274686089,0.6456436300650239,0.85727425497025256,1.1975497134029864,0.6011428417637944,1.219472872465849,0.83108182735741143,1.3276420097798109,0.68900808561593285,1.3051506262272596,0.71840288005769248,1.0708005685359239,0.82982007134705782,0.66866995003074403,0.71703737732022998,0.89374261982738967,0.66268611662089816,1.0064990961924196,0.95332481786608692,1.161822072416544,1.0583581401035189,0.93164458032697439,0.86487788911908869,1.288993790373206,1.0160275436937809,0.87549645062535997,1.1748422170057893,0.6822278691455721,0.604051998257637,1.0968807538971306,0.91456201002001758,1.2433576622977853,1.2567010844126343,0.63766362089663742,1.1478630401194096,0.61527635026723138,0.76307862456887965,0.7526914017274976,1.3599866177886724,1.0604614827781915,1.1217783845961093,0.60496642943471668,0.61519531644880776,1.0975981609895826,0.72946771066635852,0.89423160497099152,0.9814403712749481,0.90411024093627934,0.89777917806059127,1.2127141941338779,1.2061915330588819,0.95497689284384246,0.75172234680503602,1.3613400828093289,1.3424652425572277,1.3265636231750251,1.0583683578297496,0.75146831572055817,0.83813355509191756,1.211549728550017,1.3924885069951416,1.1918516736477613,0.63646705932915215,0.8402330650016665,0.62305722199380398,0.82271460928022866,1.0631362957879902,0.68356964439153667,1.1765013357624412,1.142723642103374,0.76156576555222277,0.82629174627363677,0.93694994859397407,0.6476365046575665,0.71220709178596731,1.0400796219706536,0.73188399039208885,0.66650449819862834,1.1528521556407214,0.96364364270120861,1.0646970307454466],"y":[7.2701671789641837,7.1075214257578168,7.2321318991151449,7.4958143885696149,7.1157351905464097,8.252217108022279,8.2923790613904949,7.2250191366171661,7.2122266625996003,7.9059466061200743,6.5070236942338955,7.2148946993877052,9.0391302714691104,7.6694979257053104,6.9943875508390363,6.6164308564910366,7.12668007076104,7.3386989306992714,8.0538101228215968,6.3393904831668477,8.0195096756910456,7.5594062476083703,7.9305213885710097,7.615813729316522,6.5595668936827094,7.5752270663679235,6.8469334729409299,6.7767331116367879,7.298722724993552,6.7926913181210669,6.5918220415155178,6.9715476551725217,7.1972837630718312,7.2064126378686861,9.5268228211525958,6.6795481367455611,6.8520885334536406,7.8266877080759718,7.0782205032324113,9.4319951175999535,8.1851764686455564,10.539531478618946,9.8208786542204702,6.7444167820230572,7.0636184122823407,8.7302358016848096,6.9969669957474041,7.6356032024402403,6.8919680310102578,7.5420306028566992,7.3152303396609684,7.1541435541455289,7.3147006485639698,8.6393727640595674,7.0540137082765977,7.0833362289024562,6.8287656486345423,7.0618275037884271,7.5697854655440304,7.6487981667184224,7.923154670338084,7.4225045016637967,7.6962726441033311,7.2038248905464863,7.2494004638068326,8.2126011763663183,6.7979890069233608,7.6008669224936529,6.7141288069182146,6.1882409427849572,6.5412628186910222,7.630029685373839,6.8389828746304122,7.0036165918335884,6.985993983666801,7.3601636012442899,6.4607150827294078,7.7077567244627598,6.660897113964853,7.6440795910988664,8.5698837526100906,7.0089223348018139,7.4103474432292424,7.3039486604570492,6.533034702217023,6.3927518678447095,7.5217098924964816,null,6.5570647347625535,9.0038357989547606,8.1327000647776977,7.3825887098722562,6.8400890624862329,7.6041655944685731,7.0470506564458235,9.3845953344759234,8.2674825111579704,7.6361854255102246,7.7036694145735973,6.7808230342854392,8.4856389900988631,6.3500353780686076,7.3753791348922917,9.1578793038342106,8.0026464586258541,6.585541294802888,7.6792216336628067,7.430044988351737,8.2224241105093565,7.1121953016122434,6.9213499509510665,7.8248356782515875,6.6586850753366882,7.2464886653610279,6.7206403176742944,8.7574137656833297,8.3475836972438575,7.883050923934257,6.9800354105719196,7.459008100695387,8.0091155335240831,7.6190010405075261,7.7497504277280029,7.6091521335392756,5.9704760433411792,7.68400519252405,7.5244532413609164,6.1163910612927905,7.9425329989962519,7.4736827247244761,7.910638246232983,7.8463932870600424,8.0194883865624895,6.7023135719692233,7.027787114947655,7.053388713186302,7.1862473360940093,7.8782375365336748,6.8122763272235654,8.0381680449182209,7.1694642037360188,6.7037509403925242,6.7822583871693478,6.5870177495683651,7.8285266972727952,7.3764877506733537,6.8784680576176997,7.9735398668476476,7.4241266584636136,6.9419334292290138,7.7339161012683419,7.9315761569235157,6.5491839988533718,8.0076913791298026,7.3899660664000901,7.2600473700378796,7.2550995894284682,7.989858341944311,8.9988697833609752,7.4039232887939201,7.1520413998968673,7.8501532770783315,6.771174987144545,6.9852800464323259,6.9608674944499542,6.7663632619786966,7.6034836792643006,7.0176241359352103,7.6122703775496632,7.8975324883793991,8.0243485366810088,7.3751958218033611,7.1965479432026314,6.6316458701324841,7.1321195787304683,7.5088835564279197,7.4334209401157194,6.0921098371201232,7.08872435121806,6.9905812668806639,6.9938286627342894,7.0793198768275483,7.0456518482280304,6.8136346942835129,7.031068113016417,8.3338902839516855,8.7128378352300295,9.412309413100937,8.7622208168018396,8.4771644922556231,8.2849799054963675,6.4522721381042869,6.579142888192866,8.637307507858214,7.511108841909607,8.3691270626045462,8.0800071617395712,7.8528563233164101,7.822546302583298,7.6564512058179277,8.081924831351369,8.1104042265449507,7.7899066302868665,7.0405328324428851,7.1675651999565675,7.6277513151905465,7.6680838788965549,7.1796984647299738,7.4250163959370887,7.5105887730100642,8.0401557295861465,6.6170612366034973,7.527781299276568,7.1876072589195514,7.5245439594841024,7.9261009014522905,7.0918110931888974,7.2632663031811306,7.9924267540439367,7.1360971119071879,7.2354322501371504,7.3943175358088471,7.4580561062893294,7.3494179728103628,7.3404645646897793,7.8427987931788659,6.8104074080076131,6.8181592787898486,6.9470081367328431,7.0594606953453587,7.157023719705057,7.6095686183769695,8.4972125266111895,9.3458602521082312,7.1785454937005237,6.4758450180108795,8.3971606710545661,7.9188487648149488,7.4935158998353053,7.5827167896718439,7.6688106307755648,8.1866829060722885,6.770936282464862,6.8885424811087201,6.0688035896395274,6.4234733266070112,7.6146451334195797,7.4844733007597197,6.2570176320769475,6.4403545134027809,6.5624839221132865,8.0361535488500717,7.6188211854157357,7.2845549081517005,7.2020448741795677,7.1844750467973473,6.6911735464439746,6.7370615814938741,7.1278692232241392,7.005666527753263,6.7132541307020324,7.2162694830945666,8.1599009518124674,7.6703573393946343,6.4675270458257863,6.8285572202877081,6.3096119046761903,6.9626329665149953,7.514997705601477,7.8433997479124775,7.0660687868266203,5.6154846746995721,7.6238275804208389,7.1673034673908367,7.141416122033994,7.4310774478241637,7.7220203464285015,6.1677126595724925,7.1025039916880548,7.9750836273995063,6.3360321446645402,7.3348765740363753,6.8971469592146208,5.6258593990627102,5.4252413065035592,8.530990750009666,5.8787344313259746,9.1356862908529664,6.6772192274991085,7.2719152103725895,6.0740597380547463,5.9920508367743199,7.4427669722615555],"text":["County <br> Total: $18,628,397","County <br> Total: $12,809,135","County <br> Total: $17,066,313","County <br> Total: $31,319,206","County <br> Total: $13,053,681","County <br> Total: $178,737,611","County <br> Total: $196,057,922","County <br> Total: $16,788,653","County <br> Total: $16,301,416","County <br> Total: $80,526,682","County <br> Total: $3,213,822","County <br> Total: $16,401,919","County <br> Total: $1,094,267,993","County <br> Total: $46,718,613","County <br> Total: $9,871,426","County <br> Total: $4,134,658","County <br> Total: $13,387,094","County <br> Total: $21,811,824","County <br> Total: $113,190,784","County <br> Total: $2,184,724","County <br> Total: $104,594,566","County <br> Total: $36,258,411","County <br> Total: $85,216,300","County <br> Total: $41,287,175","County <br> Total: $3,627,181","County <br> Total: $37,603,674","County <br> Total: $7,029,764","County <br> Total: $5,980,322","County <br> Total: $19,894,122","County <br> Total: $6,204,272","County <br> Total: $3,906,743","County <br> Total: $9,365,891","County <br> Total: $15,750,433","County <br> Total: $16,084,711","County <br> Total: $3,363,739,754","County <br> Total: $4,781,385","County <br> Total: $7,113,604","County <br> Total: $67,093,413","County <br> Total: $11,973,537","County <br> Total: $2,703,933,278","County <br> Total: $153,172,909","County <br> Total: $34,635,624,986","County <br> Total: $6,620,408,908","County <br> Total: $5,551,606","County <br> Total: $11,577,526","County <br> Total: $537,312,319","County <br> Total: $9,930,318","County <br> Total: $43,212,410","County <br> Total: $7,797,566","County <br> Total: $34,836,592","County <br> Total: $20,664,613","County <br> Total: $14,260,721","County <br> Total: $20,639,566","County <br> Total: $435,888,286","County <br> Total: $11,324,139","County <br> Total: $12,115,421","County <br> Total: $6,741,762","County <br> Total: $11,529,890","County <br> Total: $37,135,822","County <br> Total: $44,544,103","County <br> Total: $83,781,170","County <br> Total: $26,454,491","County <br> Total: $49,690,252","County <br> Total: $15,989,195","County <br> Total: $17,758,308","County <br> Total: $163,152,403","County <br> Total: $6,280,414","County <br> Total: $39,890,008","County <br> Total: $5,177,665","County <br> Total: $1,542,529","County <br> Total: $3,477,506","County <br> Total: $42,660,348","County <br> Total: $6,902,208","County <br> Total: $10,083,609","County <br> Total: $9,682,769","County <br> Total: $22,917,570","County <br> Total: $2,888,823","County <br> Total: $51,022,464","County <br> Total: $4,580,278","County <br> Total: $44,063,515","County <br> Total: $371,436,562","County <br> Total: $10,207,370","County <br> Total: $25,724,129","County <br> Total: $20,134,553","County <br> Total: $3,412,135","County <br> Total: $2,470,284","County <br> Total: $33,243,205","County <br> Total: $0","County <br> Total: $3,606,368","County <br> Total: $1,008,872,007","County <br> Total: $135,734,778","County <br> Total: $24,131,578","County <br> Total: $6,919,806","County <br> Total: $40,193,846","County <br> Total: $11,144,214","County <br> Total: $2,424,390,420","County <br> Total: $185,129,216","County <br> Total: $43,270,210","County <br> Total: $50,544,373","County <br> Total: $6,037,045","County <br> Total: $305,946,321","County <br> Total: $2,238,891","County <br> Total: $23,734,477","County <br> Total: $1,438,410,635","County <br> Total: $100,612,488","County <br> Total: $3,850,718","County <br> Total: $47,777,695","County <br> Total: $26,918,273","County <br> Total: $166,890,338","County <br> Total: $12,947,661","County <br> Total: $8,343,596","County <br> Total: $66,809,917","County <br> Total: $4,557,107","County <br> Total: $17,639,748","County <br> Total: $5,255,832","County <br> Total: $572,029,960","County <br> Total: $222,627,071","County <br> Total: $76,391,159","County <br> Total: $9,550,863","County <br> Total: $28,774,427","County <br> Total: $102,121,294","County <br> Total: $41,590,702","County <br> Total: $56,202,155","County <br> Total: $40,659,215","County <br> Total: $934,267","County <br> Total: $48,306,578","County <br> Total: $33,454,990","County <br> Total: $1,307,345","County <br> Total: $87,604,403","County <br> Total: $29,762,947","County <br> Total: $81,403,101","County <br> Total: $70,207,993","County <br> Total: $104,589,111","County <br> Total: $5,038,635","County <br> Total: $10,660,664","County <br> Total: $11,307,980","County <br> Total: $15,354,944","County <br> Total: $75,550,895","County <br> Total: $6,490,484","County <br> Total: $109,184,855","County <br> Total: $14,772,825","County <br> Total: $5,055,384","County <br> Total: $6,056,925","County <br> Total: $3,863,760","County <br> Total: $67,380,336","County <br> Total: $23,795,159","County <br> Total: $7,559,222","County <br> Total: $94,089,166","County <br> Total: $26,554,056","County <br> Total: $8,748,580","County <br> Total: $54,189,584","County <br> Total: $85,422,417","County <br> Total: $3,541,464","County <br> Total: $101,784,853","County <br> Total: $24,545,590","County <br> Total: $18,199,034","County <br> Total: $17,992,602","County <br> Total: $97,691,447","County <br> Total: $997,416,557","County <br> Total: $25,347,252","County <br> Total: $14,191,650","County <br> Total: $70,820,429","County <br> Total: $5,904,298","County <br> Total: $9,666,741","County <br> Total: $9,138,190","County <br> Total: $5,839,401","County <br> Total: $40,131,581","County <br> Total: $10,414,205","County <br> Total: $40,950,712","County <br> Total: $78,983,736","County <br> Total: $105,768,254","County <br> Total: $23,724,811","County <br> Total: $15,723,511","County <br> Total: $4,281,959","County <br> Total: $13,555,418","County <br> Total: $32,275,980","County <br> Total: $27,127,893","County <br> Total: $1,236,244","County <br> Total: $12,266,464","County <br> Total: $9,785,557","County <br> Total: $9,858,796","County <br> Total: $12,003,957","County <br> Total: $11,108,272","County <br> Total: $6,510,725","County <br> Total: $10,741,656","County <br> Total: $215,716,866","County <br> Total: $516,223,270","County <br> Total: $2,584,076,203","County <br> Total: $578,391,757","County <br> Total: $300,029,886","County <br> Total: $192,742,980","County <br> Total: $2,833,164","County <br> Total: $3,794,343","County <br> Total: $433,825,927","County <br> Total: $32,442,689","County <br> Total: $233,947,946","County <br> Total: $120,230,723","County <br> Total: $71,260,335","County <br> Total: $66,458,873","County <br> Total: $45,336,195","County <br> Total: $120,761,123","County <br> Total: $128,947,321","County <br> Total: $61,646,007","County <br> Total: $10,978,017","County <br> Total: $14,708,696","County <br> Total: $42,437,758","County <br> Total: $46,567,239","County <br> Total: $15,125,253","County <br> Total: $26,608,569","County <br> Total: $32,403,121","County <br> Total: $109,686,694","County <br> Total: $4,140,654","County <br> Total: $33,712,169","County <br> Total: $15,402,826","County <br> Total: $33,460,960","County <br> Total: $84,354,104","County <br> Total: $12,354,233","County <br> Total: $18,334,565","County <br> Total: $98,270,991","County <br> Total: $13,680,547","County <br> Total: $17,196,101","County <br> Total: $24,792,612","County <br> Total: $28,711,386","County <br> Total: $22,357,297","County <br> Total: $21,900,907","County <br> Total: $69,629,799","County <br> Total: $6,462,615","County <br> Total: $6,579,090","County <br> Total: $8,851,355","County <br> Total: $11,467,481","County <br> Total: $14,355,757","County <br> Total: $40,697,570","County <br> Total: $314,209,379","County <br> Total: $2,217,493,004","County <br> Total: $15,085,164","County <br> Total: $2,991,163","County <br> Total: $249,555,734","County <br> Total: $82,954,888","County <br> Total: $31,153,955","County <br> Total: $38,256,999","County <br> Total: $46,646,158","County <br> Total: $153,702,148","County <br> Total: $5,901,184","County <br> Total: $7,736,535","County <br> Total: $1,171,665","County <br> Total: $2,651,414","County <br> Total: $41,175,952","County <br> Total: $30,512,311","County <br> Total: $1,807,210","County <br> Total: $2,756,446","County <br> Total: $3,651,543","County <br> Total: $108,679,153","County <br> Total: $41,573,132","County <br> Total: $19,255,622","County <br> Total: $15,923,800","County <br> Total: $15,292,325","County <br> Total: $4,911,083","County <br> Total: $5,458,261","County <br> Total: $13,423,360","County <br> Total: $10,131,419","County <br> Total: $5,167,274","County <br> Total: $16,454,134","County <br> Total: $144,510,279","County <br> Total: $46,811,826","County <br> Total: $2,934,440","County <br> Total: $6,738,497","County <br> Total: $2,039,913","County <br> Total: $9,175,550","County <br> Total: $32,733,922","County <br> Total: $69,725,444","County <br> Total: $11,642,988","County <br> Total: $412,560","County <br> Total: $42,055,780","County <br> Total: $14,699,394","County <br> Total: $13,849,196","County <br> Total: $26,982,703","County <br> Total: $52,725,526","County <br> Total: $1,471,368","County <br> Total: $12,662,202","County <br> Total: $94,424,001","County <br> Total: $2,167,868","County <br> Total: $21,621,136","County <br> Total: $7,891,318","County <br> Total: $422,539","County <br> Total: $266,217","County <br> Total: $339,611,560","County <br> Total: $756,378","County <br> Total: $1,366,717,121","County <br> Total: $4,755,681","County <br> Total: $18,702,926","County <br> Total: $1,185,928","County <br> Total: $981,845","County <br> Total: $27,718,264"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(248,118,109,1)"}},"hoveron":"points","name":"State-DR","legendgroup":"State-DR","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1.9859302995726467,1.7574642717838287,1.6670575104653835,2.1404035720974206,2.3193846780806782,1.6663004469126463,1.7447738381102682,2.1306011820212007,2.0649879045784472,2.0006039679050445,1.8594346202909946,1.8068514255806805,2.1211171738803385,1.7778375858440996,2.1560085982084276,1.873570990934968,2.2748966705054046,1.9960552981123327,1.8708036364987493,2.3089737398549914],"y":[6.569051275064993,5.4770198378826782,4.080240232067605,7.2046406142616517,5.7877245141936049,5.8372235125110015,6.7543689783981584,5.7755185281897239,6.3654258549878424,5.8218820015948527,5.8995333078935053,5.8682045589574852,6.084584788448165,6.1949481100734713,6.9399492171683272,6.128924836702244,6.4297409303219464,6.0301154984730125,6.134479304686165,5.3554794902343632],"text":["Hoopa Valley Tribe <br> Total: $3,707,168","Resighini Rancheria <br> Total: $299,928","Seminole Tribe of Florida <br> Total: $12,029","Pueblo of Acoma <br> Total: $16,019,463","Confederated Tribes of the Colville Reservation <br> Total: $613,372","Havasupai Tribe <br> Total: $687,415","Tohono O'odham Nation <br> Total: $5,680,232","La Jolla Band of Luiseno Indians <br> Total: $596,378","Cahuilla Band of Indians <br> Total: $2,319,679","Soboba Band of Luiseno Indians <br> Total: $663,569","Sac & Fox Tribe of the Mississippi in Iowa <br> Total: $793,474","Navajo Nation <br> Total: $738,256","County <br> Total: $1,215,036","Oglala Sioux Tribe <br> Total: $1,566,548","Muscogee (Creek) Nation <br> Total: $8,708,489","Sac & Fox Tribe of the Mississippi in Iowa <br> Total: $1,345,622","County <br> Total: $2,689,900","County <br> Total: $1,071,803","County <br> Total: $1,362,959","County <br> Total: $226,715"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","opacity":1,"size":5.6692913385826778,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(0,191,196,1)"}},"hoveron":"points","name":"Tribal","legendgroup":"Tribal","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],"y":[7.2701754951055291,7.1075197987899852,7.2321397064276578,7.4958107469596538,7.1157329953959234,8.2522159483153708,8.2923843951901901,7.0636157796048913,7.2250158521139722,7.2122253349622634,7.9059398062010491,6.5070217896306231,7.2148946641493517,9.039123696488268,7.6694899378208179,6.9943798753117727,6.6164396093268758,8.6393751983250002,7.1266863132038489,7.3386919920659963,8.0538110668594971,6.3393965935061534,8.019509121391863,7.5594087688362004,7.9305226713815875,7.6158151706042689,6.559569256591014,7.5752302768907214,6.8469407212362281,6.7767245771607918,7.2987247753813138,6.7926908566588216,6.5918148293277499,6.9715490959215094,7.1972924945798855,7.2064132581271299,9.5268223878419889,6.6795537456392999,6.8520896888870775,7.8266798856329682,7.0782224558563298,9.4319959707684582,8.1851819610954895,10.539523028718678,9.8208848143712757,7.0089138779236491,6.7444186747958108,7.4103406727153667,8.7302267974133247,6.9969631575046813,7.63560848602103,6.8919590393406667,7.5420356659197987,7.3152272698561935,7.1541414690149798,7.3147005634168805,8.1326911370583375,7.3825857263227466,7.0540051727524791,7.0833385000419842,6.8287734136215263,7.0618251571824144,7.5697930448412416,7.6487902138238759,7.9231464196731327,7.4224994156124753,7.6962711999388373,7.2038265914843764,7.2494015870535709,8.2125934754753995,6.7979882668113012,7.6008641252590889,6.7141339443740815,6.1882334080330939,6.5412679404377716,7.6300243893875601,6.8389880710548212,7.0036159812795162,6.985999573786164,7.3601685674916091,6.4607208625056307,7.7077614309907947,6.6608918847709635,7.6440791389852141,8.5698846504223543,7.7339158203400187,7.8830430992125082,6.5491827750181244,7.3039419926499187,6.5330261495509321,6.392746962818455,7.6189962529774276,7.5217028814861226,null,6.5570700986079462,9.0038360719364778,7.6840062733167542,7.5244609069971995,6.8400939014052575,7.6041595671253743,7.0470494330293656,9.3846025592426159,8.2674749612022733,7.6361890015268719,7.7036728113679587,6.7808243828729395,8.4856452349522016,6.350032915630103,7.3753796607938877,9.1578828854572052,8.0026518889037348,7.878239611656995,6.5855417274639763,7.6792251947899581,7.4300471912469082,8.2224311951670028,7.1121913210419709,6.9213532630642485,7.8248409326717709,6.6586892715976909,7.2464923829458581,6.7206414822078733,8.7574187755172854,8.3475779722056469,6.9419375431881178,8.4771645171403183,7.9315718539122146,6.4522716641514517,6.9800426341229134,7.4590066811903277,8.009116309967613,7.2550939678288611,7.9898565427064803,7.7497529719885598,7.6091589890050582,5.9704710044035645,7.8501585562059306,6.7711682711587713,6.9852800953207304,6.1163902434663147,7.9425259334287839,7.4736759379932414,7.9106409493798191,7.8463865595568372,8.0194864695063384,6.7023128672011971,7.0277842665861803,7.053385012494795,6.631642505598303,7.1862482388751729,7.5088794312227574,7.433416059151809,6.8122770803101709,8.0381624027171696,7.1694635443527384,6.7037541659125388,6.7822521637701616,7.0456465031088085,6.5870101263568683,7.8285331712154846,7.3764886081936236,6.8784770838466081,7.9735396182079716,7.4241308700776507,8.7622220949528291,7.0594680406014101,8.2849785695538571,7.6095684805371242,8.0076831549095395,7.3899734807873703,7.2600483280062456,8.3691192362778182,8.0800154582130226,8.9988765730543037,7.4039308907919219,7.1520329019787505,8.0819271443210834,8.110412325179583,7.7899049500661777,6.9608601641198486,6.7663682790727231,7.6034862646257304,7.0176261127680153,7.6122614590428253,7.8975376697020723,8.0243553370186085,7.3752027644859108,7.1965495346329611,7.5277866890868843,7.1321129013766313,7.5245384024902693,7.9261062176308004,6.0921040316890371,7.0887193779729367,6.9905855434652331,6.9938238929814753,7.0793244466037919,7.394322292057292,6.8136293453336831,7.0310712347873814,8.3338841013350766,8.7128375775212099,9.4123053166347788,6.8181658518817239,6.9470097396567985,7.5149980403028218,7.1570261032580271,7.0660644684990137,6.5791366276236642,8.6373155034863487,7.5111168465775116,6.4758400306908364,8.3971675533758816,7.8528478621074251,7.8225529696225768,7.6564450694445707,7.6688158753600826,8.1866799363624629,6.7709391260580229,7.0405238946208701,7.1675741868036189,7.6277524345830834,7.6680804935879294,7.1797026547171612,7.4250215165182301,7.5105868390613653,8.0401539479826347,6.6170689909386624,7.2719095615710527,7.1876003952846439,7.2845575586424118,7.2020467238331314,7.0918157832735575,7.2632706074081383,7.9924253353022356,7.1361034716730334,7.2354299895212737,6.7132614700544986,7.4580541580822155,7.3494192895264652,7.3404620962424953,7.8427951434975363,6.8104083131873914,6.3096116499066008,6.9626321255514885,7.3348785119760604,7.8433912912023311,6.0688033864364135,8.4972191439243243,9.34586225830496,7.1785500435016072,7.1414245713600648,7.43108545888352,7.918841978687678,7.493513195085769,7.5827109036813773,7.9750823969677525,6.336032844794012,7.4427660218339762,6.8885464822713978,6.6911772660500626,6.4234774877852745,7.6146436508712618,7.4844751003248629,6.2570086594901797,6.4403494778481383,6.5624763847005543,8.0361462464076272,6.4675253065138687,7.6188127491437392,5.9920427205240632,7.1278612331227009,7.1844735192846549,6.8971495645077141,6.7370543129972944,8.159898740350723,7.0056702967720135,7.1672994243107819,7.2162750343007378,6.0740583046539367,7.6703555840339845,6.1677212534400301,6.8285630303317637,5.6258668412698229,5.6154867078524884,7.623825692777296,8.5309824646482042,5.8787389229240263,9.1356786347791399,7.7220209170848051,5.4252362581549791,7.1025092443965638,6.6772127154617733],"hoverinfo":"y","type":"box","fillcolor":"rgba(255,255,255,0.75)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.8897637795275593,"color":"rgba(0,0,0,1)"},"size":5.6692913385826778},"line":{"color":"rgba(248,118,109,1)","width":1.8897637795275593},"name":"State-DR","legendgroup":"State-DR","showlegend":false,"xaxis":"x","yaxis":"y","mode":"","frame":null},{"x":[2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2],"y":[5.7755217890057571,6.3654278627671079,4.0802342183737439,6.1289229304665209,5.4770163164957868,6.5690422172897582,5.7877241026312856,5.8218860430463426,6.0301149237370391,6.0845892350866944,6.9399428200712325,5.8372189856388594,5.8995326240401411,5.8682068852704816,6.1344828427368236,6.429736149456871,7.2046479648082107,6.1949436510271836,6.7543660787248854,5.355480810548686],"hoverinfo":"y","type":"box","fillcolor":"rgba(255,255,255,0.75)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.8897637795275593,"color":"rgba(0,0,0,1)"},"size":5.6692913385826778},"line":{"color":"rgba(0,191,196,1)","width":1.8897637795275593},"name":"Tribal","legendgroup":"Tribal","showlegend":false,"xaxis":"x","yaxis":"y","mode":"","frame":null},{"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","mode":"","frame":null},{"x":[1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999,null,1.8500000000000001,2.1499999999999999],"y":[4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675,null,4.989934568532675,4.989934568532675],"text":"","type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(0,0,139,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":56.381901203819019,"r":10.62681610626816,"b":37.193856371938566,"l":156.214196762142},"font":{"color":"rgba(0,0,0,1)","family":"serif","size":21.253632212536321},"title":{"text":"Proportionally Scaled Thresholds","font":{"color":"rgba(0,0,0,1)","family":"serif","size":25.504358655043593},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.40000000000000002,2.6000000000000001],"tickmode":"array","ticktext":["State-DR","Tribal"],"tickvals":[1,2],"categoryorder":"array","categoryarray":["State-DR","Tribal"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":5.3134080531340802,"tickwidth":0.96607419147892382,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"serif","size":17.002905770029059},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"serif","size":21.253632212536321}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[3.7572693553614838,10.862496341631205],"tickmode":"array","ticktext":["$100,000","$10,000,000","$1,000,000,000"],"tickvals":[5,7,9],"categoryorder":"array","categoryarray":["$100,000","$10,000,000","$1,000,000,000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":5.3134080531340802,"tickwidth":0.96607419147892382,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"serif","size":17.002905770029059},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"Total Disaster Cost (log$)","font":{"color":"rgba(0,0,0,1)","family":"serif","size":21.253632212536321}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"serif","size":17.002905770029059}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"47d6535f1d57":{"x":{},"y":{},"colour":{},"text":{},"type":"scatter"},"47d697f6584":{"x":{},"y":{},"colour":{}},"47d6ae1c1fb":{"x":{},"y":{},"colour":{},"xend":{},"yend":{}},"47d66b864b51":{"x":{},"y":{},"colour":{},"xend":{},"yend":{}},"47d63dbae9fd":{"x":{},"y":{},"colour":{},"label":{}},"47d63a5800b4":{"x":{},"y":{},"colour":{},"label":{}},"47d667964758":{"x":{},"y":{},"colour":{},"label":{}},"47d63b261789":{"x":{},"y":{},"colour":{},"label":{}}},"cur_data":"47d6535f1d57","visdat":{"47d6535f1d57":["function (y) ","x"],"47d697f6584":["function (y) ","x"],"47d6ae1c1fb":["function (y) ","x"],"47d66b864b51":["function (y) ","x"],"47d63dbae9fd":["function (y) ","x"],"47d63a5800b4":["function (y) ","x"],"47d667964758":["function (y) ","x"],"47d63b261789":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

- State DRs and Tribal DRs are propotional rescalings of one another.
- Tribal incidents being around a tenth of the size of State incidents.

## \_\_\_\_\_\_\_\_\_

## PDA Data and Overall Costs

- Relationship Between PDA and Final Cost is consistent for Tribes and
  Counties.
- Tribal DRs and Non-Tribal DRs
- Similar linear relationship.

![](/images/unnamed-chunk-34-1.png)<!-- -->

[^1]: Section 2. Distribution of Tribal Declarations

[^2]: Tribal Declarations Pilot Guidance, January 2017, pg 25

[^3]: See Appendix - Previous Analysis Figure

[^4]: <a href="https://bookdown.org/mike/data_analysis/survivorship-bias.html" target="_blank">
    Survivorship Bias</a>

[^5]: See Appendix - Fig. 1
