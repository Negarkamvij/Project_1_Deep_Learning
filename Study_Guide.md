# My taxi project, explained simply

## The whole story

Imagine a huge pile of cards. Each card describes one taxi ride: when it started, when it ended, how far it went, and how many passengers it carried. I am a detective who checks the cards, fixes or excludes problem records using clear rules, makes pictures, and explains the patterns.

My professor’s exact project instructions are:

> Perform an explanatory data analysis (EDA) on NYC’s Yellow Taxi Trip Records from 2020. Although there will be no need to build a model based on the data provided, you are asked to look for issues in the data and find correlation among the various variables to improve ride time predictions.

> Create the training dataset on data based on March of 2020, and evaluation dataset on data based on May of 2020.

Source: the assignment PDF, page 1. I do not train an AI model in this assignment.

## The words I need to know

| Word | Simple meaning | My project example |
| --- | --- | --- |
| Notebook | My digital science workbook | Code, explanations, charts, and outputs in one `.ipynb` file |
| Dataset | A pile of information cards | All March taxi records |
| Row | One card | One taxi ride |
| Column | One kind of information | Distance or passenger count |
| Numeric | A measurement or count | 2 passengers or 3.5 miles |
| Categorical | A label or group | Payment type or pickup area; a numeric label is still a category |
| Datetime | A date and clock time | Pickup on March 1 at 10:00 |
| Parquet | A compact box storing a table | The monthly downloaded files |
| EDA | Exploring the cards before building anything | Counts, charts, missing values, and correlations |
| Missing/null/NaN | A blank entry | Unknown passenger count |
| Outlier | A very unusual card that needs checking | A short ride claiming hundreds of thousands of miles |
| Cleaning | Applying clear rules to choose usable records | Correct pickup month and positive duration |
| Feature | A clue | Distance or pickup hour |
| Target | The answer I study | Ride duration in minutes |
| Mean | Add the values, then divide by how many | (2 + 3 + 10) / 3 = 5 |
| Median | The middle sorted value | The median of 2, 3, 10 is 3 |
| Histogram | A picture of how often ranges occur | How many rides last 5–10 minutes |
| Scatterplot | One dot for each sampled ride | Distance across, duration up |
| Correlation | How two things tend to change together | Longer rides in miles usually last more minutes |
| Spearman correlation | Compare the ordering of the values | A bigger distance usually ranks with a bigger duration |
| Training/reference | My main practice pile | March |
| Evaluation | A later pile for checking differences | May; I do not fit or score a model here |
| Baseline | My earlier comparison pile | January |
| Imputation | Filling a blank using a declared rule | Use March's median passenger count in both months |
| Leakage | Using a clue I would learn only after the ride | Actual drop-off time cannot predict the ride at pickup |
| Schema | A rule sheet for the data | Expected feature names, types, and presence |
| Anomaly | A rule violation | A required feature missing from a dataset |
| Distribution shift | The pile changes | Missing passenger counts become much more common in May |
| TFDV | My automatic data inspector | Statistics, charts, schema, and validation |
| Apache Beam | A tool that helps process data | A dependency used by TFDV |
| GraphViz | A tool that draws connected steps | My workflow diagram |

## What the code does

1. `pd.read_parquet(...)` opens the box of ride cards.
2. `.head()` shows a few example cards.
3. `.isna().sum()` counts blanks in each information field.
4. `.describe()` summarizes values such as the smallest, middle, and largest.
5. Subtracting pickup from drop-off gives elapsed time. `.dt.total_seconds() / 60` converts it into minutes.
6. `frame.loc[valid]` keeps cards that pass my stated rules. It does not mean every excluded ride is definitely an error.
7. `.groupby("pickup_hour")` makes little piles for each hour so I can compare their medians.
8. `.corr(method="spearman")` checks which selected clues tend to rank together with duration.
9. TFDV creates statistics, learns a schema from March, and checks May against it.
10. `.fillna(fill_values)` fills missing inputs using values learned from March only.

## My findings

**Distance:** farther rides usually take longer. My March Spearman correlation is about 0.859. That is a strong association, but it does not prove causation or a model's accuracy.

**Time of day:** hourly median durations vary. A single hour-duration correlation is small because the relationship can curve and repeat over a day. Route mix can also change by hour.

**March and May:** May has fewer retained records. Its median trip is farther in miles but shorter in minutes. Passenger count is missing in about 1.26% of retained March records and 17.08% of retained May records.

**January extra credit:** after identical cleaning, January has 6,320,684 rides and March 2,969,781. March has about 53% fewer retained rides. January's median duration is 10.50 minutes and March's is 10.42. Median distance is 1.61 versus 1.69 miles. March's daily record count falls sharply around mid-month. These observations do not prove that COVID caused every change.

**Validation:** zero schema anomalies means May followed the configured rule sheet. It does not mean the months have identical distributions, that every ride is correct, or that a future model will work well.

## My 30-second presentation

“I analyzed NYC Yellow Taxi records to understand trip duration and data quality. I used March as training/reference data and May as evaluation data. I investigated missing values and unusual records, applied consistent cleaning rules, and explored the results with pandas and TFDV. Distance showed the strongest association with duration among my selected inputs. For extra credit, I compared January with March and found about 53% fewer retained rides in March, with a sharp decline around mid-month. My recommendation is to monitor data quality and changing travel patterns before building a future prediction system.”

## Practice questions

- Why did I keep the months separate? To examine a later evaluation month without mixing it into the reference data.
- Why use a median? A few long rides can pull the mean upward.
- Why fill May using March values? To avoid using evaluation information to fit preprocessing.
- Why does no schema anomaly not mean no change? The cards can follow the same rules while their values and missingness change.
- Why not claim model feature importance? I did not train a model. My ranking describes observed associations.
- Can I predict at pickup using final distance? I would need an estimated route distance instead.
- Is my 180-minute/100-mile limit a law? No. It defines the scope of my analysis and may exclude real unusual trips.
