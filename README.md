# Labelled package for R

This package is built on the new class `labelled` introduced by `haven` package
and propose additional functions to manipulate labelled data.

Please note that this package is still under active development.

## Basic manipulation


```r
library(labelled)

## Creating a labelled vector
v <- labelled(c(1,2,2,2,3,9,1,3,2,NA), c(yes = 1, no = 3, "don't know" = 9))
v
```

```
## <Labelled double> 
##  [1]  1  2  2  2  3  9  1  3  2 NA
## 
## Labels:
##  value      label is_na
##      1        yes FALSE
##      3         no FALSE
##      9 don't know FALSE
```

```r
## Adding a value label
val_label(v, 2) <- "maybe"

## Adding a variable label
var_label(v) <- "do you agree?"

## Specifying missing values
missing_val(v) <- c(9)
v
```

```
## <Labelled double> do you agree?
##  [1]  1  2  2  2  3  9  1  3  2 NA
## 
## Labels:
##  value      label is_na
##      1        yes FALSE
##      3         no FALSE
##      9 don't know  TRUE
##      2      maybe FALSE
```

```r
## Getting the variable label
var_label(v)
```

```
## [1] "do you agree?"
```

```r
## Getting the list of value labels
val_labels(v)
```

```
##        yes         no don't know      maybe 
##          1          3          9          2
```

```r
## Getting a specific label for a given value
val_label(v, 1)
```

```
## [1] "yes"
```

```r
## Getting the list of missing values
missing_val(v)
```

```
## don't know 
##          9
```

```r
## Specifying missing values when some of them doesn't have a label
# missing_val(v) <- c(8, 9) # will not be executed
# Error: no value label found for 8, please specify `force`

missing_val(v, force = FALSE) <- c(8, 9) # only 9 will be defined as a missing value
```

```
## no value label found for 8, no missing value defined
```

```r
v
```

```
## <Labelled double> do you agree?
##  [1]  1  2  2  2  3  9  1  3  2 NA
## 
## Labels:
##  value      label is_na
##      1        yes FALSE
##      3         no FALSE
##      9 don't know  TRUE
##      2      maybe FALSE
```

```r
missing_val(v, force = TRUE) <- c(8, 9) # a value label (equal to "8") will be created for 8
```

```
## some value label (8) have been created
```

```r
v
```

```
## <Labelled double> do you agree?
##  [1]  1  2  2  2  3  9  1  3  2 NA
## 
## Labels:
##  value      label is_na
##      1        yes FALSE
##      3         no FALSE
##      9 don't know  TRUE
##      2      maybe FALSE
##      8          8  TRUE
```

```r
## Redefining all value labels in one call
## Note: allows to reorder them
## Note 2: labels not provided will be removed 
## Note 3: missing values are preserved (as long than there is still a label)
val_labels(v) <- c(yes = 1, maybe = 2, nnno = 3, "don't know" = 9)
```

```
## some missing values (8) have been removed
```

```r
## Modifying an existing value label
val_label(v, 3) <- "no"

## Removing a specific value label
val_label(v, 3) <- NULL

## Removing the variable label
var_label(v) <- NULL

## Removing missing values
missing_val(v) <- NULL
v
```

```
## <Labelled double> 
##  [1]  1  2  2  2  3  9  1  3  2 NA
## 
## Labels:
##  value      label is_na
##      1        yes FALSE
##      2      maybe FALSE
##      9 don't know FALSE
```

```r
## Removing all value labels
## Note: the vector will be unclassed
val_labels(v) <- NULL
```

```
## `x` is not anymore a labelled variable
```

```r
v
```

```
##  [1]  1  2  2  2  3  9  1  3  2 NA
```

```r
## Adding a variable label will not change the class of v
var_label(v) <- "my variable label"
v
```

```
##  [1]  1  2  2  2  3  9  1  3  2 NA
## attr(,"label")
## [1] "my variable label"
```

```r
## Adding a value label to a non labelled vector will transform it into a labelled variable
val_label(v, 1) <- "yes"
```

```
## `x` has been converted to a labelled variable
```

```r
v
```

```
## <Labelled double> my variable label
##  [1]  1  2  2  2  3  9  1  3  2 NA
## 
## Labels:
##  value label is_na
##      1   yes FALSE
```
