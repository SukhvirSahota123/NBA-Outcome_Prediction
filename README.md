# NBA_Outcome_Prediction
This project analyzes NBA box score data from the 2023-2024 season to predict whether a team won or lost a game, based on its team-level performance statistics.

This starts by aggregating player-level box score stats into team-level summaries for each game. Then additional features such as field goal percentage (FG%) and free throw percentage (FT%) are calculated to better capture team efficiency. 

Team-Level Features:
1. team_pts: Total points scored by the team in the game
2. team_reb: Total rebounds
3. team_ast: Total assists
4. team_to: Total turnovers
5. team_pm: Plus-minus
6. team_stl: Steals
7. team_blk: Blocks
8. team_pf: Personal fouls
9. FG_pct: Field goal percentage
10. FT_pct: Free throw percentage
11. Result: Win or Loss (W/L)

To prevent data leakage, identifiers (e.g., game IDs, team abbreviations) and the final score column margin (PLUS_MINUS) are excluded from modeling. The target variable is a binary outcome indicating win (W) or loss (L) determined after the game has already occurred. This means that the project is not a real-time prediction tool but instead a statistical analysis of historical results, identifying which performance metrics are most strongly associated with winning.

Two models are compared:
1. Random Forest (RF) - An ensemble machine learning method that handles non-linear relationships and interactions between features.
2. Logistic Regression (Logit) - A statistical baseline model that estimates win probability as a function of the selected features.

To improve model interpretability, feature selection is performed inside each training fold using Random Forest's variable importance score. The top six features are retained for both models to ensure a fair comparison. 

Model evaluation is done using 5-fold cross-validation, where the dataset is repeatedly split into training and testing folds.
Performance is measured using:
1. Accuracy: The percentage of games correctly classified as wins or losses.
2. Area under the receiver operating characteristic curve (AUC): Evaluates how well models separate winning and losing teams across probability thresholds.

Results are visualized through:
1. An average feature importance bar plot (highlighting the most influential features).
2. A performance comparison bar chart (Comparing RF vs Logit Reg in terms of accuracy and AUC).

## Results:
Top six features (Mean Decrease Gini):
1. FG_pct ~ 174.8
2. team_pts ~ 138.7
3. team_reb ~ 133.9
4. FT_pct ~ 76.5
5. team_pf ~ 73.7
6. team_ast ~ 72

We see that field goals are the most important feature, this is not surprising since this feature directly reflects scoring efficiency, which is highly predictive of wins. The large gap in importance between FG% and everything else suggests that shooting accuracy is the single most critical determinant of outcome.

Total points in second behind field goals suggests that the quality of each shot is a stronger predictor than the volume of shots taken, though points in general are the strongest indicators.

Rebounding being tied with points as a strong predictor is to be expected as high rebounding means more scoring opportunities (offensive rebounding) or denying opponents second chances (defensive rebounds). (i.e., controlling the boards is strongly tied to winning)

Free throws in fourth suggests that while free throws have potential to swing games, they don't dominate the outcome.

A high foul count can mean giving away free points and also hurting defense if key players get into trouble.

Assists being the least important of the six suggests that while ball movement is important, pure shot making and rebounding is more critical for determining the outcome.

Model Performance:
- RF: ~ 0.78 Accuracy, ~ 0.87 AUC
- Logit Reg ~ 0.79 Accuracy, ~ 0.88 AUC

Logistic Regression model outperforms Random Forest model in both AUC and Accuracy for the following reasons:
1. The feature set is relatively small and well structured:
     Thus the aggregated team stats already capture performance in a direct and interpretable way. Logistic regression can model the relationship between these predictors and win/loss efficiently without needing       complex interactions.
2. There is a strong linear relationship with the outcome:
     Many of the features have nearly linear relationships with winning probability. Logistic regression handles that well, while the random forest might overfit noise or redundant splits.
3. Cross validation stabilizes the comparison:
     Because both models were trained with feature selection and CV, the performance estimates are robust. The close scores suggest that both models are viable.

## Takeaway:
Logistic regression slightly outperformed random forest (~0.79 vs ~0.78 accuracy and ~0.88 vs ~0.87 AUC). This shows that NBA game outcomes are largely explained by straightforward, linear relationships in team-level stats like shooting efficiency and rebounding, so a simple statistical model can be just as or more effective than a complex ensemble. 
   
## Limitations:
1. Analysis is retrospective, so it cannot be used for pregame predictions.
2. Only one season of data is included.
3. Does not include contextual factors (injuries, rest days, home/away status).
