![WhatsApp Image 2025-06-25 at 09 33 43_933907a2](https://github.com/user-attachments/assets/5072f155-9160-44b4-8d34-ae6a6096106b)


# RedBus Data Decode Hackathon

This repository contains all relevant notebooks and code that helped our team **𝐋𝐨𝐰 𝐁𝐢𝐚𝐬 𝐇𝐢𝐠𝐡 𝐕𝐢𝐛𝐞𝐬** secure **#5 on the private leaderboard** (out of 8000+ participants) in the **redBus Data Decode Hackathon** hosted by **Analytics Vidhya**.

We made a strong leap from **#11 on the public leaderboard to #5 on the private**, with **CV and test scores staying closely aligned** — always a good sign of a generalizable solution.

Our final score was **533.58587541**, just **0.1 RMSE** shy of the **2nd Runner-Up** spot — a better seed or refined hyperparameters could have made the difference.

---

## Problem Statement

Forecast the number of bus seats booked for a given route on a specific **Date of Journey (DOJ)** using only data available **15 days before the DOJ** — including seat search and booking history, route metadata, and other features.

---

## Final Solution Overview

We opted for a simple yet effective strategy centered around a **single LightGBM model**, powered by exploratory data analysis, thoughtful feature engineering, and targeted post-processing.


### Feature Engineering Highlights

- **Lag features**: 3-day lag of cumulative `seat_searches` for the current route to mitigate cumulative seat_searches as seen during EDA.
- **Time-based features**: Extracted **weekday**, **day-of-month**, **long weekends**, etc., which provided more signal than raw dates.
- **External calendar features**: Added regional and national **holidays/festivals** as a single feature ordinally encoded with respect to its relevance.

### Post-Processing and Strategy

- **Train/Test Distribution Analysis**: Boosted prediction **mean and variance** to offset LGBM's lack of extrapolation capabilities (unlike neural nets).
- **A trusty CV split to hold it all together**: Stratified based on route and time, enabling the model to generalize better across unseen patterns.


## Notes on Test Distribution Shift

The **test data corresponded to January and February 2025**, which saw an **unusual spike in bookings** due to **special events like Mahakumbh and Coldplay concert**. These events were not represented in the training data, making CV-to-LB correlation weaker than expected.
However, our **distribution-aware boosting technique** — originally designed to handle LGBM’s extrapolation limitations — turned out to be highly effective in compensating for this shift, giving us a significant edge on the private leaderboard.

---


## What Didn’t Work

Several experiments did not yield improved performance on validation/test splits:

- **Target normalization**: To battle mean shift trends, as they are not catered by tree-based models (highly train distribution dependent) , but failed probably due to unrelaiable batch-statisitcs.
- **TabNet, RuleNet, and MLPs**: Unable to generalise seasonality effectively on the CV set.
- **Other Route's features**: Including bookings/searches on correlated routes and 3-day lag of return journey routes only slightly improved validation scores.
- **Classical time-series models**: SARIMA and LSTMs failed to outperform CV baseline and suffered on unseen trends.

---

## Acknowledgments

Thanks to **redBus** and **Analytics Vidhya** for organizing a well-structured and practical competition based on real-world data.

A huge shoutout to my teammates **Mukil M**, **MOHD ASHAZ KHAN**, and **Dilshad Raza** — your collaboration, breadth of exploration, and ability to converge on the right path made this solution possible.



