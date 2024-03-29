# Illustrations of ISM, CoDA, and LPA applied to 24HAC data

On this Github repository, we provided R code based on hypothetical data to illustrate and repeat the analysis we performed in our paper *Analysis of the 24-Hour Activity Cycle: An illustration examining the association with cognitive function in the Adult Changes in Thought (ACT) Study [Wu Y et al (2023)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10087899/)

## Background

The 24-hour activity cycle (24HAC) is a new paradigm for studying activity behaviors in relation to health outcomes. This approach inherently captures the interrelatedness of the daily time spent in physical activity (PA), sedentary behavior (SB), and sleep, and allows for the study of increasing one activity in exchange for decreasing another and the associated estimated effects on health outcomes. The 24HAC is a multivariate exposure and requires specialized methods of analysis. We simulated hypothetical data and illustrated two popular approaches, namely isotemporal substitution model (ISM) and compositional data analysis (CoDA). Latent profile analysis (LPA) is another approach for summarizing 24HAC data and modeling outcome associations with the 24HAC exposure, and is discussed and compared with ISM and CoDA in our paper [Wu Y et al (2023)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10087899/). However, our analysis approaches requires specialized software Mplus (https://www.statmodel.com/) or LatentGold (https://www.statisticalinnovations.com/latent-gold-6-0/). Hence, we only presented here how to perform the basic LPA using the R package `tidyLPA`(https://cran.r-project.org/web/packages/tidyLPA/vignettes/Introduction_to_tidyLPA.html). For modeling the associations of external variables with latent profiles, please see the section 3.3 in our paper and LatentGold syntax in the supplemental materials.

## Data and code

The hypothetical data (example_dat.csv) contains a continuous outcome variable, 24HAC formed by four activity behaviors (sit, stand, step, and sleep) and a binary covariate sex. N = 1000. The primary research question of interest is the effect of time reallocation e.g. reallocating one hour from sit to step on the outcome.

The Rmarkdown file (illustration_codes.Rmd) contains code to repeat the analysis we performed in our paper but using the hypothetical data. The illustration_codes.pdf is knitted using that Rmarkdown file. It provides step-by-step illustrations and nicely formatted results.

## ISM approach

The ISM is formulated by including the total activity and all but one of the activity variables, excluding the activity you will explore displacing – in the model.  For example, with a continuous health outcome an ISM that leaves out the time stepping can be formulated, as below:

$$E(Y) = \beta_0 + \beta_1Sit + \beta_2 Stand + \beta_3 Step + \beta_4 Ttoal + \beta_5 Sex$$

where $E(Y)$ abbreviates the conditional mean of the health outcome given the time allocation variables (Sit, Stand, Sleep, Total measured on the same unit, e.g., hours in a 24-hour day), and the covariate sex. When *Total* is exactly a constant 24 hours/day for every subject, like in this hypothetical example, only one of the intercept or the Total terms can be included in the model.

In our illustration, four linear ISMs adjusted for sex were fit to the data, with each of the four activities omitted from the model one at a time. Please find the code and interpretations in the R markdown file.

The linear model assumption can be relaxed by fitting a more flexible ISM with each activity term modeled by a spline function, while keeping the total activity as a linear term. The nonlinear ISM analysis can be done in R with package `mgcv`.

## CoDA approach

CoDA is another widely used analytic approach to handle 24HAC data and its associations with health outcomes. Unlike ISM, treating each activity behavior as an univariate variable, the fundamental unit of observation is the multivariate vector of the proportions or percentages of the 24 hours that are spent in each type of activity. By using the operations defined by Aitchison on the space of compositions [Aitchison (1994)](https://www.jstor.org/stable/4355794), this approach provides a natural way to compare health outcomes between any two compositions, including substitution of one behavior for another.

In our illustration, we visualized the 24HAC compositions in ternary diagrams by using the R package `ggtern`[Hamilton NE, Ferry M (2018)](https://www.jstatsoft.org/article/view/v087c03). We then provided two different applications of the CoDA analysis: one which considers the effect of increasing time in a particular activity, while proportionally decreasing the other activities; and one which considers a composition that captures a pairwise time reallocation. The results of both can be obtained and visualized by using the package `codaredistlm` (CoDA redistribution linear model) available on github: [github.com/tystan/codaredistlm](https://github.com/tystan/codaredistlm).

## LPA approach

Unlike ISM and CoDA, LPA is a more exploratory method used to identify distinct latent subgroups with respect to activity profiles based on observed 24HAC data. This analysis can be done in R using the package `tidyLPA` (https://cran.r-project.org/web/packages/tidyLPA/vignettes/Introduction_to_tidyLPA.html). Another objective of LPA is to analyze the potential correlates of latent profiles and the associations of the profiles with outcomes. However, our analysis approach used specialized regression methods that account for class assignment uncertainty, which can be performed in Mplus and LatentGold. Both are commercial software. To the best of our knowledge, those methods have not been implemented in any R package. To perform similar analysis as in our paper, please find the LatentGold syntax in the supplemental materials of our paper.



