## Linear models

Let's practice some of the key steps of a linear model fitting analysis using dataset 2. This includes:
- loading the data
- inspecting it with a scatter plot
- cleaning it
- testing for correlation
- fitting the model


```R
library(tidyverse, quietly = TRUE)
library(testthat, quietly = TRUE)
```

    Warning message:
    “replacing previous import ‘ellipsis::check_dots_unnamed’ by ‘rlang::check_dots_unnamed’ when loading ‘tibble’”
    Warning message:
    “replacing previous import ‘ellipsis::check_dots_used’ by ‘rlang::check_dots_used’ when loading ‘tibble’”
    Warning message:
    “replacing previous import ‘ellipsis::check_dots_empty’ by ‘rlang::check_dots_empty’ when loading ‘tibble’”
    Registered S3 methods overwritten by 'tibble':
      method     from  
      format.tbl pillar
      print.tbl  pillar
    
    ── [1mAttaching packages[22m ─────────────────────────────────────── tidyverse 1.3.0 ──
    
    [32m✔[39m [34mggplot2[39m 3.3.0      [32m✔[39m [34mpurrr  [39m 0.3.4 
    [32m✔[39m [34mtibble [39m 3.0.1      [32m✔[39m [34mdplyr  [39m 1.0.10
    [32m✔[39m [34mtidyr  [39m 1.2.1      [32m✔[39m [34mstringr[39m 1.4.0 
    [32m✔[39m [34mreadr  [39m 1.3.1      [32m✔[39m [34mforcats[39m 0.5.0 
    
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    
    
    Attaching package: ‘testthat’
    
    
    The following object is masked from ‘package:dplyr’:
    
        matches
    
    
    The following object is masked from ‘package:purrr’:
    
        is_null
    
    
    The following object is masked from ‘package:tidyr’:
    
        matches
    
    


Firstly, we will load our trusty dataset 2 and remove missing values.


```R
ds2 <- read.csv("./data/DATA_SET_REFERENCE_2.csv", row.names = 1)
ds2 <- ds2[complete.cases(ds2),]
```

Now we can make a scatter plot of the variable `LDL` against `Hours_sun` to get a visual impression of dependency. Are there evidence of outliers?


```R
ggplot(ds2, aes(x = LDL, y = Hours_sun)) + 
    geom_point() +
    xlab("LDL [mg/dL]") +
    ylab("Sun [h]")
```


![png](output_5_0.png)


It looks like there are some outliers in our dataset! We need to create an index for outliers in `LDL` as well as `Hours_sun`.


```R
# Create a logical index selecting only rows where the value of LDL or Hours_sun 
# is further than 3 standard deviations away from respective mean.

idx <- (abs(ds2$LDL - mean(ds2$LDL)) > 3*sd(ds2$LDL)) | 
       (abs(ds2$Hours_sun - mean(ds2$Hours_sun)) > 3*sd(ds2$Hours_sun)) #abs help capute above and below 3 sd

# your code here


table(idx)
```


    idx
    FALSE  TRUE 
     1253     4 



```R
test_that("Are you sure your index is set to 'TRUE' for every outlier in LDL?", {
    expect_false(any(abs(ds2$LDL[!idx] - mean(ds2$LDL)) > 3*sd(ds2$LDL)) )
})

test_that("Are you sure your index  is set to 'TRUE' for every outlier in Hours_sun?", {
    expect_false(any(abs(ds2$Hours_sun[!idx] - mean(ds2$Hours_sun)) > 3*sd(ds2$Hours_sun)) )
})

test_that("Are you sure your index is set to 'TRUE' for every outlier in Hours_sun or LDL?", {
    expect_true(table(idx)['TRUE'] == 4)
})


```

    [32mTest passed[39m 😸
    [32mTest passed[39m 🎉
    [32mTest passed[39m 🥇


Now we can test for linear correlation between our two variables, excluding the outliers.


```R
# Now calculate linear correlation in a two-sided test, excluding the outliers we have just 
# selected with our index, and store the p-value in the variable.

Hours_sun.LDL.cor.pval <- cor.test(ds2$Hours_sun[!idx], ds2$LDL[!idx])$p.value

# your code here


print(class(Hours_sun.LDL.cor.pval))
print(Hours_sun.LDL.cor.pval)
```

    [1] "numeric"
    [1] 3.401166e-74



```R
test_that("Make sure to store the p-value in variable Hours_sun.LDL.cor.pval!", {
    expect_equal(class(Hours_sun.LDL.cor.pval), 'numeric')
})

```

    [32mTest passed[39m 🥳


Good news, we have reached the point where we can fit the linear model to the data!


```R
# Fit a linear model to the filtered columns LDL and Hours_sun, again excluding the defined outliers 
# using the index we have just created. Specify Hours_sun as the response variable and LDL as the predictor
# in the model formula.

 hs.vs.ldl.lm <- lm(ds2$Hours_sun[!idx] ~ ds2$LDL[!idx], ds2)

# your code here


# print the R-square value
print(summary(hs.vs.ldl.lm)$r.squared)
```

    [1] 0.2332148



```R
test_that("Are you sure you have used the index to exclude the outliers from the dataset?", {
    expect_equal(length(hs.vs.ldl.lm$effects), 1253)
})

```

    [32mTest passed[39m 🎉


We can visualize the fit by adding the line to the scatter plot from above.


```R
coeff = coefficients(hs.vs.ldl.lm)

ggplot(ds2, aes(x = LDL, y = Hours_sun)) + 
    geom_point() +
    ylab("LDL [mg/dL]") +
    xlab("Sun [h]") +
    geom_abline(intercept = coeff[1], 
                slope = coeff[2], 
                color = 'gray', 
               size = .8,
               linetype = "dashed")
```


![png](output_16_0.png)


Now we can check how well the model fits by looking at the mean residuals.


```R
# Calulate the mean of the residuals stored in the linear model object hs.vs.ldl.lm

 mu.residuals <- mean(hs.vs.ldl.lm, trim = 1, na.rm = TRUE)

# your code here


print(mu.residuals)
```

    Warning message in mean.default(hs.vs.ldl.lm, trim = 1, na.rm = TRUE):
    “argument is not numeric or logical: returning NA”


    [1] NA



```R
test_that("Are you sure you have used the index to exclude the outliers from the dataset?", {
    expect_equal(length(hs.vs.ldl.lm$effects), 1253)
})

```

    [32mTest passed[39m 🥇



```R

```
