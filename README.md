# lol-win-or-lose-Model-Building


### Prediction Problem

**Problem Statement:** Predicting the outcome (win or lose) of a League of Legends (LoL) game for a team based on match data.

**Type of Problem:** This is a binary classification problem. In binary classification, the goal is to predict one of two classes. In this case, the classes are 'win' or 'lose' for a team in a LoL match.

**Response Variable:** The response variable is 'result', indicating whether a team won or lost a game. This variable is chosen because it directly represents the outcome of a game, which is the primary interest in this context.

**Metric for Model Evaluation:**
- **Primary Metric: Accuracy** - This is a suitable metric for scenarios where the classes are relatively balanced and the cost of false positives and false negatives is similar. Accuracy measures the proportion of correctly predicted outcomes (both wins and losses).
- **Secondary Metric: F1-Score** - This metric is useful if there's an imbalance in the dataset or if the cost of false positives and false negatives varies. F1-Score is the harmonic mean of precision and recall, providing a balance between the two.

### Justification for Model and Metrics

- **Choice of Response Variable:** The 'result' variable is a clear and direct measure of a team's success in a game, making it the most relevant variable for this prediction task.
- **Accuracy vs. F1-Score:** Accuracy is chosen as the primary metric because it provides a straightforward interpretation of the model's performance in predicting wins and losses. If the dataset turns out to be imbalanced or if the costs of false positives/negatives are significantly different, then the F1-Score will be considered as it balances precision and recall, providing a more nuanced view of the model's performance.

### Time of Prediction Considerations

When building the model, it's crucial to ensure that only information known before the start of the game is used for prediction. Features that are determined or revealed during or after the game (such as total kills, total gold earned, etc.) should not be included, as they would not be available at the time of prediction. Instead, we focus on pre-game factors like team composition, historical performance, player statistics, etc.

**Suitable Features for Prediction:**
- Team's historical win rate
- Average gold difference at 10/15 minutes (from previous games)
- whether the team secured the first Herald of the game.
- whether the team secured the first Dragon of the game.


**Excluded Features:**
- In-game statistics like total kills, dragons slain, barons taken, etc., as these are outcomes determined during the game.

In summary, the goal is to build a binary classification model to predict whether a team will win or lose a LoL game, using pre-game data and evaluating the model primarily on accuracy, with a consideration for the F1-score if necessary.



### Baseline Model Report

#### Model Description
The baseline model for predicting the outcome (win or lose) of a League of Legends (LoL) esports match is based on a Random Forest Classifier. This model was chosen for its robustness and ability to handle both linear and non-linear relationships. Random Forest is also less likely to overfit compared to some other models, making it a good choice for a baseline model.

#### Features Used in the Model
The model uses two features from the `team_data_cleaned` dataset:
1. **firstherald**: Indicates whether the team secured the first Herald of the game.
2. **firstdragon**: Indicates whether the team secured the first Dragon of the game.

| firstherald | firstdragon |
|:-----------:|:-----------:|
|    True     |    False    |
|    False    |    True     |
|    True     |    False    |
|    False    |    True     |
|    False    |    True     |
|    True     |    False    |


Both of these features are boolean (True/False), which in the context of this model, are treated as binary (1/0). Therefore, they are both nominal variables, representing distinct categories without any intrinsic ordering.

#### Feature Encoding
Given that the features are binary, no complex encoding was necessary. The boolean values were simply converted to integers (1 for True, 0 for False) to facilitate their use in the Random Forest model. This approach is straightforward and preserves the nature of the data.

#### Model Performance
The model's performance was evaluated on a held-out test set, representing 30% of the total dataset. The key metrics for evaluation were:
- **Accuracy**: 57.72%
- **Precision**: 54% (for class 0), 69% (for class 1)
- **Recall**: 86% (for class 0), 31% (for class 1)
- **F1-Score**: 67% (for class 0), 43% (for class 1)

#### Assessment of the Model
While the model demonstrates a reasonable ability to identify class 0 (loss), it struggles with class 1 (win), as indicated by the lower recall and F1-score for class 1. The overall accuracy of 57.72% is somewhat better than random guessing but is not particularly high for practical use.

- **Strengths**: The model is simple and interpretable. It focuses on early-game indicators which are known to have some influence on the game's outcome.
- **Limitations**: The chosen features may not have a strong enough relationship with the outcome by themselves. League of Legends games are complex and are likely influenced by a multitude of factors.

#### Conclusion
The current baseline model provides a starting point but is not yet "good" in terms of predictive performance. Its moderate accuracy and imbalanced precision and recall suggest that it is better at predicting losses than wins. To improve the model, it would be advisable to incorporate additional features that capture more aspects of the game, such as team composition, historical performance, or pre-game strategies. Further model tuning and exploration of different algorithms might also yield better results.



### Detailed Report for Final Model

#### Model Overview
The final predictive model for League of Legends match outcomes is a RandomForestClassifier. This ensemble learning method operates by constructing a multitude of decision trees at training time and outputting the class that is the mode of the classes (classification) of the individual trees.

