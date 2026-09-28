# aml-alert-prioritization

Tests whether layering simple AML red flags cuts false alarms while still catching laundering. Built with Python (pandas) and Excel.

## Data
IBM "Transactions for Anti-Money Laundering (AML)" dataset on Kaggle (HI-Small_Trans.csv), a synthetic dataset with laundering labels. I analyzed the first 500,000 rows in time order. Download it from https://www.kaggle.com/datasets/ealtman2019/ibm-transactions-for-anti-money-laundering-aml/download/ZVBaXx89xreumbTShVsT%2Fversions%2FgILrniHDdOILKN595Jag%2Ffiles%2FHI-Small_Trans.csv?datasetVersionNumber=8.

## Method
1. Data quality audit (missing values, duplicates, self-transfers, currencies)
2. Removed self-transfers (297,563 rows), leaving 202,437 transactions
3. Built three red flags: ACH payment, sender paid 10+ different accounts (fan-out), amount of 10,000 or more
4. Evaluated each flag and combinations using precision and recall against the dataset's laundering labels

## Key results
| Scenario | Alerts | Precision | Recall |
|---|---|---|---|
| ACH only | 27,436 | 0.6% | 85.4% |
| ACH + fan-out (5+ recipients) | 264 | 11.0% | 15.1% |

Layering flags cut alerts by about 99% and made alerts far more precise, at the cost of catching less of the laundering.

## Limitations
- Flags were chosen after seeing the labels, so real-world results would likely be lower
- ACH being the riskiest payment type is specific to this synthetic dataset
- Amounts are in 15 currencies and were not converted

## How to run
1. Download the CSV and update the file path in the code
2. `pip install pandas openpyxl`
3. Run the notebook or script; it writes `AML_Analysis.xlsx`
