# Subscription-Churn-Prediction

## Model Interpretability and Feature Analysis

To better understand the factors influencing the Gradient Boosting model's churn predictions, I evaluated feature importance using three complementary approaches: **built-in feature importance, permutation importance, and SHAP (SHapley Additive exPlanations)**.

Using multiple approaches provides a more comprehensive understanding of model behavior by examining overall feature contributions, the effect of individual features on predictive performance, and the direction of their influence on individual predictions.

### 1. Built-in Feature Importance

The Gradient Boosting classifier provides feature importance scores based on each feature's contribution to reducing impurity across the decision trees.

The results identified four primary features:

| Feature | Importance |
|---|---:|
| Payment failures | 0.3548 |
| Days since last login | 0.2493 |
| Average weekly usage hours | 0.2457 |
| Support tickets | 0.1461 |
| Tenure months | 0.0037 |
| Plan type (Standard) | 0.0005 |
| Monthly fee | 0.0000 |
| Plan type (Premium) | 0.0000 |

Payment failures represented the largest share of built-in feature importance, followed by days since last login and average weekly usage hours. Together, these four primary features accounted for approximately 99.6% of the model's total feature importance.

These results suggest that payment-related difficulties, customer engagement, and support activity are the primary factors the model uses when distinguishing between churned and retained customers.

### 2. Permutation Importance

To further evaluate the predictive contribution of individual features, I performed permutation importance analysis using ROC-AUC as the evaluation metric.

Permutation importance measures the decrease in model performance when the values of a feature are randomly shuffled. A larger decrease indicates that the model relies more heavily on that feature to make predictions.

The analysis produced the following results:

| Feature | Mean Importance | Standard Deviation |
|---|---:|---:|
| Days since last login | 0.0795 | 0.0175 |
| Payment failures | 0.0792 | 0.0188 |
| Average weekly usage hours | 0.0757 | 0.0133 |
| Support tickets | 0.0435 | 0.0073 |
| Plan type | 0.0004 | 0.0002 |
| Monthly fee | 0.0000 | 0.0000 |
| Tenure months | -0.0003 | 0.0006 |

The permutation results reinforce the findings from the built-in feature importance analysis. Days since last login, payment failures, and average weekly usage hours had the greatest impact on ROC-AUC when their values were shuffled.

Support tickets also contributed to predictive performance, although its measured contribution was smaller than the three leading features.

Plan type, monthly fee, and tenure demonstrated little to no measurable contribution in this experiment. The slightly negative importance for tenure indicates that shuffling this feature produced a negligible improvement in validation ROC-AUC rather than a meaningful predictive benefit.

### 3. SHAP Analysis

SHAP was used to examine how individual feature values influence the model's predictions. Unlike traditional feature importance scores, SHAP provides information about both the magnitude and direction of a feature's contribution to an individual prediction.

The SHAP bar plot identified payment failures, days since last login, average weekly usage hours, and support tickets as the most influential features. This finding was consistent with the built-in and permutation importance results.

The SHAP summary (beeswarm) plot provided additional insight into the relationship between feature values and model predictions:

- **Payment failures:** Higher payment failure values generally pushed predictions toward the churn class, while lower values generally pushed predictions toward the retained class.
- **Days since last login:** Higher values generally increased churn predictions, suggesting that customers who had not logged in recently were more likely to be classified as churn risks.
- **Average weekly usage hours:** Lower usage values generally pushed predictions toward the churn class, while higher usage values generally pushed predictions toward the retained class.
- **Support tickets:** Higher support ticket counts generally pushed predictions toward the churn class, indicating that customers with more support interactions were more likely to receive higher churn predictions.

The remaining features, including tenure, plan type, and monthly fee, demonstrated comparatively limited influence on the model's predictions.

### 4. Key Findings and Business Implications

The three feature-importance approaches produced broadly consistent results, identifying payment failures, customer inactivity, weekly usage, and support tickets as the most influential variables in the Gradient Boosting model.

These findings suggest several potential areas for customer retention efforts:

1. **Payment intervention:** Identify customers experiencing repeated payment failures and consider proactive payment assistance or payment-method updates.
2. **Customer engagement:** Monitor declining login activity and consider targeted engagement campaigns for customers demonstrating reduced platform usage.
3. **Product engagement:** Identify customers with consistently low weekly usage and consider personalized recommendations, onboarding assistance, or educational resources.
4. **Customer support:** Examine recurring support interactions to identify potential service difficulties and opportunities to improve the customer experience.

These recommendations represent potential applications of the model's findings rather than proven interventions. Additional analysis would be necessary to determine whether these actions actually reduce customer churn.

### 5. Limitations and Considerations

Although the feature-importance analyses provide useful insight into model behavior, several limitations should be considered.

- **Predictive importance does not establish causation:** The identified features are useful for distinguishing between churned and retained customers, but their importance does not demonstrate that they directly cause churn.
- **Correlated features:** Features containing overlapping information may share or mask predictive importance, particularly in permutation importance analysis.
- **Model-specific interpretation:** The importance rankings describe how the selected Gradient Boosting model uses the available features. Different algorithms may produce different rankings.
- **Temporal data limitations:** The dataset does not provide a specific observation date or snapshot date for individual customers. Consequently, it is not possible to confirm that all predictor variables were recorded before the churn outcome occurred. This creates a potential risk of temporal data leakage, particularly for last login activity, support tickets, and payment failures.

Because of this temporal limitation, the model's results should be interpreted as exploratory classification performance rather than evidence of reliable prospective churn prediction. Future analysis should use timestamped customer records and establish a consistent observation window before the churn outcome.

Overall, the feature-importance analyses indicate that payment activity, customer engagement, and support interactions are the most influential signals used by the selected Gradient Boosting model to classify customer churn.
