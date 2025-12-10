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

2. Applied PCA to understand underlying structure. One thing I noticed was that bias_scores were identical across outlets within each cluster. Therefore, outlets that appear further right along PC1 tend to have higher agreement, and diagonals/clusters are vertically separated by ideology (Top of PC2 corresponds to 'right' outlets, bottom corresponds to 'left')

**Clustering**

1. Used HDBSCAN to discover natural groupings without assuming a fixed number of clusters

**Visualization**

1. Mapped outlets in PCA space and annotated selected outlets for interpretability

**Unclustered Outlets**

1. Several major, recognizable outlets that were not assigned to any cluster. Although we often group outlets like CNN, Fox News, ABC News, and others under the umbrella of ‘mainstream media,’ their behavioral patterns differ significantly across key features—particularly bias_score and agree_ratio_log. These differences are large enough that they do not form a dense cluster in our model, causing HDBSCAN to treat them as isolated outliers rather than members of a shared group.
   
<img width="512" height="179" alt="Screenshot 2025-12-09 at 4 08 13 PM" src="https://github.com/user-attachments/assets/d62931ef-41ff-44ba-83ac-d981a7ea6518" />

2. There's a noticeable difference in political leaning b/w outlets like Fox News, ABC News (Online), CNN (Online News), and Vox, despite similarities in name recognition and mainstream visibility.

3. If we compare Vox and CNN (Online News), which have identical scaled bias_scores, there's still quite a bit of difference in their agree_ratio, indicating that the consensus agrees more with Vox having a bias label of 'left' versus CNN

4. If we look at Fox News and ABC News (Online), the former has a clear right/right-leaning bias_score, while the latter is left of center (but not as far left as Vox and CNN, according to the dataset)

**5 Clusters That Emerged**

1. **Cluster 0:** outlets that AllSides described as having a "right" bias, majority of the public agrees with the bias label, relatively high engagement (using total_votes as a proxy), outlets in this cluster show very strong belonging (average cluster_prob of 0.92)

2. **Cluster 1:** outlets that AllSides described as having a "left" bias, majority of the public agrees with the bias label (strongest across all clusters), relatively high engagement, outlets in this cluster show very strong belonging (average cluster_prob of 0.79)

3. **Cluster 2:** outlets that AllSides described as having a "right-leaning" bias, voters tends to slightly disagree on the bias label, engagement is lower than clusters 0 and 1, outlets in this cluster show very strong belonging (average cluster_prob of 0.82)

4. **Cluster 3:** outlets that are slightly left of center, slightly more voters disagree than agree with bias label, javerage level of engagement, might have a higher number of outlets that don't fit the profile of a typical outlet in that cluster (average cluster_prob of 0.48)

5. **Cluster 4:** Centrist/very slightly right of center, has the strongest level of disagreement about bias label, lower engagement on average, outlets in this cluster show very strong belonging (average cluster_prob of 0.86)

<img width="748" height="422" alt="Screenshot 2025-12-09 at 3 57 36 PM" src="https://github.com/user-attachments/assets/b77db271-f5f3-4cd9-98e7-2699861dfaa1" />

**Key Insights**

1. Ideology was the dominant factor distinguishing clusters, as evidenced by the fact that each cluster contains only one ideological label. Other features (agreement ratio, engagement) explain variation within each ideological cluster, but not across clusters.

2. Since each cluster contains only one ideology, horizontal separation within each cluster reflects differences in public agreement with the bias label. Outlets with higher public agreement appear farther to the right along PC1, while outlets with lower agreement appear to the left. For example, in cluster 1 (firmly left outlets), CNN (Opinion), MSNBC, and The New Yorker appear further to the right because there's more agreement with their bias labels than with Vice or NewsOne.

3. The diagonals show that within each ideology, outlets with more public agreement also tend to have more audience engagement — and outlets with less agreement tend to have less engagement.

4. Clusters 0 (blue) and 1 (orange) begin further to the right in the chart because the outlets in these groups have higher minimum agreement levels compared to other clusters. However, they also display wider internal variation in agreement (i.e., longer diagonals) compared to others. Since clusters 0 and 1 correspond to firmly right-leaning and firmly left-leaning outlets, respectively, this pattern raises an interesting question: **do strongly ideological outlets tend to exhibit more polarized levels of public agreement?**

5. Ideology drives the vertical separation between diagonals/clusters (PC2). From top to bottom in our PCA chart, the clusters follow this structure:

**Top:** Cluster 0 (Right)
**Upper Middle:** Cluster 2 (Right-leaning)
**Middle:** Clusters 4 and 3 (Center, left of center)
**Bottom:** Cluster 1 (Left)

6. An outlet’s position should be interpreted both within its own cluster and across clusters. For example, NPR (Cluster 4, centrist) appears to the right of The New York Times (Cluster 3, left-leaning) along PC1, meaning NPR has higher public agreement with its assigned bias label in relative terms. However, both outlets still fall within regions where disagreement outweighs agreement overall. In fact, NPR’s cluster (Cluster 4) exhibits the lowest agreement of any cluster, meaning the average outlet within this group receives lower consensus on bias label than outlets in other segments. **In other words, the public often disagrees with outlets being labelled as purely "neutral", which is why centrist outlets form the lowest-agreement cluster in our analysis.**
