## Instance Selection

***Overview***

1. two main method: select training set, select prototype.

2. instance selection is to select the data subset such as the whole set is used. In other words, it is like reduce redundancy.

3. It is like select a set from the training set and test it over the test set(the test set is the same for comparison purpose)

4. PS methods: prototype selection. This method uses an instance base classifier to divide the training set with measure like similarity and distance measure. 

5. TSS method: training set selection. It is more like a way to inmprove the interpretability and precision of a predictive model. Most common applications are ANNs, SVMs and decision trees.

***Prototype method***

1. Direction of search
	- incremental
	- decremental
	- batch: based on decremental, remove instances in batch
	- mixed: start with a preselected subset, and hthen add or remove instances 

2. Type of selection
	- condensation: retain the points in the decision boundary, and remove the internal one: high reduction scale but low accuracy on test sets (due to overfitting)
	- edition: remove noisy border point: high accruracy but low reduction
	- Hybrid

3. Evaluation of search
	- filter
	- wrapper

4. Criteria 
	- storage reduction
	- generalization accuracy
	- noise tolenrance
	- time complexity

5. Method
    - condensation algorithms
    
    	+ incrementa  CNN: condensed nearest neighbors
    	
    	  randomly select one instance belonging to each output class from TR and put them in a set S. Classify other instances in the TR using the instances in S, using the nearest neighbors. When misclassified, it is added to S, thus ensuring it is not misclassifed.   
        
        + decremental 
          
          * RNN: reduced nearest neighbors
           	
			S = TR, remove an instance that does not impact other instances' classification. 
          
          * Shrink 
          	
          	Similar to RNN, but consider only the removed one is correctly classified or not.
                
          * Minimal Consistent Set (This method is like Go, the instance with more own class neighbor wins)
      
          	consistency is the key word. And before that we need to know NUN dist = Nearest unlike neighbots dist. Instance is consistent when it has NUN dist greater than the dist between it and the nearest unlike neighbor( by definition )
            The iteration starts with the entire set. Then remove inconsistent instances.
            Each instance has a vote, which is given by the own-class neighbors who are within the NUN distance range. Then we would like to starts with the candidate with the largest votes and then eliminate the voters that vote it (because it is a selected representitive.) Repeat this procedure until all instanse in the set has been considered.
            Repeat since NUN may change during the above procedures. All procedures can be repeated until convergence.     
                	
	- Edition algorithm
	
    	* Edited  NN: instance is deleted when it does not agree with the nearest k neighbors
    	* 