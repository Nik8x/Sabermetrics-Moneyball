Sample\_Students\_Grades
================
Niket

``` r
sample.grades = read.table(file.choose(), header = T) # select and load grades.txt # With read.table load data either from a web source or from a local file.
head(sample.grades)
```

    ##   Student_ID Semester Grades
    ## 1          1  14_Fall     82
    ## 2          2  14_Fall     73
    ## 3          3  14_Fall     88
    ## 4          4  14_Fall     96
    ## 5          5  14_Fall     77
    ## 6          6  14_Fall     51

------------------------------------------------------------------------

**Data Cleaning**

``` r
# read.table() does some data cleaning.
summary(sample.grades)
```

    ##    Student_ID         Semester      Grades     
    ##  Min.   :  1.0   14_Fall  :48   Min.   :21.00  
    ##  1st Qu.: 32.5   15_Fall  :51   1st Qu.:72.00  
    ##  Median : 64.0   15_Spring:28   Median :81.50  
    ##  Mean   : 64.0                  Mean   :77.98  
    ##  3rd Qu.: 95.5                  3rd Qu.:87.03  
    ##  Max.   :127.0                  Max.   :99.28

``` r
# for semester we get frequencies
```

``` r
sample.grades$Student_ID <- as.factor(sample.grades$Student_ID) # Convert it to factor
```

``` r
# Alternatively set it to a row name (and remove it from the dataset)
row.names(sample.grades) <- as.character(sample.grades$Student_ID)
sample.grades <- sample.grades[, -1]
sample.grades["1", ]
```

    ##   Semester Grades
    ## 1  14_Fall     82

------------------------------------------------------------------------

**readLines**

``` r
data2 = readLines(file.choose())
```

    ## Warning in readLines(file.choose()): incomplete final line found on 'C:
    ## \Users\nick2\Documents\R\GitHub\Extra\sample_grades\Grades.txt'

``` r
grades.raw = strsplit(data2, "\t") # Split each line by tab

grades.colnames <- grades.raw[[1]] # First line is the column names so let's save them

# Creating vectors for each variable and identify their variable type
student.ids <- sapply(grades.raw[-1], function(x) x[1])
semester <- sapply(grades.raw[-1], function(x) x[2])
s.grades <- sapply(grades.raw[-1], function(x) x[3])

# Create a dataframe and identifying the variable types all at once
grades.df <- data.frame(semester = as.factor(semester), grade = as.numeric(s.grades), row.names = student.ids)

summary(grades.df)
```

    ##       semester      grade      
    ##  14_Fall  :48   Min.   :21.00  
    ##  15_Fall  :51   1st Qu.:72.00  
    ##  15_Spring:28   Median :81.50  
    ##                 Mean   :77.98  
    ##                 3rd Qu.:87.03  
    ##                 Max.   :99.28
