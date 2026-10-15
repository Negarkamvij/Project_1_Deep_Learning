# NYC Yellow Taxi EDA: CS672 Project 1

**Negar Kamvij, Fall 2026**

[Notebook](Taxi_Project_1.ipynb) | [Open in Colab](https://colab.research.google.com/github/Negarkamvij/Project_1_Deep_Learning/blob/main/Taxi_Project_1.ipynb)

My project investigates trip duration, data quality, and changes between March training data and May evaluation data. It also completes the January-versus-March extra credit. No prediction model is trained.

## Findings

- March distance-duration Spearman correlation is approximately 0.859.
- Retained rides: January 6,320,684; March 2,969,781; May 337,572.
- March has about 53% fewer retained records than January, with a sharp decline around mid-March in the daily chart.
- January and March median duration: 10.50 and 10.42 minutes. Median recorded distance: 1.61 and 1.69 miles.
- The corrected March/May TFDV run reports zero schema anomalies and validates exactly five intended fields. Distribution differences can still exist without schema anomalies.

## Reproduce

Download January, March, and May 2020 Yellow Taxi Parquet files from [NYC TLC](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) into `data/` beside the notebook. January downloads automatically if missing. Data files are excluded from Git.

Use a compatible environment for pandas, NumPy, PyArrow, Matplotlib, seaborn, TensorFlow Data Validation, Apache Beam, and GraphViz. The notebook contains an optional pip setup cell. GraphViz also needs the system `dot` executable; install it through the operating system package manager (for example, `brew install graphviz` on macOS or `apt-get install graphviz` in Colab).

The notebook records package versions and contains actual saved outputs for every analysis cell, including TFDV and the January extra credit. The optional package-install cell is separate from the analysis. An installed version report verifies the tools used.

## Interpretation

Cleaning limits define a selected positive-distance population. Correlation does not establish causation, and the feature ranking is descriptive rather than fitted-model importance. Recorded distance is available after the ride, so pickup-time predictions require estimated route distance. The January/March comparison does not isolate COVID effects from seasonality, route mix, or reporting changes.

See [My simple study guide](Study_Guide.md) for beginner explanations and presentation practice.
