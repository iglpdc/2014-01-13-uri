Function Exercise
========================================================

# Write a function to return the mean of a vector


```r
#' Function to return mean of a vector
#'
#' @param inVec this is the input vector
#'
#' @return A numeric value representing the mean of the input
#'         vector is returned

myMean <- function(inVec) {
    return(mean(inVec))
}

test <- 1:10
myMean(test)
```

```
## [1] 5.5
```


# Add to that function to deal with NA


```r
#' Function to return mean of a vector
#'
#' @param inVec this is the input vector
#' @param ... Allows passage of al options to mean()
#'
#' @return A numeric value representing the mean of the input
#'         vector is returned

myMean <- function(inVec, ...) {
    return(mean(inVec, ...))
}

test <- c(test, NA, test)
myMean(test)
```

```
## [1] NA
```

```r
myMean(test, na.rm = TRUE)
```

```
## [1] 5.5
```


# Allow calc of mean and median


```r
#' Function to return mean of a vector
#'
#' @param inVec this is the input vector
#' @param stat Character string indicating which stat to calculate
#' @param ... Allows passage of al options to mean()
#'
#' @return A numeric value representing the mean of the input
#'         vector is returned

myMean <- function(inVec, stat = c("mean", "sd"), ...) {
    if (stat == "mean") {
        return(mean(inVec, ...))
    } else {
        return(sd(inVec, ...))
    }
}

myMean(test, "mean", na.rm = TRUE)
```

```
## [1] 5.5
```

```r
myMean(test, "sd", na.rm = TRUE)
```

```
## [1] 2.947
```



