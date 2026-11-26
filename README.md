# Spatial analysis using the Google Earth Engine_

## Has forest cover change occurred across protected areas?
## Data Science Hack-a-thon

### Google Earth Engine link
https://code.earthengine.google.com/


Login to find the challenge script for your group.

Link to the GEE scripts: 

- Group 1: [https://code.earthengine.google.com/ac3ddea0c65a560931f17bd9bb178136](https://code.earthengine.google.com/dccd370a77c76697c72e154966af36ab)
- Group 2: [https://code.earthengine.google.com/d34419a235b1e84f802c7007210ba170](https://code.earthengine.google.com/64cbd05e6727618e1f570d35e39f46a8)
- Group 3: https://code.earthengine.google.com/75f0b6804457fa8c00c62f67b40ea5d9
- Group 4: [https://code.earthengine.google.com/3f911d57652eb8ae74af9fb5f4a4c34d](https://code.earthengine.google.com/0aa119eaa5c359a35fb841fecf4ba929)
- Group 5: [https://code.earthengine.google.com/8e3b174404344696b6a5c1d1c2625a22](https://code.earthengine.google.com/13bb4502b7be4b12cc6ee0751d63c9b2)
- Group 6: [https://code.earthengine.google.com/13bb4502b7be4b12cc6ee0751d63c9b2](https://code.earthengine.google.com/49f49302ffd64862a86202b3410d0961)
- Group 7: https://code.earthengine.google.com/6262f6238b491f9ca8a1340f22fd0f31

# Overarching goal
_*Investigate forest cover change across protected areas.*_


# Challenge aims
- Map forest cover change from the Hansen et al. Database for your group's protected area as well for the larger region surrounding the protected area
- Create a figure in R visualising the amounts of forest cover change over time
- Report the state of forests in your group's protected area in a Markdown document


_Use this document as a template to fill in as you progress through the challenge._

# Title

Has forest cover change occurs across Yellowstone National Nark?


# Authors

Yanting Shen, Xinyi Zhang, Ziyang Liu, Jiaxuan Gong


# Research question
__How much forest cover loss and gain has occurred in your protected area?__


# Introduction

Yellowstone National Park was established in 1872 and is located in the northwestern United States, mainly in Wyoming. Forests cover approximately 80% of the park, with lodgepole pine (*Pinus contorta*) being the dominant tree species (National Park Service, n.d.).

Although Yellowstone is a protected area, its forest cover is constantly changing due to natural disturbances such as wildfires and insect outbreaks. Wildfires play an important role in forest ecosystems, as they cause tree mortality but also create favourable conditions for forest generation. Understanding these changes is therefore important for assessing forest recovery and supporting protected area management.

This study uses satellite-derived data to investigate forest cover change in Yellowstone National Park between 2000 and 2016. It aims to quantify forest cover loss and gain within the available observation periods and examine their spatial and temporal patterns.


# Workflow
<img width="1448" height="1086" alt="89fc021d665a9a843b7ed2b2acf4ffbb" src="https://github.com/user-attachments/assets/5bfc72e7-89c6-4ca5-a015-a67ae0497e5b" />




# Specific hypotheses and predictions

_*What do you know about this protected area?  What sort of factors could influence forest cover change in this part of the world?*_


# Methods

_*Describe your methods in brief including all of the datasets that you used with appropriate crediting/referencing/copyright for the datasets.*_


# Data vis and summary methods

Forest cover and change were visualised in Google Earth Engine using the Hansen Global Forest Change dataset, with the WDPA boundary identifying Yellowstone National Park. Tree cover in 2000 was shown in green, forest loss in purple, and forest gain in orange. The surrounding region was also displayed to provide spatial context.

Following the supplied script, pixel areas were multiplied by the tree-cover fraction in 2000 and converted from square metres to square kilometres. These values were summed within the park boundary, producing baseline-tree-cover-weighted area estimates. Loss was grouped by its recorded year from 2001 to 2016. Gain represents the cumulative period 2000–2012 and cannot be interpreted as annual gain. The analysis therefore focused on descriptive spatial patterns and area summaries, rather than statistical hypothesis testing.

```r
Your code
```

# 1. Maps of forest cover change for your protected area

Figure shows an uneven spatial distribution of forest loss and gain across Yellowstone National Park. Purple loss patches are visible in the western part of the park, with several distinct patches also occurring in central and eastern areas. Orange gain patches are particularly apparent in western areas and parts of the southern half of the park. Both types of change are also visible outside the park boundary.

These patterns indicate that forest change varied across the study area. However, the map alone does not establish the causes of these changes or whether the park experienced a net increase or decrease in forest cover. In particular, the loss and gain layers cover different time periods.

Figure 1. Forest cover and change in Yellowstone National Park and the surrounding region, visualised in Google Earth Engine using the Hansen Global Forest Change dataset. Green represents tree cover in 2000, purple indicates forest loss during 2001–2016, and orange indicates forest gain during 2000–2012. The red polygon shows the WDPA protected-area extent; red shading does not represent forest change.

_<img width="261" height="274" alt="Screenshot 2026-11-26 at 14 44 11" src="https://github.com/user-attachments/assets/4b5a8359-d2b0-4193-9c8b-c71d174fd2da" />
*_


# 2. Visualisation of the amount of forest cover loss and gain for your protected area
(you can use `R`)

__*Your text and/or figures.*__


# 3. How do your results compare with your predictions? What do you think might explain the patterns you found?

_*Use your scientific and critical thinking skills to provide a conclusion summary statement of how land cover is changing across the protected area that you studied.*_

__*Your text and/or figures.*__


# 4. What other datasets, available within the GEE, could you use to test the potential drivers of forest cover change in your group's protected area that you identified in point #3. ?

_*Browse through the GEE Data catalogue and provide a link to your chosen dataset with a brief statement about why you chose it. Note you don't need to do analyses with this new dataset, you are just making a suggestion for future studies.*_

__*Link and description of your chosen dataset*__

_*BONUS! If you have the time, for some bonus recognition, map your chosen dataset in the GEE and include a visualisation of the dataset for your protected area (screencap from GEE).*_


# 5. What research question and hypotheses would you test with those additional datasets in your proposed future research and why does that research matter for the park management?

_*Add your answer here.*_


# Conclusions

_*Summarise what you have found in this challenge and its broader implications.*_
