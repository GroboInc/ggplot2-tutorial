Smarter graphs - an introduction to ggplot2 with R
========================================================
author: Alice I. Cecile
date: 2015-06-16



Why should you switch from Excel?
========================================================

1. Prettier plots
2. More complicated plots
3. Generate plots automatically
4. Faster and easier*
5. PDF / svg output

Getting started
========================================================

1. [Install R (programming language)](http://cran.r-project.org/)
2. [Install RStudio (IDE)](http://www.rstudio.com/products/rstudio/download/)
3. [Install ggplot2 (library)](https://www.youtube.com/watch?v=u1r5XTqrCTQ)

Loading your data
========================================================

1. Make a new project
2. Save your data into that folder
3. Load your data


```r
    cars <- read.csv("./cars.csv")
    summary(cars)
```

```
       X             speed           dist       
 Min.   : 1.00   Min.   : 4.0   Min.   :  2.00  
 1st Qu.:13.25   1st Qu.:12.0   1st Qu.: 26.00  
 Median :25.50   Median :15.0   Median : 36.00  
 Mean   :25.50   Mean   :15.4   Mean   : 42.98  
 3rd Qu.:37.75   3rd Qu.:19.0   3rd Qu.: 56.00  
 Max.   :50.00   Max.   :25.0   Max.   :120.00  
```

Standard plotting library
========================================================

Easy but boring. It's hard to make more complex plots.


```r
plot(x=cars$dist, y=cars$speed)
```

<img src="presentation-figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" width="2200px" height="400px" />

Introducing ggplot2
========================================================

Let's try the same thing with ggplot2.


```r
    library(ggplot2)
    basic_cars <- ggplot(cars, aes(x=dist, y=speed)) + geom_point()
    basic_cars
```

<img src="presentation-figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" width="2200px" height="400px" />

Basic customization
========================================================


```r
    pretty_cars <- basic_cars +
        xlab("Distance") + ylab("Speed") + 
        theme_bw()
    pretty_cars
```

<img src="presentation-figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" width="2200px" height="400px" />

Lines of best fit
========================================================


```r
    pretty_cars +
        geom_smooth(colour="red") + 
        stat_smooth(method="lm", colour="blue")
```

<img src="presentation-figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" width="2200px" height="400px" />

A new data set
========================================================


```r
    data(iris)
    iris_plot <- ggplot(iris, aes(x=Sepal.Length, y=Sepal.Width, colour=Species)) + geom_point() + theme_bw()
    iris_plot
```

<img src="presentation-figure/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" width="2200px" height="400px" />

Changing colour palette
========================================================

There are [all sorts of predifined colours](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf) in R!
Or you can just use a hex code.


```r
    iris_plot + scale_color_manual (values=c("purple", "darkseagreen3", "#AA0234"))
```

<img src="presentation-figure/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="2200px" height="400px" />

Other ways to distinguish groups
========================================================


```r
    iris_plot + geom_point(aes(shape=Species, size=Species))
```

<img src="presentation-figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="2200px" height="400px" />

Line graphs
========================================================


```r
    nile <- read.csv("./Nile.csv")
    ggplot(nile, aes(x=Year, y=Nile.Height)) + geom_line() + 
        geom_hline(y=mean(nile$Nile.Height))
```

<img src="presentation-figure/unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" width="2200px" height="400px" />

Bar graphs
========================================================


```r
    data("diamonds")
    ggplot(diamonds, aes(x=cut)) + geom_bar()
```

<img src="presentation-figure/unnamed-chunk-11-1.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" width="2200px" height="400px" />

Box plots
========================================================


```r
    ggplot(diamonds, aes(x=color, y=price)) + geom_boxplot()
```

<img src="presentation-figure/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" width="2200px" height="400px" />

Violin plots
========================================================


```r
    ggplot(diamonds, aes(x=color, y=price)) + geom_violin()
```

<img src="presentation-figure/unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" width="2200px" height="400px" />

Histograms
========================================================


```r
    ggplot(diamonds, aes(x=price, fill=cut)) + geom_histogram()
```

<img src="presentation-figure/unnamed-chunk-14-1.png" title="plot of chunk unnamed-chunk-14" alt="plot of chunk unnamed-chunk-14" width="2200px" height="400px" />

Kernel density plots
========================================================


```r
    ggplot(diamonds, aes(x=price, fill=cut)) + geom_density(alpha=0.3)
```

<img src="presentation-figure/unnamed-chunk-15-1.png" title="plot of chunk unnamed-chunk-15" alt="plot of chunk unnamed-chunk-15" width="2200px" height="400px" />

Big data - transparency
========================================================


```r
    diamonds_points <- ggplot(diamonds, aes(x=carat, y=price)) + geom_point(alpha=0.1) + theme_bw()
    diamonds_points
```

<img src="presentation-figure/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" width="2200px" height="400px" />

Big data - 2D density estimates
========================================================


```r
library("MASS")
data(geyser, "MASS")

ggplot(geyser, aes(x = duration, y = waiting)) + geom_point() + geom_density2d()
```

<img src="presentation-figure/unnamed-chunk-17-1.png" title="plot of chunk unnamed-chunk-17" alt="plot of chunk unnamed-chunk-17" width="2200px" height="400px" />

Faceting - simple
========================================================


```r
    diamonds_points + facet_wrap(~cut)
```

<img src="presentation-figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" width="2200px" height="400px" />

Faceting - grids
========================================================


```r
    diamonds_points + facet_grid(color~cut)
```

<img src="presentation-figure/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" width="2200px" height="400px" />
