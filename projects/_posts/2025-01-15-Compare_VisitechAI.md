---
title: "Compare Viz to Visitech.ai"
date: "2025-01-15"
always_allow_html: true
image: /images/Annual-Average.png
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../projects/_posts") })
description: "Prompted from an interesting conversation."
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

### Reading in Data on weather.

    ## [1] 96317    29

First Initial Plot ![](/images/unnamed-chunk-1-1.png)<!-- --> Converting
to Farenheit manually ![](/images/unnamed-chunk-2-1.png)<!-- -->

### And a quick comparison to the image created by Visitech.ai

![](/images/Annual-Average.png)
