---
title: Classification Metrics
subtitle: How to calculate them and how to understand them
mathjax: true
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

### Fundamentals

| Graphic | Metric Name | Calculation | Plain Language Definition|
|--------------------|:------------:|:--------:|---------------------|
|![](/assets/img/FullLayout.png)|Full Population Diagram|$$ TP+FP+TN+FN $$ |The full population|
|![](/assets/img/TP.png)|True Positive| $$ TP $$ |The group of people get a positive result on their test for a disease _and_ truly have the disease.|
|![](/assets/img/TN.png)|True Negative| $$ TN $$ |The group of people who get a negative result on their test for a disease _and_ do not have the disease|
|![](/assets/img/FP.png)|False Positive| $$ FP $$ |The group of people who get a positive test result but do not have the disease.  Also called a Type I error.|
|![](/assets/img/FN.png)|False Negative| $$ FN$$ |The group of people who get a negative test result but _do_ have the disease.  Also called a Type II error.|
|![](/assets/img/allHealthy.png)|No Disease| $$ TN + FP$$  |All healthy individuals - some may get positive test results|
|![](/assets/img/allSick.png)|Has Disease| $$ TP + FN$$  |All people in the population who are infected - some may get still get negative test results|
|![](/assets/img/allPos.png)|Positive Tests| $$ TP + FP$$  |Everyone who gets a positive test result - this may include both sick and healthy individuals|
|![](/assets/img/allNeg.png)|Negative Tests| $$ TN + FN$$  |Everyone who gets a negative test result - this may include both sick and healthy individuals|

### Depicting how good your test is at detecting a disease in a population

| Graphic | Metric Name | Calculation | Plain Language Definition|
|:-----:|:------------:|:--------:|-------------------------------|
|![](/assets/img/TPSmall.png) <br> _______ <br> ![](/assets/img/TP-FNSmall.png)|Sensitivity|$$ \frac{TP}{TP+FN}$$ |True positive test results divided by total number of sick individuals. Also called "Recall" in information retrieval settings, this is the proportion of your positive tests actually caught disease. Put another way, what proportion of infected individuals did you identify.  Other names for same proportion are hit rate or true positive rate.  Inverse is called miss rate or false negative rate|
|![](/assets/img/TPSmall.png) <br> _______ <br> ![](/assets/img/TP-FPSmall.png)|Positive Predictive Value|$$ \frac{TP}{TP+FP}$$ |So you just tested positive for Disease X, how sure can you be that you have the disease? In an information retrieval setting, this called precision - for all the records you returned, how many were "good"? Inverse value is called false discovery rate.|

### Depicting how good your test is at identifying healthy individuals in a population

| Graphic | Metric Name | Calculation | Plain Language Definition|
|:-----:|:------------:|:--------:|-------------------------------|
|![](/assets/img/TNSmall.png) <br> _______ <br> ![](/assets/img/TN-FPSmall.png)|Specificity|$$ \frac{TN}{TN+FP}$$ |What proportion of the healthy individuals in the population did you find?  Put another way, given that a patient is healthy, what are chances they get a negative test? Inverse is called false positive rate.|
|![](/assets/img/TNSmall.png) <br> _______ <br> ![](/assets/img/TN-FNSmall.png)|Negative Predictive Value|$$ \frac{TN}{TN+FN}$$ |Given that your test has come back negative, how certain can you be that you're in the clear? Inverse value is called false omission rate|
