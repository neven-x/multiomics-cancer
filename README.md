# Introduction
Identification of reliable biomarkers of disease is crucial for easy and timely diagnosis which is especially important in cancer.
This dataset contains contains measurements of the fecal protein content, metabolite content and gut microbiome data from mice that have colorectal cancer and healthy controls.
Together they represent a large set of roughly 14,000 potential biomarkers of colorectal cancer. The aim of this project was to identify a much narrower set of candidate biomarkers
that can be investigated in more detail with targetted follow-up studies.

# Approach
A simple way to model the effects of multiple predictive variables on the probability of a cancer diagnosis is a general linear model. Since the predicted variable is binary
(either a healthy or cancer sample) a logistic linear model was used, where the logistic linker function converts a continuous GLM output into a probability function, which can be
thresholded to produce a binary prediction. A problem is presented by the size of the feature set, which was many times larger than the number of samples (168). This situation
in a linear regression model generally leads to overfitting, since a very large number of features can be used to perfectly or very closely approximate any function. 

To avoid inputing the raw dataset of ~14,000 features into the model, each of the datasets (proteomic, metabolomic, microbiome) was first dimensionally reduced using PCA
by retaining only the principal components that account for 80% of dataset variability. The reduced set of principal components close to the number of samples was then used 
to predict the disease state with a very high cross-validated accuracy (>90%). L1 regularisation was used to produced an even sparses set of principal components with 
non-zero predictive weight.

To infer the predictive weight of the initial 14,000 features in this model, the predictive weight of each principal component $i$ ($w_i$) was multiplied by the basis vector for that
component ($\mathbf{B}_{i}$), which maps original features to the latent space. This produced a matrix with dimensions $i,j$, where $j$ are the original features,
containing the weighting of each original feature in each principal component multiplied by the weight of that component in predicting disease state. The absolute values of these weights
were summed over all components $i$ to produce a vector $\mathbf{u}$ with length $j$ indicating the relative weight of each original feature in predicting disease:

$$\mathbf{u} = \sum_{i=1}^{n} abs(\mathbf{B}_i \cdot w_i)$$

# Results
By separating the weights in $\mathbf{u}$ by dataset and plotting their cumulative distributions, I found that the proteomics dataset contains the most promising candidates wih the highest
weights. The top N biomarkers from each dataset were identified and visualised (IDs have been redacted), reassuringly showing the involvement of some proteins already known for involvement in
colorectal cancer as well as some novel canidates. The fold change in the expression level of each candidate biomarker was also visualised to show whether the biomarker is more or less
abundant in disease compared to healthy samples.

# Disclaimer
The analysis was performed on data that is not publicly available and intellectual property of Sophia Pedersen and The Francis Crick Institute. The identities of the identified candidate
biomarkers have therefore been redacted in this public version of the notebook.
