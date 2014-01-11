Data Visualization in R
========================================================

# Lesson Index
- [Base](#base-grahics-in-R)
- [ggplot2](#ggplot2)
- [R Data Viz Examples](#r-data-viz-examples)

Although visualizing data isn't programming, per se, it is quite often the end result of much of your programming efforts.  Additionally, if you write functions to create your visualizations then recreating them, editting them, and sharing them becomes trivial.  

R has multiple options available for creating visualizations and include `base` graphics, `lattice`, and `ggplot2`.  The great thing about these tools are that they are infinetely customizable.  The bad thing about these tools are that they are infinetely customizable.  As such their can be a significant learning curve, especially for `base`.  `lattice` has a less steep learning curve (debatable), but is not as widely used. `ggplot2` is a more recent development, is seeing very wide use, and is easier to learn, especially for more complex visualizations.

For this lesson the focus is going to be on `base` (25%) and `ggplot2` (75%)

# Base Graphics in R

Base graphics have been a part of R since its inception and can provide very good visualizations, but in order to do this it requires climbing a very steep learning curve.  The creation of packges like `ggplot2` argue against spending too much time on `base`.  That being said simple data vizualisations that are part of an exploratory data analysis are very simple with `base` and `base` graphics are often used in example.  Hence the reason I like to introduce them.

We will show the default use of `plot()`, `pairs()`, `boxplot()` ,`hist()`, and `barplot()`.

First let's create a fake dataset to use


```r
set.seed(45)
x <- c(c(1:40) + rnorm(40, sd = 10), c(41:60) + rnorm(20, sd = 10), c(61:100) + 
    rnorm(40, sd = 10))
xf <- c(rep("low", 50), rep("high", 50))
y <- x + rnorm(100, sd = 2) + 10
z <- x + rnorm(100, sd = 10) + 100
df <- data.frame(xf, x, y, z)
```


Now, let's do a simple scatter plot


```r
plot(df[["x"]], df[["y"]])
# Now with some labels and a title
plot(df$x, df$y, main = "My First Scatterplot", xlab = "The x", ylab = "The y")
```


Now, how about a multivariate plot


```r
pairs(df)
```


Boxplots


```r
boxplot(df)
# Not very interesting since these all have the same distribution Can use
# factors and formula calls (haven't go on over these) To get more inf
with(df, boxplot(x ~ xf))
title("High/Low Boxplot")
```


Histograms


```r
hist(df[, 2])
# Kinda Blocky, let's add more bins
hist(df[["x"]], 20)
```


Barplots


```r
barplot(df[, 2])
# Ok, not very informative, lets try it
barplot(sapply(df[, -1], mean))
# Oh yeah, same distribution so lets fake it
barplot(sapply(df[, -1], mean) + c(0, 10, 50))
```


# ggplot2

So, now that we've seen some basic plots with `base`, it is obvious that the stock look of these plots is adequate, but they are very simple plots and may not be as flashy or publication ready as you'd like.  In part, `ggplot2` was created to ease the creation of more complex figures.

A lot has been written and discussed about `ggplot2`.  In particular see [here](http://ggplot2.org/), [here](http://docs.ggplot2.org/current/) and [here](https://github.com/karthikram/ggplot-lecture).  The gist of all this, is that `ggplot2` is an implementation of something known as the "grammar of graphics."  This separates the basic components of a graphic into distinct parts (e.g. like the parts of speach in a sentence).  You add these parts together and get a figure.

Before we start developing some graphics, we need to do a bit of package maintenance as `ggplot2` is not installed by default.


```r
# Install it if not already done so
if (!"ggplot2" %in% installed.packages()) {
    install.packages("ggplot2")
}
library(ggplot2)
```


First we create our ggplot object.  Everything we do will build off of this

**Note: Programmers rely on API documentation. The `ggplot2` equivalent of that is the [documentation](http://docs.ggplot2.org/current/).  I will try to remember to search through that, as if I were building a plot from scratch.  I work this way (editor in one monitor, API documentaion in another)**


```r
# aes() are the 'aesthetics' info.  When you simply add the x and y that can
# seem a bit of a confusing term.  You also use aes() to change color,
# shape, size etc.
ggDf <- ggplot(df, aes(x = x, y = z))
```


So if we want to simply plot a scatterplot we can add it to the ggplot object.  These are known as the geometries.


```r
# Different syntax than you are used to
ggDf + geom_point()
```


Not appreciably better than base, in my opinion.  But what if we want to add some stuff...

First a title and some axes labels.  These are part of scale.


```r
ggDf + labs(title = "My First ggplot2 Figure", x = "My Random X", y = "My Random Y") + 
    geom_point()
```


Now to add some colors, shapes etc to the point.  Look at the geom_point() documentation for this.


```r
ggDf + labs(title = "My First ggplot2 Figure", x = "My Random X", y = "My Random Y") + 
    geom_point(aes(color = xf, shape = xf), size = 5)
```


Much easier that using base.  Now `ggplot2` really shines when you want to add stats (regression lines, intervals, etc.). 

Lets add a loess line with 95% confidence intervals


```r
ggDf + labs(title = "My First ggplot2 Figure", x = "My Random X", y = "My Random Y") + 
    geom_point(aes(color = xf, shape = xf), size = 5) + geom_smooth()
```


Try that in `base` with four lines of code!

So a few other types of plots can be easily built with the same ggplot object.

boxplots


```r
ggplot(df, aes(factor(xf), x)) + geom_boxplot()
```


Histogram


```r
ggplot(df, aes(x = x)) + geom_histogram(aes(fill = xf))
```


Barplots (FIX ME)


```r
# I am not up-to-speed on reshape2 or plyr.  So for the time-being, melt is
# magic
library(reshape2)
library(plyr)
mdf <- ddply(melt(df), .(variable), summarise, meanVal = mean(value), low = mean(value) - 
    sd(value)/sqrt(length(value)), high = mean(value) + sd(value)/sqrt(length(value)))
myBarChart <- ggplot(mdf, aes(x = factor(variable), y = meanVal, fill = variable)) + 
    geom_bar(stat = "identity") + geom_errorbar(aes(ymin = low, ymax = high, 
    width = 0.3)) + labs(title = "Bar Plot with SE error bars", x = "Treatment", 
    y = "Mean Response")

myBarChart
```


So, not spectacular yet, but with a bit more dithering, the plot would be publication ready, provided you could save it as an image file.  Well, you can do that too!


```r
ggsave(plot = myBarChart, file = "myBarChart.jpg")
```


## Data Viz Exercise
Write a documented function that creates a data visualization of the NLA data frame we have been working with.  Use your imagination!  When youa are done and have an image on your screen, raise your hands.  We will pick some favorites and show the rest of the class.

# R Data Viz Examples

- Base examples: [R Gallery]()
- ggplot examples: [Google Image Search](http://goo.gl/P0q2Lx)
- R Data Viz, gone wrong:[Accidental aRt](http://accidental-art.tumblr.com) 
