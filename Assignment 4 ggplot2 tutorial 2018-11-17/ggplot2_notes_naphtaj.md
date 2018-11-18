---
title: "Data Visualization with ggplot2"
author: "James Naphtali"
date: "11/15/2018"
output:
  html_document:
    keep_md: yes
    number_sections: yes
    toc: yes
---



# Introduction  
* Exploratory Plots - Data visualization and analyses intended for a specialist audience studying the dataset. It is a rough draft of the final presentation of a plot.  
* Explanatory Plots - Intended for a broader audience, presentable for the general public. It is the final copy of plots with proper formatting and analyzed with all necessary statistical treatments. 

## Exploring ggplot2, part 1
Import and generate ggplot of mtcars dataset

```r
# Load the ggplot2 package
library(ggplot2)
library(magrittr)
library(tidyr)
```

```
## 
## Attaching package: 'tidyr'
```

```
## The following object is masked from 'package:magrittr':
## 
##     extract
```

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
# Explore the mtcars data frame with str()
str(mtcars)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```

```r
# Execute the following command
ggplot(mtcars, aes(x = cyl, y = mpg)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

'cyl' variable was treated as a numeric (continuous) variable, instead of a categorical variable

## Exploring ggplot2, part 2

```r
# Load the ggplot2 package
library(ggplot2)

# Change the command below so that cyl is treated as factor
ggplot(mtcars, aes(x = factor(cyl), y = mpg)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

Converted `cyl` to a categorical variable on the x-axis of the scatter plot.

##Grammar of Graphics  
2 Principles  

* Graphics - distinct layers of grammatical elements  
* Meaningful Plots through aesthetic mapping  

Grammatical Elements 

- Data: variables of interest  
- Aesthetics: x-axis, y-axos, colour, fill, size, labels.  
- Geometries: points, line, histogram, bar.  
- Facets: columns, rows.  
- Statistics: binning, smoothing, descriptive.  
- Coordinates: cartesian, fixed, polar.  
- Themes


```r
# A scatter plot has been made for you
ggplot(mtcars, aes(x = wt, y = mpg)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
# Replace ___ with the correct column
ggplot(mtcars, aes(x = wt, y = mpg, color = disp)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-3-2.png)<!-- -->

```r
# Replace ___ with the correct column
ggplot(mtcars, aes(x = wt, y = mpg, size = disp)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-3-3.png)<!-- -->

For my project, determining the change of a bacterial species' abundance over a continuous variable such as time or a chemical parameter can be performed using ggplot2's scatter plot abilities

##Understanding Variables  
Certain arguments within ggplot2 such as `shape` require specific types of variables. In the case of `shape` to obtain the shape of points, it requires categorical data.

##ggplot2 layers  

1. Data: will use `iris` dataset  
2. Aesthetics: mapping of data, ex. scatter plot of `sepal.width` vs. `sepal.length`  
3. Geometry: How the plot looks
4. Facets: How to split up our plots (scatter plot for each category for example)   
5. Statistics: what model to apply on the data  
6. Coordinates: Scaling of visualized data  
7. Theme: appearance of all non-data parts of the plot

##Exploring ggplot2, part 4  

```r
# Explore the diamonds data frame with str()
str(diamonds)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	53940 obs. of  10 variables:
##  $ carat  : num  0.23 0.21 0.23 0.29 0.31 0.24 0.24 0.26 0.22 0.23 ...
##  $ cut    : Ord.factor w/ 5 levels "Fair"<"Good"<..: 5 4 2 4 2 3 3 3 1 3 ...
##  $ color  : Ord.factor w/ 7 levels "D"<"E"<"F"<"G"<..: 2 2 2 6 7 7 6 5 2 5 ...
##  $ clarity: Ord.factor w/ 8 levels "I1"<"SI2"<"SI1"<..: 2 3 5 4 2 6 7 3 4 5 ...
##  $ depth  : num  61.5 59.8 56.9 62.4 63.3 62.8 62.3 61.9 65.1 59.4 ...
##  $ table  : num  55 61 65 58 58 57 57 55 61 61 ...
##  $ price  : int  326 326 327 334 335 336 336 337 337 338 ...
##  $ x      : num  3.95 3.89 4.05 4.2 4.34 3.94 3.95 4.07 3.87 4 ...
##  $ y      : num  3.98 3.84 4.07 4.23 4.35 3.96 3.98 4.11 3.78 4.05 ...
##  $ z      : num  2.43 2.31 2.31 2.63 2.75 2.48 2.47 2.53 2.49 2.39 ...
```

```r
# Add geom_point() with +
ggplot(diamonds, aes(x = carat, y = price)) + geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
# Add geom_point() and geom_smooth() with +
ggplot(diamonds, aes(x = carat, y = price)) + geom_point() + geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-4-2.png)<!-- -->

##Exploring ggplot2, part 5

```r
# 1 - The plot you created in the previous exercise
ggplot(diamonds, aes(x = carat, y = price)) + geom_point() +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
# 2 - Copy the above command but show only the smooth line
ggplot(diamonds, aes(x = carat, y = price)) +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-5-2.png)<!-- -->

```r
# 3 - Copy the above command and assign the correct value to col in aes()
ggplot(diamonds, aes(x = carat, y = price, color = clarity)) +
  geom_smooth()
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-5-3.png)<!-- -->

```r
# 4 - Keep the color settings from previous command. Plot only the points with argument alpha.
ggplot(diamonds, aes(x = carat, y = price, color = clarity)) +
  geom_point(alpha=0.4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-5-4.png)<!-- -->

##Understanding the grammar, part 1  

```r
# Create the object containing the data and aes layers: dia_plot
dia_plot <- ggplot(diamonds, aes(x = carat, y = price))

# Add a geom layer with + and geom_point()
dia_plot + geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
# Add the same geom layer, but with aes() inside
dia_plot + geom_point(aes(color = clarity))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-6-2.png)<!-- -->

##Understanding the grammar, part 2

