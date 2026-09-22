# FIFA 22 Player Market Value Analysis

## Overview
This repository contains an exploratory data analysis (EDA) and value estimation model for FIFA 22 players. The goal of this project is to identify the key drivers of player market value, estimate continuous market values, identify the top 10% most valuable players, and screen for potentially under-priced players.

The provided FIFA 22 dataset contains 5,969 players and 21 original columns, with `value_eur` as the target variable.

## Key Findings
*   **Extreme Value Concentration:** Player market value is highly right-skewed, with the median player worth about €1.6M and the mean at about €5.21M. The top 10% threshold is €13.0M, representing roughly 61% of total player value.
*   **Nonlinear Quality Premium:** Overall and Potential are the strongest individual signals of player value, and their relationship with market value is strongly nonlinear. Value rises increasingly sharply as players move into higher quality levels.
*   **Youth and League Context:** Younger players consistently command a premium, and comparable players in the "Big Five" leagues show a median value premium of about 16% after controlling for age, Overall, and Potential.

## Data Preparation
*   **Target Variable:** Converted `value_eur` from text formatting to a numeric format and removed the redundant index column.
*   **Structural Missing Values:** Identified that the 543 missing values in outfield skills (pace, shooting, passing, dribbling, defending, physic) belong exclusively to Goalkeepers, representing structural missing values rather than data errors.
*   **Feature Engineering:** Summarized the highly sparse `player_tags` feature into a compact `number_of_tags` feature to capture accumulated notable attributes without overfitting.

## Modeling Approach
A regression-first approach was chosen to support both Top-10% screening and under-pricing analysis.
*   **Algorithm:** Global XGBoost.
*   **Target:** Log-transformed `value_eur`.
*   **Key Features:** Potential and Overall dominate the model (accounting for roughly 46% and 42% of total importance, respectively), followed by Age and Dribbling.
*   **Performance:** The final global model achieved an R-squared of 0.988 and an RMSE of €1.304M in out-of-fold testing.

## Top-10% Identification
The continuous value predictions were used as ranking scores to identify the most valuable 10% of players in a held-out test set (1,194 players).
*   **Results:** The model identified the held-out test set Top 10% with 99.2% precision and 99.2% recall.
*   **Accuracy:** 118 of 119 true Top-10% players were correctly identified.

## Files in this Repository
*   `df_players.csv`: The primary dataset containing FIFA 22 player attributes.
*   `main.ipynb`: The Jupyter Notebook containing the data loading, EDA, and data profiling steps.
*   `FIFA22_Player_Market_Value_Report.pdf`: The detailed executive summary, methodology, and full analytical report.
