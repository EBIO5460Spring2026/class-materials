# Assignment 1

**Due:** Tue 27 Jan 3:30 PM

**Grading criteria:** Complete all the check boxes below. On time submission.

**Percent of grade:** 7%

**Format for submitting solutions:**

* Submit only one file of R or Python code.
* The filename should be `ml4e_assignment1.R` or `ml4e_assignment1.py`. This will help me find the assignments in your repository. 

**Push your files to your GitHub repository**



## **Learning goals:** 

* Understand the steps of the cross validation algorithm
* Build competence in translating algorithms to code
* Practice tuning a machine learning algorithm using the CV inference algorithm

Investigating algorithms line by line or coding algorithms from scratch gives you much deeper understanding for how they work, provides detailed knowledge of what they are actually doing, and builds intuition that you can draw on throughout your career.



## Understanding the random partitions algorithm

Here is the random partitions algorithm that we used in class:

The R version

```R
# Function to divide a dataset into random partitions for cross-validation
# n:       length of dataset (scalar, integer)
# k:       number of partitions (scalar, integer)
# return:  partition labels (vector, integer)
# 
random_partitions <- function(n, k) {
    min_n <- floor(n / k)
    extras <- n - k * min_n
    labels <- c(rep(1:k, each=min_n),rep(seq_len(extras)))
    partitions <- sample(labels, n)
    return(partitions)
}
```

The Python version

```python
# Function to divide a data set into random partitions for cross-validation
# n:       length of dataset (scalar, integer)
# k:       number of partitions (scalar, integer)
# rng:     numpy random generator, set ahead rng = np.random.default_rng()
# return:  partition labels ranging from 0 to k-1 (vector, integer)
# 
def random_partitions(n, k, rng):
    min_n = n // k
    extras = n - k * min_n
    labels = np.concatenate([np.repeat(np.arange(k), min_n), 
             np.arange(extras)])
    partitions = rng.choice(labels, n, replace=False)
    return partitions
```

- [ ] Choose either the R or Python version, and describe what each line of code does in the above random partitions algorithm



## Coding the LOOCV algorithm


The leave one out cross validation algorithm is a special case of the k-fold CV algorithm. We can use the k-fold CV algorithm that we coded in class to do LOOCV by setting k equal to the number of data points. But LOOCV is a special case that suggests an even simpler algorithm. This algorithm **does not need** the function `random_partitions()`. Code up the LOOCV algorithm in R or Python from the following pseudocode (literally translate the pseudocode line by line).

```
# LOOCV algorithm
# for each data point
#     fit model without point
#     predict for that point
#     measure prediction error (compare to observed)
# CV_error = mean error across points
```

Use the first section of [02_3_ants_cv_polynomial.R](02_3_ants_cv_polynomial.R) or [02_3_ants_cv_polynomial.py](02_3_ants_cv_polynomial.py) to get going with reading in the data and using a polynomial model.

- [ ] As we did for coding the k-fold CV algorithm, first code the LOOCV algorithm line by line. Include this line-by-line version in your submission.

- [ ] Then turn it into a function. Include the function separately from your original code.

- [ ] Finally, use the function to investigate the LOOCV error for different orders of the polynomial model to determine the order with the best predictive accuracy. This code will be substantially similar to the code we wrote in class but you'll be using the LOOCV function you just wrote.