```r
# 1 - The dia_plot object has been created for you
dia_plot <- ggplot(diamonds, aes(x = carat, y = price))

# 2 - Expand dia_plot by adding geom_point() with alpha set to 0.2
dia_plot <- dia_plot + geom_point(alpha=0.2)

# 3 - Plot dia_plot with additional geom_smooth() with se set to FALSE
dia_plot + geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
# 4 - Copy the command from above and add aes() with the correct mapping to geom_smooth()
dia_plot + geom_smooth(aes(col = clarity), se = FALSE)
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-7-2.png)<!-- -->

#Plotting the Data Layer  
For univariate and bivariate data, base plot package can achieve quick and clean plots.  

Limitations of the base plot package  

But for multivariate data, representing the factors is more difficult than using ggplot2.  

* Plot does not get redrawn when adding data  

* Plot is drawn as an image, not updatable as an object  

* Need to manually add legend  

* Plotting commands are not unified  

##base package and ggplot2, part 1 - plot  

```r
# Plot the correct variables of mtcars
plot(mtcars$wt, mtcars$mpg, col = mtcars$cyl)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
# Change cyl inside mtcars to a factor
mtcars$fcyl <- as.factor(mtcars$cyl)

# Make the same plot as in the first instruction
plot(mtcars$wt, mtcars$mpg, col = mtcars$fcyl)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-8-2.png)<!-- -->
Basically used the different cylinder # as a factor to color in points  

##base package and ggplot2, part 2 - lm

```r
# Use lm() to calculate a linear model and save it as carModel
carModel <- lm(mpg ~ wt, data = mtcars)

# Basic plot
mtcars$cyl <- as.factor(mtcars$cyl)
plot(mtcars$wt, mtcars$mpg, col = mtcars$cyl)

# Call abline() with carModel as first argument and set lty to 2
abline(carModel, lty = 2)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
# Plot each subset efficiently with lapply
# You don't have to edit this code
plot(mtcars$wt, mtcars$mpg, col = mtcars$cyl)
lapply(mtcars$cyl, function(x) {
  abline(lm(mpg ~ wt, mtcars, subset = (cyl == x)), col = x)
  })
```

```
## [[1]]
## NULL
## 
## [[2]]
## NULL
## 
## [[3]]
## NULL
## 
## [[4]]
## NULL
## 
## [[5]]
## NULL
## 
## [[6]]
## NULL
## 
## [[7]]
## NULL
## 
## [[8]]
## NULL
## 
## [[9]]
## NULL
## 
## [[10]]
## NULL
## 
## [[11]]
## NULL
## 
## [[12]]
## NULL
## 
## [[13]]
## NULL
## 
## [[14]]
## NULL
## 
## [[15]]
## NULL
## 
## [[16]]
## NULL
## 
## [[17]]
## NULL
## 
## [[18]]
## NULL
## 
## [[19]]
## NULL
## 
## [[20]]
## NULL
## 
## [[21]]
## NULL
## 
## [[22]]
## NULL
## 
## [[23]]
## NULL
## 
## [[24]]
## NULL
## 
## [[25]]
## NULL
## 
## [[26]]
## NULL
## 
## [[27]]
## NULL
## 
## [[28]]
## NULL
## 
## [[29]]
## NULL
## 
## [[30]]
## NULL
## 
## [[31]]
## NULL
## 
## [[32]]
## NULL
```

```r
# This code will draw the legend of the plot
# You don't have to edit this code
legend(x = 5, y = 33, legend = levels(mtcars$cyl),
       col = 1:3, pch = 1, bty = "n")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-9-2.png)<!-- -->

##base package and ggplot2, part 3

```r
# Convert cyl to factor (don't need to change)
mtcars$cyl <- as.factor(mtcars$cyl)

# Example from base R (don't need to change)
plot(mtcars$wt, mtcars$mpg, col = mtcars$cyl)
abline(lm(mpg ~ wt, data = mtcars), lty = 2)
lapply(mtcars$cyl, function(x) {
  abline(lm(mpg ~ wt, mtcars, subset = (cyl == x)), col = x)
  })
legend(x = 5, y = 33, legend = levels(mtcars$cyl),
       col = 1:3, pch = 1, bty = "n")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
# Plot 1: add geom_point() to this command to create a scatter plot
ggplot(mtcars, aes(x = wt, y = mpg, col = cyl)) +
  geom_point()  # Fill in using instructions Plot 1
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-10-2.png)<!-- -->

```r
# Plot 2: include the lines of the linear models, per cyl
ggplot(mtcars, aes(x = wt, y = mpg, col = cyl)) +
  geom_point()+ # Copy from Plot 1
  geom_smooth(method = 'lm', se = F)   # Fill in using instructions Plot 2
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-10-3.png)<!-- -->

```r
# Plot 3: include a lm for the entire dataset in its whole
ggplot(mtcars, aes(x = wt, y = mpg, col = cyl)) + 
  geom_point() + # Copy from Plot 2
  geom_smooth(method = 'lm', se = F) + # Copy from Plot 2
  geom_smooth(aes(group = 1), method = 'lm', se = F, linetype = 2)   # Fill in using instructions Plot 3
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-10-4.png)<!-- -->

##ggplot2 compared to base package  

- it creates manipulatable objects
- simplifies customizing graph visualization options
- structured in an easy to understand "grammar of graphics"

##Proper Data Format and Tidy Data  

- Rearrange the dataset so that while plotting in ggplot2, the variables that are being plotted will be as clear as possible according to the function's arguments. For example, individual observations (sample) should be placed row-wise and the columns should contain characteristics of each sample (ie. setosa/petal in the `iris.wide` dataset) as well as the measurements. Simplify multiple categorical variables into a single column. 

##Variables to visuals, part 1

```r
# Consider the structure of iris, iris.wide and iris.tidy (in that order)
str(iris)
```

```
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

```r
iris$Flower <- 1:nrow(iris)
iris.wide <- iris %>%
  gather(key, value, -Flower, -Species) %>%
  separate(key, c("Part", "Measure"), "\\.") %>%
  spread(Measure, value)
str(iris.wide)
```

```
## 'data.frame':	300 obs. of  5 variables:
##  $ Species: Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Flower : int  1 1 2 2 3 3 4 4 5 5 ...
##  $ Part   : chr  "Petal" "Sepal" "Petal" "Sepal" ...
##  $ Length : num  1.4 5.1 1.4 4.9 1.3 4.7 1.5 4.6 1.4 5 ...
##  $ Width  : num  0.2 3.5 0.2 3 0.2 3.2 0.2 3.1 0.2 3.6 ...
```

```r
iris.tidy <- iris %>%
  gather(key, Value, -Species) %>%
  separate(key, c("Part", "Measure"), "\\.")
