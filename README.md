
# 00. Hierarchical Model
We have assumed that all the observations were independent so far, but there is often a natural grouping to our data points which leads us to believe that some observation pairs should be more similar to each other than to others. For example, let's say a company plan to sample 150 test products for quality check, but they do 30 from your location and then 30 from 4 other factory locations(30 from each of 5 locations). We might expect a product from your location to be more similar to another product from your batch than to a product from another locations' batch. And we might be able to account for the likely differences between products in our Poisson model by making it a hierarchical model. That is, your lambda is not only estimated directly from your 30 products, but also indirectly from the other 120 products leveraging this hierarchical structure. Being able to account for relationships in the data while estimating everything with the single model is a primary advantage of using hierarchical models. And most Bayesian Models are hierarchical. 
<img src="https://user-images.githubusercontent.com/31917400/48874302-7ff9af00-edea-11e8-835e-ff0b7ff2f098.jpg" />

How we might use hierarchical modeling to extend a linear model? 
<img src="https://user-images.githubusercontent.com/31917400/48876484-3911b680-edf6-11e8-892b-ec6e8ed8b284.jpg" />

> Application
 - **Mixture models** provide a nice way to build nonstandard probability distributions from simper distributions, as well as to identify unlabeled clusters/populations in the data. Mixture models can be formulated **hierarchically**, allowing us to estimate unobserved(latent) variables in a technique called `data augmentation`.
 - **GLM** generalize normal linear regression models in the sense that the likelihood belongs to a more general class of distributions. `Data augmentation` techniques similar to those used for mixture models make GLMs amenable to **hierarchical modeling**. 
 - Observations from distinct **spatial locations** can exhibit dependence(just as observations collected across time are often correlated) For example, we might expect a measurement at location x to be more similar to `measurement y` 5 meters away than to `measurement z` 100 meters away. `State-space models` and `Non-parametric models` for response surfaces are common for spatial data. 
 - **DeepLearning models** involve layers of “neurons” that separate inputs from outputs, allowing nonlinear relationships. These **intermediate nodes** can be thought of as `latent variables` in a **hierarchical probabilistic model**, although Bayesian inference of neural networks is uncommon. 
 - **Bayesian Non-parametric models** move beyond inference for `parameters` to inference for `functions and distributions`. Finite-dimensional representations of the necessary priors often appear as **hierarchical models**. 2 of the most popular non-parametric priors are the `Gaussian process prior`(typically used as a prior on continuous functions), and `Dirichlet process prior`(as a prior on probability distributions). 








# 01. Latent Variable Model
### What is Latent Variable?
Latent variable is just a random variable which is unobservable to you nor in training nor in test phase. This is the variable you can't just measure with some quantitative scale. 
<img src="https://user-images.githubusercontent.com/31917400/48974117-ebd85380-f046-11e8-913b-f788ec6bf63f.jpg" />

> __[Note]:__  
 - `Latent variable` acting like a **parameter**, can make things interpretable as we, for example, can estimate his intelligence on the scale from 1 to 100 although you don't know what the "scale" means. However, you can compare each data point according to this scale because **`it gives weights`**!
> Sometimes adding latent variables **restrict(overly simplify) your model** too much. If, for example, a student is doing 2 tests in the same day, it doesn’t make sense to assume that these two grades are caused only by his intelligence and doesn’t influence each other directly. Even if we know that he is very smart, if he failed the first test he is more likely to fail the second one because he may have a headache or maybe he didn’t have time to prepare the day before.

## Latent Variable Modeling_01> [Probabilistic Clustering with GMM]
> 1. Let's say, several datasets were hacked and mixed up..How to retrieve the originals? The assumption is each dataset follows a certain statistical distribution....perhaps Gaussian.  
 - Latent variable: membership labeling vectors(0,1,0,..): **Z_i**
   - membership prob vector (weight) = **`P(membership labeling vectors(0,1,0,..))`** 
 
> 2. Let's say, for a set of given customers, if we know how much money they withdraw / deposit into the game app this month, let's see if we can predict if they will continue using the game app next month or not...Gaussian_YES-churning VS Gaussian_NO-churning.
 - Latent variable: membership labeling vectors(0,1,0,..): **Z_i**
   - membership prob vector (weight) = **`P(membership labeling vectors(0,1,0,..))`** 

