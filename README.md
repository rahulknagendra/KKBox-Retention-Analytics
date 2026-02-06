# KKBox Retention Analytics

Analyzing subscription churn patterns for KKBox, a music streaming service, to identify factors associated with users canceling.

## Overview

What predicts subscription churn in a music streaming service? Using 970,960 subscribers' transaction history, listening logs, and demographic data, I built cohort retention curves, identified churn drivers, and trained a logistic regression model to predict which users are at risk. Churn is defined as failing to renew within 30 days of membership expiration.

## Key Findings

- **Churn is strongly associated with subscription mechanics, not engagement.** Users without auto-renew churn at 30.5%, compared to 3.8% with auto-renew. This 8x difference dominates all other factors.
- **Long-term plans have near-total churn.** The 30-day plan has 4.4% churn; plans of 90+ days have 93-99% churn. These longer plans lack auto-renew, requiring users to actively re-subscribe.
- **Behavioral analysis: Listening activity doesn't predict churn overall, but the pattern of disengagement does among at-risk users.** Among users without auto-renew (the population that actually churns at meaningful rates), those whose listening volume declined most steeply churned at 32.9%, versus 28.1% for those whose completion rate declined most-a 4.8pp gap (16% relative difference). The pattern held across threshold choices: 4.9pp at 10%, 4.8pp at 15%, 4.0pp at 20%. Pathway type has no effect among auto-renew users, which makes sense: they barely churn anyway (3.8%).
- **Younger users churn more.** The 13-17 age group churns at 28.2%, decreasing steadily with age to 8.8% for 55-64 year olds.
- **The churn model achieves 0.92 AUC** with precision of 88% and recall of 61%. The features explain about half of what determines churn (pseudo R-squared = 0.51). Strongest predictors by odds ratio (OR > 1 increases churn, OR < 1 decreases): payment plan length (OR = 4.4), cancellation history (OR = 2.4), and auto-renew (OR = 0.5, meaning 50% lower odds).

## Disengagement Pathway Analysis

Beyond standard churn analysis (correlating engagement metrics with outcomes), the analysis examines how the pattern of behavioral decline reveals distinct disengagement processes. Users are classified by which behavioral dimension declines most steeply: volume (total listening time), completion (finishing songs they start), or exploration (trying new songs). Among non-auto-renew users, Volume-Led users churn at 32.9% versus 28.1% for Completion-Led. This gap matters because it reveals that among users who actually face the renewal decision, how they disengage predicts whether they leave. Different disengagement patterns may reflect different underlying factors, which could warrant different intervention approaches. As hypotheses to test: Volume-Led users may face competing time demands, while Completion-Led users may be experiencing content dissatisfaction, but these interpretations require further research to validate. My sociology background informs this framing: behavioral change follows distinct trajectories that reveal distinct causes, not uniform decline.

## Business Implications

- **Auto-renew is the primary lever.** Converting users to auto-renew would have more impact than any engagement intervention. The 8x churn difference makes this the highest priority retention mechanism.
- **Long-term plan users need renewal reminders.** Users on 90+ day plans churn at near 100% rates because they must actively re-subscribe. Targeted reminders before expiration could reduce this.
- **Different disengagement pathways may warrant different interventions (hypotheses to test).** If Volume-Led users are experiencing time constraints, they may respond to convenience features like offline mode or shorter playlists. If Completion-Led users are dissatisfied with content, they may benefit from different recommendation approaches. Validating the underlying causes would require additional research (e.g., user surveys, A/B testing intervention types).
- **Focus retention efforts on non-auto-renew users.** Among auto-renew users, behavioral patterns don't predict churn. Intervention resources should target users who must actively decide on renewal.

## Notebooks

1. **01_exploratory_data_analysis.ipynb**: Dataset structure, data quality assessment, coverage analysis
2. **02_cohort_retention.ipynb**: Monthly cohort activity retention curves
3. **03a_churn_drivers.ipynb**: Demographics, transaction behavior, listening patterns
4. **03b_disengagement_pathways.ipynb**: Behavioral clustering, disengagement pathway analysis
5. **04_feature_engineering.ipynb**: Creating user-level features from raw data
6. **05_churn_model.ipynb**: Logistic regression model, evaluation metrics, feature importance

## Dashboard

[Interactive Dashboard (Looker Studio)](https://lookerstudio.google.com/reporting/e3242a25-decf-4b1d-a453-1b9ebd23ddb8): Explore churn rates by segment with cross-filtering. Filter by age group, auto-renew status, and plan type to see how churn varies across user populations.

[Static Version (PDF)](dashboards/static_retention_dashboard.pdf): Summary dashboard for offline viewing.

## Data Source

Kaggle KKBox Churn Prediction Challenge dataset.

- **Train set**: 970,960 users with churn labels
- **Members**: 6.8 million user demographic records
- **Transactions**: 1.4 million payment records (Jan 2015 - Mar 2017)
- **User logs**: 392 million daily listening records (Jan 2015 - Feb 2017)

See [docs/data_dictionary.md](docs/data_dictionary.md) for field definitions and data quality notes.

## Setup

1. Install dependencies:
```
pip install -r requirements.txt
```

2. Create a `.env` file in the project root with your GCP project ID:
```
GCP_PROJECT=your-project-id
```

3. Authenticate with Google Cloud:
```
gcloud auth application-default login
```

4. Run notebooks in order (01 through 05).