```

```
## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 150 rows
## [601, 602, 603, 604, 605, 606, 607, 608, 609, 610, 611, 612, 613, 614, 615,
## 616, 617, 618, 619, 620, ...].
```

```r
str(iris.tidy)
```

```
## 'data.frame':	750 obs. of  4 variables:
##  $ Species: Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Part   : chr  "Sepal" "Sepal" "Sepal" "Sepal" ...
##  $ Measure: chr  "Length" "Length" "Length" "Length" ...
##  $ Value  : num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
```

```r
# Think about which dataset you would use to get the plot shown right
# Fill in the ___ to produce the plot given to the right
ggplot(iris.tidy, aes(x = Species, y = Value, col = Part)) +
  geom_jitter() +
  facet_grid(. ~ Measure)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

##Variables to visuals, part 1b

```r
# Load the tidyr package
library(tidyr)

# Fill in the ___ to produce to the correct iris.tidy dataset
iris.tidy <- iris %>%
  gather(key, Value, -Species) %>%
  separate(key, c("Part", "Measure"), "\\.")
```

```
## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 150 rows
## [601, 602, 603, 604, 605, 606, 607, 608, 609, 610, 611, 612, 613, 614, 615,
## 616, 617, 618, 619, 620, ...].
```

```r
str(iris.tidy)
```

```
## 'data.frame':	750 obs. of  4 variables:
##  $ Species: Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Part   : chr  "Sepal" "Sepal" "Sepal" "Sepal" ...
##  $ Measure: chr  "Length" "Length" "Length" "Length" ...
##  $ Value  : num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
```

##Variables to visuals, part 2

```r
# The 3 data frames (iris, iris.wide and iris.tidy) are available in your environment
# Execute head() on iris, iris.wide and iris.tidy (in that order)
head(iris)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species Flower
## 1          5.1         3.5          1.4         0.2  setosa      1
## 2          4.9         3.0          1.4         0.2  setosa      2
## 3          4.7         3.2          1.3         0.2  setosa      3
## 4          4.6         3.1          1.5         0.2  setosa      4
## 5          5.0         3.6          1.4         0.2  setosa      5
## 6          5.4         3.9          1.7         0.4  setosa      6
```

```r
head(iris.wide)
```

```
##   Species Flower  Part Length Width
## 1  setosa      1 Petal    1.4   0.2
## 2  setosa      1 Sepal    5.1   3.5
## 3  setosa      2 Petal    1.4   0.2
## 4  setosa      2 Sepal    4.9   3.0
## 5  setosa      3 Petal    1.3   0.2
## 6  setosa      3 Sepal    4.7   3.2
```

```r
head(iris.tidy)
```

```
##   Species  Part Measure Value
## 1  setosa Sepal  Length   5.1
## 2  setosa Sepal  Length   4.9
## 3  setosa Sepal  Length   4.7
## 4  setosa Sepal  Length   4.6
## 5  setosa Sepal  Length   5.0
## 6  setosa Sepal  Length   5.4
```

```r
# Think about which dataset you would use to get the plot shown right
# Fill in the ___ to produce the plot given to the right
ggplot(iris.wide, aes(x = Length, y = Width, color = Part)) +
  geom_jitter() +
  facet_grid(. ~ Species)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

##Variables to visuals, part 2b

```r
# Load the tidyr package
library(tidyr)

# Add column with unique ids (don't need to change)
iris$Flower <- 1:nrow(iris)

# Fill in the ___ to produce to the correct iris.wide dataset
iris.wide <- iris %>%
  gather(key, value, -Flower, -Species) %>%
  separate(key, c("Part", "Measure"), "\\.") %>%
  spread(Measure, value)
```

#Aesthetics  
##Visible Aesthetics  

- Aesthetics are mappings of a variable onto an axis, shape, color, size or other visual properties of the graph using the `aes()` function  

- Aim is to keep X and Y axis consistent throughout a dataframe with many levels in categorical variables  

##All about Aesthetics, part 1

```r
# 1 - Map mpg to x and cyl to y
ggplot(mtcars, aes(mpg, cyl)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
# 2 - Reverse: Map cyl to x and mpg to y
ggplot(mtcars, aes(cyl, mpg)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-15-2.png)<!-- -->

```r
# 3 - Map wt to x, mpg to y and cyl to col
ggplot(mtcars, aes(x=wt, y=mpg, col=cyl)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-15-3.png)<!-- -->

```r
# 4 - Change shape and size of the points in the above plot
ggplot(mtcars, aes(wt, mpg, col = cyl)) +
  geom_point(shape = 1, size = 4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-15-4.png)<!-- -->

##All about aesthetics, part 2

```r
# am and cyl are factors, wt is numeric
class(mtcars$am)
```

```
## [1] "numeric"
```

```r
class(mtcars$cyl)
```

```
## [1] "factor"
```

```r
class(mtcars$wt)
```

```
## [1] "numeric"
```

```r
# From the previous exercise
ggplot(mtcars, aes(x = wt, y = mpg, col = cyl)) +
  geom_point(shape = 1, size = 4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
# 1 - Map cyl to fill
ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl)) +
  geom_point(shape = 1, size = 4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-16-2.png)<!-- -->

```r
# 2 - Change shape and alpha of the points in the above plot
ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl)) +
  geom_point(shape = 21, size = 4, alpha= .6)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-16-3.png)<!-- -->

```r
# 3 - Map am to col in the above plot
ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl, col = am)) +
  geom_point(shape = 21, size = 4, alpha= .6, stroke = 1.5)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-16-4.png)<!-- -->
- `geom_point()` has different shape configurations, such as `shape = 1` which is circle with black in the inside and for the outline, and `shape = 21` which is configurable by using `fill` argument to color the inside and `col` to color the outline with the desired categorical variables  

##All about aesthetics, part 3

```r
# Map cyl to size
ggplot(mtcars, aes(wt, mpg, size = cyl)) + geom_point()
```

```
## Warning: Using size for a discrete variable is not advised.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

```r
# Map cyl to alpha
ggplot(mtcars, aes(wt, mpg, alpha = cyl)) + geom_point()
```

```
## Warning: Using alpha for a discrete variable is not advised.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-17-2.png)<!-- -->

```r
# Map cyl to shape 
ggplot(mtcars, aes(wt, mpg, shape = cyl)) + geom_point() ##This produces the clearest graph by relying on different shapes for classifying cyl variables
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-17-3.png)<!-- -->

```r
# Map cyl to label
ggplot(mtcars, aes(wt, mpg, label = cyl)) + geom_text()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-17-4.png)<!-- -->

##All about attributes, part 1  
R has different `shapes` configurations  
- Shapes 1-20: accepts only `color` aesthetics
- Shapes 21-25: can customize `color` and `fill`


```r
# Define a hexadecimal color
my_color <- "#4ABEFF"