__Hard/Soft Clustering:__ Usually clustering is done in a hard way, so for each data point we assign its **membership**. But sometimes, instead of assigning each data point a particular membership, we will assign `each data point` from **probability** distributions over clusters(for example, 40% probability to belong to the A_cluster, and 60% probability to belong to the B_cluster, and 0% to the C_cluster). That is, instead of just assigning each data point a particular cluster, we assume that each data point belongs to every cluster, but with some **different probabilities**: `P(membership|x)` instead of `membership = f(x)`. 

### Why Soft Clustering?
 - 1. For handling **missing data**(latent, hidden variable)
 - 2. For tunning **hyperparameters**(meta-parameters...it can be the No.of clusters of course)
 <img src="https://user-images.githubusercontent.com/31917400/51439273-b69b5b00-1caf-11e9-99ee-a39f00c652bc.jpg" /> 
 
 - 3. For building a **generative model** of our data.. treating everything probabilistically, we may sample new data points from our model of the data.

### A. Gaussian Mixture Model: Soft Clustering with EM-Algorithm
__GMM:__ For a given dataset, each point is generated by linearly combining multiple multivariate Gaussians. And each Gaussian is a cluster.  
 <img src="https://user-images.githubusercontent.com/31917400/66716974-d9d0e200-edcb-11e9-857e-b0ec66af3523.jpg" />
 <img src="https://user-images.githubusercontent.com/31917400/66709023-465bca80-ed53-11e9-9889-53ba5b8f842e.jpg" />
 
**Hidden Variable:** Each Z is a membership vector. (0,0,1) or (0,1,0) or (1,0,0) : These vectors can be the column names(membership names) in the dataset if you want.
 <img src="https://user-images.githubusercontent.com/31917400/66716730-1d761c80-edc9-11e9-84e0-d44cec76e9e6.JPG" /> 

__General form of EM-Algorithm:__ This is an algorithm for obtaining MLE/MAP of θ **`when some data is missing`**(latent variables). When the likelihood(data) is a member of Exponential family, it is particularly applicable. For example, HMM(Hidden Markov) for ?, or GMM(Gaussian Mixture) for clustering. so... it's a point estimate at the end? 
 - Introduce a **hidden variable** and **`make a joint`** such that its knowledge would simplify the maximization of the likelihood. 
 - The weight(it's another parameter) comes from the **joint** over **marginal**.
 <img src="https://user-images.githubusercontent.com/31917400/66720528-b15ddd80-edf5-11e9-9deb-904c8db6fe6d.jpg" />
 
> **Such latant variable behaves like new parameter**? 
# Weight!!! is a posterior.  
------------------------------------------------------------------------------------------------------------------------

**How to fit it?**(How to find the model parameters?) 
 - The simplest way to fit a probability distribution is to use **maximum likelihood**. Find the parameters maximizing the likelihood(density)! 
 - Setting:
   - Given `X` is our data variable = {x1,x2,...xn} 
   - Given `Z` is **latent** variable (memberships) = {vector, vector, ...}
   - Joint `X, Z` likelihood ~ some distribution(exponential family) with unknown θ
 - Problem: <img src="https://user-images.githubusercontent.com/31917400/66717151-bd35a980-edcd-11e9-967a-ecb42f9ff7fe.jpg" />
 
   - But the issue here is that `maximizing the marginal of X is difficult coz the joint distribution can be "multimodal"`!!! 
   
## EM can address this by iteratively improving our parameter estimate.
<img src="https://user-images.githubusercontent.com/31917400/51492177-c3e84080-1da8-11e9-8386-e1ce3e4eb595.jpg" /> 

### Q-function(objective): "log(**`P(joint)`**)|data" ???? what? 
<img src="https://user-images.githubusercontent.com/31917400/51533344-71576480-1e3a-11e9-8570-c0128a7cc197.jpg" /> 

# 1> Define how many `K`(cluster) we have, then initialize `θ_knot`.
 - give the initial values...whatever..(mu,var)..the weight parameter(mixing proportion) naturally comes from these mu,var parameters.
   
# 2> E-Step
<img src="https://user-images.githubusercontent.com/31917400/67166002-70307500-f383-11e9-9ee4-ed32720d4fc3.jpg" />

 - Deal with the **latent variable** as a parameter (which can represent all other varibales you have..so almost "feature-reduction"?).  
 - Estimate the distribution of the variable and **hidden variable** P(X,Z) given the data + current parameters (create function for the expectation of the log-likelihood).
 - **Get the Q-function** by a) computing the **Conditional Expectation** of the log(joint distribution) by simply feeding current θ and data, and b) estimating next parameters. 
       
   a)
   <img src="https://user-images.githubusercontent.com/31917400/67166777-7f66f100-f38a-11e9-8cbc-bb5312298b42.jpg" />
     
   b) for example..the change of distribution from `theta` to `theta_t`
   <img src="https://user-images.githubusercontent.com/31917400/66701345-45934c00-ecf3-11e9-87d6-3c21f9e088f0.jpg" />
   
