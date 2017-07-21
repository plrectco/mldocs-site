# Data Cleaning
* Problem I: Noise Points
    - Behavior:
        + Class noise: misclassification & conflict class labels
        + Attribute noise: attributes that influence the classification preformance
    - Solution:
        + Noise Filters
            * Group original datasets into T subsets
            * For each subset, run m filter/learning algorithms over other `T-1` subsets 
            * Compare the trained label with the actual label for each subset
            * Decide the correctness of the initial labels by consensus vote scheme
            * Remove the subset if far from idealism
        + Robust Strategies (usually based on Machine Learning algorithms)
            * Focus on the selection of the algorithms (eg. clustering...)
            * Not much to deal with the data itself.
* Problem II: Missed Data Fields 
    - Behavior:
        + Entry A has attribute a, while Entry B doesn't have;
        + Entry A originally has attribute a, while being eliminated by manual processing/error recording;
    - Solution:
        + Omitting
            * Intro:
                - Do nothing special
                - Just assume they don't exist
            * Strategies:
                - **Listwise Deletion**: Delete the whole problematic entry by ignoring other useful fields
                - **Pairwise Deletion**: Only delete the problematic field while keeping the useful fields
        + Setting default values
            * Intro:
                - Deletion strategy may fail for small datasets
                - Get imputed values, kind of guessing
            * Strategies:
                - **Set the mean as default values**: Retain the mean, but bad for field relationship
                - **Linear Regression**: Amplify the current trends
                - **Other Rob