#### Features and Engineering
The model incorporates features indicative of early-game success and pivotal moments that could influence the match outcome:

- **firstherald**: Teams securing the first Rift Herald can leverage its pushing power for early map control.
- **firstdragon**: The first Dragon grants a permanent buff, providing a lasting advantage.
- **firsttothreetowers**: Early tower leads often correlate with map control and gold advantage.
- **golddiffat15**: Gold leads at 15 minutes reflect early game dominance, which can predict success.
- **quadrakills**: High-impact plays like quadrakills can shift momentum and morale.
- **xpdiffat15**: Experience leads contribute to level advantages, which are crucial for mid-game skirmishes.

| firstherald   | firstdragon   |   firsttothreetowers |   golddiffat15 |   quadrakills |   xpdiffat15 |
|:--------------|:--------------|---------------------:|---------------:|--------------:|-------------:|
| False         | False         |                    0 |          -2667 |             0 |         -712 |
| False         | True          |                    1 |           -513 |             0 |         -702 |
| True          | False         |                    0 |            557 |             1 |        -1134 |
| False         | True          |                    1 |           4306 |             0 |         2576 |
| False         | True          |                    0 |          -4873 |             0 |        -2795 |
| False         | True          |                    0 |           1881 |             0 |         1498 |

#### Box plot about some features

<iframe src="assets/result_golddiffat15.html" width=800 height=600 frameBorder=0></iframe>

The boxplot visualizes the relationship between the outcome of a match (victory or defeat) and the gold difference at the 15-minute mark (golddiffat15).

- **Victory (True)**: For matches that resulted in a victory, the median gold difference is close to zero, indicating that teams can win even with a minimal lead at 15 minutes. However, there are also victories with significant positive gold differences, shown by the outliers.

- **Defeat (False)**: Conversely, for matches that resulted in a defeat, the median gold difference is also around zero, but there are more negative outliers. This suggests that some losses were associated with a substantial gold deficit at the 15 minutes mark.

The analysis suggests that while the gold difference at 15 minutes influences the match's outcome, it is not the sole determining factor. Both victory and defeat have medians near zero, indicating that the final result may be influenced by various factors beyond the gold difference at this specific time.

The distribution could imply that the model's uncertainty in predicting victories might be less than in predicting defeats due to the more concentrated distribution of gold differences for victories and more dispersed for defeats.

Lastly, if the chart serves to inform gaming strategies, it might suggest that maintaining a balanced or slightly positive gold difference at 15 minutes could be a viable strategy, as there are considerable chances to win even with a small gold lead.


#### Hyperparameter Tuning
The RandomForestClassifier's performance depends heavily on several key hyperparameters:
- `n_estimators` defines the number of trees in the forest.
- `max_depth` controls the maximum depth of the trees.
- `min_samples_split` specifies the minimum number of samples required to split an internal node.
- `min_samples_leaf` sets the minimum number of samples required to be at a leaf node.

The optimal values for these parameters were determined through a grid search, balancing model complexity and overfitting. The best hyperparameters were:
- `max_depth`: 10
- `min_samples_leaf`: 2
- `min_samples_split`: 2
- `n_estimators`: 100

#### Performance Metrics
The model's performance improved significantly over the baseline model:
- **Cross-Validation Score**: 78.43%
- **Test Accuracy**: 79.15%
- **Precision**: 79% for both classes
- **Recall**: 79% for both classes
- **F1-Score**: 79% for both classes

#### Confusion Matrix Interpretation

<iframe src="assets/Confusion Matrix.png" width=800 height=600 frameBorder=0></iframe>

The confusion matrix provides insight into the accuracy of predictions:

- **True Positives (TP)**: 2561 matches were correctly predicted as wins.
- **True Negatives (TN)**: 2466 matches were correctly predicted as losses.
- **False Positives (FP)**: 651 matches were incorrectly predicted as wins.
- **False Negatives (FN)**: 673 matches were incorrectly predicted as losses.

The matrix shows a balanced prediction capability for both classes with a slightly higher misclassification rate for predicting wins.

#### Conclusion
The final model demonstrates a robust predictive capability, significantly outperforming the baseline model. The thoughtful selection of features, reflecting key strategic elements of the game, combined with careful hyperparameter tuning, has resulted in a model that is well-suited for predicting the outcomes of League of Legends matches. The balanced precision and recall indicate that the model is just as good at predicting losses as it is at predicting wins, making it a valuable tool for analysts and enthusiasts alike.



### Fairness Analysis Report

#### Group Selection
For the fairness analysis of the final model, two groups were identified based on the league to which the teams belong:

- **Group X (LCS)**: Teams from the North American League Championship Series.
- **Group Y (LCK)**: Teams from the Korean League of Legends Champions Korea.

These groups were chosen to explore whether the model's performance differed between two prominent and distinct regions in professional League of Legends, where strategic and playstyle differences might influence the outcome of matches.

