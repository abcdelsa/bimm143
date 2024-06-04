# Lab 6: functions
Elsa Chen (A16632961)

- [writing a simple function](#writing-a-simple-function)
- [for the lab worksheet](#for-the-lab-worksheet)
  - [write a function grade() to determine an overall grade from a
    vector of student
    homework](#write-a-function-grade-to-determine-an-overall-grade-from-a-vector-of-student-homework)
  - [use the function on the
    gradebook](#use-the-function-on-the-gradebook)

All functions in R have:

- a name
- input arguments
- the body (what to do to the arguments)

name \<- function(arg1, arg2, etc.){ what to do(arg1, arg2) }

## writing a simple function

a function to add numbers named `add()`

``` r
# what we want `add()` to do
x <- 10
y <- 10
x + y
```

    [1] 20

``` r
add <- function(x){
  y <- 10
  x + y
} # this function will take a value x and add 10 to it
```

remember to run the code before using the function

``` r
add(1)
```

    [1] 11

now to make the function more flexible

``` r
add <- function(x, y){
  x + y
}
```

``` r
add(1, 10)
```

    [1] 11

but now we need to input two numbers for the function to work. To change
this we can add a default to one of the arguments

``` r
add <- function(x, y = 1){
  x + y
}
```

``` r
add(5)
```

    [1] 6

## for the lab worksheet

### write a function grade() to determine an overall grade from a vector of student homework

assignment scores dropping the lowest single score. If a student misses
a homework (i.e. has an NA value) this can be used as a score to be
potentially dropped.

``` r
# Example input vectors to start with
student1 <- c(100, 100, 100, 100, 100, 100, 100, 90)
student2 <- c(100, NA, 90, 90, 90, 90, 97, 80)
student3 <- c(90, NA, NA, NA, NA, NA, NA, NA)
```

``` r
# what we want the `grade()` function to do
which.min(student2) # get the lowest number
```

    [1] 8

``` r
student1[-which.min(student2)]# remove the lowest number
```

    [1] 100 100 100 100 100 100 100

``` r
mean(student1) # average the values but doesn't work with NA values
```

    [1] 98.75

``` r
mean(student2, na.rm = TRUE) # now can remove all NAs
```

    [1] 91

``` r
student3[is.na(student3)] <- 0 # this can replace all NAs with 0
```

``` r
grade <- function(x){
  x[is.na(x)] <- 0 # replace NAs with 0
  mean(x[-which.min(x)]) # drops lower score and find mean
} 
```

``` r
grade(student1)
```

    [1] 100

``` r
grade(student2)
```

    [1] 91

``` r
grade(student3)
```

    [1] 12.85714

yay it works!

### use the function on the gradebook

``` r
url <- "https://tinyurl.com/gradeinput"
gradebook <- read.csv(url, row.names = 1)
View(gradebook)
```

``` r
grades <- apply(gradebook, 1, grade)
```

> who is the top scoring student?

``` r
which.max(grades)
```

    student-18 
            18 

``` r
max(grades)
```

    [1] 94.5

> which homework was toughest on students?

``` r
which.min(colMeans(gradebook, na.rm = TRUE))
```

    hw3 
      3 

``` r
min(colMeans(gradebook, na.rm = TRUE))
```

    [1] 80.8

> which homework was most predictive of overall score?

``` r
gradebook2 <- gradebook
gradebook2[is.na(gradebook2)] <- 0
```

``` r
which.max(apply(gradebook2, 2, cor, y = grades))
```

    hw5 
      5 
