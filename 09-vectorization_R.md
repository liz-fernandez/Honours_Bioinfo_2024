---
title: Vectorization
teaching: 10
exercises: 15
source: Rmd
---

> ## Learning Objectives {.objectives}
> 
> * To understand vectorized operations in R.
> 
> 
> ##### Questions {.questions}
> 
> * How can I operate on all the elements of a vector at once?
> 

~~~ {.r}
gapminder <- read.csv("data/gapminder_data.csv", header = TRUE)
library("ggplot2")
~~~

Most of R's functions are vectorized, meaning that the function will
operate on all elements of a vector without needing to loop through
and act on each element one at a time. This makes writing code more
concise, easy to read, and less error prone.

~~~ {.r}
x <- 1:4
x * 2
~~~
~~~ {.output}
[1] 2 4 6 8
~~~

The multiplication happened to each element of the vector.

We can also add two vectors together:

~~~ {.r}
y <- 6:9
x + y
~~~
~~~ {.output}
[1]  7  9 11 13
~~~

Each element of `x` was added to its corresponding element of `y`:

~~~ {.r}
x:  1  2  3  4
    +  +  +  +
y:  6  7  8  9
---------------
    7  9 11 13
~~~

Here is how we would add two vectors together using a for loop:

~~~ {.r}
output_vector <- c()
for (i in 1:4) {
  output_vector[i] <- x[i] + y[i]
}
output_vector
~~~
~~~ {.output}
[1]  7  9 11 13
~~~

Compare this to the output using vectorised operations.

~~~ {.r}
sum_xy <- x + y
sum_xy
~~~
~~~ {.output}
[1]  7  9 11 13
~~~


> ## Challenge 1 {.challenge}
> 
> Let's try this on the `pop` column of the `gapminder` dataset.
> 
> Make a new column in the `gapminder` data frame that
> contains population in units of millions of people.
> Check the head or tail of the data frame to make sure
> it worked.
> 
> 
> ##### Solution
> 
> Let's try this on the `pop` column of the `gapminder` dataset.
> 
> Make a new column in the `gapminder` data frame that
> contains population in units of millions of people.
> Check the head or tail of the data frame to make sure
> it worked.
> 
> ~~~ {.r}
> gapminder$pop_millions <- gapminder$pop / 1e6
> head(gapminder)
> ~~~




> ## Challenge 2 {.challenge}
> 
> On a single graph, plot population, in
> millions, against year, for all countries. Do not worry about
> identifying which country is which.
> 
> Repeat the exercise, graphing only for China, India, and
> Indonesia. Again, do not worry about which is which.
> 
> 
> ##### Solution
> 
> Refresh your plotting skills by plotting population in millions against year.
> 
> ~~~{.r}
> ggplot(gapminder, aes(x = year, y = pop_millions)) +
>  geom_point()
> countryset <- c("China","India","Indonesia")
> ggplot(gapminder[gapminder$country %in% countryset,],
>        aes(x = year, y = pop_millions)) +
>   geom_point()
> ~~~


Comparison operators, logical operators, and many functions are also
vectorized:

**Comparison operators**

~~~ {.r}
x > 2
~~~
~~~ {.output}
[1] FALSE FALSE  TRUE  TRUE
~~~


**Logical operators**

~~~ {.r}
a <- x > 3  # or, for clarity, a <- (x > 3)
a
~~~
~~~ {.output}
[1] FALSE FALSE FALSE  TRUE
~~~


> ## Tip: some useful functions for logical vectors {.callout}
> 
> `any()` will return `TRUE` if *any* element of a vector is `TRUE`.  
> `all()` will return `TRUE` if *all* elements of a vector are `TRUE`.



Most functions also operate element-wise on vectors:

**Functions**

~~~ {.r}
x <- 1:4
log(x)
~~~
~~~ {.output}
[1] 0.0000000 0.6931472 1.0986123 1.3862944
~~~

Vectorized operations work element-wise on matrices:

~~~ {.r}
m <- matrix(1:12, nrow=3, ncol=4)
m * -1
~~~
~~~ {.output}
     [,1] [,2] [,3] [,4]
