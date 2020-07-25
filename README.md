
# 00. Hierarchical Model
We have assumed that all the observations were independent so far, but there is often a natural grouping to our data points which leads us to believe that some observation pairs should be more similar to each other than to others. For example, let's say a company plan to sample 150 test products for quality check, but they do 30 from your location and then 30 from 4 other factory locations(collecting 30 from each of 5 locations). We might expect a product from your location to be more similar to another product from your batch than to a product from another locations' batch. And we might be able to account for the likely differences between products in our Poisson model by making it a hierarchical model. That is, your lambda is not only estimated directly from your 30 products, but also indirectly from the other 120 products leveraging this hierarchical structure. Being able to account for relationships in the data while estimating everything with the single model is a primary advantage of using hierarchical models. And most Bayesian Models are hierarchical. 
<img src="https://user-images.githubusercontent.com/31917400/48874302-7ff9af00-edea-11e8-835e-ff0b7ff2f098.jpg" />

From the above, we have 5 different factory locations(l1, l2, ...l5), and 150 samples from each location. This looks like a one way ANOVA model. **What make this a hierarchical model is that** instead of placing an independent prior for each of the Poisson λ parameters, we're going to assume that they(λ) come from a common distribution(here `GA(α, β)`) with hyper parameters - α, β - that need to be estimated as well. That means each α and β also has its own prior distribution - `P(α)`, `P(β)`   

How we might use hierarchical modeling to extend a linear model? 
<img src="https://user-images.githubusercontent.com/31917400/48876484-3911b680-edf6-11e8-892b-ec6e8ed8b284.jpg" />

The "Random Intercept model" as a hierarchical model assumes each "region" has its own intercept - `α1`, `α2`,.. instead of `β0`. And  all intercepts(α) come from a common distribution(here `N(μ, 𝜏)`) with hyper parameters - μ, 𝜏 - that need to be estimated as well. That means each μ and 𝜏 also has its own prior distribution - `P(μ)`, `P(𝜏)`.

### What's going on?
<img src="https://user-images.githubusercontent.com/31917400/69556755-40d1e100-0f9d-11ea-8fc8-589a3b5749f9.jpg" />

### Linear Model 01
<img src="https://user-images.githubusercontent.com/31917400/69586448-852fa200-0fda-11ea-840d-a9789629b05e.jpg" />
<img src="https://user-images.githubusercontent.com/31917400/69586456-8b258300-0fda-11ea-8e2f-d7caeac5bd1d.jpg" />

### Normal Hierarchical 02
<img src="https://user-images.githubusercontent.com/31917400/69587895-d772c200-0fde-11ea-8a6f-835381c96c85.jpg" />

### Poisson Hierarchical 03
<img src="https://user-images.githubusercontent.com/31917400/69635421-3c183600-104c-11ea-87db-614ed52e6f16.jpg" />

### Survival Analysis 04
<img src="https://user-images.githubusercontent.com/31917400/69587588-d9885100-0fdd-11ea-95c5-925a2967c32f.jpg" />









> Application
 - **Mixture models** provide a nice way to build nonstandard probability distributions from simper distributions, as well as to identify unlabeled clusters/populations in the data. Mixture models can be formulated **hierarchically**, allowing us to estimate unobserved(latent) variables in a technique called `data augmentation`.
 - **GLM** generalize normal linear regression models in the sense that the likelihood belongs to a more general class of distributions. `Data augmentation` techniques similar to those used for mixture models make GLMs amenable to **hierarchical modeling**. 
 - Observations from distinct **spatial locations** can exhibit dependence(just as observations collected across time are often correlated) For example, we might expect a measurement at location x to be more similar to `measurement y` 5 meters away than to `measurement z` 100 meters away. `State-space models` and `Non-parametric models` for response surfaces are common for spatial data. 
 - **DeepLearning models** involve layers of “neurons” that separate inputs from outputs, allowing nonlinear relationships. These **intermediate nodes** can be thought of as `latent variables` in a **hierarchical probabilistic model**, although Bayesian inference of neural networks is uncommon. 
 - **Bayesian Non-parametric models** move beyond inference for `parameters` to inference for `functions and distributions`. Finite-dimensional representations of the necessary priors often appear as **hierarchical models**. 2 of the most popular non-parametric priors are the `Gaussian process prior`(typically used as a prior on continuous functions), and `Dirichlet process prior`(as a prior on probability distributions). 


