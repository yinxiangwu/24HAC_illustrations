# Illustrations of ISM, CoDA, and LPA applied to 24HAC data

## Background

The 24-hour activity cycle (24HAC) is a new paradigm for studying activity behaviors in relation to health outcomes. This approach inherently captures the interrelatedness of the daily time spent in physical activity (PA), sedentary behavior (SB), and sleep, and allows for the study of increasing one activity in exchange for decreasing another and the associated estimated effects on health outcomes. The 24HAC is a multivariate exposure and requires specialized methods of analysis. We illustrated three popular approaches, namely isotemporal substitution model (ISM), compositional data analysis (CoDA) and latent profile analysis (LPA) for modeling outcome associations with the 24HAC exposure.

The hypothetical data contains a continuous outcome variable, 24HAC formed by four activity behaviors (sit, stand, step, and sleep) and a covariate sex. N = 1000. The primary research question of interest is the effect of time reallocation e.g. reallocating one hour from sit to step on the outcome.

### ISM approach

## ISM approach

The ISM is formulated by including the total activity and all but one of the activity variables – the activity you will explore displacing – in the model.  For example, with a continuous health outcome an ISM that leaves out the time stepping can be formulated, as below:

`E(Y) = \beta_0 + \beta_1Sit + \beta_2 Stand + \beta_3 Step + \beta_4 Ttoal + \gamma Sex`

where `E(Y)` abbreviates the conditional mean of the health outcome given the time allocation variables (Sit, Stand, Sleep, Total measured on the same unit, e.g., hours in a 24-hour day), and the covariate sex. When *Total* is exactly a constant 24 hours/day for every subject, like in this hypothetical example, only one of the intercept or the Total terms can be included in the model.

In our illustration, four linear ISMs adjusted for sex were fit to the data, with each of the four activities omitted from the model one at a time. Please find the codes and interpretations in the R markdown file.

The linear model assumption can be relaxed by fitting a more flexible ISM with each activity term modeled by a spline function, while keeping the total activity as a linear term. The nonlinear ISM analysis can be done in R with package “mgcv”.

### CoDA approach

CoDA is another widely used analytic approach to handle 24HAC data and its associations with health outcomes. Unlike ISM still treating each activity behavior as an univariate variable, the fundamental unit of observation is the multivariate vector of the proportions or percentages of the 24 hours that are spent in each type of activity. By using the operations defined by Aitchison on the space of compositions [Aitchison (1994)](https://www.jstor.org/stable/4355794), this provides a natural way to compare health outcomes between any two compositions, including substitution of one behavior for another.

Visualizations and compositional descriptive statistics of 24HAC can be helpful, before fitting any models. In the illustration, we provided codes to make nicely formatted Simplex plots by using the R package `ggtern`[Hamilton NE, Ferry M (2018)](https://www.jstatsoft.org/article/view/v087c03).

CoDA relies on the isometric log-ratio (ilr) transformation, which transforms each D-part composition to a unique D-1 vector on a new coordinate system where each new coordinate is a log-ratio which falls along the real line [Egoczue et al 2003]. There are many R packages that can implement the transformation. The one we used is called `robCompositions`.

With the transformed data and under normality assumptions, we performed the James multivariate analysis of variance to compare the compositional means between groups defined by covariates e.g. sex. The function for the James test belongs to the R package `Compositional`.

We then provided two different applications of the CoDA analysis: one which considers the effect of increasing time in a particular activity, while proportionally decreasing the other activities; and one which considers a composition that captures a pairwise time reallocation. The results of both can be obtained and visualized by using the package `codaredistlm` (CoDA redistribution linear model) available on github here: [github.com/tystan/codaredistlm](https://github.com/tystan/codaredistlm).

### LPA approach

Unlike ISM and CoDA, LPA is a more exploratory method used to identify distinct latent subgroups with respect to activity profiles based on observed 24HAC data. Another aim of LPA is to explore the correlates of identified profiles and the associations of the profiles with outcomes. LPA assumes the observed data are sampled from a population composed of distinct subpopulations with heterogeneous distributions of activity behaviors of a day and models the data using a mixture of multivariate normal distributions.




`






