---
title: "2024-08-28 Eternal returns of bad boxplots"
author: "CTA"
date: "2024-08-28"
always_allow_html: true
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
![](/images/beetles_pinned.png)

## The violin plot

Yes, violin plots. I know you're familiar. See the shape of the distribution while also giving the traditional vibe of box and whisker. I'm not opposed to them except for the self-inflicted trauma that will be unveiled. 
Advantages 
- Clean look for large data.
- More intuitive view of the distribution 'normality'.
Disadvantages
- Still 'obscures' sample/size and data.
- Reflected distributions may cause confusion.

## Jitter, scatter... 
Skip, slide and spin! Plot all your points with a random adjustment on the orthogonal axis and label outliers in colors. If you are an R user, ggplot() makes this a breeze. 

![](/images/pure_jitter_size.png)


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
![](/images/box-jitter.png)
*I'd bet these differences aren't significant.*

![](/images/box-shadow-jitter.png)
*Large sample size, 'decluttered' with low alpha level.*

![](/images/box-jitter-alpha.png)
*Clear box, heavily implies unequal sample sizes across the IQR.*

![](/images/multi-box-jitter-alpha.png)
*A bit complex, attempt to emphasize median differences with solid boxes over colored backgrounds.*

## Violin plus+
This is where it all went wrong. I wish the lesson was "Don't let others circulate your charts and slides without supervision" but it was a bad plot to begin with. I combined the violin plot with a few other elements, thinking it would show the distribution, the samples... AND the change between groups. 

The chart ended up being called the "Bug Splat" chart, and I can see it now, cold and precisely pinned beetles on a peg board. 

You may point out that my true sin was violating Tufte (and other's) rules on 3+ perceptual elements, position, shape, color, size - oh my!  Which is also quite true. 

![](/images/RFI_Dist_Example.png)

## Conclusions...
... are for those with significant data or publishing quotas, which I've given up on, so I'll just leave you with the visuals. Code attached for those interested. Be careful out there. 
