### Conceptual Overview

The clustering analysis was composed of several steps conducted in *R*, resulting in a Smooth Ensemble of the observational similarities. Conceptually, the Smooth Ensemble is a result of a threshold approximated ensemble of similarities between the tools (i.e. observations) in various subsets of the data (i.e. subsets of predictor space). The unsupervised approach we took involved multiple steps (see below) to generate similarities among the observations in various subsets of the predictor space. Following the definitions below, similarity matrices are generated for each ki subspaces in K, including the predictor set; i.e. all predictors. The cluster assignments for all tools is generated using: 1) all predictors, 2) each subspace, ki, and 3) all predictors except each ki (thus, there are also i number of leave-one-out analyses performed). Observational co-occurrences are generated across all analyses to obtain an ensemble of probabilistic assignments; i.e. a correlational matrix of co-assignment among observations (see Figure 3). Finally, a threshold approximation is used to dropout low pairwise observational assignments (force to zero). The resulting Smooth Ensemble, a n x n correlational matrix of observations, is used with Kmeans clustering to generate the final clustering of tools.

n - number of observations
<br>
p - number of predictors, i.e. amount of data
<br>
K - set of predictor subspace sizes, i.e. amount of data in each subgroup ki
<br>
Gap statistic (Robert Tibshirani et al. 2000) - Using within-cluster dispersion as an error measurement versus the number of clusters (i.e. Elbow method), to obtain the optimal number of clusters.

### Steps to generate Smooth Ensemble:

	* Input data of n x p (observations x predictors)
	* Intra-observational similarity matrices are generated through Gap statistic in various subsets of predictor space
    * Subset predictor space (i.e. subsets of the features) is calculated manually by selecting groups of features by use / type
	* Analysis without each predictor subset is performed (i.e. leave-one-out)
	* Resulting similarity matrices are used to independently cluster observations through the optimal number of clusters in Kmeans
	* Resulting cluster assignments are used to obtain co-occurrence of observations in relation to total number of analyses
	* A threshold approximation is applied to perform observational similarity dropout
	* Smooth Ensemble of observational correlations for Kmeans analysis
