# Citi Bike Analytics with PySpark

An academic data engineering and analytics project that processes **1.3 million Citi Bike trip records** using PySpark, explores operational patterns through Spark SQL, compares Spark ML classifiers, and presents selected findings in an interactive Bokeh dashboard.

[Open the notebook](bigdata%20%282%29%20%281%29.ipynb)

Place this README beside `bigdata (2) (1).ipynb`. The notebook is provided in its original form, with saved outputs; its experiments have not been independently rerun for this description.

## Project scope

| Component | What it includes |
| --- | --- |
| Data preparation | Column standardization, missing-value inspection, validation rules, and filtering |
| Feature engineering | Age, duration, endpoint distance, speed proxy, time-of-day, month, age group, and season |
| Analytics | 10 core Spark SQL analyses using aggregations, CTEs, and window functions |
| Machine learning | Logistic Regression, Decision Tree, and Random Forest pipelines |
| Visualization | Four linked Bokeh charts controlled by a user-type selector |

## Dataset and recorded scale

The notebook loads a CSV containing 2019 Citi Bike trips from a Google Drive link embedded in its first code cell. The loaded file has **1,300,000 rows and 15 columns**, including an index column that is subsequently removed.

Recorded cleaning steps remove:

- **33 records** with missing station identifiers/names.
- **654 records** with calculated rider age above 100.
- **1 record** with calculated speed above the project's 40 km/h threshold.

This leaves **1,299,312 records** for analysis. These are rule-based exclusions, not independently verified corrections of erroneous records.

## Analytical questions

The 10 core analyses cover:

1. Round-trip percentages by user type.
2. Most popular starting stations.
3. Hourly trip demand.
4. Trip duration by age group.
5. Seasonal trip behavior.
6. High cumulative bike usage.
7. Popular ending stations by user type.
8. High-demand station pairs.
9. Recorded gender-category differences in trip behavior.
10. Weekday versus weekend behavior.

The cumulative-usage rule flags **134 bikes** above a threshold of approximately **320,295 trip seconds** (mean plus three standard deviations). This identifies candidates for further review; it does not verify maintenance needs or measure a reduction in breakdowns.

## Machine-learning experiment

The notebook predicts the dataset's recorded gender categories 1 and 2 using five features: rider age, trip duration, endpoint distance, calculated speed, and start month. Records labeled unknown are excluded from this experiment.

| Split | Recorded rows |
| --- | ---: |
| Eligible ML data | 1,197,934 |
| Training | 958,057 |
| Test | 239,877 |

Feature assembly and standardization are fitted within each training pipeline, using a seeded 80/20 random split.

| Model | Test accuracy | Spark weighted F1 |
| --- | ---: | ---: |
| Logistic Regression | 73.87% | 62.81% |
| Decision Tree | 73.85% | 62.85% |
| Random Forest, 100 trees | 73.88% | 62.78% |

The overall eligible dataset is approximately **73.95% category 1**, so these accuracies are close to a majority-class reference. The exact majority baseline on the test split is not reported. These results do not establish a useful predictive improvement; per-class precision/recall and a test-set baseline are needed for a fuller assessment. The reported F1 comes from Spark's `MulticlassClassificationEvaluator(metricName="f1")` and is weighted F1, not macro-F1.

## Interactive dashboard

The Bokeh dashboard contains four charts:

- Hourly trip demand.
- Top 10 ending stations.
- Average calculated speed by recorded gender category.
- High-use bikes.

A shared selector switches between all users, subscribers, and customers using JavaScript callbacks. Run the notebook in a compatible environment to interact with the dashboard; a static notebook preview may not execute its JavaScript.

## Running the project

1. Open the notebook in a Python notebook environment with Java, sufficient memory, and internet access. Saved outputs show a Kaggle run using Spark 4.0.2.
2. Run the installation and download cells. The notebook installs `gdown`, `pyspark`, and `bokeh`; other imports include pandas and Python's standard library.
3. Confirm that the embedded Google Drive dataset link is accessible. The original CSV is not bundled with this README.
4. Run the remaining cells in order. Spark is configured to request 12 GB of driver memory; this is a configuration setting, not a measured minimum requirement.

Package versions are not pinned. A fresh environment may require compatibility adjustments; the saved dashboard output includes a Bokeh deprecation warning for `circle(size=...)`.

## Interpretation and limitations

- **Distance and speed are proxies:** Haversine distance measures separation between station coordinates, not the route traveled. Calculated speed is endpoint distance divided by elapsed time, not observed cycling speed. Same-station rides have zero endpoint distance even if a rider traveled a substantial route.
- **Sampling is not established:** Monthly record counts are close to round sampling quotas. Without the source file's sampling documentation, seasonal totals should not be treated as population-wide Citi Bike demand estimates.
- **Usage does not verify maintenance:** Long trip durations can strongly affect cumulative usage. The notebook retains some very long trips, so flagged bikes require investigation before operational action.
- **Descriptive associations are not causes:** Commute purpose, leisure intent, physical endurance, and safety needs are not directly measured. Some original notebook interpretations go beyond the available evidence.
- **Repeated observations:** Records represent trips, not necessarily independent riders. This limits simple statistical-significance interpretations and random-split generalization claims.
- **Daily-demand denominator:** The station-pair daily average uses days with observed trips for that pair, rather than every calendar day in the study period.
- **Performance scope:** The notebook uses Spark APIs, but does not demonstrate a multi-node deployment or measured speedup. Some operations collect data to pandas, and unpartitioned ranking windows generate Spark warnings. No runtime-improvement claim is made.
- **Reproducibility:** Dataset access depends on an external Drive link. The source/sampling provenance and exact dependency versions should be documented for a durable reproduction.

## Technologies

Python, PySpark, Spark SQL, Spark ML, pandas, Bokeh, and JavaScript callbacks.

## Project context

Developed as a Big Data / NoSQL academic project. Repository contributors should identify their individual responsibilities when presenting this work in a portfolio.
