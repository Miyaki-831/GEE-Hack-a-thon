# Title
Spatial and Temporal Patterns of Forest Cover Change in Yellowstone National Park (2000–2016)

# Authors
Yanting Shen, Xinyi Zhang, Ziyang Liu, Jiaxuan Gong

# Research question
How much forest cover loss and gain occurred in Yellowstone National Park during the study period, and how did forest cover change vary across space and time?

# Introduction
Yellowstone National Park was established in 1872 and is located in the northwestern United States, mainly in Wyoming. Forests cover approximately 80% of the park, with lodgepole pine (*Pinus contorta*) being the dominant tree species (National Park Service, n.d.).

Although Yellowstone is a protected area, its forest cover is constantly changing due to natural disturbances such as wildfires and insect outbreaks. Wildfires play an important role in forest ecosystems, as they cause tree mortality but also create favourable conditions for forest generation. Understanding these changes is therefore important for assessing forest recovery and supporting protected area management.

This study uses satellite-derived data to investigate forest cover change in Yellowstone National Park between 2000 and 2016. It aims to quantify forest cover loss and gain within the available observation periods and examine their spatial and temporal patterns.

# Workflow
<img width="1448" height="1086" alt="89fc021d665a9a843b7ed2b2acf4ffbb" src="https://github.com/user-attachments/assets/5bfc72e7-89c6-4ca5-a015-a67ae0497e5b" />

# Specific hypotheses and predictions
Natural disturbances, particularly wildfires and insect outbreaks, are important drivers of forest cover change in Yellowstone National Park. However, fire also supports the regeneration of some tree species, particularly lodgepole pine (National Park Service, n.d.).

**Hypothesis 1: Forest cover loss is spatially uneven.**

Prediction: Forest loss will occur in large clusters rather than being evenly distributed across the park due to the localised nature of disturbances.

**Hypothesis 2: Forest cover loss varies between years.**

Prediction: Annual forest loss will show considerable fluctuations, with peaks potentially associated with major disturbance events.

**Hypothesis 3: Natural regeneration contributes to forest cover gain.**

Prediction: Forest cover gain will occur in some previously disturbed areas due to natural forest recovery.'

# Methods
This study uses Google Earth Engine (GEE) to analyse forest cover change in Yellowstone National Park. Two datasets are used:

- **Hansen Global Forest Change v1.13:** A Landsat-derived dataset with a spatial resolution of approximately 30 m, providing information on tree cover, forest loss and forest gain (Hansen et al., 2013).
- **World Database on Protected Areas (WDPA):** A global dataset containing the boundaries and associated information of protected areas (UNEP-WCMC and IUCN, 2026).

The WDPA dataset is filtered to extract the boundary of Yellowstone National Park. Four bands are selected from the Hansen dataset: `treecover2000`, `loss`, `gain` and `lossyear`.

The original tree cover percentage is converted into a proportion and multiplied by pixel area to estimate tree cover area in 2000. Forest loss and gain areas are calculated by multiplying the corresponding binary values by pixel area. All area measurements are converted from m² to km².

The analysis calculates annual forest loss between 2001 and 2016. However, the gain band only provides cumulative forest gain between 2000 and 2012. Therefore, loss and gain are compared over the same observation period where appropriate.

Forest cover, loss and gain are visualised within Yellowstone National Park and its surrounding region. The calculated area values are exported as a CSV file for further analysis and visualisation in R.

# Data vis and summary methods

Forest cover and change were visualised in Google Earth Engine. Tree cover in 2000 was displayed in green, forest loss in purple, and forest gain in orange. The Yellowstone protected-area boundary was overlaid in red to show the location of changes within and around the park.

Following the supplied script, change indicators were multiplied by pixel area and the tree-cover fraction in 2000, then converted to square kilometres. The resulting values represent areas weighted by baseline tree cover. Summation reducers were used to summarise these values within the park. The loss-year band was used to group loss by year from 2001 to 2016, while gain represents cumulative change during 2000–2012.
```r
// Visualise tree cover and forest change.
// treeCover has already been divided by 100.
Map.addLayer(
  treeCover.updateMask(treeCover),
  {palette: ['#009900', '#99ff99'], min: 0, max: 1},
  'Tree cover in 2000'
);

Map.addLayer(
  loss.updateMask(loss),
  {palette: ['#9900ff']},
  'Forest loss: 2001–2016'
);

Map.addLayer(
  gain.updateMask(gain),
  {palette: ['#ffae00']},
  'Forest gain: 2000–2012'
);

// Calculate loss area weighted by tree cover in 2000.
var areaLoss = loss.gt(0)
  .multiply(ee.Image.pixelArea())
  .multiply(treeCover)
  .divide(1000000)
  .rename('arealoss');

// Sum the weighted loss area within each park feature.
var statsLoss = areaLoss.reduceRegions({
  collection: parks,
  reducer: ee.Reducer.sum(),
  scale: scale
});
```

# 1. Maps of forest cover change for your protected area

Figure shows an uneven spatial distribution of forest loss and gain across Yellowstone National Park. Purple loss patches are visible in the western part of the park, with several distinct patches also occurring in central and eastern areas. Orange gain patches are particularly apparent in western areas and parts of the southern half of the park. Both types of change are also visible outside the park boundary.

These patterns indicate that forest change varied across the study area. However, the map alone does not establish the causes of these changes or whether the park experienced a net increase or decrease in forest cover. In particular, the loss and gain layers cover different time periods.

Figure 1. Forest cover and change in Yellowstone National Park and the surrounding region, visualised in Google Earth Engine using the Hansen Global Forest Change dataset. Green represents tree cover in 2000, purple indicates forest loss during 2001–2016, and orange indicates forest gain during 2000–2012. The red polygon shows the WDPA protected-area extent; red shading does not represent forest change.

_<img width="261" height="274" alt="Screenshot 2026-11-26 at 14 44 11" src="https://github.com/user-attachments/assets/4b5a8359-d2b0-4193-9c8b-c71d174fd2da" />
*_


# 2. Visualisation of the amount of forest cover loss and gain for your protected area
<img width="1800" height="1260" alt="Yellow_stone_forest_change" src="https://github.com/user-attachments/assets/84f7e70e-ded5-4155-813f-2cad7ff0b111" />

Figure 1. Annual forest loss and gain in Yellowstone National Park from 2001 to 2016, expressed as a percentage of the total park area. Red bars represent forest loss, while orange bars represent forest gain.


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