# Draw a scatter plot with color *aesthetic*
ggplot(mtcars, aes(wt, mpg, col = cyl)) + 
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

```r
# Same, but set color *attribute* in geom layer 
ggplot(mtcars, aes(wt, mpg, col = cyl)) + 
  geom_point(col=my_color)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-18-2.png)<!-- -->

```r
# Set the fill aesthetic; color, size and shape attributes
ggplot(mtcars, aes(wt, mpg, fill = cyl)) + 
  geom_point(size = 10, shape = 23, color = my_color, stroke = 1.5)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-18-3.png)<!-- -->

##All about attributes, part 2

```r
# Expand to draw points with alpha 0.5
ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl)) +
  geom_point(alpha = 0.5)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

```r
# Expand to draw points with shape 24 and color yellow
ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl)) +
  geom_point(shape = 24, color = 'yellow')
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-19-2.png)<!-- -->

```r
# Expand to draw text with label rownames(mtcars) and color red
ggplot(mtcars, aes(x = wt, y = mpg, fill = cyl)) +
  geom_text(label = rownames(mtcars), color = 'red')
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-19-3.png)<!-- -->

##Going all out

```r
# Map mpg onto x, qsec onto y and factor(cyl) onto col
ggplot(mtcars, aes(mpg, qsec, col = factor(cyl))) + 
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

```r
# Add mapping: factor(am) onto shape
ggplot(mtcars, 
  aes(mpg, qsec, 
    col = factor(cyl), 
    shape = factor(am)
    )) + 
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-20-2.png)<!-- -->

```r
# Add mapping: (hp/wt) onto size
ggplot(mtcars, 
  aes(mpg, qsec, 
    col = factor(cyl), 
    shape = factor(am),
    size = (hp/wt)
    )) + 
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-20-3.png)<!-- -->

##Aesthetics for categorical and continuous variables  

- `label` and `shape` arguments are only applicable to categorical data

##Modifying Aesthetics  

Position  of data
- Identity (categorical/continuous)  
- Dodge  
- stack  
- fill  
- jitter: introduce random noise and variation to the location of each point to alleviate overplotting
- jitterdodge  

## Position  

```r
cyl.am <- ggplot(mtcars, aes(x = factor(cyl), fill = factor(am)))
# The base layer, cyl.am, is available for you
# Add geom (position = "stack" by default)
cyl.am + geom_bar()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

```r
# Fill - show proportion

cyl.am +  geom_bar(position = "fill")  
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-21-2.png)<!-- -->

```r
# Dodging - principles of similarity and proximity
cyl.am +
  geom_bar(position = "dodge") 
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-21-3.png)<!-- -->

```r
# Clean up the axes with scale_ functions
val = c("#E41A1C", "#377EB8")
lab = c("Manual", "Automatic")
cyl.am +
  geom_bar(position = "dodge") +
  scale_x_discrete(name = "Cylinders") + 
  scale_y_continuous(name = "Number") +
  scale_fill_manual(name = "Transmission", 
                    values = val,
                    labels = lab)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-21-4.png)<!-- -->

##Setting a dummy aesthetic  

```r
## This will give an error because its missing y aesthetic
# ggplot(mtcars, aes(x = mpg)) + geom_point()

# 1 - Create jittered plot of mtcars, mpg onto x, 0 onto y
ggplot(mtcars, aes(x = mpg, y = 0)) +
  geom_jitter()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

```r
# 2 - Add function to change y axis limits
ggplot(mtcars, aes(x = mpg, y = 0)) +
  geom_jitter() +
  scale_y_continuous(limits = c(-2,2))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-22-2.png)<!-- -->

##Aethetics Best Practices  
From lowest to highest accuracy of representing contiuous and categorical data  

Continuous - Color, Grey-scale spectrum, point/length sizes, position on a common scale.  

Categorical - Filled Shapes, sequential colors, qualitative colors.  

```r
##Overplotting 1  - Point shape and transparency  
# Basic scatter plot: wt on x-axis and mpg on y-axis; map cyl to col
ggplot(mtcars, aes(wt, mpg, col = cyl)) +
  geom_point(size = 4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

```r
# Hollow circles - an improvement
ggplot(mtcars, aes(wt, mpg, col = cyl)) +
  geom_point(size = 4, shape = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-23-2.png)<!-- -->

```r
# Add transparency - very nice
ggplot(mtcars, aes(wt, mpg, col = cyl)) +
  geom_point(size = 4, alpha = .6)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-23-3.png)<!-- -->

##Overplotting 2 - alpha with large datasets  

```r
# Scatter plot: carat (x), price (y), clarity (color)
ggplot(diamonds, aes(carat, price, col = clarity)) + 
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

```r
# Adjust for overplotting
ggplot(diamonds, aes(carat, price, col = clarity)) + 
  geom_point(alpha = 0.5)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-24-2.png)<!-- -->

```r
# Scatter plot: clarity (x), carat (y), price (color)
ggplot(diamonds, aes(clarity, carat, col = price)) + 
  geom_point(alpha = 0.5)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-24-3.png)<!-- -->

```r
# Dot plot with jittering
ggplot(diamonds, aes(clarity, carat, col = price)) + 
  geom_point(alpha = 0.5, position = "jitter")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-24-4.png)<!-- -->

#Geometry  
##Scatter Plots  

Customize the geometry of scatter plots using 37 different options. But the 3 common plots are:  

- scatter plots: `points`, `jittter`, `abline`  
- bar plots: `histogram`, `bar`, `errorbar`  
- line plots: `line`  

##Scatter plots and jittering  

```r
# Shown in the viewer:
ggplot(mtcars, aes(x = cyl, y = wt)) +
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

```r
# Solutions:
# 1 - With geom_jitter()
ggplot(mtcars, aes(x = cyl, y = wt)) +
  geom_jitter()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-25-2.png)<!-- -->

```r
# 2 - Set width in geom_jitter()
ggplot(mtcars, aes(x = cyl, y = wt)) +
  geom_jitter(width = 0.1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-25-3.png)<!-- -->

