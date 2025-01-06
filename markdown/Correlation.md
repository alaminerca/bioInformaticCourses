## Correlation

The goal of this exercise is to perform a correlation analysis. We will use dataset 2, analysing patient Weight with respect to the blood LDL level.


Let's take a close look at the dependency of the body weight to the different variables in our patient dataset 2.


```R
require(testthat, quietly = TRUE)

# Load dataset 2 ("./data/DATA_SET_REFERENCE_2.csv")
# and make sure the quality control is performed.

ds2 <- read.csv("./data/DATA_SET_REFERENCE_2.csv", row.names = 1)
summary(ds2)

ds2 <- ds2[complete.cases(ds2),]
```

A scatterplot suggests a relationship between our variables Hours_sun and LDL. How do we quantify such a 
relationship? Lets calculate the covariance according to this formula in R:

$ cov(x,y) = \frac{\sum(x_i - \mu_x) (y_i - \mu_y)}{ N }$


```R
# Calculate and return the covariance between variable "Weight" and "LDL". Round the obtained value
# to three decimal places after the comma and assign it to the requested variable.

# Weight_LDL_cov <- 

# your code here


# check the obtained value:
print(Weight_LDL_cov)
```


```R
test_that("The covariance variable type needs to be 'numeric'", {
    expect_equal(class(Weight_LDL_cov), 'numeric')
})

```

Now let's calculate the correlation instead:

$ cor(x,y) = \rho_{x,y} = \frac{cov_{x,y}}{ \sigma_x  \sigma_y } $


```R
# Now calculate and return the Pearson correlation to see for ourselves if correlation is 
# independent of the data scaling. Again, round the correlation value to three decimal places:

# Weight_LDL_cor <-

# your code here


# First let's make sure the variable has the correct type:
print(class(Weight_LDL_cor))

# and then check the actual value
print(Weight_LDL_cor)
```


```R
test_that("The correlation variable type needs to be 'numeric'", {
    expect_equal(class(Weight_LDL_cor), 'numeric')
})

test_that("The correlation 'Weight_LDL_cor' must be in the range [-1,1]", {
    expect_true(Weight_LDL_cor >= -1 & Weight_LDL_cor <= 1)
})
```


```R
# Calculate the p-value for the correlation between Weight and LDL, allowing either 
# positive or negative correlation as alternative hypothesis:

# weight.ldl.test.pval <- 

# your code here


# Let's check the type of the obtained value, remember we are looking for the p-value of the correlation value:
print(class(weight.ldl.test.pval))

# And now let's check the value itself:
print(weight.ldl.test.pval)
```


```R
test_that("The correlation p-value needs to be 'numeric'", {
    expect_equal(class(weight.ldl.test.pval), 'numeric')
})

test_that("The correlation p-value 'weight.ldl.test.pval' must be in the range [0,1]", {
    expect_true(weight.ldl.test.pval >= 0 & weight.ldl.test.pval <= 1)
})

```


```R
# Now calculate and return the p-value for the positive correlation between Weight and LDL

# weight.ldl.pos.cor.pval <- 


# your code here


print(class(weight.ldl.pos.cor.pval))
print(weight.ldl.pos.cor.pval)
```


```R
test_that("The correlation p-value 'weight.ldl.pos.cor.pval' needs to be 'numeric'", {
    expect_equal(class(weight.ldl.pos.cor.pval), 'numeric')
})

test_that("The correlation p-value 'weight.ldl.pos.cor.pval' must be in the range [0,1]", {
    expect_true(weight.ldl.pos.cor.pval >= 0 & weight.ldl.pos.cor.pval <= 1)
})

```


```R
# And finally, calculate the p-value for the negative correlation between Weight and LDL

# weight.ldl.neg.cor.pval <- 


# your code here


print(class(weight.ldl.neg.cor.pval))
print(weight.ldl.neg.cor.pval)
```


```R
test_that("The correlation p-value 'weight.ldl.neg.cor.pval' needs to be 'numeric'", {
    expect_equal(class(weight.ldl.neg.cor.pval), 'numeric')
})

test_that("The correlation p-value 'weight.ldl.neg.cor.pval' must be in the range [0,1]", {
    expect_true(weight.ldl.neg.cor.pval >= 0 & weight.ldl.neg.cor.pval <= 1)
})

```


```R

```
