## `Mixture model` means a linear combination of responses while `hierachicial model` means the nesting distribution of parameters. `Mixture` is used to reflect the presence of subpopulations within an overall population (but no need to identify the sub-population to which an individual observation belongs) and `Hierarchical` is used otherwise. 

# 00. {Hierarchical Model}
We have assumed that all the observations were independent so far, but there is often a natural grouping to our data points which leads us to believe that some observation pairs should be more similar to each other than to others. For example, let's say a company plan to sample 150 test products for quality check, but they do 30 from your location and then 30 from 4 other factory locations(collecting 30 from each of 5 locations). We might expect a product from your location to be more similar to another product from your batch than to a product from another locations' batch. And we might be able to account for the likely differences between products in our Poisson model by making it a hierarchical model. That is, your lambda is not only estimated directly from your 30 products, but also indirectly from the other 120 products leveraging this hierarchical structure. Being able to account for relationships in the data while estimating everything with the single model is a primary advantage of using hierarchical models. And most Bayesian Models are hierarchical. 
<img src="https://user-images.githubusercontent.com/31917400/48874302-7ff9af00-edea-11e8-835e-ff0b7ff2f098.jpg" />

From the above, we have 5 different factory locations(l1, l2, ...l5), and 150 samples from each location. This looks like a one way ANOVA model. **What make this a hierarchical model is that** instead of placing an independent prior for each of the Poisson λ parameters, we're going to assume that they(λ) come from a common distribution(here `GA(α, β)`) with hyper parameters - α, β - that need to be estimated as well. That means each α and β also has its own prior distribution - `P(α)`, `P(β)`   

How we might use hierarchical modeling to extend a linear model? 
<img src="https://user-images.githubusercontent.com/31917400/48876484-3911b680-edf6-11e8-892b-ec6e8ed8b284.jpg" />

The "Random Intercept model" as a hierarchical model assumes each "region" has its own intercept - `α1`, `α2`,.. instead of `β0`. And  all intercepts(α) come from a common distribution(here `N(μ, 𝜏)`) with hyper parameters - μ, 𝜏 - that need to be estimated as well. That means each μ and 𝜏 also has its own prior distribution - `P(μ)`, `P(𝜏)`.
<img src="https://user-images.githubusercontent.com/31917400/89210774-a3618f00-d5b8-11ea-8d79-590575be7dad.jpg" />

## > When `Y` ~ Binomial ?????
<img src="https://user-images.githubusercontent.com/31917400/89211262-7792d900-d5b9-11ea-9460-fcfe9d3e3b14.jpg" />

## > When `Y` ~ Gaussian 
<img src="https://user-images.githubusercontent.com/31917400/89212858-246e5580-d5bc-11ea-82a1-46d5cb821bc5.jpg" />

## > When `Y` ~ Poisson 
<img src="https://user-images.githubusercontent.com/31917400/89213880-b32fa200-d5bd-11ea-8907-ebd6d68a2532.jpg" />

## > When `Y` ~ Exponential 
<img src="https://user-images.githubusercontent.com/31917400/89229504-6fe32c80-d5d9-11ea-94f5-eda153e57880.jpg" />

> Application
 - **Mixture models** provide a nice way to build nonstandard probability distributions from simper distributions, as well as to identify unlabeled clusters/populations in the data. Mixture models can be formulated **hierarchically**, allowing us to estimate unobserved(latent) variables in a technique called `data augmentation`.
 - **GLM** generalize normal linear regression models in the sense that the likelihood belongs to a more general class of distributions. `Data augmentation` techniques similar to those used for mixture models make GLMs amenable to **hierarchical modeling**. 
 - Observations from distinct **spatial locations** can exhibit dependence(just as observations collected across time are often correlated) For example, we might expect a measurement at location x to be more similar to `measurement y` 5 meters away than to `measurement z` 100 meters away. `State-space models` and `Non-parametric models` for response surfaces are common for spatial data. 
 - **DeepLearning models** involve layers of “neurons” that separate inputs from outputs, allowing nonlinear relationships. These **intermediate nodes** can be thought of as `latent variables` in a **hierarchical probabilistic model**, although Bayesian inference of neural networks is uncommon. 
 - **Bayesian Non-parametric models** move beyond inference for `parameters` to inference for `functions and distributions`. Finite-dimensional representations of the necessary priors often appear as **hierarchical models**. 2 of the most popular non-parametric priors are the `Gaussian process prior`(typically used as a prior on continuous functions), and `Dirichlet process prior`(as a prior on probability distributions). 