```r
# 3 - Set position = position_jitter() in geom_point() ()
ggplot(mtcars, aes(x = cyl, y = wt)) +
  geom_point(position = position_jitter(0.1))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-25-4.png)<!-- -->

##Scatter plot and jittering - part 2  

```r
# Examine the structure of Vocab
library(car)
```

```
## Loading required package: carData
```

```
## 
## Attaching package: 'car'
```

```
## The following object is masked from 'package:dplyr':
## 
##     recode
```

```r
str(Vocab)
```

```
## 'data.frame':	30351 obs. of  4 variables:
##  $ year      : num  1974 1974 1974 1974 1974 ...
##  $ sex       : Factor w/ 2 levels "Female","Male": 2 2 1 1 1 2 2 2 1 1 ...
##  $ education : num  14 16 10 10 12 16 17 10 12 11 ...
##  $ vocabulary: num  9 9 9 5 8 8 9 5 3 5 ...
##  - attr(*, "na.action")= 'omit' Named int  1 2 3 4 5 6 7 8 9 10 ...
##   ..- attr(*, "names")= chr  "19720001" "19720002" "19720003" "19720004" ...
```

```r
# Basic scatter plot of vocabulary (y) against education (x). Use geom_point()
ggplot(Vocab, aes(education, vocabulary)) + 
  geom_point()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

```r
# Use geom_jitter() instead of geom_point()
ggplot(Vocab, aes(education, vocabulary)) + 
  geom_jitter()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-26-2.png)<!-- -->

```r
# Using the above plotting command, set alpha to a very low 0.2
ggplot(Vocab, aes(education, vocabulary)) + 
  geom_jitter(alpha = 0.2)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-26-3.png)<!-- -->

```r
# Using the above plotting command, set the shape to 1
ggplot(Vocab, aes(education, vocabulary)) + 
  geom_jitter(alpha = 0.2, shape = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-26-4.png)<!-- -->
##Histograms


```r
# 1 - Make a univariate histogram
ggplot(mtcars, aes(x = mpg)) +
  geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-27-1.png)<!-- -->

```r
# 2 - Plot 1, plus set binwidth to 1 in the geom layer
ggplot(mtcars, aes(x = mpg)) +
  geom_histogram(binwidth = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-27-2.png)<!-- -->

```r
# 3 - Plot 2, plus MAP ..density.. to the y aesthetic (i.e. in a second aes() function)
ggplot(mtcars, aes(x = mpg)) +
  geom_histogram(aes(y = ..density..), binwidth = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-27-3.png)<!-- -->

```r
# 4 - plot 3, plus SET the fill attribute to "#377EB8"
ggplot(mtcars, aes(x = mpg)) +
  geom_histogram(aes(y = ..density..), binwidth = 1, fill = "#377EB8")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-27-4.png)<!-- -->

##Position  

```r
# Draw` a bar plot of cyl, filled according to am
ggplot(mtcars, aes(x = cyl, fill = am)) +
  geom_bar()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

```r
# Change the position argument to stack
ggplot(mtcars, aes(x = cyl, fill = am)) +
  geom_bar(position = "stack") 
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-28-2.png)<!-- -->

```r
# Change the position argument to fill
ggplot(mtcars, aes(x = cyl, fill = am)) +
  geom_bar(position = "fill")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-28-3.png)<!-- -->

```r
# Change the position argument to dodge
ggplot(mtcars, aes(x = cyl, fill = am)) +
  geom_bar(position = "dodge")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-28-4.png)<!-- -->

I can potentially use the stacking feature in bargraphs to plot relative genera/species abundances in metagenomic data.

##Overlapping Bar plots  


```r
# 1 - The last plot form the previous exercise
ggplot(mtcars, aes(x = cyl, fill = am)) +
  geom_bar(position = "dodge")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

```r
# 2 - Define posn_d with position_dodge()
posn_d <- position_dodge(width = 0.2)

# 3 - Change the position argument to posn_d
ggplot(mtcars, aes(x = cyl, fill = am)) + 
  geom_bar(position = posn_d)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-29-2.png)<!-- -->

```r
# 4 - Use posn_d as position and adjust alpha to 0.6
ggplot(mtcars, aes(x = cyl, fill = am)) + 
  geom_bar(position = posn_d, alpha = 0.6)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-29-3.png)<!-- -->

##Overlapping Histograms  

```r
# A basic histogram, add coloring defined by cyl
ggplot(mtcars, aes(mpg, fill = cyl)) +
  geom_histogram(binwidth = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-30-1.png)<!-- -->

```r
# Change position to identity
ggplot(mtcars, aes(mpg, fill = cyl)) +
  geom_histogram(binwidth = 1, position = 'identity')
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-30-2.png)<!-- -->

```r
# Change geom to freqpoly (position is identity by default)
ggplot(mtcars, aes(mpg, col = cyl)) +
  geom_freqpoly(binwidth = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-30-3.png)<!-- -->

##Bar plots with color ramp, part 1

```r
# Example of how to use a brewed color palette
ggplot(mtcars, aes(x = cyl, fill = am)) +
  geom_bar() +
  scale_fill_brewer(palette = "Set1")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-31-1.png)<!-- -->

```r
# Use str() on Vocab to check out the structure
Vocab$education <- as.factor(Vocab$education)
Vocab$vocabulary <- as.factor(Vocab$vocabulary)
str(Vocab)
```

```
## 'data.frame':	30351 obs. of  4 variables:
##  $ year      : num  1974 1974 1974 1974 1974 ...
##  $ sex       : Factor w/ 2 levels "Female","Male": 2 2 1 1 1 2 2 2 1 1 ...
##  $ education : Factor w/ 21 levels "0","1","2","3",..: 15 17 11 11 13 17 18 11 13 12 ...
##  $ vocabulary: Factor w/ 11 levels "0","1","2","3",..: 10 10 10 6 9 9 10 6 4 6 ...
##  - attr(*, "na.action")= 'omit' Named int  1 2 3 4 5 6 7 8 9 10 ...
##   ..- attr(*, "names")= chr  "19720001" "19720002" "19720003" "19720004" ...
```

```r
# Plot education on x and vocabulary on fill
# Use the default brewed color palette
ggplot(Vocab, aes(x = education, fill = vocabulary)) +
  geom_bar(position = 'fill') + 
  scale_fill_brewer()
```

```
## Warning in RColorBrewer::brewer.pal(n, pal): n too large, allowed maximum for palette Blues is 9
## Returning the palette you asked for with that many colors
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-31-2.png)<!-- -->
Error refers to the fact that the dataset contains 11 categories but there exists only 9 blue colors in the palette.

##Bar plots with color ramp, part 2

```r
library(RColorBrewer)
# Final plot of last exercise
ggplot(Vocab, aes(x = education, fill = vocabulary)) +
  geom_bar(position = "fill") +
  scale_fill_brewer()
