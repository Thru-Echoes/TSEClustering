### 0. Background

This writing introduces and explains some background, theoretical logic, and conceptual overview of the **Threshold Smoothing Ensemble Clustering (TSEC or TSEClustering)** method (currently being developed as *R and Python* libraries). TSEC can be used to produce unsupervised ensemble models that can use the following components:

* similarity or dissimilarity of low observational data (low sample size)
* ... especially low sample data with high dimensionality (high number of features / variables)
* i.e. large p, p > n
* unsupervised clustering with or without feature / variable weights
* Out-of-Bag (*OOB*) information since it is Bagged + Bootstrapped, compare obs to those left *OOB*

TSEClustering is based on core concepts from ensemble methods like Random Forest and unsupervised clustering methods like K-means. These methods, in various ways, can be used to assess variable (or feature) importance and *model goodness* evaluation / measurements.

##### 0.1. Random Uniform Forests (randomUniformForest package)

Unlike other variants of Random Forests, *Random Uniform Forests* seek strong randomization and minimize a global optimization function, promote low correlation between trees and residuals [REFERENCE 1,2,3]. The optimization function maximizes **Information Gain** or minimizes **L2 Distance** via *Bootstrapping (grab n samples out of N in each iteration)* and *Bagging (?)*.   

**Note:** extremely random across trees and nodes - thus, importance can be measured as variables showing up frequently = decreases most entropy at each node.

**Global Variable Importance:** measurement of variable lowering the prediction error

**Local Variable Importance:** measurement of variable occurrence in terminal nodes - thus, local importance of that variable to that node it is frequently occurring in


##### 0.2. Entropy Weighting (WSKM package)

TSEC uses components of the **Weighted K-Means Clustering (wskm = [ewkm, twkm, fgkm]) R package** for entropy-weighting as importance of variables to membership in subspace clustering (K-means) [REFERENCE 4]. This approach uses a few algorithms relating to weighted K-means clustering, feature weighting, and measuring importance of variables. Importance can be evaluated both as weights on individual variables and weights on variable groups. The latter can be especially informative in the TSEC method where variable groups can be manually and / or randomly created. These two forms of generating the variable groups can then be compared to the created groups. Further, another instance of TSEC could be used to analyze the similarity / dissimilarity of created groups and entropy weighted variable groups and, finally, compared to cluster membership in the former TSEC model.            

### 1. Conceptual Overview

The clustering analysis was composed of several steps conducted in *R*, resulting in a Smooth Ensemble of the observational similarities. Conceptually, the Smooth Ensemble is a result of a threshold approximated ensemble of similarities between the tools (i.e. observations) in various subsets of the data (i.e. subsets of predictor space). The unsupervised approach we took involved multiple steps (see below) to generate similarities among the observations in various subsets of the predictor space. Following the definitions below, similarity matrices are generated for each ki subspaces in K, including the predictor set; i.e. all predictors. The cluster assignments for all tools is generated using: 1) all predictors, 2) each subspace, ki, and 3) all predictors except each ki (thus, there are also i number of leave-one-out analyses performed). Observational co-occurrences are generated across all analyses to obtain an ensemble of probabilistic assignments; i.e. a correlational matrix of co-assignment among observations (see Figure 3). Finally, a threshold approximation is used to dropout low pairwise observational assignments (force to zero). The resulting Smooth Ensemble, a n x n correlational matrix of observations, is used with Kmeans clustering to generate the final clustering of tools.

n - number of observations
<br>
p - number of predictors, i.e. amount of data
<br>
K - set of predictor subspace sizes, i.e. amount of data in each subgroup ki
<br>
Gap statistic (Robert Tibshirani et al. 2000) - Using within-cluster dispersion as an error measurement versus the number of clusters (i.e. Elbow method), to obtain the optimal number of clusters.

### 2. Steps to generate Smooth Ensemble:

	* Input data of n x p (observations x predictors)
	* Intra-observational similarity matrices are generated through Gap statistic in various subsets of predictor space
    * Subset predictor space (i.e. subsets of the features) is calculated manually by selecting groups of features by use / type
	* Analysis without each predictor subset is performed (i.e. leave-one-out)
	* Resulting similarity matrices are used to independently cluster observations through the optimal number of clusters in Kmeans
	* Resulting cluster assignments are used to obtain co-occurrence of observations in relation to total number of analyses
	* A threshold approximation is applied to perform observational similarity dropout
	* Smooth Ensemble of observational correlations for Kmeans analysis

### X. References

1. [Saip Ciss (2014). *Random Uniform Forests: an overview*](https://cran.r-project.org/web/packages/randomUniformForest/vignettes/randomUniformForestsOverview.pdf)
2. [Saip Ciss (2015). *Variable Importance in Random Uniform Forests*](https://cran.r-project.org/web/packages/randomUniformForest/vignettes/VariableImportanceInRandomUniformForests.pdf)
3. [Breiman (2001). *OG Random Forest Paper*](https://www.google.com/)
4. [Graham Williams, Joshua Z Huang, Xiaojun Chen, Qiang Wang and Longfei Xiao (2015). *wskm: Weighted k-Means Clustering. R package version 1.4.28.*](http://CRAN.R-project.org/package=wskm)

**BibTeX for all weighted K-Means Entropy:**
```
@Manual{wskm2014hz,
    title = {{wskm}: Weighted $k$-Means Clustering},
    author = {Graham Williams and Joshua Zhexue Huang and Xiaojun Chen
      and Qiang Wang and Longfei Xiao},
    year = {2015},
    note = {R package version 1.4.28},
    url = {http://CRAN.R-project.org/package=wskm},
  }
  @Article{ewkm2007lpj,
    title = {An Entropy Weighting $k$-Means Algorithm for Subspace
      Clustering of High-Dimensional Sparse Data},
    author = {Liping Jing and Michael K. Ng and Joshua Zhexue Huang},
    journal = {{IEEE} Transactions on Knowledge and Data Engineering},
    volume = {19},
    number = {8},
    pages = {1026--1041},
    year = {2007},
    url = {http://dx.doi.org/10.1109/TKDE.2007.1048},
    doi = {10.1109/TKDE.2007.1048},
  }
  @Article{fgkm2012xjc,
    title = {A Feature Group Weighting Method for Subspace Clustering
      of High-Dimensional Data},
    author = {Xiaojun Chen and Yunming Ye and Xiaofei Xu and Joshua
      Zhexue Huang},
    journal = {Pattern Recognition},
    volume = {45},
    number = {1},
    pages = {434--446},
    year = {2012},
    url = {http://dx.doi.org/10.1016/j.patcog.2011.06.004},
    doi = {10.1016/j.patcog.2011.06.004},
  }
  @Article{twkm2013xjc,
    title = {TW-$k$-Means: Automated Two-Level Variable Weighting
      Clustering Algorithm for Multiview Data},
    author = {Xiaojun Chen and Xiaofei Xu and Joshua Zhexue Huang and
      Yunming Ye},
    journal = {IEEE Transactions on Knowledge and Data Engineering},
    volume = {25},
    number = {4},
    pages = {932--944},
    year = {2013},
    url = {http://doi.ieeecomputersociety.org/10.1109/TKDE.2011.262},
    doi = {10.1109/TKDE.2011.262},
  }
```