-----------------------------------------------------------------------------------------------------------
## > Basic Hierarchical Model 01.
What's your priors?
<img src="https://user-images.githubusercontent.com/31917400/79686064-550cc800-8235-11ea-96e8-164c6717541f.jpg" /> 








-----------------------------------------------------------------------------------------------------------
## > Basic Hierarchical Model 02. Duration(Time-to-Event) 
What's your priors?







## Intro
In logistic regression, you're interested merely in whether the outcome happens, it does not matter when it happened. In contrast, in survival analysis, we're interested not just in whether the outcome of interest happens, but also **how long it took them to get that outcome**, that is, the time to event. Survival analysis explores the relation between features of interest and the time to any binary outcome. 

## Duration `t` modelling
Why it's special? Why LM won't work?
 - 1) all values `t` > 0
 - 2) multiple measures are of interest
   <img src="https://user-images.githubusercontent.com/31917400/89131359-1957ee80-d504-11ea-9e8c-a24be2fb3097.jpg" /> 
   
   - __For Survival probability:__ S(`t`) = P(T > `t`)
     - Q. What's the probability that the duration is longer than 5 years ?
     - Q. What's the typical duration ? the median time? (What is the `t` when the the survival probability is 0.5 ?)
     - Q. *Out of 100 unemployeed, how many, do we expect, to be back to work again in 5 months ? 
       <img src="https://user-images.githubusercontent.com/31917400/89131174-b6198c80-d502-11ea-9b6a-2e1f0fff23be.jpg" /> 
       
   - __For Hazard rate:__ λ(`t`) = {1-S(`t`)}' / S(`t`)
       <img src="https://user-images.githubusercontent.com/31917400/89231492-5ba12e80-d5dd-11ea-8efb-42aea830ceaa.jpg" /> 
       
 - 3) Censoring...(obv -> Not decided yet: 'survived' or 'missing'?) 
   <img src="https://user-images.githubusercontent.com/31917400/89123205-89934f80-d4c5-11ea-9cc2-70f909ea8080.jpg" /> 

# In short, we would be interested in something around the **event time `t`** and the **event rate `λ`**. 
 - ## S(`t`): Probability that the duration is longer than `t`
 - ## λ(`t`): Rate of particular event happening under observation at a time `t` 

----------------------------------------------------------------------------------------------------------------------
## [Tool I] Event time Analysis (Kaplan-Meier's Table)
This table is used to measure the probability of event at a given time and the duration at varying times. The probability of event to time `t=0` is **1**, or **survival 100%**. This probability is technically known as the `survival rate`. Let’s now say that two people die the day after they are enrolled. The life table then looks like this:
   <img src="https://user-images.githubusercontent.com/31917400/88457347-807b0080-ce7d-11ea-9660-13ce029886b4.jpg" /> 

 - The calculations continue in that way. However, it ignores the more realistic case when people drop out or are "lost to follow-up". The technical term for this is that **these people are censored**. `Censoring` refers to the type due to people dropping out – or when people are still alive at the study end – is the most common.

`Kaplan-Meier table` and associated plot is the simplest (but not the only) way of estimating the survival time when you have drop-outs. The plot of the **`survival rate` versus `time`** is called the **[survival curve]**. **This is all about the shirinking AUC**. The Kaplan-Meier method can be used to **estimate** this curve from the observed survival times without the assumption of an underlying probability distribution...so Non-parametric! One reason why the **KM method** is so popular is that it doesn't make any such assumptions. <img src="https://user-images.githubusercontent.com/31917400/89131888-1101b280-d508-11ea-9885-af748fcc5285.jpg" /> 

 - For example,... the basic idea underlying Kaplan-Meier tables comes into play here: **Probability of surviving past day `t`** is simply = {**proportion** of survivors on day `t`} * {**probability** of surviving past day `t-1`}  
   <img src="https://user-images.githubusercontent.com/31917400/88457766-16645a80-ce81-11ea-9f86-8ded66f2b17f.jpg" /> 
   
 - then...plot the time column against the probability column, we end up with a survival curve. 
   <img src="https://user-images.githubusercontent.com/31917400/88457881-bf12ba00-ce81-11ea-8cf5-6fe9676a1cf0.jpg" /> 

 - Note that we might have some drop outs. These data are censored and should be treated differently. When a data is missing at time `t`, it seems the subject was alive at time `t`, but we don't know whether the subject has died or survived....
 - But why use Kaplan-Meier? Why don't we just compute the simple cumulative distribution function and use 1−CDF as the estimate for the survivor curve? coz... we must think about `censoring`. Kaplan-Meier curves allow for this. Plus, it's a Non-parametric Method! No assumption required!

