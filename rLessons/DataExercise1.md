Excercise 1 Answers
-------------------
  
#Download data into data frames

First create a character vector to hold the name of the URL


```r
siurl <- "http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_SampledLakeInformation_20091113.csv"
wqurl <- "http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_WaterQuality_20091123.csv"
luurl <- "http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_Buffer_Landuse_Metrics_20091022.csv"
```


Next create data frames, using `read.csv()` to grab the remote file


```r
si <- read.csv(siurl)
wq <- read.csv(wqurl)
lu <- read.csv(luurl)
```


#Rows and columns for water quality and land use


```r
nrow(wq)
```

```
## [1] 1326
```

```r
ncol(wq)
```

```
## [1] 158
```

```r
nrow(lu)
```

```
## [1] 1157
```

```r
ncol(lu)
```

```
## [1] 61
```


#Name and class of the first columns


```r
names(si)[1]
```

```
## [1] "SITE_ID"
```

```r
names(wq)[1]
```

```
## [1] "SITE_ID"
```

```r
names(lu)[1]
```

```
## [1] "SITE_ID"
```

```r

class(names(si)[1])
```

```
## [1] "character"
```

```r
class(names(wq)[1])
```

```
## [1] "character"
```

```r
class(names(lu)[1])
```

```
## [1] "character"
```


#Mean, min and max

I used `summary()` which gives a bit more info,  `mean()`, `min()`, and `max()` would have also worked.


```r
summary(lu["PCT_DEVELOPED_BUFR"])
```

```
##  PCT_DEVELOPED_BUFR
##  Min.   : 0.00     
##  1st Qu.: 0.30     
##  Median : 3.20     
##  Mean   : 7.13     
##  3rd Qu.: 8.50     
##  Max.   :78.20
```

```r
summary(wq["NTL"])
```

```
##       NTL       
##  Min.   :    5  
##  1st Qu.:  306  
##  Median :  572  
##  Mean   : 1192  
##  3rd Qu.: 1174  
##  Max.   :26100
```

