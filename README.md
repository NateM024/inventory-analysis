# DAT 400 Capstone: Inventory Analysis

## Project Summary
The objective of this project was to acquire, explore, clean, and export data pertaining to the inventory metrics and stock performance of S&P 500 companies to answer these two questions: 

1. How do inventory metrics vary across sectors and among companies within those sectors?​
2. Is there a relationship between these inventory metrics and stock performance?​


## Capstone Notebook


### Data Acquisition
This project acquires data from 3 different sources:

1. **SP500.csv** - A csv file obtained from Kaggle contains information for each company in the S&P 500, including their ticker symbol, sector, sub-industry, and more. This file is kept in `SP500.csv/data`.

2. **SEC EDGAR API** - An api used to retrieve data from company filings. For this project, we can obtain company's quarterly dates, inventory, cost of goods sold, and net sales for each company that lists this data by using the CIK from the SP500.csv. The collected and cleaned data is kept in `financial_data_cleaned.csv/data`.

3. **yfinance** - A Python library that uses Yahoo Finance's API to fetch historical stock market data. This project uses the company ticker from SP500.csv and the dates gathered from the SEC EDGAR API to get specific company stock prices at exact dates. The collected and cleaned data is kept in `stock_data_cleaned.csv/data`.


### Data Exploration

For the SP500.csv:
- Examine the beginning and end of the dataset
- Look at the shape of the data (503, 7)
- Check for nulls

For the SEC EDGAR data:

- View the shape of the data (109, 5)
- Look at counts, frequencies, and uniqueness of values
- Look at the first 5 rows of data
- Check for nulls and duplicates

For the yfinance data: 
- Vuew  at the shape of the data (3221, 3)
- Look at counts, frequencies, and uniqueness of values
- Look at the first 5 rows of data
- Check for nulls and duplicates

### Data Cleaning 

For the financial data dataframe: 
- Remove duplicate values and irrelavent information 
- Create a separate date column
- Changed the date values to a 'YYYY-MM-DD' pandas datetime format
- Created an average inventory column
- Added a column with for company names 
- Changed a ticker symbol from BF.B to BF-B for it to work with yfinance
- Removed the company with the K ticker symbol (delisted in yfinance)  

For the stock data dataframe:
- Removed null values
- Changed the date values to a 'YYYY-MM-DD' pandas datetime format
- Created a stock return column
- Added a column with for company names 

### Data Exporting

1. Create an engine from the sqlalchemy library (note: start a postgres docker container first)
2. Export the data to three different tables in the Postgres database:

<div align="center">

| Table | Columns |
|:---:|:---:|
| Companies | Company Name, Sector |
| Stock_data | Company Name, Date, Price, Return |
| Financial_data |Company Name, Date, Inventory, <br></bt>Avg. Inventory, COGS, Sales|
</div>

3. Dispose the engine
4. Reconnect to the engine and verify the data exists in the database 

## Visualizations
After the data is exported into the Postgres database, it can easily be connected to a data visualization tool, such as Tableau or Power BI, to create insightful visualizations. For the specific questions for this project, `workbook.twb` includes the Tableau workbook structure of the  visualizations used to answer them.

## Key Project Takeways

1. How do inventory metrics vary across sectors and among companies within those sectors?

    Inventory metrics show clear systematic differences across sectors​. For example, companies in sectors like industrials and energy held billions of dollars of inventory on average while other sectors like financials held less than even one billion. This reflects the strong industry effects and reflects structural factors (like production cycles) across sectors.

    Within sectors, there exists substantial variation in inventory metrics at the company-level. Within sectors, companies with similar inventory products have noticably different inventory metrics, noteably inventory turnover and inventory-to-sales. Comparing these metrics among companies or to the sector average can help us see which companies are under/overperforming on these metrics. 
​
2. Is there a relationship between these inventory metrics and stock performance?​

    There does not appear to be a relationship between inventory metrics and stock performance. Plotting future stock returns with inventory turnover and inventory-to-sales did not result in meaningful R<sup>2</sup> or significant p-values, except for one exception. When filtering specifically for the materials sector and plotting future returns with inventory turnover, the linear regression line produced a p-value of 0.03,  meaning that relationship is statistically significant. However, even in this case, the R<sup>2</sup> was still small, meaning that inventory turnover can not explain the variance in future stock returns. 

## How to Run

### Step 1. Create a Python Virtual Environment
From the project root:

```bash
python3 -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows
pip install -r requirements.txt
```
### Step 2. Prepare the Data
Either: 

- a.) Run through all cells in `capstone.ipynb` up to the Exporting Data section, or 

- b) Directly load in all the data by creating and running a Python cell that includes: 
```python
clean_financial_data = pd.read_csv('financial_data_cleaned.csv/data')
stocks_data = pd.read_csv('stock_data_cleaned.csv/data')
```

### Step 3. Start the Docker Stack
To start a container for the PostgreSQL (and also one for Metabase), in the terminal run: 
``` bash
cd docker
docker compose up -d
```
### Step 4. Export the Data
Go to the Exporting Data section of `capstone.ipynb` and run all of the cells



## Project File Structure

```
inventory-analysis/
├── capstone.ipynb         ← Jupyter notebook (your main work) 
├── requirements.txt       ← Python dependencies
├── proposal.pdf           ← Intial project proposal
├── workbook.twb           ← Tableau workbook
├── data/
│   └── SP500.csv                     ← Source CSV (do not modify)
|   └── financial_data_uncleaned.csv  ← Data Acquired in capstone.ipynb
|   └── financial_data_cleaned.csv    ← Data Acquired in capstone.ipynb
|   └── stock_data_cleaned.csv        ← Data Acquired in capstone.ipynb 
└── docker/
    └── docker-compose.yml ← Defines Postgres + Metabase containers
    └── .gitignore         ← Ignore local data 
```
