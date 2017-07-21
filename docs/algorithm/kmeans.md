# K-means Clustering
* Desciption
Given a set of observation \((x_1,x_2,...,x_n)\), where each observation is a d-dimensional real vector, k-means clustering aims to partition the n observation into k sets \(S=(S_1,S_2,...,S_k)\) so as to minimize the within-cluster sum of squares (WCSS), that is sum of distance functions of each point in the cluster to the K center.


* Initialization methods
	- Commonly used initialization methods are Forgy and Random Partition.
	- The Forgy method randomly chooses k observations from the data set and uses these as the initial means. 
	- The Random Partition method first randomly assigns a cluster to each observation and then proceeds to the update step, thus computing the initial mean to be the centroid of the cluster's randomly assigned points. 
	- The Forgy method tends to spread the initial means out, while Random Partition places all of them close to the center of the data set. 
	- The Random Partition method is generally preferable for algorithms such as the k-harmonic means and fuzzy k-means. For expectation maximization and standard k-means algorithms, the Forgy method of initialization is preferable.
