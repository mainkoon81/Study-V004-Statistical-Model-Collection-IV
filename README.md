
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

In logistic regression, you're interested merely in whether the outcome happens, it does not matter when it happened. In contrast, in survival analysis, we're interested not just in whether the outcome of interest happens, but also **how long it took them to get that outcome**, that is, the time to event. Survival analysis explores the relation between features of interest and the time to any binary outcome. 





























