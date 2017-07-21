# Data Reduction

- Data redcution can be devided into 3 categories

	* dimension reduction
	* sample numerosity reduction
	* cardinality reduction

- Dimension Reduction (DR):

	* why we need it? the curse of dimension
	 number of training samples should grows linearly or even exponentially with the dimensions. For exaample, when dealing with non-parametric learners, such as decision tree.
	
    * PCA: normalize the input data, get the principal components by calculating the eigenvalues-eigenvectors of the covariance matrix of the original data, sort by the eigenvalues, reduce weaker components

    * Factor Analysis: Discover hiddeen factos in the current variables. Assume that there is a set of latent varaibels that generate the original data. Major method is using PCA. (, the eigenvalues of PCA are inflated component loadings, i.e., contaminated with error variance, from wikipedia)
    Therefore in my opinion, this maybe that factor analysis introduce an error item and it will estimate it recursively until it converge(it may not!) before hand.

    * Mutidimensional scaling: it reduces the data into a space that the distance between each other is minimized. Too much mathematics. 
    
    * Locally linear embedding: Assume data lies in a manifold with dimension d < D like a line in a plain. Each point can be revcovered by its neightbor. It preserve the invariance to rotation and scaling. Then we can use the weight to get the d-dimensional coordinate by fixing the weight and finding the d-dimensional coordinate to minimize teh sqarue distances of all points.

- Data Sampling 

	* why?
		+ reduce the number of instances submitted to the data mining algorithm. Predictive learning often can have 10%-20% of data to learn without the dererioration of performance.
		+ balancing the data, like oversampling the rare case and undersampling the common case
		+ partition into training , validation and testing sets.
	
	* method
		+ simple random sample without replacement
		+ simple random sample with replacement
		+ balanced sample
		+ clusters sample, like samples in geographically separated areas.
		+ stratified ssample
	
	* data condensation: selection of small representatives
	
	* data squashing: use a compressed set of data such that hte staistical properties are preserved
	
	* data clustering: use the cluster instead of the data itself

- bining and reduction of cardinality

	* bining is discretize the continuous data into discrete ones which cover different ranges. combine different categories into one indicator variable, reducing the possibility that original category has trivially almost all 0 or 1.

***

## Feature Selection 

- Feature Relevance

	Feature relevance is an abstract concept in terms of an ideal Bayes classifier

	* strongly relevant if remove the feature X will result in the deterioration of the classification results

	* weakly relevant if there exisits a set S that performance is better when using S U {X} than S.

	* irrelevant if neither weakly or strong relevant

	***Note***: The difference between stong and weakly is that one is for  all set and one only requires existence.

- Feature Selection Method

	* indivisual searching vs subset searching

	* filters, wrappers and embedded methods
	
	* Filter method
		+ chi-Sqaured: univariate, the higher teh chi-squared, the more relkevant the feature with tespect to the class.
		+ information gain: entorpy orginal - entropy psterior
		+ Correlea=ation-Based Feature Selection.: multivariate, the more the fetaure related with the class and the less with the orther feature, the more it is selected.
		+ RelieF: improved version of Relief algorithm. The attributes that separate the same class is insisgificant and the attributes that separate different calsses is desirable.
		[RelieFCode](RelieF.py)


***

## Instance Selection
- Overview

	- two main method: select training set, select prototype.

	- instance selection is to select the data subset such as the whole set is used. In other words, it is like reduce redundancy.

	- It is like select a set from the training set and test it over the test set(the test set is the same for comparison purpose)

	- PS methods: prototype selection. This method uses an instance base classifier to divide the training set with measure like similarity and distance measure. 

	- TSS method: training set selection. It is more like a way to inmprove the interpretability and precision of a predictive model. Most common applications are ANNs, SVMs and decision trees.

