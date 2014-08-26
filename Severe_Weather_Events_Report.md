---
title: "Severe Weather Events Report"
output: pdf_document
---

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


##Loading required libraries
```{r}
library(ggplot2)
```

## Download the data
```{r}
fileUrl = "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
download.file(fileUrl, destfile = "..//data//StormData.zip", method = "auto")
stordDataDownloaded = date()
```

## Loading the data

```{r}
stormData = read.csv("..//data//repdata_data_StormData.csv", sep = ",")
```

###Summary of Storm Data
```{r}
str(stormData)
```

## Preprocessing the data
```{r}
fatalities = tapply(stormData$FATALITIES, stormData$EVTYPE, sum)
injuries = tapply(stormData$INJURIES, stormData$EVTYPE, sum)
propertyDamage = tapply(stormData$PROPDMG, stormData$EVTYPE, sum)
cropDamage = tapply(stormData$CROPDMG, stormData$EVTYPE, sum)

consequences  = data.frame(dimnames(fatalities), injuries, fatalities, propertyDamage)

consequences$health = consequences$injuries + consequences$fatalities
consequences$economic = consequences$propertyDamage + consequences$cropDamage
consequences$economic = propertyDamage + cropDamage

consequences[which(consequences$health == max(consequences$health)), 1]
max(consequences$health)

consequences[which(consequences$economic == max(consequences$economic)), 1]
max(consequences$economic)

```
You can also embed plots, for example:
```{r, echo=FALSE}
plot(cars)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
