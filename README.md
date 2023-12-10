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