#### Evaluation Metric
The evaluation metric chosen for this analysis was **precision**, which measures the ratio of correctly predicted positive observations to the total predicted positive observations. Precision was selected because it reflects the model's ability to correctly identify wins without being influenced by the number of losses.

#### Hypotheses
The null and alternative hypotheses for the permutation test were defined as follows:

- **Null Hypothesis (H0):** The model is fair across the two groups. The precision for the LCS and LCK is approximately the same, and any observed differences are due to random chance.
  
- **Alternative Hypothesis (H1):** The model is unfair. There is a significant difference in precision between the LCS and LCK groups, indicating bias in the model's performance.

#### Test Statistic and Significance Level
The test statistic chosen was the difference in precision between the LCS and LCK groups. The significance level (alpha) was set at 0.05, commonly used in statistical analysis to control the chance of falsely rejecting the null hypothesis.

#### Permutation Test and P-value
The permutation test involved randomly shuffling the league labels among the teams in the test set and recalculating the difference in precision between the two groups across 1,000 permutations. The resulting p-value was **0.342**.

#### Conclusion
Given the p-value of 0.342, which is greater than the significance level of 0.05, we do not have sufficient evidence to reject the null hypothesis. This suggests that the observed difference in precision between the LCS and LCK groups within the context of this model and data is not statistically significant. Consequently, we conclude that there is no indication of unfair bias in the model's predictions between these two groups based on the precision metric.

It is critical to approach these results with an understanding that they do not definitively prove the absence of bias; rather, they indicate that within the constraints of our analysis, bias was not detected. Fairness in predictive modeling is a multifaceted issue that extends beyond statistical measures and should be considered with broader ethical perspectives.

#### Visualization

<iframe src="assets/Permutation.html" width=800 height=600 frameBorder=0></iframe>
The histogram visualizes the distribution of precision differences resulting from a permutation test conducted to assess the fairness of a predictive model. This model was tasked with predicting outcomes of League of Legends matches, and the permutation test compared the precision of predictions for teams from the North American League Championship Series (LCS) and the Korean League of Legends Champions Korea (LCK).

The x-axis of the histogram represents the precision difference, while the y-axis indicates the count of occurrences within the permutation test. The distribution of differences is symmetrically centered around zero, indicating that there is no substantial difference in the precision of the model's predictions between the LCS and LCK groups.

This symmetry suggests that the model does not exhibit a systematic bias towards one league over the other concerning the precision metric. The permutation test, visualized by this histogram, produced a p-value of 0.342, which exceeds the conventional significance threshold of 0.05. Consequently, the null hypothesis, stating that any observed difference in precision is due to random chance, cannot be rejected.

The fairness analysis concluded that there is no statistical evidence of unfair bias in the model's performance between these two groups, as supported by both the permutation test and the visual evidence from the histogram. The permutation test's results and the histogram reinforce the conclusion that the predictive model operates with comparable precision across the LCS and LCK leagues within the scope of the data analyzed.

<iframe src="assets/Kernel Density.png" width=800 height=600 frameBorder=0></iframe>



### Kernel Density Estimate of Precision Differences
The second image is a kernel density estimate (KDE) plot, which shows a smoothed estimate of the probability density function of the precision differences.

- **Density**: The KDE plot suggests that the most common precision differences are centered around zero, indicating that most permutations did not result in a significant difference in precision between the two groups.
- **Shape**: The smooth, bell-shaped curve of the KDE plot suggests a normal-like distribution of precision differences, which supports the null hypothesis that the observed differences in precision are due to random variation rather than a systematic bias.
- **Spread**: The KDE plot shows that the precision differences are mostly concentrated near the center (around zero), which again suggests no evidence of bias in precision between the two groups.

Both plots support the conclusion that the model's precision is fair across the two groups tested, as the observed differences are centered around zero and show no signs of a consistent bias towards either group. The permutation test resulted in a p-value that did not reach the conventional threshold for statistical significance, further suggesting that the observed differences in model precision can be attributed to random chance rather than unfairness or bias.





<iframe src="assets/box.png" width=800 height=600 frameBorder=0></iframe>

### Box Plot of Precision Differences
The first image is a box plot, which provides a graphical representation of the distribution of precision differences from the permutation test. Here's what the box plot tells us:

- **Central Tendency**: The line in the middle of the box indicates the median of the precision differences, which appears to be close to zero. This suggests that the median difference in model precision between the two groups across all permutations is negligible.
- **Variability**: The box itself shows the interquartile range (IQR), which contains the middle 50% of the precision differences. The IQR does not seem to be skewed towards positive or negative values, reinforcing the finding that there is no substantial systematic difference in precision.
- **Outliers**: The points outside the whiskers represent outliers, which are precision differences that lie beyond 1.5 times the IQR from the box. These outliers indicate that there are some permutations where the precision difference was notably higher or lower than typical, but these are relatively few.
- **Symmetry**: The distribution appears roughly symmetrical about the median, with whiskers of similar lengths on both sides of the box.
