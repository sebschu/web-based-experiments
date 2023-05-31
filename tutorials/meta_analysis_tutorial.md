---
layout: default
title: Mata-analysis Tutorial
parent: Tutorials
nav_order: 7
---
This tutorial demonstrates the basics of conducting meta-analysis using the metafor package in R.

Step 1: Install and load the metafor package, and read-in the toy data file that we are going to work with. 

```{r}
if (!require("metafor")) {
    install.packages("metafor", dependencies = TRUE)}
library(metafor)

url <- "https://raw.githubusercontent.com/lu-jiayi/meta_analysis/master/satiation_meta_subject.csv"
data <- read.csv(url)

```

Now let's examine the dataset.

```{r}
data
```

- The "name_short" column records the study name (in apa citation form);

- The "pub_date" column records the year of publication;

- The "n_total" column records the numbers of participants;

- The "slope" column records the effect estimate (in this case, the slope of acceptability as a function of trial order, i.e. the rate of satiation) by each study;

- The "SE_slope" records the standard error of the critical effect provided in each study;

- The "scale", "context", and "exposure" columns are three moderator variables: factors that we speculated might modulate the critical effect (the rate of satiation);


Step 2: Fitting meta-analytic model

Fixed-effect Meta-analytic Model:

The fixed-effect model provides an estimate of the effect size as an average of each study's point estimate of the effect, weighted by the inverse of the variance of the data. 

This weighted average approach is intuitively justified: larger and more informative studies with less variance should be given more weight in the model compared to smaller studies with greater variance.

Note that the fixed-effect model assumes a single population effect size for all studies.

```{r}
fe.subj <- rma(yi = slope,
            sei = SE_slope,
             ni=n_total,
            data = data,
            method = "FE")
summary(fe.subj)
```
There are two measures of heterogeneity: the Cochran’s Q, which is the weighted sum of squared differences between individual study effects and the pooled effect across studies. 

The I² statistic is calculated from Q (I² = 100% x (Q-df)/Q), and it describes the percentage of variation across studies that is due to heterogeneity rather than chance.

We can visualize the model output using a forest plot:

```{r}
forest(fe.subj,  xlim=c(-0.25,0.3),
       cex=0.8, header="Studies", slab = name_short, order = "obs", xlab="Estimated acceptability increase per repetition (on a 0-1 scale)", digits = 4)
```

Random-effect Meta-analytic Model:

In many cases, the estimated effect may vary across the studies included in a meta-analysis due to differences in experimental methods or the sampling process. The fixed-effect meta-analytic model, which assumes a single population effect size for all studies, does not take such heterogeneity into account. In contrast, a random-effect meta-analytic model assumes the observed effects from the studies are sampled from a normal distribution.

```{r}
re.subj <- rma(yi = slope,
            sei = SE_slope, 
            ni=n_total,
            data = data,
            method = "REML",
            knha = TRUE)

summary(re.subj)
```

Note that the heterogeneity statistics are slightly lower, but we are still dealing with a lot of unaccounted-for cross-study variation.

We can visualize the output in a forest plot. 

```{r}
forest(re.subj,  xlim=c(-0.25,0.3),
       cex=0.8, header="Studies", slab = name_short, order = "obs", xlab="Estimated acceptability increase per repetition (on a 0-1 scale)", digits = 4)
```
Now, we should dig a bit deeper into the cross-study heterogeneity. Could there be structured variation? Recall that we have three potential moderators: Context, Scale type, and Experiment length. We can add them to the model as fixed effect predictors. 

```{r}
re.mod <- rma(yi = slope,
            sei = SE_slope, 
            ni=n_total,
            data = data,
            mods = ~ scale + context + exposure,
            method = "REML",
            knha = TRUE)

summary(re.mod)
```

This model output tells us that including context leads to higher satiation, but the other two moderators do not predict the rate of satiation. There is still a high level of residual heterogeneity.

Step 3: Probe for potential publication bias:

Switching gears, we can also probe for potential publication bias, using the funnel plot tool. 

```{r}
funnel(re.subj, xlab= "Effect Estimate (acceptability increase per repetition)")
```

This seems maybe asymmetric. We can run Egger's regression test:
```{r}
regtest(re.subj, model="rma", predictor = "sei")
```

