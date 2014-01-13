Data Manipulation Exercise
========================================================

# Just making sure we still have the data


```r
siurl <- "http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_SampledLakeInformation_20091113.csv"
wqurl <- "http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_WaterQuality_20091123.csv"
luurl <- "http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_Buffer_Landuse_Metrics_20091022.csv"

si <- read.csv(siurl)
wq <- read.csv(wqurl)
lu <- read.csv(luurl)
```



# Subset data into just what we want


```r
si_subset <- si[c("SITE_ID", "VISIT_NO", "SITE_TYPE", "STATE_NAME")]
wq_subset <- wq[c("SITE_ID", "NTL", "PTL", "CHLA")]
lu_subset <- lu[c("SITE_ID", "PCT_DEVELOPED_BUFR", "PCT_FOREST_BUFR", "PCT_AGRIC_BUFR", 
    "PCT_WETLAND_BUFR")]
```


# Create a single data frame


```r
nlam1 <- merge(si_subset, wq_subset, by = "SITE_ID")
nla <- merge(nlam1, lu_subset, by = "SITE_ID")
```


# How many rows and columns in new data frame?


```r
nrow(nla)
```

```
## [1] 1590
```

```r
ncol(nla)
```

```
## [1] 11
```


# Calculate mean, median, max ... for each of the columns


```r
apply(nla[, 5:11], 2, summary)
```

```
## $NTL
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       5     312     579    1220    1180   26100 
## 
## $PTL
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0       9      27     122      99    4860 
## 
## $CHLA
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##     0.1     2.8     7.8    31.8    27.5   936.0       5 
## 
## $PCT_DEVELOPED_BUFR
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.30    3.50    7.22    8.50   78.20 
## 
## $PCT_FOREST_BUFR
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     0.0     1.7    16.0    22.7    38.0    86.9 
## 
## $PCT_AGRIC_BUFR
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.00    1.20    7.05    8.60   81.30 
## 
## $PCT_WETLAND_BUFR
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00    0.30    3.10    8.06   10.70   78.50
```


# Write data to .csv


```r
write.csv(nla, "nla.csv", row.names = FALSE)
```

