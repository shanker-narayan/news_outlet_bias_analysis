**Overview**

This project looks at the American news ecosystem — how outlets lean politically (based on AllSides dataset), how much the public agrees with those ratings, and which outlets command the most engagement. This project attempts to answer three key questions

1. How is political bias distributed across major U.S. news outlets?

2. How strongly does the public agree or disagree with these bias ratings?

3. What patterns emerge when outlets are clustered by ideology, public perception, and engagement?

**Methods**

Feature Engineering 

1. Encoded bias labels (left, lean left, center, lean right, right) into numeric bias scores

2. Log-transformed agreement ratios and engagement levels to account for outliers

**Scaling and Dimensionality Reduction**

1. Standardized features to ensure equal weighting during clustering (i.e making sure that the range of feature columns were more or less equal)

2. Applied PCA to understand underlying structure. One thing I noticed was that bias_scores were identical across outlets within each cluster. Therefore, PC1 signalled level of agreement with bias label, and PC2 signalled level of engagement.

**Clustering**

1. Used HDBSCAN to discover natural groupings without assuming a fixed number of clusters

**Visualization**

1. Mapped outlets in PCA space and annotated selected sources for interpretability

**Key Findings**

1. Outlets with clear partisan identities and strong consensus cluster tightly, while centrist outlets show the most disagreement and scatter widely.

2. 
