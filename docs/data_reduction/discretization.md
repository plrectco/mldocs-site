## Discretization

***Overview***

+ Motivation
	- some data mining algorithms require discrete values.
	- it can be more efficient when training
	** Note: it may lose information of the data, thus minimizatinon of information loss is the main goal of a discretizer.

+ More
	- obtaining an optimal discrezation is NP complete.
	- arity: the number of patitions
	- procedures: sort attribute -> obtain cut points -> perform evaluation -> split/merge attribute -> stop criterion

***Details***

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
	- 