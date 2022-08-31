---
share: true
tags: []
---

# 2: Learning R
> [!seealso]- Links
> [Youtube Video](https://www.youtube.com/watch?v=erBZ97sOq1Q&list=PLfe6IcA_dEWkcHFfBA6XSXW31H8t4XSbB&index=2)
> [Github](https://github.com/kjmazidi/Machine_Learning_2nd_edition)
> [R Homepage](https://www.r-project.org/about.html)
## TLDR;

## Setting Up R
Head to [R Homepage](https://www.r-project.org/about.html) to get a little background about R. An Open source alternative to the S langauge developed at Bell Labs.
- Click on [CRAN](https://cran.r-project.org/mirrors.html)
- I used [this](https://cran.microsoft.com/) mirror
- Run the installer for your system
	- Installed at `C:\Program Files\R\R-4.2.1`
	- Used Defaults

Then download [RStudio](https://www.rstudio.com/products/rstudio/download/) and open it (She was specific that it was in this order)
- Installed at `C:\Program Files\RStudio`

Open the software and get ready to go!

## Using RStudio
This section uses Chapter 2 of "Machine Learning Handbook: Using R and Python", the chapter can be found at http://karenmazidi.blogspot.com/, however I am going to get the book real fast... I went ahead and bought the paper back!

R is an interpreted language, which means we can easily get to know it in the console.

### Fast Track on Typing
```R
3 + 4 - 1 * 8 / 2
[1] 3 # Output
```
While it does follow the normal order of operations, and returns as expected, note that that `[1]` is telling you the dimension of what is returned. There aren't Scalars in R, and the `3` returned is just a vector of length 1

If we were to save the result to a variable (note the assignment operator is commonly `<-`) we would see it in the environment tab. The Assignment operator could be `=`, but `<-` is preferred. She notes assignment is bidirectional but I'm not sure what that means, check [here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/assignOps.html).

```R
x <- 3 + 4 - 1 * 8 / 2
[1] 3 # Output
```

In the environment we can now see `x`, and it looks like an integer. But if we use the type of function
```R
typeof(x)
[1] "double" # Output
```

We can see that numbers are by default stored as doubles. We can instead store things as an integer using `L`:

```R
x <- c(1L, 2L, 3L)
typeof(x) 
[1] "integer" # Output Environment: int [1:3] 1 2 3 (Int vector, with 3 values)
```

R is dynamically typed. Note that an identifier like `x` is actually just a pointer to an object and if you change the thing it's pointing to, it changes it's type.

A *vector*  is a sequence of objects of the same type. The `c()` function we saw above will combine the objects into a vector.

If we try and combine objects of different types, we will see that R *coerces* all the objects to be of the most inclusive type

```R
x <- c(1L, 1.2, "hi")
typeof(x)
[1] "character" # Output Environment: chr [1:3] "1" "1.2" "hi"
```

Another way to create vectors is using a sequence operator, the colon `:`.

```R
x <- 1:10
[1] 1 2 3 4 5 6 7 8 9 10 # Output
```

The above code produces a vector, of the numbers between 1 and 10 (in sequence). If we multiply this vector by 5 we will see every element of the vector multiply.

```R
x*5
[1] 5 10 15 20 25 30 35 40 45 50 # Output
```

In normal code we would write a loop, but R was written for statistics. We also have:
- `sum()`
- `min()`
- `max()`
- `mean()`
- `range()`
- `median()`

We can also use logical vectors.

```R
x>5
[1] FALSE FALSE FALSE FALSE FALSE TRUE TRUE TRUE TRUE TRUE # Output
```

We can then sum those vectors

```R
sum(x>5)
[1] 5 # Output
```

Knowing all this we may try a more complicated expression:

```R
x[x<5] <- 0
```

The brackets above selects all elements less than 5, then the right replaces them with 0.

### Vector Example
Now for a shocker: R indexes arrays at 1, meaning we have some boundries to learn:

```R
> x <- 1:10
> x
[1] 1 2 3 4 5 6 7 8 9 10
> x[1]
[1] 1
> x[0]
integer(0)
> x[11]
[1] NA
```

- Vectors start at 1
- If you index out of bounds, you don't get an error
- Index [0] seems to store some info about the vector

### List Example

A list is a sequence of objects of varied types. Lists can also hold lists as elements
```R
> x <- list('a', 1.2, TRUE, 5)
> x[[1]]
[1] "a"
> length(x)
[1] 4
```

To index into a list you use double brackets

### Matrix Example
- A matrix is a 2D object with elements of the same type
- An array is like a matrix but can have more than 2 dimensions

Basically a matrix is just 2D, while arrays can be defined with more dimensions

```R
> m <- matrix(1:10, nrow=2)
> m
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    3    5    7    9
[2,]    2    4    6    8   10
> m [2,3]
[1] 6
```

Also note that indexing of matrices are in row column order.

### Dataframe Example
A data frame is a 2d structure where each column can be of a different type

```R
> x <- c(1:3)
> y <- c(1.1, 2.2, 3.3)
> z <- c('a', 'b', 'c')
> df <- data.frame(cbind(x,y,z))
> df
  x   y z
1 1 1.1 a
2 2 2.2 b
3 3 3.3 c
```

Most of the time we will read in a dataframe, but they can be constructed manually. In the case above using vectors and the `cbind()` operator. We can access a given column with the `$` operator. As well as change the names of the columns.

```R
> df$x
[1] "1" "2" "3"
> colnames(df) <- c('Ticket', 'Discount', 'Section')
> df
  Ticket Discount Section
1      1      1.1       a
2      2      2.2       b
3      3      3.3       c
```

### Reading Structures
- `getwd()` returns the defualt directory
- `setwd()` sets the default directory
- `read.csv()` is used to read in a data frame
- `str()` is the structure command

```R
> getwd()
[1] "C:/Users/ZaiquiriW/Documents"
> setwd('C:/Users/ZaiquiriW/Documents/Fall2022/fall-2022/machine-learning/r-directory')
> df <- read.csv("titanic.csv")
> str(df)
```

### Working With Environment
We can list all the values in the environment with `ls()`, and we can remove them with `rm(list=ls())`. That means we have nothing in that environment

### Datasets
R has a lot of built in data sets we can list with `data()`. To bring a data set into working memory we use `data("airquality")` which we can learn more about using `?airquality`. Then if we want to then work with that dataset we simply:
```R
> df <- airquality[]
> rm(airquality)
```

Note that we removed the original dataset in memory to save space

If we want to explore those data sets we have:
- `head()` which returns the start of the data, we may pass how many rows/observations
- `tail()` which returns the end of the data, we may pass how mnay rows/observations
- `dim()` returns the size of the data (row and columns)
- `str()` shows the structure of the data
- `summary(df) ` gives statistical info about each column

#### Removing NAs
Datasets like airquality above often have missing data that can lead to problems with your code.  First we can quantify how many NAs we have
```R
> sum(is.na(df$Ozone))
[1] 37
```

The above line takes the column/attribute ozone, and checks if it is NA (returning a boolean value). Then it sums the number of TRUE results from that test. In the dataset then, there are 37 NAs

A problem that can occur if we try and find statistics on a column with NAs, like finding the mean of a column with NAs. We would then want to ignore them.

```R
> mean(df$Ozone)
[1] NA
> mean(df$Ozone, na.rm=TRUE)
[1] 42.12931
```

This is an assumption about what the data means however. What if the value for NA had some sort of value we could assume for NA. Like the mean we found above?

```R
> m <- mean(df$Ozone, na.rm=TRUE)
> df&Ozone[is.na(df$Ozone)] <- m
> mean(df&Ozone)
[1] 42.12931
```

### Visual Analysis
If we want to look at our data visually we may try and use the built in graphing functions
- `hist(df&Temp)` - Creates a historam of temperatures
- `plot(df$Temp)` - Now we see all the temperatures on a scatterplot
- `plot(df$Temp, df$Ozone)` - Shows us a scatter plot with temperature on the X axis and Ozone on the Y axis of a scatterplot.
- `plot(df$Temp, df$Ozone, pch=16, col="blue", cex=1.5, main="Airquality", xlab=Ozone", ylab="Temperature")` - Modifies the graph to be much more visually appealing


### Correlation
Correlation is the factor that tells us how much one variable goes up or down in relation to another. Correlations range from +1 to -1, and corrrelations near +/-1 indicate strong correlations while correlations near 0 indicate no correlation.

```R
> cor(df[1:4], use="complete")
             Ozone    Solar.R       Wind       Temp
Ozone    1.0000000  0.3483417 -0.6124966  0.6985414
Solar.R  0.3483417  1.0000000 -0.1271835  0.2940876
Wind    -0.6124966 -0.1271835  1.0000000 -0.4971897
Temp     0.6985414  0.2940876 -0.4971897  1.0000000
> pairs(df[1:4])
```

The pairs function at the end their gives us a graph plotting each attribute/column with every other attribute/column. The label of a column is the x axis for each graph in its column. While the label of a row is the y axis for each graph in it's row.

![[pair-airquality.jpeg]]

Thus you can see in the lower left corner that as Ozone goes up (X axis), the Temperature (Y axis) goes up. Meaning there is a positive correlation from Ozone to Temperature. I'm at least.. half sure of this.

### Factors
```R
> df$Hot <- FALSE
> df$Hot[df$Temp>89] <- TRUE
> df$Hot <- factor(df$Hot)
> str(df)
```

The code above:
1. Creates a new column/attribute Hot, and assigns all it's values to FALSE
2. Then for row in HOT where Temp>89, it sets HOT to TRUE.
3. Then it factors it... *a factor is a qualitative variable that takes on  only one of set of values*. Because the Hot value is only either TRUE or FALSE, it can enumerate them, or store them as integers referencing the set of values

***!!! Lecture Notes !!!***
- Conditional Density Plot: cdplot ()
- \install("package")
- require(package) returns a boolean if it loaded

### NOTEBOOKS
As someone who set up Jupyter in the past for Python, I'm pretty interested in this. File, New File, and selecting R Notebook updated the Studio for me to be able use them.

Because I already understand how these works, it really is just a matter of figureing out how I want to interwieve my usage of Obsidian with R notebook. Really it should be as simple as having a link to a R notebook file (`.rmd`)  and opening it.

[[default-notebook.Rmd]] opens RStudio, but unfortunately there really isn't a way to open it inside of Obsidian. There is a Jupyter Plugin which allows an obsidian note to access a Python interpreter/kernel but I don't know if it is worth modding into Obsidian. However, RStudio does have a live preview mode of their notebook editor that mirrors the functionallity of obsidian.

This means I have to come to a conclusion:
1. Write all of my notes in Obsidian to utilize the easy linking of the software
2. Write all of my notes in RStudio and utilize the ability to write code in live preview
3. Do all of my notes in Obsidian and RStudio, referencing between the too...

I don't like any of these.

1. First I tried out the obsidian-jupyter plugin, and saw their was a pull request from 8 months ago that added support for[arbitrary kernel/language support](https://github.com/tillahoffmann/obsidian-jupyter/pull/26)
2. I saw version required you to install the kernel before hand, so I installed IRkernel via these [instructions](https://irkernel.github.io/installation/#binary-panel)
3. Now I need to figure out how to set up an Obsidian Dev environment so I can build the plguin with the changes neccessary for arbitary kernel support. Something I have done in the past, but not understood.