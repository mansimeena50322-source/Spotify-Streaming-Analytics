# Spotify Streaming Analytics

An end-to-end Spotify streaming analytics platform built to analyze 150K+ streaming events, uncover listening behavior and retention patterns, predict track skips using machine learning, segment users based on behavioral patterns, and visualize insights through an interactive Streamlit dashboard.

The project combines advanced SQL analytics, Python feature engineering, machine learning, cohort analysis, user segmentation, and interactive visualization into a complete analytics workflow.

---

## Project Highlights

* Processed and analyzed 150K+ Spotify streaming events
* Engineered 82 analytical and machine learning features using Python
* Applied advanced SQL techniques including CTEs, window functions, sessionization, cohort analysis, and rolling metrics
* Developed machine learning models for skip prediction using Random Forest, Gradient Boosting, and Logistic Regression
* Performed user segmentation using K-Means clustering
* Conducted cohort-based retention analysis
* Analyzed session duration, listening behavior, and cross-platform engagement
* Built an interactive Streamlit analytics dashboard
* Created interactive Plotly visualizations for behavioral and retention analysis

---

## Objectives

The project focuses on understanding user listening behavior and answering key business and analytical questions.

### User Behavior

* How do users interact with music across different sessions?
* What factors influence listening duration?
* How does engagement vary across platforms?
* Which users demonstrate the highest engagement?

### Skip Prediction

* Can track skipping be predicted using user and session behavior?
* Which features are most associated with skip behavior?
* How do different machine learning models perform on the prediction task?

### User Segmentation

* Can users be grouped based on their listening behavior?
* What distinguishes highly engaged users from casual listeners?
* Which behavioral characteristics define each user segment?

### Retention

* How does user retention change across cohorts?
* Which cohorts demonstrate stronger long-term engagement?
* How does engagement evolve over time?

---

## System Architecture

```text
                    Spotify Streaming Events
                              |
                              v
                    Data Cleaning & Processing
                              |
                +-------------+-------------+
                |                           |
                v                           v
          SQL Analytics              Python Feature
                                     Engineering
                |                           |
                |                      82 Features
                |                           |
                +-------------+-------------+
                              |
                +-------------+-------------+
                |                           |
                v                           v
        Analytical Insights          Machine Learning
                |                           |
        +-------+-------+           +-------+-------+
        |       |       |           |       |       |
     Sessions Cohorts Rolling    Logistic Random Gradient
                         Metrics   Reg.   Forest  Boosting
                                        |
                                        v
                                  K-Means Clustering
                                        |
                +-----------------------+
                |
                v
             Streamlit
             Dashboard
                |
                v
          Plotly Visualizations
```

---

# Data Processing

The project begins with a large collection of Spotify streaming events containing information related to users, tracks, timestamps, sessions, platforms, and listening behavior.

The raw data is cleaned and transformed before being used for analytics and machine learning.

### Data preprocessing includes

* Handling missing values
* Removing inconsistent records
* Timestamp conversion and normalization
* Categorical variable processing
* Creation of temporal features
* Preparing user-level and session-level datasets
* Creating variables required for machine learning

---

# Advanced SQL Analytics

SQL is used extensively to transform raw streaming events into meaningful behavioral and business metrics.

## Common Table Expressions

CTEs are used to divide complex analytical queries into modular and readable steps.

```sql
WITH user_streams AS (
    SELECT
        user_id,
        track_id,
        played_at,
        duration_ms
    FROM streaming_events
)
SELECT *
FROM user_streams;
```

## Window Functions

Window functions are used to calculate sequential and rolling metrics without collapsing individual event records.

Applications include:

* Running totals
* Rolling metrics
* User-level rankings
* Previous and next listening events
* Sequential listening behavior

Example:

```sql
SELECT
    user_id,
    played_at,
    track_id,
    LAG(played_at) OVER (
        PARTITION BY user_id
        ORDER BY played_at
    ) AS previous_play
FROM streaming_events;
```

---

# Sessionization

Streaming events are grouped into listening sessions using the time gap between consecutive events from the same user.

Conceptually:

```text
User Events

10:02  Track A
10:05  Track B
10:08  Track C
          |
          v
       Session 1

11:25  Track D
11:29  Track E
          |
          v
       Session 2
```

Session-level features are then calculated, including:

* Session duration
* Number of tracks per session
* Average track duration
* Skip rate
* Listening frequency
* Platform used during the session

Sessionization enables analysis at a more meaningful behavioral level than individual streaming events.

---

# Cohort and Retention Analysis

Users are grouped into cohorts based on their initial activity period. Their subsequent activity is then tracked to measure retention over time.

Example:

```text
              Week 0   Week 1   Week 2   Week 3
------------------------------------------------
Cohort A       100%      72%      58%      49%
Cohort B       100%      75%      61%      53%
Cohort C       100%      69%      51%      43%
```

The analysis helps identify:

* Retention trends
* Cohort performance
* Engagement decay
* User activity patterns
* Potential churn behavior

---

# Feature Engineering

Python is used to transform event-level streaming data into user-level, session-level, temporal, and behavioral features.

A total of 82 features were engineered for analytical and machine learning workflows.

## User Features

* Total streams
* Unique tracks
* Unique artists
* Listening frequency
* Average listening duration
* Repeat listening behavior

## Session Features

* Session duration
* Tracks per session
* Average track duration
* Session skip rate
* Session frequency

## Temporal Features

* Hour of day
* Day of week
* Weekend indicator
* Listening time patterns
* Time-based activity metrics

## Engagement Features

* Track diversity
* Artist diversity
* Listening frequency
* Repeat consumption
* Platform usage
* Overall engagement

