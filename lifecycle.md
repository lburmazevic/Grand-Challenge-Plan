# Lifecycle Transition - Propensity Modeling Plan
## 1. Goal:
Find users who are approaching diaper graduation and predict: 
1. Which users are still engaged and cross-sellable
2. Who requires early transition intervention

## 2. Datasets
| Dataset                     | Role in Lifecycle Campaign                                 |
| --------------------------- | ---------------------------------------------------------- |
| utenti_reduced              | Child age, region, consent flags                           |
| rewusers_reduced            | User-level engagement summary, total points, last activity |
| accessi_reduced             | Login behavior (activity frequency and recency)            |
| codici_reduced              | Product scans (core behavioral signal)                     |
| missioni_reduced            | Mission engagement and reward interaction                  |
| premi_reduced               | Reward redemption behavior                                 |
| Anagrafica_prodotti_digital | Optional product category enrichment                       |


## 3. Transition Windows
As in the previous plan, we assume that the diaper usage ends at **36 - age months**

Now, we use child age to define lifecycle groups.

**Lifecycle Groups:**

| Stage      | Age Range | Strategic Meaning        |
| ---------- | --------- | ------------------------ |
| **Core**       | <24       | Normal diaper usage      |
| **Late**       | 24–30     | Pre-transition           |
| **Transition** | 30–36     | Critical campaign window |
| **Post**       | >36       | Likely natural exit      |


## 4. Feature Groups

For each user, summarize recent activity patterns (how often they log in, scan products, complete missions, and redeem rewards) to understand whether engagement is stable or declining.

### 4.1 Engagement Momentum
Using logins, scans, missions:

| Variable                     | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| login_recency_days          | Number of days since the user last logged into the app                     |
| scan_recency_days           | Number of days since the user last scanned a product code                  |
| mission_recency_days        | Number of days since the user last completed or interacted with a mission  |
| last_activity_recency_days  | Number of days since the user’s most recent activity (login, scan, mission)|
| logins_30d                  | Number of logins in the last 30 days                                        |
| logins_90d                  | Number of logins in the last 90 days                                        |
| scans_30d                   | Number of product scans in the last 30 days                                 |
| scans_90d                   | Number of product scans in the last 90 days                                 |
| missions_90d                | Number of missions completed or interacted with in the last 90 days        |


**Purpose:** Detect whether engagement is stable, increasing, or fading as age approaches 36 months.

**ABOVE VARIABLES ARE JUST IDEAS**

### 4.2 Value Interaction

Using points, missions, and redemptions:

| Variable                 | Description                                                                             |
| ------------------------ | --------------------------------------------------------------------------------------- |
| totalPoints              | Total points accumulated by the user                                                    |
| counterMissions          | Total number of missions completed by the user                                          |
| scan_points_90d          | Total points earned from product scans in the last 90 days                              |
| mission_points_90d       | Total points earned from missions in the last 90 days                                   |
| redeems_lifetime         | Total number of rewards redeemed by the user                                            |
| redeem_recency_days      | Number of days since the user last redeemed a reward                                    |
| points_redeemed_lifetime | Total points spent on rewards historically                                              |
| redemption_ratio         | Share of earned points that have been redeemed (points_redeemed_lifetime / totalPoints) |
| avg_points_per_redeem    | Average number of points spent per reward redemption                                    |

**Purpose:** Detect whether high-value or reward-engaged users behave differently as the child approaches graduation.

**ABOVE VARIABLES ARE JUST IDEAS**


### 4.3 Loyalty & Tenure

Using user profile and engagement summary data:

| Variable                   | Description                                                 |
| -------------------------- | ----------------------------------------------------------- |
| tenure_days                | Number of days since the user registered in the app         |
| tenure_months              | Number of months since registration                         |
| user_active_flag           | Whether the user is currently marked as active              |
| last_activity_recency_days | Number of days since the user’s last recorded activity      |
| app_last_recency_days      | Number of days since the user last used the mobile app      |
| site_last_recency_days     | Number of days since the user last used the website         |
| platform                   | Primary platform used by the user (e.g., iOS, Android, Web) |
| source                     | Original acquisition source of the user                     |

**Purpose:** Distinguish long-term loyal users from recently acquired users and understand whether tenure influences engagement near diaper graduation.

**ABOVE VARIABLES ARE JUST IDEAS**


## 5. Prediction

We are predicting short-term continuation of engagement during late lifecycle.

Among users aged 24+ months, who will continue engaging (e.g., scanning) in the next 30–60 days?

| Component   | Description                                                                    |
| ----------- | ------------------------------------------------------------------------------ |
| Population  | Users with `age > 24`                                         |
| Focus Group | Special attention to `30–36 months` (transition phase)                         |
| Target      | Future engagement (e.g., at least one scan in next 30–60 days)                 |
| Features    | Engagement Momentum + Value Interaction + Loyalty & Tenure + Lifecycle context |
| Output      | `propensity_continuation_score`                                                |
| Validation  | Time-based split to simulate real deployment                                   |

This creates a **Lifecycle Continuation Propensity Score**.

The score reflects the probability that a user remains behaviorally active despite approaching diaper graduation.

## 6. Different Outcomes

After scoring, users should be grouped into clear, campaign-ready segments.

| Segment                   | Description                                | Campaign Direction                            |
| ------------------------- | ------------------------------------------ | --------------------------------------------- |
| High-Potential Transition | Age 30–36 months + high continuation score | Cross-sell other brands, premium missions     |
| At-Risk Transition        | Age 30–36 months + low continuation score  | Retention incentive (points boost, nudges)    |
| Early Transition Prep     | Age 24–29 months + medium/high score       | Soft education, transition preparation        |
| Natural Exit              | Age >36 months + low score                 | Reduce incentive spend, minimal communication |


## 7. Deliverables

| Deliverable                     | Purpose                                   |
| ------------------------------- | ----------------------------------------- |
| Scored user table               | Campaign activation dataset               |
| `propensity_continuation_score` | Rank users by likelihood to remain active |
| Lifecycle segment label         | Operational targeting logic               |
| Feature importance analysis     | Explain key behavioral drivers            |
| Behavioral insights summary     | Competition-ready narrative               |