```

```
## Warning in RColorBrewer::brewer.pal(n, pal): n too large, allowed maximum for palette Blues is 9
## Returning the palette you asked for with that many colors
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-32-1.png)<!-- -->

```r
# Definition of a set of blue colors
blues <- brewer.pal(9, "Blues") # from the RColorBrewer package

# 1 - Make a color range using colorRampPalette() and the set of blues
blue_range <- colorRampPalette(blues)

# 2 - Use blue_range to adjust the color of the bars, use scale_fill_manual()
ggplot(Vocab, aes(x = education, fill = vocabulary)) +
  geom_bar(position = "fill") +
  scale_fill_manual(values = blue_range(11))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-32-2.png)<!-- -->

##Overlapping Histograms  

```r
# 1 - Basic histogram plot command
ggplot(mtcars, aes(mpg)) +
  geom_histogram(binwidth = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-33-1.png)<!-- -->

```r
# 2 - Plot 1, Expand aesthetics: am onto fill
ggplot(mtcars, aes(mpg, fill = am)) + 
  geom_histogram(binwidth = 1)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-33-2.png)<!-- -->

```r
# 3 - Plot 2, change position = "dodge"
ggplot(mtcars, aes(mpg, fill = am)) + 
  geom_histogram(binwidth = 1, position = "dodge")
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-33-3.png)<!-- -->

```r
# 4 - Plot 3, change position = "fill"
ggplot(mtcars, aes(mpg, fill = am)) + 
  geom_histogram(binwidth = 1, position = "fill")
```

```
## Warning: Removed 8 rows containing missing values (geom_bar).
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-33-4.png)<!-- -->

```r
# 5 - Plot 4, plus change position = "identity" and alpha = 0.4
ggplot(mtcars, aes(mpg, fill = am)) + 
  geom_histogram(binwidth = 1, 
    position = "identity",
    alpha = 0.4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-33-5.png)<!-- -->

```r
# 6 - Plot 5, plus change mapping: cyl onto fill
ggplot(mtcars, aes(mpg, fill = cyl)) + 
  geom_histogram(binwidth = 1, 
    position = "identity",
    alpha = 0.4)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-33-6.png)<!-- -->
##Line Plots  

```r
# Print out head of economics
head(economics)
```

```
## # A tibble: 6 x 6
##   date         pce    pop psavert uempmed unemploy
##   <date>     <dbl>  <int>   <dbl>   <dbl>    <int>
## 1 1967-07-01  507. 198712    12.5     4.5     2944
## 2 1967-08-01  510. 198911    12.5     4.7     2945
## 3 1967-09-01  516. 199113    11.7     4.6     2958
## 4 1967-10-01  513. 199311    12.5     4.9     3143
## 5 1967-11-01  518. 199498    12.5     4.7     3066
## 6 1967-12-01  526. 199657    12.1     4.8     3018
```

```r
# Plot unemploy as a function of date using a line plot
ggplot(economics, aes(x = date, y = unemploy)) +
  geom_line()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-34-1.png)<!-- -->

```r
# Adjust plot to represent the fraction of total population that is unemployed
ggplot(economics, aes(x = date, y = unemploy/pop)) +
  geom_line()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-34-2.png)<!-- -->

##Periods of recession  

```r
#Create recess dataframe (obtained from https://rpubs.com/gjeffroy/ggplotportfolio)
recess <- data.frame(
  begin = c("1969-12-01","1973-11-01","1980-01-01","1981-07-01","1990-07-01","2001-03-01"), 
  end = c("1970-11-01","1975-03-01","1980-07-01","1982-11-01","1991-03-01","2001-11-01"),
  stringsAsFactors = F
  )

library(lubridate) #Setup code for recess obtained from https://rpubs.com/gjeffroy/ggplotportfolio
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
recess$begin <- ymd (recess$begin)
recess$end <- ymd (recess$end)

ggplot(economics, aes(x = date, y = unemploy/pop)) +
  geom_rect(data = recess, aes(xmin = begin, xmax = end, ymin = -Inf, ymax = Inf),
            inherit.aes = FALSE, fill = "red", alpha = 0.2) + 
  geom_line()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-35-1.png)<!-- -->

##Multiple time series, Part 1

```r
# Check the structure as a starting point (obtained fish.species dataframe from https://github.com/JoshuaHaden/Data-Visualization-in-R-with-ggplot2-Part-1-Data-Camp/blob/master/fish.RData)
load(url("https://github.com/JoshuaHaden/Data-Visualization-in-R-with-ggplot2-Part-1-Data-Camp/raw/master/fish.RData"))
str(fish.species)
```

```
## 'data.frame':	61 obs. of  8 variables:
##  $ Year    : int  1950 1951 1952 1953 1954 1955 1956 1957 1958 1959 ...
##  $ Pink    : int  100600 259000 132600 235900 123400 244400 203400 270119 200798 200085 ...
##  $ Chum    : int  139300 155900 113800 99800 148700 143700 158480 125377 132407 113114 ...
##  $ Sockeye : int  64100 51200 58200 66100 83800 72000 84800 69676 100520 62472 ...
##  $ Coho    : int  30500 40900 33600 32400 38300 45100 40000 39900 39200 32865 ...
##  $ Rainbow : int  0 100 100 100 100 100 100 100 100 100 ...
##  $ Chinook : int  23200 25500 24900 25300 24500 27700 25300 21200 20900 20335 ...
##  $ Atlantic: int  10800 9701 9800 8800 9600 7800 8100 9000 8801 8700 ...
```

```r
# Use gather to go from fish.species to fish.tidy
fish.tidy <- gather(fish.species, Species, Capture, -Year)

str(fish.tidy)
```

```
## 'data.frame':	427 obs. of  3 variables:
##  $ Year   : int  1950 1951 1952 1953 1954 1955 1956 1957 1958 1959 ...
##  $ Species: chr  "Pink" "Pink" "Pink" "Pink" ...
##  $ Capture: int  100600 259000 132600 235900 123400 244400 203400 270119 200798 200085 ...
```

##Multiple time series, part 2

