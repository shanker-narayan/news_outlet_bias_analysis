**Overview**

This project looks at the American news ecosystem — how outlets lean politically (based on AllSides dataset), how much the public agrees with those ratings, and which outlets command the most engagement. This project attempts to answer three key questions

1. How is political bias distributed across major U.S. news outlets?

   <img width="605" height="382" alt="Screenshot 2025-12-09 at 4 02 41 PM" src="https://github.com/user-attachments/assets/55b5cceb-b5ca-4469-a1a0-c3d622af3efc" />

3. How strongly does the public agree or disagree with these bias ratings?

4. What patterns emerge when outlets are clustered by ideology, public perception, and engagement?

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

**5 Clusters That Emerged**

1. **Cluster 0:** outlets that AllSides described as having a "right" bias, majority of the public agrees with the bias label, relatively high engagement (using total_votes as a proxy), outlets in this cluster show very strong belonging (average cluster_prob of 0.92)

2. **Cluster 1:** outlets that AllSides described as having a "left" bias, majority of the public agrees with the bias label (strongest across all clusters), relatively high engagement, outlets in this cluster show very strong belonging (average cluster_prob of 0.79)

3. **Cluster 2:** outlets that AllSides described as having a "right-leaning" bias, voters tends to slightly disagree on the bias label, engagement is lower than clusters 0 and 1, outlets in this cluster show very strong belonging (average cluster_prob of 0.82)

4. **Cluster 3:** outlets that are slightly left of center, slightly more voters disagree than agree with bias label, javerage level of engagement, might have a higher number of outlets that don't fit the profile of a typical outlet in that cluster (average cluster_prob of 0.48)

5. **Cluster 4:** Centrist/very slightly right of center, has the strongest level of disagreement about bias label, lower engagement on average, outlets in this cluster show very strong belonging (average cluster_prob of 0.86)

<img width="748" height="422" alt="Screenshot 2025-12-09 at 3 57 36 PM" src="https://github.com/user-attachments/assets/b77db271-f5f3-4cd9-98e7-2699861dfaa1" />

**Key Insights**

1. Each cluster contains only one ideology, so horizontal separation within each cluster reflects differences in public agreement with the bias label. Outlets with higher public agreement appear farther to the right along PC1, while outlets with lower agreement appear to the left. For example, in cluster 1 (firmly left outlets), CNN (Opinion), MSNBC, and The New Yorker appear to the right because they have higher public agreement with the bias label than Vice or NewsOne.

2. The diagonals show that within each ideology, outlets with more public agreement also tend to have more audience engagement — and outlets with less agreement tend to have less engagement.

3. Clusters 0 (blue) and 1 (orange) begin further to the right in the chart because the outlets in these groups have higher minimum agreement levels compared to other clusters. However, they also display wider internal variation in agreement (i.e., longer diagonals) compared to others. Since clusters 0 and 1 correspond to firmly right-leaning and firmly left-leaning outlets, respectively, this pattern raises an interesting question: **do strongly ideological outlets tend to exhibit more polarized levels of public agreement?**

4. Ideology drives the vertical separation between diagonals/clusters (PC2). From top to bottom in our PCA chart, the clusters follow this structure:

**Top:** Cluster 0 (Right)
**Upper Middle:** Cluster 2 (Right-leaning)
**Middle:** Clusters 4 and 3 (Center, left of center)
**Bottom:** Cluster 1 (Left)

5. An outlet’s position should be interpreted both within its own cluster and across clusters. For example, NPR (Cluster 4, centrist) appears to the right of The New York Times (Cluster 3, left-leaning) along PC1, meaning NPR has higher public agreement with its assigned bias label in relative terms. However, both outlets still fall within regions where disagreement outweighs agreement overall. In fact, NPR’s cluster (Cluster 4) exhibits the lowest average agreement of any cluster, meaning even the “more agreed-upon” outlets within this group receive lower consensus than outlets in other segments. **The public often disagrees with outlets being labelled as "neutral", which is why centrist outlets form the lowest-agreement cluster in our analysis.**
