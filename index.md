---
title       : Shiny App on US International Students Trend
subtitle    : 
author      : Eugenia
job         : 
framework   : io2012        # {io2012, html5slides, revealjs, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      #  zenburn
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- 

## Motivation and Overview

In 2013/14, international students contributed over **$27 billion** to the US economy and the growth is only getting stronger. 

The understanding of where these students come from and how it has evolved over time will give us insights to where to attract international students in the future.

The Shiny app (available [here](https://eugeniax.shinyapps.io/USintlStu) ) is built to

- visualize the geographical distributions of the international students studying in US

- track the change over the period of 2001-2014 of the top places of origin 

--- .class #id 

## Obtaining and Cleaning Data 

The data was obtained from the Open Door project website <http://iie.org/Research-and-Publications/Open-Doors/Data> 

In specific, information on top 25 places of origin from 2001 to 2014 was extracted. 

Data tables were scrubbed from the website manually due to the inconsistency of the data presentation and format over different time period, though we wishes to code the process for better reproducibility. The raw datasets (in `csv` files) are archived on [GitHub] (https://github.com/eugeniax/USintlStu) for each retrieval.

Country names were first standardized using regex.

---

## Mapping out the Top Places of Origin

Google geochart is chosen, for its visual appeal and ease of use, to visualize the geographical locations of the top places of origin. We used `gvisGeoChart` from `googleVis` package for the map.


```r
gvisGeoChart(myData,locationvar="Country", colorvar="Students",
    options=list(region="world", displayMode="regions", 
            resolution="countries", width=800, height=600,
            colorAxis="{colors:['#feb24c', '#fc4e2a', '#b10026']}"
))
```

Users can move the slider to pick a year and a choropleth map will be rendered to show the distribution of international students based on their places of origin. 

The slider is animated. With the play button, the viewer can see the change over time.

---

## Plotting the Trend of Top Places of Origin over Time

To show how number of students from each country has changed over time, line chart is plotted using `gvisLineChart` from the same `googleVis` package.


```r
gvisLineChart(mylines, options=list(width=800, height=800))
```

Google line chart expects data in the wide format (in contrast to tidy data as defined by Hadley Wickham). The dataset was transformed using `melt` and `dcast` functions from `reshape2` package.


```r
linedata <- melt(linedata, id.var=c("Year", "Country"))
linedata <- dcast(linedata, Year ~ Country, value.var="value")
```

Users can tick the checkboxes to select the countries and the corresponding time line will be plotted on the line chart. 

The list of countries is dynamicly generated and corresponds to `names(linedata)` so any future change in the dataset will be automatically updated. 

