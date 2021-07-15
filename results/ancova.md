ANCOVA test for `fss`\~`dfs`+`scenario`\*`achievement`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “fss”](#ancova-plots-for-the-dependent-variable-fss)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for fss [../data/table-for-fss.csv](../data/table-for-fss.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | achievement | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr | symmetry | skewness | kurtosis |
|:-------------|:------------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|:---------|---------:|---------:|
| gamified     | lower       | fss      |   8 | 3.514 |  3.389 | 2.667 | 4.556 | 0.744 | 0.263 | 0.622 | 1.250 | YES      |    0.241 |   -1.798 |
| gamified     | upper       | fss      |   8 | 3.542 |  3.278 | 2.889 | 4.556 | 0.709 | 0.251 | 0.593 | 1.083 | YES      |    0.467 |   -1.707 |
| non.gamified | lower       | fss      |   7 | 3.810 |  3.667 | 3.222 | 4.556 | 0.543 | 0.205 | 0.502 | 0.833 | YES      |    0.226 |   -1.825 |
| non.gamified | upper       | fss      |   7 | 3.286 |  3.333 | 2.556 | 3.889 | 0.528 | 0.200 | 0.489 | 0.722 | YES      |   -0.178 |   -1.724 |
| NA           | NA          | fss      |  30 | 3.537 |  3.444 | 2.556 | 4.556 | 0.638 | 0.116 | 0.238 | 0.944 | YES      |    0.281 |   -1.212 |

![](/home/rstudio/report/ancova/c398fc7079f00970/results/ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(
"fss" = c("P03","P04","P07","P08","P19")
)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|     | var |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----|:----|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| fss | fss |  25 |    0.366 |   -1.069 | YES      |     0.957 | Shapiro-Wilk | 0.351 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | achievement | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:------------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|:----------|:-------------|----------:|------:|:---------|
| gamified     | lower       | fss      |   5 | 3.622 |  3.444 | 2.889 | 4.444 | 0.607 | 0.271 | 0.753 | 0.667 | YES       | Shapiro-Wilk |     0.970 | 0.876 | ns       |
| gamified     | upper       | fss      |   7 | 3.397 |  3.111 | 2.889 | 4.556 | 0.625 | 0.236 | 0.578 | 0.722 | YES       | Shapiro-Wilk |     0.842 | 0.104 | ns       |
| non.gamified | lower       | fss      |   7 | 3.810 |  3.667 | 3.222 | 4.556 | 0.543 | 0.205 | 0.502 | 0.833 | YES       | Shapiro-Wilk |     0.902 | 0.344 | ns       |
| non.gamified | upper       | fss      |   6 | 3.407 |  3.389 | 2.667 | 3.889 | 0.459 | 0.187 | 0.482 | 0.528 | YES       | Shapiro-Wilk |     0.917 | 0.483 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["fss"]], x=covar, y="fss", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](/home/rstudio/report/ancova/c398fc7079f00970/results/ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|       | var | method         | formula                           |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------|:----|:---------------|:----------------------------------|----:|--------:|--------:|----------:|------:|:---------|
| fss.1 | fss | Levene’s test  | `.res`\~`scenario`\*`achievement` |  25 |       3 |      21 |     0.070 | 0.975 | ns       |
| fss.2 | fss | Anova’s slopes | `.res`\~`scenario`\*`achievement` |  25 |       3 |      17 |     2.122 | 0.135 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|       | scenario     | achievement | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr |
|:------|:-------------|:------------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|
| fss.1 | gamified     | lower       | fss      |   5 | 3.622 |  3.444 | 2.889 | 4.444 | 0.607 | 0.271 | 0.753 | 0.667 |
| fss.2 | gamified     | upper       | fss      |   7 | 3.397 |  3.111 | 2.889 | 4.556 | 0.625 | 0.236 | 0.578 | 0.722 |
| fss.3 | non.gamified | lower       | fss      |   7 | 3.810 |  3.667 | 3.222 | 4.556 | 0.543 | 0.205 | 0.502 | 0.833 |
| fss.4 | non.gamified | upper       | fss      |   6 | 3.407 |  3.389 | 2.667 | 3.889 | 0.459 | 0.187 | 0.482 | 0.528 |

![](/home/rstudio/report/ancova/c398fc7079f00970/results/ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var | Effect               | DFn | DFd |   SSn |   SSd |     F |     p |   ges | p.signif |
|:----|:---------------------|----:|----:|------:|------:|------:|------:|------:|:---------|
| fss | dfs                  |   1 |  20 | 0.027 | 6.611 | 0.081 | 0.778 | 0.004 | ns       |
| fss | scenario             |   1 |  20 | 0.061 | 6.611 | 0.184 | 0.672 | 0.009 | ns       |
| fss | achievement          |   1 |  20 | 0.629 | 6.611 | 1.902 | 0.183 | 0.087 | ns       |
| fss | scenario:achievement |   1 |  20 | 0.069 | 6.611 | 0.208 | 0.654 | 0.010 | ns       |

### Pairwise comparison

| var | scenario     | achievement | group1   | group2       | estimate | conf.low | conf.high |    se | statistic |     p | p.adj | p.adj.signif |
|:----|:-------------|:------------|:---------|:-------------|---------:|---------:|----------:|------:|----------:|------:|------:|:-------------|
| fss | NA           | lower       | gamified | non.gamified |   -0.233 |   -1.010 |     0.544 | 0.373 |    -0.625 | 0.539 | 0.539 | ns           |
| fss | NA           | upper       | gamified | non.gamified |   -0.007 |   -0.675 |     0.660 | 0.320 |    -0.023 | 0.982 | 0.982 | ns           |
| fss | gamified     | NA          | lower    | upper        |    0.206 |   -0.511 |     0.922 | 0.344 |     0.599 | 0.556 | 0.556 | ns           |
| fss | non.gamified | NA          | lower    | upper        |    0.431 |   -0.269 |     1.131 | 0.336 |     1.284 | 0.214 | 0.214 | ns           |

### Descriptive Statistic of Estimated Marginal Means

| var | scenario     | achievement |   n | emmean |  mean | conf.low | conf.high |    sd | sd.emms | se.emms |
|:----|:-------------|:------------|----:|-------:|------:|---------:|----------:|------:|--------:|--------:|
| fss | gamified     | lower       |   5 |  3.600 | 3.622 |    3.040 |     4.160 | 0.607 |   0.601 |   0.269 |
| fss | gamified     | upper       |   7 |  3.394 | 3.397 |    2.941 |     3.848 | 0.625 |   0.575 |   0.218 |
| fss | non.gamified | lower       |   7 |  3.833 | 3.810 |    3.348 |     4.317 | 0.543 |   0.614 |   0.232 |
| fss | non.gamified | upper       |   6 |  3.402 | 3.407 |    2.910 |     3.893 | 0.459 |   0.577 |   0.236 |

### Ancova plots for the dependent variable “fss”

``` r
plots <- twoWayAncovaPlots(sdat[["fss"]], "fss", between
, aov[["fss"]], pwc[["fss"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `fss` \~ `scenario`

``` r
plots[["scenario"]]
```

![](/home/rstudio/report/ancova/c398fc7079f00970/results/ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

#### Plot for: `fss` \~ `achievement`

``` r
plots[["achievement"]]
```

![](/home/rstudio/report/ancova/c398fc7079f00970/results/ancova_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “dfs”, ANCOVA tests with
independent between-subjects variables “scenario” (gamified,
non.gamified) and “achievement” (lower, upper) were performed to
determine statistically significant difference on the dependent varibles
“fss”. For the dependent variable “fss”, there was not statistically
significant effects.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.
