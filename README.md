# Title
Spatial and Temporal Patterns of Forest Cover Change in Yellowstone National Park (2001–2016) 

# Authors
Jiaxuan Gong, Xinyi Zhang, Yanting Shen, Ziyang Liu

# Research question
How much forest cover loss and gain occurred in Yellowstone National Park during the study period, and how did forest cover change vary across space and time?

# Introduction
Yellowstone National Park was established in 1872 and is located in the northwestern United States, mainly in Wyoming. Forests cover approximately 80% of the park, with lodgepole pine (*Pinus contorta*) being the dominant tree species (National Park Service, n.d.).

Although Yellowstone is a protected area, its forest cover is constantly changing due to natural disturbances such as wildfires and insect outbreaks. Wildfires play an important role in forest ecosystems, as they cause tree mortality but also create favourable conditions for forest generation. Understanding these changes is therefore important for assessing forest recovery and supporting protected area management.

This study uses satellite-derived data to investigate forest cover change in Yellowstone National Park between 2001 and 2016. It aims to quantify forest cover loss and gain within the available observation periods and examine their spatial and temporal patterns.

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

_<img width="261" height="274" alt="Screenshot 2026-11-26 at 14 44 11" src="https://github.com/user-attachments/assets/4b5a8359-d2b0-4193-9c8b-c71d174fd2da" />
Figure 1. Forest cover and change in Yellowstone National Park and the surrounding region, visualised in Google Earth Engine using the Hansen Global Forest Change dataset. Green represents tree cover in 2000, purple indicates forest loss during 2001–2016, and orange indicates forest gain during 2000–2012. The red polygon shows the WDPA protected-area extent; red shading does not represent forest change.
*_


# 2. Visualisation of the amount of forest cover loss and gain for your protected area
<img width="1800" height="1260" alt="Yellow_stone_forest_change" src="https://github.com/user-attachments/assets/84f7e70e-ded5-4155-813f-2cad7ff0b111" />

Figure 2. Annual forest loss and gain in Yellowstone National Park from 2001 to 2016, expressed as a percentage of the total park area. Red bars represent forest loss, while orange bars represent forest gain.


# 3. How do your results compare with your predictions? What do you think might explain the patterns you found?

_*Use your scientific and critical thinking skills to provide a conclusion summary statement of how land cover is changing across the protected area that you studied.*_

__*Your text and/or figures.*__


# 4. What other datasets, available within the GEE, could you use to test the potential drivers of forest cover change in your group's protected area that you identified in point #3. ?
The MODIS Burned Area Monthly Global 500m dataset (MCD64A1.061) provides monthly information on burned areas at a spatial resolution of 500 m.

Wildfires are an important natural disturbance in Yellowstone National Park. Comparing the location and timing of burned areas with forest loss could help determine whether wildfires are associated with the observed forest cover changes.

However, the MODIS dataset has a lower spatial resolution than the Hansen dataset, which should be considered when comparing the two datasets.

Dataset link:
https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MCD64A1

# 5. What research question and hypotheses would you test with those additional datasets in your proposed future research and why does that research matter for the park management?
**Research question:**

To what extent are wildfires associated with forest cover loss in Yellowstone National Park?

**Hypothesis:**

Wildfires are an important driver of forest cover loss in Yellowstone National Park.

**Predictions:**

- Forest loss will be greater in burned areas than in unburned areas with comparable initial forest cover.
- Years with larger burned areas will generally show higher levels of forest loss.

**Relevance to park management:**

Understanding the relationship between wildfires and forest cover loss can help park managers identify areas that require further monitoring. However, fire is also a natural ecological process that supports forest regeneration in Yellowstone. This research could therefore help managers distinguish natural forest dynamics from changes that may require management intervention.

# Conclusions

_*Summarise what you have found in this challenge and its broader implications.*_

## References

Hansen, M.C. et al. (2013) ‘High-resolution global maps of 21st-century forest cover change’, *Science*, 342(6160), pp. 850–853. https://doi.org/10.1126/science.1244693.

Hansen/UMD/Google/USGS/NASA (2026) *Hansen Global Forest Change v1.13 (2000–2025)*. Available at: https://developers.google.com/earth-engine/datasets/catalog/UMD_hansen_global_forest_change_2025_v1_13.

UNEP-WCMC and IUCN (2026) *Protected Planet: The World Database on Protected Areas (WDPA)*. Cambridge, UK: UNEP-WCMC and IUCN. Available at: https://www.protectedplanet.net/.

National Park Service (n.d.) *Forests – Yellowstone National Park*. Available at: https://www.nps.gov/yell/learn/nature/forests.htm.

National Park Service (n.d.) *Ecological Consequences of Fire – Yellowstone National Park*. Available at: https://www.nps.gov/yell/learn/nature/ecological-consequences-of-fire.htm.

National Park Service (n.d.) *Park Facts – Yellowstone National Park*. Available at: https://www.nps.gov/yell/planyourvisit/parkfacts.htm.

NASA (n.d.) *MCD64A1.061 MODIS Burned Area Monthly Global 500m*. Available at: https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MCD64A1.

Our Coding Club (n.d.) *Google Earth Engine Tutorial*. Available at: https://ourcodingclub.github.io/tutorials/earth-engine/.