[1,]   -1   -4   -7  -10
[2,]   -2   -5   -8  -11
[3,]   -3   -6   -9  -12
~~~


> ## Tip: element-wise vs. matrix multiplication {.callout}
> 
> Very important: the operator `*` gives you element-wise multiplication!
> To do matrix multiplication, we need to use the `%*%` operator:
> 
> ~~~ {.r}
> m %*% matrix(1, nrow=4, ncol=1)
> matrix(1:4, nrow=1) %*% matrix(1:4, ncol=1)
> ~~~
> 
> For more on matrix algebra, see the [Quick-R reference
> guide](https://www.statmethods.net/advstats/matrix.html)




> ## Challenge 3 {.challenge}
> 
> Given the following matrix:
> 
> ~~~ {.r}
> m <- matrix(1:12, nrow=3, ncol=4)
> m
> ~~~
> 
> Write down what you think will happen when you run:
> 
> 1. `m ^ -1`
> 2. `m * c(1, 0, -1)`
> 3. `m > c(0, 20)`
> 4. `m * c(1, 0, -1, 2)`
> 
> Did you get the output you expected? If not, ask a helper!
> 
> 
> ##### Solution
> 
> Given the following matrix:
> 
> ~~~ {.r}
> m <- matrix(1:12, nrow=3, ncol=4)
> m
> ~~~
> 
> Write down what you think will happen when you run:
> 
> 1. `m ^ -1`
> 
> ~~~{r, echo=FALSE}
> m ^ -1
> ~~~
> 
> 2. `m * c(1, 0, -1)`
> 
> ~~~{r, echo=FALSE}
> m * c(1, 0, -1)
> ~~~
> 
> 3. `m > c(0, 20)`
> 
> ~~~{r, echo=FALSE}
> m > c(0, 20)
> ~~~




> ## Challenge 4 {.challenge}
> 
> We're interested in looking at the sum of the
> following sequence of fractions:
> 
> ~~~ {.r}
>  x = 1/(1^2) + 1/(2^2) + 1/(3^2) + ... + 1/(n^2)
> ~~~
> 
> This would be tedious to type out, and impossible for high values of
> n.  Use vectorisation to compute x when n=100. What is the sum when
> n=10,000?
> 
> 
> ##### Solution
> 
> We're interested in looking at the sum of the
> following sequence of fractions:
> 
> ~~~ {.r}
>  x = 1/(1^2) + 1/(2^2) + 1/(3^2) + ... + 1/(n^2)
> ~~~
> 
> This would be tedious to type out, and impossible for
> high values of n.
> Can you use vectorisation to compute x, when n=100?
> How about when n=10,000?
> 
> ~~~ {.r}
> sum(1/(1:100)^2)
> sum(1/(1:1e04)^2)
> n <- 10000
> sum(1/(1:n)^2)
> ~~~
> 
> We can also obtain the same results using a function:
> 
> ~~~ {.r}
> inverse_sum_of_squares <- function(n) {
>   sum(1/(1:n)^2)
> }
> inverse_sum_of_squares(100)
> inverse_sum_of_squares(10000)
> n <- 10000
> inverse_sum_of_squares(n)
> ~~~



> ## Tip: Operations on vectors of unequal length {.callout}
> 
> Operations can also be performed on vectors of unequal length, through
> a process known as *recycling*. This process automatically repeats the smaller vector
> until it matches the length of the larger vector. R will provide a warning
> if the larger vector is not a multiple of the smaller vector.
> 
> ~~~ {.r}
> x <- c(1, 2, 3)
> y <- c(1, 2, 3, 4, 5, 6, 7)
> x + y
> ~~~
> 
> Vector `x` was recycled to match the length of vector `y`
> 
> ~~~ {.r}
> x:  1  2  3  1  2  3  1
>     +  +  +  +  +  +  +
> y:  1  2  3  4  5  6  7
> -----------------------
>     2  4  6  5  7  9  8
> ~~~


## Keypoints {.objectives}

* Use vectorized operations instead of loops.