```r
# Recreate the plot shown on the right
ggplot(fish.tidy, aes(x = Year, y = Capture, col = Species)) +
  geom_line()
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-37-1.png)<!-- -->

#Qplot  
In terms of syntax, qplot is analogous to base R's `plot()` function. It is more explicit in coding ggplot2's layers.  

##Using aesthetics  

```r
# basic qplot scatter plot:
qplot(wt, mpg, data = mtcars)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-38-1.png)<!-- -->

```r
# Categorical variable mapped onto size:
# cyl
qplot(wt, mpg, data = mtcars, size = factor(gear))
```

```
## Warning: Using size for a discrete variable is not advised.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-38-2.png)<!-- -->

```r
# gear
qplot(wt, mpg, data = mtcars, size = factor(gear))
```

```
## Warning: Using size for a discrete variable is not advised.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-38-3.png)<!-- -->

```r
# Continuous variable mapped onto col:
# hp
qplot(wt, mpg, data = mtcars, col = hp)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-38-4.png)<!-- -->

```r
# qsec
qplot(wt, mpg, data = mtcars, col = qsec)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-38-5.png)<!-- -->

##Choosing geoms, part 1

```r
# qplot() with x only
qplot(x = factor(cyl), data = mtcars)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-39-1.png)<!-- -->

```r
# qplot() with x and y
qplot(x = factor(cyl), y = factor(vs), data = mtcars)
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-39-2.png)<!-- -->

```r
# qplot() with geom set to jitter manually
qplot(x = factor(cyl), y = factor(vs), data = mtcars, geom = 'jitter')
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-39-3.png)<!-- -->

##Choosing geoms, part 2, dot plot

```r
# cyl and am are factors, wt is numeric
class(mtcars$cyl)
```

```
## [1] "factor"
```

```r
class(mtcars$am)
```

```
## [1] "numeric"
```

```r
class(mtcars$wt)
```

```
## [1] "numeric"
```

```r
# "Basic" dot plot, with geom_point():
ggplot(mtcars, aes(cyl, wt, col = am)) +
  geom_point(position = position_jitter(0.2, 0))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-40-1.png)<!-- -->

```r
# 1 - "True" dot plot, with geom_dotplot():
ggplot(mtcars, aes(cyl, wt, fill = am)) +
  geom_dotplot(binaxis = "y", stackdir = "center")
```

```
## `stat_bindot()` using `bins = 30`. Pick better value with `binwidth`.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-40-2.png)<!-- -->

```r
# 2 - qplot with geom "dotplot", binaxis = "y" and stackdir = "center"
qplot(
  cyl, wt, 
  data = mtcars, 
  fill = am, 
  geom = "dotplot", 
  binaxis = "y", 
  stackdir = "center"
)
```

```
## `stat_bindot()` using `bins = 30`. Pick better value with `binwidth`.
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-40-3.png)<!-- -->

#Wrap up  
##Chicken Weight  

```r
# ChickWeight is available in your workspace
# 1 - Check out the head of ChickWeight
head(ChickWeight)
```

```
## Grouped Data: weight ~ Time | Chick
##   weight Time Chick Diet
## 1     42    0     1    1
## 2     51    2     1    1
## 3     59    4     1    1
## 4     64    6     1    1
## 5     76    8     1    1
## 6     93   10     1    1
```

```r
# 2 - Basic line plot
ggplot(ChickWeight, aes(x = Time, y = weight)) + 
  geom_line(aes(group = Chick))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-41-1.png)<!-- -->

```r
# 3 - Take plot 2, map Diet onto col.
ggplot(ChickWeight, 
    aes(x = Time, y = weight, col = Diet)) + 
  geom_line(
    aes(group = Chick))
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-41-2.png)<!-- -->

```r
# 4 - Take plot 3, add geom_smooth()
ggplot(ChickWeight, 
    aes(x = Time, y = weight, col = Diet)) + 
  geom_line(
    aes(group = Chick), alpha = 0.3) + 
  geom_smooth(lwd = 2, se = F)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-41-3.png)<!-- -->

##Titanic Unfortunately, I couldn't find the dataset used in Datacamp for this section

```r
# titanic is avaliable in your workspace
# 1 - Check the structure of titanic
library(titanic)
str(titanic)

# 2 - Use ggplot() for the first instruction
ggplot(titanic, 
    aes(x = Pclass, fill = Sex)) + 
  geom_bar(
    position = "dodge")
# 3 - Plot 2, add facet_grid() layer
ggplot(titanic, 
    aes(x = Pclass, fill = Sex)) + 
  geom_bar(
    position = "dodge") +
  facet_grid(. ~ Survived)

# 4 - Define an object for position jitterdodge, to use below
posn.jd <- position_jitterdodge(0.5, 0, 0.6)

# 5 - Plot 3, but use the position object from instruction 4
ggplot(titanic, 
    aes(x = Pclass, y = Age, col = Sex)) + 
  geom_point(
    size = 3, alpha = 0.5, position = posn.jd) +
  facet_grid(. ~ Survived)
```

#My data  
##Relative Abundances of Microbial phyla in an anaerobic digester  


```r
#####MASSTC Anaerobic Digester#######
#Import dataset. 
all.masstc.df <- (read.csv("C:/Users/james/Documents/R Analysis/Bio720/Assignment 4/ggplot2 tutorial/ggplot2_notes_naphtaj_files/MyData.csv", 
                       header = TRUE, row.names = 1))