__> Do you have predictors? Log-Rank test__
KM method estimates the survival curve and yields the **KM-table**. The log-rank test compares the **survival time** by the given feature. 

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
[Note] Kaplan-Meier method and log-rank tests explore the relation between one predictor and mortality variable over time but they can only manage **one predictor**. In contrast, `Cox proportional hazards model` can handle multiple predictors as a type of regression.  

## [Tool II] Event Rate Analysis (Cox Proportional Hazards Model) 
The major theoretical development that this model provides is the ability to model **covariate effects in the presence of censored observations**. The **data** for this model, based on a sample of size `n`, consists of (![formula](https://render.githubusercontent.com/render/math?math=t_j,\delta_j,x_j)), j=1,2,..n where
 - ![formula](https://render.githubusercontent.com/render/math?math=t_j) is the possible event time on study for the j-th individual
 - ![formula](https://render.githubusercontent.com/render/math?math=\delta_j) is the event indicator(`1` if the event has occurred, `0` if the event has been censored).    
 
 - `hazard` refers to have the outcome of interest. The way the hazard changes over time is called the `hazard rate`. The thing is hazard function `λ(t)` is the probability of the event happening at time `t` given that it has not yet happened. Note that, in contrast to S(`t`) which focuses on **not having an event**, λ(`t`) focuses on the **event occurring**.
 - Usually in survival analysis, we are interested in the difference between `survival curves`(S(t) vs t) of different groups of individuals.
 - `proportional hazards` means that the hazards assumed by the model is proportional! For example, the hazard pattern in young patients should be proportional to those whon are old...? The two curves cannot be crossed! The proportionality of hazards is crucial and should be tested.. 

At the end of the day, `S(t)` and `λ(t)` together give the **Exponential Distribution** which the variable `t` follows. The Cox model allows us to examine how specified features influence the **`rate of a particular event happening: λ(t)`** at a particular point `t` in time. 
<img src="https://user-images.githubusercontent.com/31917400/89274361-bae05c80-d638-11ea-90d9-b90f5223fcc9.jpg" /> 

In most situations, we are interested in comparing groups with respect to their hazards, and we use a hazard ratio, which is analogous to an odds ratio in the setting of multiple logistic regression analysis. The hazard ratio is the ratio of the total number of observed to expected events in two independent comparison groups. HR is defined as the ratio of the **`predicated hazard rate`**(Output of Cox) under two different values of a predictor variable. 
 - HR = `COX model_trt` / `COX model_ctrl`

When the regression coefficient is (+), or equivalently a hazard ratio(HR) greater than one, indicates that as the value of predictors increases, the event hazard increases and thus the length of survival decreases. Put another way, a hazard ratio above 1 indicates a covariate that is positively associated with the event probability, and thus negatively associated with the length of survival.
 - `HR = 1`: No effect..the risk factor does not affect survival.
 - `HR < 1`: then it means Reduction in the hazard...so the treatment is NICE!
 - `HR > 1`: then it means Increase in Hazard...so the treatment is fucked up.

so...what's the COX prediction? Rate?
<img src="https://user-images.githubusercontent.com/31917400/89591975-6ceb7480-d843-11ea-8770-5598a999181c.jpg" /> 


## [Tool III] Bayesian Style Event time & rate Analysis: PGM-Cox model
yvjvv







> Missing data in survival analysis
  - Missing data are a common problem in research. The conclusions of analyses where the data are complete can be very different from analyses with incomplete data. How do you make sure your analysis yields the correct conclusion, even though the data are not complete?
    - First, you need to understand why some data are missing. This is important, because the techniques you decide to apply depend on the reason some data are missing. Be aware that there is no statistical test telling us why the data are missing. This is done by combining reason and knowledge on how the data were collected. Something I’ve emphasised throughout this course and the previous ones in the series is that there is no substitute for getting to know your data. Part of this is by tabulation and histograms etc, but another key part of it comes before any descriptive analysis – knowing how the data were generated and the potential for missing or invalid values in each data field. Let’s now recap patterns of missingness. 
    - We say that data are 'missing completely at random' (MCAR) when the complete cases (patients without any missing values for a given data item) are a random sample of the whole dataset (all patients). One patient is just as likely to have missing values as any other patient: males just as likely as females, older patients just as likely as younger ones etc. This can happen when a participant didn't have time to fill out the questionnaire or some information was lost or misplaced - and none of these things happened in a systematic way. This is the easiest situation to deal with, though sadly it’s often rather an unrealistic assumption.
    - More often, you’ll have to deal with data that are 'missing at random' (MAR). In this case, missingness can be explained by other variables for which there is full information. For example, if people with a higher education are less likely to disclose their income, then income is MAR because the chance of income values being missing depends on the patient’s education. In this situation, which is pretty common, you can “fill in” the missing values on the basis of another variable, so if you know their education you can predict their income well. Statistical methods exist to deal with this that are beyond the scope of this course, though I’ll list them briefly below.
    - Finally, data that are 'missing not at random' (MNAR) are neither MAR nor MCAR. For example, you could be missing medical information on the severity of diabetes when they are too ill to see a doctor and provide that information; missingness depends partly on the diabetes status, as is the case for MAR, but it also depends on the severity of illness, which can’t always be captured. In general, data are MNAR when the missingness is specifically related to what’s missing and so the probability of the value being missing depends on unobserved variables, i.e., variables not in your data set. This is generally the most problematic type.

  - Now that we know what we are talking about when we say missing data, we can have a look at different methods for dealing with incomplete data. Luckily, you only need to understand the general idea and pick the right tool, as the computer will do rest of the work. Here are some of the most used techniques for handling missing data.
    - __Complete case analysis__ (or available case analysis, or listwise deletion): In this approach, the cases with missing data are simply omitted from the analysis. If the data are MCAR, this will produce unbiased estimates as long as the sample size is still sufficiently large. If the data are MAR or MNAR, the estimates will be biased. That’s a good reason why you need to understand the reason for the missing values. It’s tempting to just hope they’re completely random, but you need to think through the problem, run some descriptive analyses and ask the data provider if necessary and possible.
    - __Mean substitution__ (or mean imputation): Replace (“impute”) the missing values of a variable, with the mean of the available values of the same variable. For example, if some male patients are missing values, then just assign them the overall mean value for the male patients who do have values. This has the advantage of not changing the overall mean for that variable. However, it artificially decreases the estimated variation. It also makes it difficult to detect correlations between the imputed variable and other variables. Hence mean substitution always gives biased results and is not recommended.
    - __Multiple imputation__: Missing variables are assumed to be MAR (or MCAR) and are imputed by drawing from a distribution. This is done multiple times and yields multiple different completed datasets. Each of these datasets is analysed, and the results are combined into a single overall result. Multiple imputation has been shown to yield unbiased results for MAR or MCAR data. It can be done in R.
    - __Maximum likelihood__: This approach also gives unbiased results for MAR (or MCAR) data. Data are assumed to be normally distributed with a certain (multivariate) mean and variance. Observed data are used to compute the mean and variance, and missing data are drawn from the resulting normal distribution. We draw many times from the distribution until the mean and variance of the completed data are as close as they can get to that of the observed data. MNAR data need to be handled on a case-by-case basis. Basically, it’s more complicated.

# 01. {Mixture Model}
Mixture models are probability distributions that can account for a variety of features in the data including multi-modality and skewness. The idea of mixture models is to take multiple probability distributions and put them together, using a linear combination. While mixtures in which all components belong to the same family are the most common type of mixtures used in practice, nothing prevents you from defining a mixture model in which each component belongs to a different family of distributions.
<img src="https://user-images.githubusercontent.com/31917400/104812902-618e7300-57fd-11eb-8782-d9b54103d749.jpg" /> 

If the `moment generating function` exists for each component, then the `MGF` of the mixture is simply a linear combination of the `mgf` of the components. This means if `MGF` of the mixture is expressed as <img src="https://render.githubusercontent.com/render/math?math=M_{X}(t)=E_{f}[e^\xt]=\int e^\xt f(x)dx ">, then this is the same as <img src="https://render.githubusercontent.com/render/math?math=M_{X}(t)=\sum_{k=1}^{K} w_{k}E_{g_{k}}[e^\xt]"> where <img src="https://render.githubusercontent.com/render/math?math=g_{k}"> is each local component. 




























































































































