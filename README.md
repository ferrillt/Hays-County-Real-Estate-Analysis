# Hays County Real Estate Analysis

## Project Overview

This project examines whether housing-market conditions, mortgage rates, and seasonal patterns can predict Hays County’s median days on market one month in advance. The analysis is intended to help real estate professionals evaluate whether the county-level market may move faster or slower during the following month.

The project follows an end-to-end data science process that includes data acquisition, preparation, exploratory analysis, baseline forecasting, model development, chronological validation, sensitivity testing, and interpretation of the final forecast.

## Business Problem

Historical housing reports describe conditions that have already occurred, but real estate professionals must also prepare clients for upcoming market conditions. A short-term forecast of median days on market could provide additional information for discussions about pricing, marketing periods, negotiations, and buyer or seller expectations.

The forecast is intended to supplement professional judgment. It should not replace a comparative market analysis or be used to predict how long an individual property will remain on the market.

## Research Questions

The primary research question is:

> To what extent can housing inventory, listing prices, mortgage rates, and seasonal trends predict Hays County’s median days on market one month in advance?

Supporting questions include:

1. Which variables have the strongest relationship with the following month’s median days on market?
2. Do the regression models outperform previous-month and prior-year seasonal baselines?
3. How can the forecast be translated into useful guidance for real estate professionals?

## Data Sources

The analysis uses two public data sources.

### Realtor.com Monthly Housing Inventory