## Find `Weight parameter` by each datapoint. <img src="https://user-images.githubusercontent.com/31917400/66720104-71482c00-edf0-11e9-9768-35fd152aea12.jpg" /> 
 
# 3> M-Step
<img src="https://user-images.githubusercontent.com/31917400/67166068-12505d00-f384-11e9-8a54-556619b84db1.jpg" />

 - Deal with the real parameters. 
 - Maximize the joint distribution of the data and the hidden variable. In other words, given the current data, estimate the parameters to update the model by **`Evaluating the log-likelihood which sums for all clusters`**(Computes parameters maximizing the expected log-likelihood found on the E step). The higher the value, the more sure we are that the mixer model fits out dataset. The purpose is to maximize this value by choosing the parameters(the mixing coefficient, mean, var) of each Gaussian again and again until the value converges, reaching a maximum.
 - **Get the maximizer** from maximizing the Q-function and update... (but sometimes you cannot get the maximizer..) 
 <img src="https://user-images.githubusercontent.com/31917400/66709385-00a30000-ed5b-11e9-88d4-4ad9068c38e8.jpg" /> 

 <img src="https://user-images.githubusercontent.com/31917400/66701362-89865100-ecf3-11e9-946b-de8c491c782c.jpg" />

## Update other parameters(mu,var) using `Weight parameter`. <img src="https://user-images.githubusercontent.com/31917400/66720432-86bf5500-edf4-11e9-89f2-2fbee28d4805.jpg" />  
 
## Evaluation: 
`Are the parameter we have now is the best parameter?` No? Then go back to E-Step.  
<img src="https://user-images.githubusercontent.com/31917400/66720651-aa37cf00-edf7-11e9-8b3f-226d1cb62357.jpg" /> 

[Note]: 
 - Fitting Error:
   - When choosing the best run (highest training log-likelihood) among several training attempts with different random initializations, we can suffer from **local maxima**. and its solution would be **NP-Hard**. 
 - EM for multiple parameters:
   - We use EM-Algorithm to fit the distribution(such as Gaussian) to the **multi-dimensional** data(estimating the mean vector `μ_` and the covariance matrix `σ_`), not to the one-dimensional data.
 - EM for NA in multiple fields:
   - We can treat `missing values` by introducing **latent variable** that comprehensively reflects all multiple data fields we have, then estimate the distribution parameters of each membership that the latent variable is explaining. But with one-dimensional data, if a data field has missing values, it means that we don’t know anything about it (its only dimension is missing), so we might throw away points with missing data.
   - Note that we also don’t need EM to estimate the 'mu' vector (e.i. we need it only for the covariance matrix) in the multi-dimensional case: since each coordinate of the 'mu' vector can be treated independently, we can treat each coordinate as one-dimensional case and just throw away missing values. 


## Latent Variable Modeling_02> [HMM]
HMMs have been successfully used on both supervised and unsupervised problems with sequence data in applications like speech recognition, bioinformatics, sign language recognition, and gesture recognition. 

Problem? 
<img src="https://user-images.githubusercontent.com/31917400/67695970-ec3b4600-f99d-11e9-9057-5f89f674574c.jpg" /> 

How HMM helps? `Transition` & `Emission`
<img src="https://user-images.githubusercontent.com/31917400/67762568-aa61dc80-fa3d-11e9-8b83-7b70bead96de.jpg" /> 

This is how HMM generate the `joint probability` of the given sentence and pick the best context!
<img src="https://user-images.githubusercontent.com/31917400/67765592-6c1beb80-fa44-11e9-846c-abe650157941.jpg" /> 



















