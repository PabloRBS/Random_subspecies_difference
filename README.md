---
title: "Random Forest analysis: measuring the differentiation between two bird subspecies according to their vocalizations"
author: "Pablo Bolanos"
date: '2018-12-01'
output: html_document
---

**Access to publication: https://doi.org/10.1080/09524622.2019.1647877**

**This analysis uses measurements of vocalizations of the species *Pharomachrus mocinno* (Resplendent Quetzal)**
  
**ABSTRACT**
<br />
The Resplendent Quetzal *Pharomachrus mocinno* is a rare Neotropical bird included in the IUCN red list as Near Threatened. Fragmentation of its habitat, the cloud forest, is considered as the principal threat. Two subspecies are currently recognised but genetic and morphometric studies suggested they could be considered as full species. We assessed whether male vocalisation would support a species delimita- tion hypothesis. We recorded in the field and downloaded from sound archives vocalisation of 57 individuals from 30 different localities dis- tributed in 11 countries. We estimated the acoustic differences of all the Pharomachrus taxa with multivariate analyses and machine learn- ing techniques. Our results show vocal differences between *P. m. mocinno* and *P. m. costaricensis* that  could have amolecular basis, potentially due to genetic drift developed during the more than three million years of separation of *P. m. mocinno* (from Mexico to Nicaragua) and P. m. *costaricensis* (Costa Rica and Panama). We therefore suggest that P. mocinno could potentially be divided into two species. A possible separation of these taxa into two species could have important consequences for the conservation status of the Resplendent Quetzals, and redirect conservation efforts for these taxa.
<br />
<br />
**DATA**
<br />
In total, a 57 individuals by 22 variables matrix was obtained (one temporal parameter, seven frequency parameters, two FM parameters per note, one temporal parameter between notes, and one temporal parameter between syllables).
<br />
As the number of notes found for each individual varied from 12 to 639, a random subsample of 40 notes was applied for
the individuals that produced more than 40 notes to ensure balanced datasets. A total of 1738 notes were analysed.
<br />
To test how the 22 acoustic features could classify correctly *P. m. mocinno*,  *P. m. costaricensis* and the closely related species, two supervised classification methods where applied, used in machine learning, namely a multiclass linear discriminant analysis (LDA) for the subspecies comparison (Fisher 1936) and a balanced random forest analysis (RF) (Breiman 2001) including the subspecies and the other *Pharomachrus* species. This example shows the classification using the second methodology.


```{r include = TRUE, message=FALSE, warning=FALSE, comment=NA}
library(randomForest)
```

```{r include = FALSE}
load("QuetzaldT4020180219.RData")
```

**Data used in the analysis:**

```{r include = TRUE, message=FALSE, warning=FALSE, comment=NA}
head(Quetzal_d)
```



```{r include = FALSE, message=FALSE, warning=FALSE, comment=NA}
fitS<- randomForest(formula = ssp ~ Low.Freq..Hz.+High.Freq..Hz.+Peak.Freq..Hz.+IQR.BW..Hz.+Q1.Freq..Hz.+Center.Freq..Hz.+Q3.Freq..Hz.+Positive.FM+duration+absNegative.FM, data=Quetzal_d, ntree=4000) # sampsize=c(72,72,72,72,72,72))
varImpPlot(fitS)
```

The results of the Random Forest classification can be observed in a confussion matrix, where is showed the
quantity of vocalizations classified in each subspecies and the other *Pharomachrus* species included in the analysis. N corresponds to the *P. m. mocinno* and S to *P. m. costaricensis*. The other four categories are the other species of the genus


```{r include = TRUE}
fitS$confusion
```

```{r include = TRUE}
importanceSorted <-sort(fitS$importance)
```

The importance of the variables tested to classify the subspecies can be visualized with the mean decrease Gini graphic, being more important the higher values.

```{r, echo=TRUE}
par(mfrow=c(1,1), mar=c(5, 4, 2, 2))
dotchart(importanceSorted, xlab="Predictor accuracy", labels= c("Negative FM","Inter quartile range","Total FM", "Positive FM", "High frequency", "Peak frequency", "Low frequency", "Duration", "First quartile range", "Median frequency"), pch=19, cex=1.2, las=2) #type = 2
```


![alt text](https://github.com/PabloRBS/Random_subspecies_difference/blob/main/RF_quetzal_plot.png?raw=true)