- Prototype method

	- Direction of search
		- incremental
		- decremental
		- batch: based on decremental, remove instances in batch
		- mixed: start with a preselected subset, and hthen add or remove instances 

	- Type of selection
		- condensation: retain the points in the decision boundary, and remove the internal one: high reduction scale but low accuracy on test sets (due to overfitting)
		- edition: remove noisy border point: high accruracy but low reduction
		- Hybrid

	- Evaluation of search
		- filter
		- wrapper

	- Criteria 
		- storage reduction
		- generalization accuracy
		- noise tolenrance
		- time complexity

	- Method
	    - condensation algorithms
	    
	    	+ incremental CNN

	 			condensed nearest neighborsrandomly select one instance belonging to each output class from TR and put them in a set S. Classify other instances in the TR using the instances in S, using the nearest neighbors. When misclassified, it is added to S, thus ensuring it is not misclassifed.   

	      	+ decremental 
	          
	          * RNN: reduced nearest neighbors. S = TR, remove an instance that does not impact other instances' classification. 
	          
	          * Shrink: Similar to RNN, but consider only the removed one is correctly classified or not.
	                
	          * Minimal Consistent Set (This method is like Go, the instance with more own class neighbor wins)
	      
	          	consistency is the key word. And before that we need to know NUN dist = Nearest unlike neighbots dist. Instance is consistent when it has NUN dist greater than the dist between it and the nearest unlike neighbor( by definition )
	            The iteration starts with the entire set. Then remove inconsistent instances.
	            Each instance has a vote, which is given by the own-class neighbors who are within the NUN distance range. Then we would like to starts with the candidate with the largest votes and then eliminate the voters that vote it (because it is a selected representitive.) Repeat this procedure until all instanse in the set has been considered.
	            Repeat since NUN may change during the above procedures. All procedures can be repeated until convergence.     
	                	
		- Edition algorithm
		
	    	* Edited  NN: instance is deleted when it does not agree with the nearest k neighbors
    	
***
## Discretization

+ Motivation
	- some data mining algorithms require discrete values.
	- it can be more efficient when training
	** Note: it may lose information of the data, thus minimizatinon of information loss is the main goal of a discretizer.

+ More
	- obtaining an optimal discrezation is NP complete.
	- arity: the number of patitions
	- procedures: sort attribute -> obtain cut points -> perform evaluation -> split/merge attribute -> stop criterion

+ Common Properties
	- static vs dynamic: dynamic discretizer is in relation to the learner. Almost all disretizer is static.
	- univariate vs multivariate: multivatiate: 2D discretization - considering two attributes at the same time to find the best cut points.
	- supervised vs unsupervised: unsupervised does not consider the class labels. In supervised discretizers, the heuristic measures such as entropy and interdenpendecy are used to dermine the best cut points.
	- spliting vs merging: top down and bottom up
	- global vs local: either all the available data or use only partial information. Few discretizers are local.
	- direct vs incremental: divide into k intervals simultaneously or perfect the division step by step.
	- evaluation measrues:
		* information: entropy
		* statistical: dependency
		* rough sets: lower and uppper approximation, class separativity.
		* wrapper: error rate provided by a classifier
		* binning: absense of evaluation measure. EqualWidth and EqualFrequency.

+ Criterion to compare discretization methods
	- number of intervals, the smaller the better
	- inconsistency. unavoidable measure: two instances with the same input attribute but different class labels.
	- predictive classification rate
	- time requirement

+ Spliting Methods
	- Equal width and equal frequency. equal frequency is in terms of continuous value, then the length of the interval can be found by dividing the length of the whole interval with desired arity. It is also equal width. 
	- MDLP.
		* use entropy to measure cut points
		* minimum disccription length principle to decide the acceptation o rejection for each cut pouint as the stopping criterion. The length of bits needed to distinguish an object. It stops when the information gain exceeds a value.
	- Distance 
		* Mantaras distance
		* it chooses the cut points that minimize the distance.
	- PKID: proportional discretization (to the size of data set). Desired interval frequencies and desired interval number are equal.
	- FFD: fixed frequency discretization. Fixed frequency and then divide. Compared to equal frequncy, which fixed number of interval and then divide according to equal frequency.

+ Merging Method