The county-level housing data are available from the [Realtor.com Residential Real Estate Data Library](https://www.realtor.com/research/data/).

The original national file contains monthly housing measures for counties throughout the United States. It is filtered to Hays County, Texas, using county FIPS code `48209`.

The analysis uses the following housing measures:

* Median days on market
* Median listing price
* Active listing count
* New listing count
* Pending listing count
* Price-reduced listing count
* Pending ratio
* Quality flag

### Freddie Mac Mortgage Rates

Historical mortgage-rate data are available from the [Freddie Mac Primary Mortgage Market Survey](https://www.freddiemac.com/pmms).

The original dataset contains weekly mortgage-rate observations. The weekly 30-year fixed mortgage rates are averaged by calendar month before being joined with the Realtor.com data.

## Dataset Coverage

The combined dataset contains 122 monthly observations from July 2016 through August 2026. Because the outcome for the month following August 2026 was not yet available, 121 observations were used for model development and evaluation.

The first 97 observations were used for training. The final 24 observations, representing target months from September 2024 through August 2026, were reserved for testing.

## Analytical Methods

The analysis compares three models:

* Linear regression
* Ridge regression
* Constrained random forest

The models are evaluated against two basic forecasting methods:

* Previous-month baseline
* Prior-year seasonal baseline

The observations remain in chronological order. Time-series cross-validation is used when tuning the ridge and random-forest models so that later observations do not influence predictions for earlier months.

Model performance is evaluated using:

* Mean absolute error
* Root mean squared error
* R-squared

## Results

Ridge regression produced the strongest performance during the 24-month test period.

| Method                    |   MAE |  RMSE |    R² |
| ------------------------- | ----: | ----: | ----: |
| Ridge regression          |  4.64 |  6.02 | 0.820 |
| Linear regression         |  6.01 |  7.26 | 0.737 |
| Previous-month baseline   |  7.21 |  9.26 | 0.573 |
| Seasonal baseline         |  8.71 | 10.17 | 0.486 |
| Constrained random forest | 10.93 | 13.29 | 0.121 |

Ridge regression reduced mean absolute error by 35.7% compared with the previous-month baseline. The results also showed that additional model complexity did not improve prediction accuracy for this relatively small dataset.

Current median days on market and active listing count had the strongest positive relationships with the ridge forecast. New listings, seasonal measures, and pending ratio also contributed to the prediction. These relationships describe associations within the model and should not be interpreted as causal effects.

## September 2026 Forecast

After model evaluation, the ridge model was refitted using all 121 observations with known outcomes. August 2026 market conditions were then used to produce the September 2026 forecast.

| Measure                         | Result                 |
| ------------------------------- | ---------------------- |
| Forecast month                  | September 2026         |
| Predicted median days on market | 82.5 days              |
| August 2026 actual value        | 78 days                |
| Expected monthly change         | +4.5 days              |
| Test-period MAE                 | Approximately 4.6 days |

The forecast suggests a somewhat slower county-level market. Sellers may need to prepare for a longer marketing period, while buyers may have more time to evaluate properties and negotiate. The forecast is an estimate rather than a guaranteed result.

## Assumptions and Limitations

The analysis assumes that the source measurements remained reasonably consistent over time and that monthly mortgage-rate averages represent general financing conditions. It also assumes that current-month housing measures would be available before the following month’s forecast is generated.

Important limitations include:

* The sample contains only 122 monthly observations.
* The 24-month test set represents one historical period.
* Realtor.com data describe publicly listed properties and may not include off-market transactions.
* County-level measurements may hide differences among Kyle, Buda, San Marcos, and rural communities.
* The model does not include property condition, exact location, school district, property type, or price range.
* Sudden economic or market changes could weaken relationships learned from historical data.
* The model estimates county-level market speed and cannot predict the selling time of an individual home.

## Ethical Considerations

The forecast could be misleading if it is presented as a guarantee or used without explaining its limitations. Each forecast should therefore include information about the model’s historical error and should be considered alongside professional judgment.

Protected characteristics were not included in the model. The results should not be used to target, exclude, or treat people differently based on a protected characteristic. The appropriate purpose of the model is to provide general information about county-level housing-market conditions.

The source datasets are public, aggregated, and do not contain individual-level personal information.

## Conclusion  

The analysis found that housing-market measures can support a useful one-month-ahead forecast of median days on market for Hays County. Ridge regression produced the best test-period results, with an MAE of 4.64 days, and performed better than both baseline methods. The September 2026 forecast of 82.5 days suggests that homes may remain on the market somewhat longer than they did in August. Although the model cannot predict the selling time of an individual property, it can provide additional context for county-level market discussions.

## Recommendations

Real estate professionals can use the forecast when discussing pricing, marketing periods, negotiations, and client expectations. Each forecast should be presented as an estimate and include information about the model’s historical error. Unusual changes should be reviewed before results are shared with clients. The forecast should supplement a comparative market analysis and professional knowledge of the property and local community rather than replace them.

Future development should consider city-level forecasts for Kyle, Buda, and San Marcos, as well as separate models by property type or price range. Additional economic and housing measures could also be evaluated. Prediction intervals and testing across multiple historical periods would provide a better understanding of forecast uncertainty and stability.

## Implementation Plan

The process can be updated monthly when new Realtor.com and Freddie Mac observations become available. The new data should pass through the same preparation and validation steps documented in the notebook. After the data have been checked, the ridge model can be retrained and used to generate the following month’s forecast.

A dashboard or CRM report could display the latest actual value, next-month forecast, expected change, historical test error, and a brief explanation for clients. An analyst or broker should review the results before distribution, especially when the forecast shows an unusually large monthly change. Model performance should also be reviewed periodically to determine whether the predictors, training period, or modeling approach need to be updated.

## Obtaining and Preparing the Data

The original Freddie Mac and Realtor.com files are not included in this repository. The Realtor.com national county file exceeds GitHub’s normal file-size limit. Both original files must be downloaded from their publishers before the complete data-preparation process can be reproduced.

### Project Data Folder

Place the downloaded source files in the project’s `data` folder:

```text
Hays-County-Real-Estate-Analysis/
├── .gitignore
├── README.md
├── requirements.txt
├── analysis/
│   └── HaysCountyRealEstateAnalysis.ipynb
├── data/
│   ├── FreddieMac_1971-2026_historicalweeklydata.csv
│   ├── RDC_Inventory_Core_Metrics_County_History.csv
│   ├── freddie_mac_monthly.csv
│   ├── realtor_hays_county.csv
│   └── hays_county_modeling_data.csv
└── images/
│   ├── ActualBaselineForecasts_TestPeriod.png
│   ├── ActualPredictedMedianDaysOnMarket.png
│   ├── BaselineForecasts.png
│   ├── CorrelationAmongHousing-MarketVariables.png
│   ├── ForecastErrorByModel.png
│   ├── ForecastSummary.png
│   ├── ImprovementOverBaseline.png
│   ├── MedianDayOnMarket_CalendarMonth.png
│   ├── MedianDaysOnMarket.png
│   ├── MedianDaysOnMarket_Average30YrFixedMortgageRate.png
│   ├── ModelBaselineForecast.png
│   ├── ResidualResults.png
│   ├── RidgeCoefficients.png
│   ├── RidgeRegressionErrorsDuringTestPeriod.png
│   ├── SensitivityResults.png
│   ├── StandardizedRidgeRegressionCoefficients.png
│   └── TestPeriodPrediction.png
```

The first two data files must be downloaded separately and are excluded from GitHub. The remaining three data files are created by the notebook and included in the repository.  

### Download the Freddie Mac Dataset

1. Open the [Freddie Mac Primary Mortgage Market Survey](https://www.freddiemac.com/pmms).
2. Locate and download the historical weekly mortgage-rate data.
3. If the data are provided as an Excel workbook, save the applicable worksheet as a CSV file.
4. Name the file:

```text
FreddieMac_1971-2026_historicalweeklydata.csv
```

5. Place the file in the `data` folder.

The notebook reads the `Week` and `FRM` fields. The first two rows are skipped because they contain headings rather than data.

### Download the Realtor.com Dataset

1. Open the [Realtor.com Residential Real Estate Data Library](https://www.realtor.com/research/data/).
2. Locate the monthly housing inventory data.
3. Download the county-level historical inventory file.
4. Rename the file, if necessary, to:

```text
RDC_Inventory_Core_Metrics_County_History.csv
```

5. Place the file in the `data` folder.

The notebook imports only the required columns and filters the national dataset to Hays County using FIPS code `48209`.

## Creating the Prepared Datasets

The notebook creates three smaller datasets from the original source files.

### `freddie_mac_monthly.csv`

The Freddie Mac preparation section:

1. Loads the weekly mortgage-rate data.
2. Retains the weekly date and 30-year fixed-rate fields.
3. Converts the fields to the required data types.
4. Removes records without a valid date or mortgage rate.
5. Calculates the average mortgage rate for each calendar month.
6. Limits the results to July 2016 through August 2026.
7. Saves the prepared data as:

```text
data/freddie_mac_monthly.csv
```

### `realtor_hays_county.csv`

The Realtor.com preparation section:

1. Imports only the fields required for the analysis.
2. Standardizes the county FIPS codes.
3. Filters the national file to Hays County.
4. Converts the selected measures to numeric values.
5. Sorts the records chronologically.
6. Checks for duplicate months, missing months, missing values, negative values, and unexpected quality flags.
7. Saves the filtered data as:

```text
data/realtor_hays_county.csv
```

### `hays_county_modeling_data.csv`

The merge section:

1. Loads the two prepared datasets.
2. standardizes the `month_date_yyyymm` field.
3. Joins the datasets by month.
4. Uses one-to-one validation to confirm that each month has only one matching record in each dataset.
5. Checks for unmatched mortgage-rate records.
6. Saves the combined dataset as:

```text
data/hays_county_modeling_data.csv
```

## Installation

Clone or download this repository and install the required Python packages:

```bash
pip install -r requirements.txt
```

The analysis uses:

* pandas
* NumPy
* Matplotlib
* seaborn
* scikit-learn
* Jupyter Notebook

## Running the Analysis

After placing both original source files in the `data` folder, open:

```text
[Open the analysis notebook](analysis/HaysCountyRealEstateAnalysis.ipynb)
```

Run the notebook in order from the first cell through the final cell.

Running the cells sequentially is important because later sections depend on dataframes, variables, model settings, and results created earlier in the notebook. The notebook recreates the three prepared CSV files before completing the exploratory analysis and forecasting models.

## Files Excluded from GitHub

The following original source files should not be committed to the repository:

```text
data/FreddieMac_1971-2026_historicalweeklydata.csv
data/RDC_Inventory_Core_Metrics_County_History.csv
```

The repository’s `.gitignore` file contains:

```gitignore
# Original source datasets downloaded separately
data/FreddieMac_1971-2026_historicalweeklydata.csv
data/RDC_Inventory_Core_Metrics_County_History.csv
```

The following smaller prepared datasets are included:

```text
data/freddie_mac_monthly.csv
data/realtor_hays_county.csv
data/hays_county_modeling_data.csv
```

Including the prepared files allows visitors to review the analysis without downloading the large national Realtor.com dataset. Anyone who wants to reproduce the entire data-preparation process can download both original files and run the notebook from the beginning.

## Future Improvements

Possible extensions include:

* Adding city-level data for Kyle, Buda, and San Marcos
* Developing separate forecasts by property type or price range
* Adding home-sales, employment, building-permit, and population data
* Calculating prediction intervals
* Testing the model across additional rolling historical periods
* Presenting the forecast through a CRM dashboard

## Author  

**Teresa Ferrill**

[GitHub Portfolio](https://github.com/ferrillt)
[Hays County Real Estate Analysis Repository](https://github.com/ferrillt/Hays-County-Real-Estate-Analysis)
