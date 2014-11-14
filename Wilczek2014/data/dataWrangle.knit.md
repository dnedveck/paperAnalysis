---
title: "Data Wrangle"
output: html_document
---


```r
knitr::opts_chunk$set(tidy= FALSE, fig.height= 4)
library(dplyr)
library(reshape2)
```


Wrangling the data from the paper, as well as in the supplementary material, into a more R and data analysis friendly format.

## Supplementary Data Table 1

>"A. thaliana accessions, geographic location of origin, and field-measured fitness for each experimental planting"

Downloaded `pnas.1406314111.sd01.csv` from the website. Realized the encoding was not UTF-8, and looks like it was saved on a Mac. Converted to UTF, and changed endline characters to LF ("\n"), saved as `sd01.csv`. After importing to R it looks like all the accented/diaresis charcters were lost. At least the identifiers are still unique (and still not uniform, bah).


```r
setwd("~/Dropbox/gradschool/paperAnalysis/Wilczek2014/data/")
sd01 <- read.csv("sd01.csv")
```

Things to do with this data set

- clean up the names, and reduce when neccesary
- convert from "wide" to "long" 
- make new df with only the columns I care about


### cleaning up names


```r
names(sd01)[1] <- "Accession" # this is just a modifier
names(sd01)[3] <- "Lat" # Latitude of accession origin
names(sd01)[4] <- "Lon" # Longitude of accession origin
names(sd01)[14] <- "Year.Collected.TAIR" # fixed a space issue
```

### converting to long format

First, making a smaller df to `melt`. The only things I care about are Accession, Lat, Lon, Likely.Year.Collected, and the Fitness variables. The last two fitness variables were from 2007, so I'm going to split those off and melt them separately. The majority of the Fitness variables were collected in 2006


```r
keepCols <- c(1, 3:4, 15, 5:10)
fitness.df <- sd01[keepCols]
# renaming "Likely.Year.Collected" for simplicity
names(fitness.df)[4] <- "YearCollected"

keep2007 <- c(1,3:4,15,11:12)
fitness2007.df <- sd01[keep2007]
names(fitness2007.df)[4] <- "YearCollected"
```

melting

```r
fitnessLong.df <- melt(fitness.df, 
              id.vars=c("Accession", "Lat", "Lon", "YearCollected"))
fitness2007Long.df <- melt(fitness2007.df, 
              id.vars=c("Accession", "Lat", "Lon", "YearCollected"))
```

Now I need to separate the variable column into Location and Season, and rename value to Fitness. Then add in the year planted (have to differentiation from year, when the plant was collected)


```r
# renaming value to Fitness
names(fitnessLong.df)[6] <- "Fitness"
names(fitness2007Long.df)[6] <- "Fitness"



# gsub to the rescue
fitnessLong.df$Location <- gsub(pattern = "\\w+\\.in\\.(.+)\\.\\w+",
                                replacement = "\\1",
                                x = as.character(fitnessLong.df$variable))
fitnessLong.df$Season <- gsub(pattern = "\\w+\\.in\\..+\\.(\\w+)",
                                replacement = "\\1",
                                x = as.character(fitnessLong.df$variable))

fitness2007Long.df$Location <- gsub(pattern = "\\w+\\.in\\.(.+)\\.\\w+",
                                replacement = "\\1",
                                x = as.character(fitness2007Long.df$variable))
fitness2007Long.df$Season <- gsub(pattern = "\\w+\\.in\\..+\\.(\\w+)",
                                replacement = "\\1",
                                x = as.character(fitness2007Long.df$variable))

# add in YearPlanted
fitnessLong.df$YearPlanted <- 2006
fitness2007Long.df$YearPlanted <- 2007

# remove the variable columns
fitnessLong.df$variable <- NULL
fitness2007Long.df$variable <- NULL

# and rbind them together
fitnessAll.df <- rbind(fitnessLong.df, fitness2007Long.df)
```

### Saving files
Making both a csv and an Robject