str(all.masstc.df) #We are only going to plot relative abundances of OTU counts in the "MASSTC" anaerobic digester.
```

```
## 'data.frame':	1320 obs. of  25 variables:
##  $ OTU               : Factor w/ 49 levels "OTU1","OTU1007",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Sample            : Factor w/ 33 levels "10Lf","10Sf",..: 25 3 27 16 1 9 11 23 13 6 ...
##  $ Abundance         : num  64.4 63.1 61.6 56.8 55.3 ...
##  $ sample_id         : Factor w/ 33 levels "10Lf","10Sf",..: 25 3 27 16 1 9 11 23 13 6 ...
##  $ collection_date   : Factor w/ 1 level "11/13/2017": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Location          : Factor w/ 2 levels "MASSTC","Sewage": 1 1 1 1 1 1 1 1 1 1 ...
##  $ sublocation1      : Factor w/ 1 level "MASSTC_ITT": 1 1 1 1 1 1 1 1 1 1 ...
##  $ sublocation2      : Factor w/ 3 levels "MASSTC_ITT","MASSTC_SPT",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ distance          : Factor w/ 20 levels "0","108","12",..: 8 14 9 15 13 5 4 3 2 7 ...
##  $ distance_bin      : logi  NA NA NA NA NA NA ...
##  $ sample_type       : Factor w/ 4 levels "innertube_outlet",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ composition       : Factor w/ 3 levels "liquid","mixed",..: 1 1 1 3 1 1 1 1 1 1 ...
##  $ target_gene       : Factor w/ 1 level "16s": 1 1 1 1 1 1 1 1 1 1 ...
##  $ target_subfragment: Factor w/ 1 level "v4": 1 1 1 1 1 1 1 1 1 1 ...
##  $ TKN               : num  49.2 168 110 353 78.6 151 455 74.9 391 244 ...
##  $ NH3               : num  30.7 37.3 32.5 53.6 36.5 48.5 55.4 31.9 54.3 50.2 ...
##  $ TS                : num  0.5 1.4 1.6 1.7 1.1 1.2 1.9 1.4 1.9 1.6 ...
##  $ VS                : num  0.3 1 1.2 1.4 0.8 1 1.5 0.9 1.5 1.3 ...
##  $ TSS               : num  1.5 2.12 1.8 2.31 1.8 2.16 2.28 1.6 1.8 2.34 ...
##  $ pH                : logi  NA NA NA NA NA NA ...
##  $ DO                : num  243 828 91.5 937.5 616.5 ...
##  $ Kingdom           : Factor w/ 2 levels "Archaea","Bacteria": 2 2 2 2 2 2 2 2 2 2 ...
##  $ Phylum            : Factor w/ 40 levels "ABY1_OD1","Acidobacteria",..: 27 27 27 27 27 27 27 27 27 27 ...
##  $ composition_abb   : Factor w/ 3 levels "l","m","s": 1 1 1 3 1 1 1 1 1 1 ...
##  $ distance_id       : Factor w/ 33 levels "0L","108L","108S",..: 14 25 16 28 23 8 4 6 2 12 ...
```

```r
all.masstc.df$distance_id <- factor(all.masstc.df$distance_id, levels=c("0L","SPT-1","SPT-2 ","12L","12S","24L","24S","36L","36S","48L","48S","60L", "60S","72L","72S","84L","84S","96L","96S","97L","97S","108L","108S","120L","120S","132L","132S","144L","144S","156L","156S","ITO-L1","ITO-L2","ITO-S"), ordered=TRUE)
levels(all.masstc.df$distance)
```

```
##  [1] "0"      "108"    "12"     "120"    "132"    "144"    "156"   
##  [8] "24"     "36"     "48"     "60"     "72"     "84"     "96"    
## [15] "97"     "ITO-L1" "ITO-L2" "ITO-S"  "SPT-1"  "SPT-2 "
```

```r
labels <- c(MASSTC_ITT="MASSTC Anaerobic Digester")
#Convert Phylum to a character vector from a factor 
all.masstc.df$Phylum <- as.character(all.masstc.df$Phylum)
levels(all.masstc.df$distance)
```

```
##  [1] "0"      "108"    "12"     "120"    "132"    "144"    "156"   
##  [8] "24"     "36"     "48"     "60"     "72"     "84"     "96"    
## [15] "97"     "ITO-L1" "ITO-L2" "ITO-S"  "SPT-1"  "SPT-2 "
```

```r
# group dataframe by Phylum, calculate median rel. abundance
library(plyr)
```

```
## -------------------------------------------------------------------------
```

```
## You have loaded plyr after dplyr - this is likely to cause problems.
## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
## library(plyr); library(dplyr)
```

```
## -------------------------------------------------------------------------
```

```
## 
## Attaching package: 'plyr'
```

```
## The following object is masked from 'package:lubridate':
## 
##     here
```

```
## The following objects are masked from 'package:dplyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
```

```r
medians <- ddply(all.masstc.df, ~Phylum, function(x) c(median=median(x$Abundance)))
#Find Phyla whose rel. abundance is less than 1%
remainder <- medians[medians$median <= 0.6,]$Phylum 
levels(all.masstc.df$distance)
```

```
##  [1] "0"      "108"    "12"     "120"    "132"    "144"    "156"   
##  [8] "24"     "36"     "48"     "60"     "72"     "84"     "96"    
## [15] "97"     "ITO-L1" "ITO-L2" "ITO-S"  "SPT-1"  "SPT-2 "
```

```r
#Change their name to < 0.6% Phyla
all.masstc.df[all.masstc.df$Phylum %in% remainder,]$Phylum <- '< 0.6% Phyla'
#Barplots
#make custom palette
Phylum_colors <- c(
  "#CBD588", "#5F7FC7", "orange","#DA5724", "#508578", "#CD9BCD",
  "#AD6F3B", "#673770","#D14285", "#652926", "#C84248", 
  "#8569D5", "#5E738F","#D1A33D", "#8A7C64", "#599861", "sienna1", "purple4", "seashell3",
  "mediumorchid1", "lightseagreen", "limegreen", "cyan", "red", "bisque2", "dodgerblue", "deeppink",
  "gray38", "honeydew3", "lightgoldenrod2", "magenta4", "springgreen"
)
names(Phylum_colors) <- unique(all.masstc.df$Phylum)
Phylum_colors["< 0.6% Phyla"] <- "black"

all_ggplot <- ggplot(data=all.masstc.df, aes(x=distance_id, y=Abundance, fill=Phylum))
all_ggplot + facet_wrap(.~sublocation1, labeller=labeller(sublocation1 = labels), scales = "free_x") + geom_bar(aes(), stat="identity", position="stack") +
  scale_fill_manual(values = Phylum_colors) + 
  geom_col(width = 0.2) +
  theme(legend.position="right", legend.text=element_text(size=11.5,vjust=0.7), axis.text.x=element_text(size=12,angle=90,vjust=0.4), axis.text.y=element_text(size=13), axis.title.x = element_text(size=15, vjust=0.2), axis.title.y=element_text(size=15, vjust=2), strip.text.x=element_text(size=12)) + guides(fill=guide_legend(nrow=16)) +
  labs(x="\nDistance (in.) / Sample Type", y="Relative Abundance (%)") 
```

![](ggplot2_notes_naphtaj_files/figure-html/unnamed-chunk-43-1.png)<!-- -->

- The x axis depicts samples taken every 12 inches along a tube within the anaerobic digester in which the sewage passes through. It is hypothesized that the majority of microbial digestion of organic waste occurs within this flooded tube. The Y axis depicts the relative abundances of OTU counts in % of the top 10 phyla


