-----------------------------------------------------------------------------------------------------------
# 02. Basic Hierarchical Model: Regression
What's your priors?
<img src="https://user-images.githubusercontent.com/31917400/79686064-550cc800-8235-11ea-96e8-164c6717541f.jpg" /> 








-----------------------------------------------------------------------------------------------------------
# 03. Basic Survival(Time-to-Event) Model: Regression
What's your priors?

## Intro
In logistic regression, you're interested merely in whether the outcome happens, it does not matter when it happened. In contrast, in survival analysis, we're interested not just in whether the outcome of interest happens, but also **how long it took them to get that outcome**, that is, the time to event. Survival analysis explores the relation between features of interest and the time to any binary outcome. 

Two Core concepts
 - 1. Survival Function(probability)
 - 2. Censoring 

Time-To-Event Table
 - This table is used to measure the probability of event at a given time and the duration at varying times.
 - Everybody makes it past time zero, so the probability of event to time `t=0` is **1**, or **survival 100%**. This probability is technically known as the `survival function`, one of two core concepts in survival analysis. 
   - Let’s now say that two people die the day after they are enrolled. The life table then looks like this:
     <img src="https://user-images.githubusercontent.com/31917400/88457347-807b0080-ce7d-11ea-9660-13ce029886b4.jpg" /> 

   - The calculations continue in that way. However, it ignores the more realistic case when people drop out or are "lost to follow-up". The technical term for this is that **these people are censored**. `Censoring` has different forms, but the type due to people dropping out – or when people are still alive at the study end – is the most common.
   `Kaplan-Meier table` and associated plot is the simplest (but not the only) way of estimating the survival time when you have drop-outs. 
   - The plot of the **`survival function` versus `time`** is called the **[survival curve]**. The Kaplan-Meier method can be used to estimate this curve from the observed survival times without the assumption of an underlying probability distribution. One reason why the **KM method** is so popular is that it doesn't make any such assumptions.
   - For example,... the basic idea underlying Kaplan-Meier tables comes into play here: **Probability of surviving past day `t`** is simply 
     - = {**proportion** of survivors on day `t`} * {**probability** of surviving past day `t-1`}  
     <img src="https://user-images.githubusercontent.com/31917400/88457766-16645a80-ce81-11ea-9f86-8ded66f2b17f.jpg" /> 
   
   - then...plot the time column against the probability column, we end up with a survival curve. 
     <img src="https://user-images.githubusercontent.com/31917400/88457881-bf12ba00-ce81-11ea-8cf5-6fe9676a1cf0.jpg" /> 

   - Note that we might have some drop outs. These data are censored and should be treated differently. When a data is missing at time `t`, it seems the subject was alive at time `t`, but we don't know whether the subject has died or survived....

Kaplan-Mieier Method and Log-Rank test
 - KM method estimates the survival curve and yields the KM-table. 
 - The log-rank test compares the **survival time** by the given feature. 

  ```
  km_fit <- survfit( Surv(time_var, death_var) ~ 1 ) #No features but just having the intercept, denoted by “1”
  plot(km_fit, xlab="Survival times(days)", ylab="P(survival)")

  # The "times" argument gives us control over what time periods we want to see. it says output every day for the <first week>, then at <30, 60 and 90 days>, and then <every 90 days> thereafter.
  summary(km_fit, times = c(1:7,30,60,90*(1:10))) 

  # Let’s extend this by splitting the curve by gender:
  km_gender_fit <- survfit( Surv(time_var, death_var) ~ gender_var ) 
  plot(km_gender_fit, xlab="Survival times(days)", ylab="P(survival) by gender")

  # To compare survival by gender, we can run a "logrank" test...a sort of Contingency test..
  survdiff( Surv(time_var, death_var) ~ gender_var, rho=0 ) #With rho = 0,... which is the default so we don’t need to write this bit,... it yields the log-rank test. 
  ```






















