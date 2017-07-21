# Data Integration
* Problem I: Attribute Redundancy
    - Behavior:
        + salary/month => salary/year
    - Solution:
        + Chi-Square Correlation Test
            * Intro:
                - Hypothesis: `Attr_A` and `Attr_B` (both nominal attributes) are independent of each other
            * Strategies:
                - Construct the contingency table: $A: a_1,a_2,...a_c; B: b_1,b_2,...b_r$
                - Apply the test formula:
                  - $\chi^{2}=\sum\limits_{i=1}^{c}\sum\limits_{j=1}^{r}\frac{(o_{ij}-e_{ij})^2}{e_{ij}}$ , $e_{ij}=\frac{count(A=a_{i})\cdot count(B=b_{j})}{m}$,
                  - where $o_{ij}$ is the observed occurrence, $e_{ij}$ is the expected occurrence, $m$ is the total number of instances in the datasets
                - Compare with the Chi Square statistics:
                  - Given significant level, and the degree of freedom $(c-1)(r-1)$, get the needed value in the Chi Square statistics reference table.
                  - Reject the hypothesis if the computed result is above the needed one.
        + Correlation Coefficient and Covariance
            + Intro:
                + Similar to Chi-Square Correlation Test
                + Apply a different statistical formula (numerical):
                    + $r_{A,B}=\frac{\sum_{i=1}^{m}(a_{i}-\bar{A})(b_{i}-\bar{B})}{m\sigma_{A}\sigma_{B}}=\frac{\sum_{i=1}^{m}(a_{i}b_{i}-m\bar{A}\bar{B})}{m\sigma_{A}\sigma_{B}}$,
                    + $Cov(A,B)=E((A-\bar{A})(B-\bar{B}))=\frac{\sum_{i=1}^{m}(a_{i}-\bar{A})(b_{i}-\bar{B})}{m}$.
                + Another validation condition:
                    + Independent if covariance evaluated as 0.
                    + $Cov(A,B)=E(A,B)-\bar{A}\bar{B}=E(A)\cdot E(B)-\bar{A}\bar{B}=0$

* Problem II: Tuple Duplication and Inconsistency
    - Behavior:
        + "id" data field stored as "_id" in MySQL, while "id" in MongoDB
    - Solution:
        + Meta-data Comparison
            * Intro:
                - Meta-data: The data that describes our target data, eg: Type, range, etc. 
            * Strategies:
                - Compare how the meta data of two fields are close to each other.
        + Edit Distance
            * Intro:
                - Quantitatively evaluate how one string is close to the other.
            * Strategies
                - Set distance 1 for deletion, insertion, substitution.
                - Apply Dynamic Programming to get the target distance.
                  - $dist_{i,j}=min(dist_{i-1,j}+1,dist_{i,j-1}+1,dist_{i-1,j-1}+(match(S_{i},T_{j})?0:1))$