## Skip-Related Features

* Historical skip behavior
* Listening duration
* Track position
* Session characteristics
* User engagement metrics
* Temporal listening patterns

---

# Machine Learning

## Skip Prediction

Track skipping is formulated as a binary classification problem.

```text
Skip = 1
No Skip = 0
```

The engineered behavioral features are used to predict whether a user will skip a track.

### Models

| Model               | Purpose                                                  |
| ------------------- | -------------------------------------------------------- |
| Logistic Regression | Interpretable baseline model                             |
| Random Forest       | Captures non-linear relationships and feature importance |
| Gradient Boosting   | Strong predictive performance on complex patterns        |

The models are trained and evaluated to determine how effectively user and session behavior can predict track skips.

---

# User Segmentation

K-Means clustering is used to segment users based on their listening behavior.

The clustering process considers behavioral dimensions such as:

* Listening frequency
* Session duration
* Track diversity
* Artist diversity
* Skip rate
* Platform usage
* Overall engagement

Conceptually:

```text
                    User Engagement
                           ^
                           |
             +-------------+-------------+
             |             |             |
             |   Highly    |   Active    |
             |   Engaged   |   Listeners |
             |             |             |
             +-------------+-------------+
             |             |             |
             |   Casual    |     Low     |
             |   Users     |  Engagement |
             |             |             |
             +-------------+-------------+
                           |
                           +---------------->
                              Listening
                              Frequency
```

The resulting clusters provide a behavioral view of different listener segments.

---

# Streamlit Dashboard

The final analytics layer is implemented using Streamlit.

The dashboard provides interactive views of the underlying streaming data, user behavior, machine learning results, and retention metrics.

## Dashboard Sections

### Overview

* Total streaming events
* Active users
* Total sessions
* Average session duration
* Overall skip rate

### Listening Behavior

* Listening duration
* Tracks per session
* Listening time patterns
* Platform engagement
* User activity trends

### User Segmentation

* K-Means clusters
* Cluster distribution
* Engagement characteristics
* Behavioral comparisons between segments

### Retention Analysis

* Cohort retention
* Retention curves
* Rolling engagement
* User activity trends

### Machine Learning

* Skip prediction results
* Model comparison
* Feature importance
* Prediction distributions

---

# Visualizations

Plotly is used to create interactive visualizations for exploring streaming behavior and analytical results.

The dashboard includes visualizations such as:

* Time-series charts
* Cohort heatmaps
* Retention curves
* User segment distributions
* Feature importance charts
* Platform comparison charts
* Session-duration distributions
* Engagement trends

The interactive visualizations allow users to filter, hover, zoom, and explore different dimensions of the dataset.

---

# Technology Stack

| Category             | Technologies                                          |
| -------------------- | ----------------------------------------------------- |
| Programming          | Python                                                |
| Data Analysis        | Pandas, NumPy                                         |
| Database & Analytics | SQL                                                   |
| Machine Learning     | Scikit-learn                                          |
| Classification       | Logistic Regression, Random Forest, Gradient Boosting |
| Clustering           | K-Means                                               |
| Visualization        | Plotly                                                |
| Dashboard            | Streamlit                                             |

---

# Project Structure

```text
spotify-streaming-analytics/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── sql/
│   ├── exploratory_analysis.sql
│   ├── sessionization.sql
│   ├── cohort_analysis.sql
│   └── rolling_metrics.sql
│
├── notebooks/
│   ├── exploratory_analysis.ipynb
│   ├── feature_engineering.ipynb
│   └── machine_learning.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── feature_engineering.py
│   ├── skip_prediction.py
│   └── segmentation.py
│
├── dashboard/
│   └── app.py
│
├── models/
│   └── trained_models/
│
├── requirements.txt
└── README.md
```

The structure above should be modified to match the actual repository structure.

---

# Installation

## 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/<repository-name>.git
cd <repository-name>
```

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### Linux/macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# Running the Dashboard

Start the Streamlit application:

```bash
streamlit run dashboard/app.py
```

The dashboard will then be available locally in your browser.

---

# Example Analytical Workflow

A typical analysis can follow this workflow:

```text
Raw Streaming Events
        |
        v
Data Cleaning
        |
        v
SQL-Based Analytics
        |
        +----------------------+
        |                      |
        v                      v
Sessionization            Cohort Analysis
        |                      |
        +----------+-----------+
                   |
                   v
           Feature Engineering
             82 Features
                   |
          +--------+--------+
          |                 |
          v                 v
    Skip Prediction    User Segmentation
          |                 |
          +--------+--------+
                   |
                   v
          Streamlit Dashboard
                   |
                   v
          Interactive Insights
```

---

# Key Analytical Outcomes

The project enables analysis of:

* User engagement and listening behavior
* Session-level consumption patterns
* Cross-platform engagement
* Factors associated with track skipping
* Listener behavioral segments
* Cohort-level retention
* Rolling engagement trends
* Differences between highly engaged and casual users

---

# Future Improvements

* Real-time streaming event ingestion
* Personalized music recommendation system
* Advanced churn prediction
* Hyperparameter optimization
* Model explainability using SHAP
* Real-time skip prediction API
* Automated model retraining
* Cloud deployment
* User-level personalized recommendations
* Automated analytics reporting

---

# Disclaimer

This project is intended for educational and analytical purposes. It does not represent or reproduce Spotify's internal analytics systems.

Any datasets used should comply with their respective licensing and usage terms.

---

# Author

**Mansi Meena**

This project was developed as a self-project to explore:

* Data analytics
* Advanced SQL
* Feature engineering
* Machine learning
* User segmentation
* Cohort analysis
* Interactive data visualization
* Dashboard development


