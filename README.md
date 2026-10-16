# NYC Yellow Taxi EDA: CS672 Project 1

**Negar Kamvij, Fall 2026**

[Notebook](Taxi_Project_1.ipynb) | [Open in Colab](https://colab.research.google.com/github/Negarkamvij/Project_1_Deep_Learning/blob/main/Taxi_Project_1.ipynb)

This project studies trip duration, data quality, and differences between March reference data and May evaluation data. It includes the January-versus-March extra credit. No prediction model is trained.

## Findings

- All original rows are retained: January 6,405,008; March 3,007,687; May 348,415.
- Possible data problems are flagged. Long trips are not automatically treated as errors.
- March distance-duration Spearman correlation rounds to 0.847 both for raw recorded values and for a separate positive-duration sensitivity check.
- March has about 53% fewer file records than January. January and March median recorded duration is 10.48 and 10.40 minutes; median distance is 1.60 and 1.67 miles.
- Similar-distance comparisons explore pickup time and pickup zone. Location names come from TLC zone IDs and the official lookup, not GPS coordinates.
- TFDV reports zero inferred-schema anomalies for the five selected fields. This does not mean every record is correct or that distributions are equal.

## Reproduce

Run the notebook cells in order. Its setup downloads missing January, March, and May 2020 Parquet files from the official TLC source into `data/`. The zone lookup also downloads if missing. Existing local files are reused; data files are excluded from Git. Internet access is needed for downloads, and the analysis requires enough memory and disk space for millions of records.

Use compatible versions of pandas, NumPy, PyArrow, Matplotlib, seaborn, TensorFlow Data Validation, Apache Beam, and graphviz. Package-install instructions are included in the notebook. GraphViz also needs the system `dot` program, for example through `brew install graphviz` on macOS or `apt-get install graphviz` in Colab.

The final notebook was run from top to bottom in a new Python process using installed dependencies and existing official local data files. All 14 code cells passed, and their outputs are saved. It includes TFDV graphics, a small Beam audit of monthly row counts, and a GraphViz workflow. This verification does not guarantee compatibility with every fresh environment.

## Interpretation

Original records remain available. Separate numeric input copies fill missing values using March medians, and target-validity indicators identify unavailable durations for future model preparation. No target values are invented and no model is trained. Correlation and group comparisons describe patterns, not causes or fitted-model importance. Recorded trip distance is a post-trip value; pickup-time predictions would need an estimated distance. The January/March comparison cannot isolate COVID effects from season, routes, and reporting differences.
