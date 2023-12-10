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

**Suitable Features for Prediction:** (Examples)
- Team's historical win rate
- Average gold difference at 10/15 minutes (from previous games)
- Player performance metrics (from previous games)
- Team composition and strategy (e.g., champion picks)

**Excluded Features:**
- In-game statistics like total kills, dragons slain, barons taken, etc., as these are outcomes determined during the game.

In summary, the goal is to build a binary classification model to predict whether a team will win or lose a LoL game, using pre-game data and evaluating the model primarily on accuracy, with a consideration for the F1-score if necessary.
