---
title: "2024-08-28 Eternal returns of bad boxplots"
author: "CTA"
date: "2024-08-28"
always_allow_html: true
image: /images/beetles_pinned.png
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../projects/_posts") })
description: "Work from a FEMA short suspense analysis."
layout: post
categories:
  - R Markdown
  - Jekyll        
---

It's become legend in my analysis group that leadership (maybe one particular leader) **hates  seeing boxplots** (box and whisker to be specific). Though I'm not sure why, I did hear rationale from one lead in particular that it could be because they obscure data - in my interpretation meaning, you could have a boxplot that looks reasonable with 20 observations, and another that looks terrible with 5000 observations.  A lot of statisticians might guess that if you keep collection observations in the first group.... it may begin to resemble the latter. 

In my free time I'll tread on sacred cows and tip over sanctified grounds, but professionally I skew closer to the hump of "corrected-ness". So I've come up with a few *definitely not a boxplot* alternatives that have been well received, and one terrible one that haunted me for several months of persistent email chains "what is going on in this chart?", "does this signal the end times", "it's twisted my brain and I can't stop seeing dead bugs". 


## The violin plot

Yes, violin plots, the suggested solution to the boxplot averse. I know you're familiar. See the shape of the distribution while also giving the traditional vibe of box and whisker. I'm not opposed to them in certain cases, but if not used selectively you too can except a self-inflicted email cavalcade when including such charts (even in an appendix)!
Advantages 
- Clean look for large data.
- More intuitive view of the distribution 'normality'.
Disadvantages
- Still 'obscures' sample/size and data, particularly if you can't control kernel smoothing.
- Reflected distributions may cause confusion.
- *Opinion* VERTICAL distributions are less prevalent than horizontal ones. Reflected distributions are even more rare. 

![](/images/RFI_Dist_Example.png)
*You may point out that my true sin was violating Tufte (and other's) rules on 3+ perceptual elements, position, shape, color, size - oh my! Quite so.*

This is where it all went wrong. I wish the lesson was "Don't let others circulate your charts and slides without supervision" but it was a bad plot to begin with:
- The multiple groups are hard to distinguish, since they are actually pairs showing *change* in process time of two groups with an added validation step.
- The vertical lines are displaying time (which I think is usually perceived better horizontally) and the kernal smoothing of this high sample size ( *n* > 2000) groupings.   - The 'paired' dark-light color choices are intended to link the groups but splitting them across the x-axis is already confusing. 

The chart ended up being called the "Bug Splat" chart, and it is only to clear in restrospect, the analgous cold and precisely pinned beetles on a peg board.

## Jitter, scatter... 
Skip, slide and spin! Plot all your points with a random adjustment on the orthogonal axis and label outliers in colors. If you are an R user, ggplot() makes this a breeze. 
I really think this fixes non-intuitive strange qualituy of vertical distribtions.
![](/images/pure_jitter_size.jpeg)

Advantages
- At last, I can see sample sizes.
- Easy to add color and size elements that enhance intuitive interpretation. 
Disadvantages
- Can get very busy.
- Can lead to weird interpretations - maybe someone things its a regression plot for some reason. 
- For low observation groups, this can look strange. 


## Jitter + Box 
- At last.. the answer? Pick freely from the advantage and disadvantages lists above.
- This is my personal favorite and it usually receives a good reception, especially if you find the right blend of **opacity** and **color selection**.
- The visual perceptions are close analogues to grouped based statistical tests that I often use for validation. 
![](/images/box_jitter.png)
*I'd bet these differences aren't significant.*

![](/images/box-shadow-jitter.png)
*Large sample size, 'decluttered' with low alpha level.*

![](/images/box_jitter-alpha.png)
*Clear box, heavily implies unequal sample sizes across the IQR.*

![](/images/multi-box-jitter-alpha.png)
*A bit complex, attempt to emphasize median differences with solid boxes over colored backgrounds.*

## Conclusions...
... are for those with significant data or publishing quotas, which I've given up on, so I'll just leave you with the visuals. Code attached for those interested. Be careful out there. 
