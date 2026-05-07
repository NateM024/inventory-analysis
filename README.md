# DAT 400 Capstone: Inventory Analysis

## Project Summary
The objective of this project was to acquire, explore, clean, and export data pertaining to the inventory metrics and stock performance of S&P 500 companies to answer these two questions: 

1. How do inventory metrics vary across sectors and among companies within those sectors?​
2. Is there a relationship between these inventory metrics and stock performance?​


## Capstone Notebook


### Data Acquisition
This project acquires data from 3 different sources:

1. **SP500.csv** - A csv file obtained from Kaggle contains information for each company in the S&P 500, including their ticker symbol, sector, sub-industry, and more. This file is included in the `/data folder`.

2. **SEC EDGAR API** - An api used to retrieve data from company filings. For this project, we can obtain company's quarterly dates, inventory, cost of goods sold, and net sales for each company that lists this data by using the CIK from the SP500.csv. The collected and cleaned data is kept in `financial_data_v3.csv/data`.

3. **yfinance** - A Python library that uses Yahoo Finance's API to fetch historical stock market data. This project uses the company ticker from SP500.csv and the dates gathered from the SEC EDGAR API to get specific company stock prices at exact dates. The collected and cleaned data is kept in `stock_data.csv/data`.


### Data Exploration

For the `SP500.csv`:
- Examine the beginning and end of the dataset
- Look at the shape of the data (503, 7)
- Check for nulls

For the SEC EDGAR data:
- 
- Look at the first 5 rows of data
- 

### Data Cleaning 

From the data from the SP500.csv:
- Changed a ticker symbol from BF.B to BF-B for it to work with yfinance
- Removed the company with the K ticker symbol (delisted in yfinance)  

From the retrieved SEC EDGAR API data: 
- Remove duplicate values and irrelavent information 
- Create a separate date column
- Changed the date values to a 'YYYY-MM-DD' pandas datetime format
- Created an average inventory column
- Added a column with for company names 

From the data gathered via yfinance:
- Removed null values
- Changed the date values to a 'YYYY-MM-DD' pandas datetime format
- Created a stock return column
- Added a column with for company names 

### Data Exporting

1. Create an engine from the sqlalchemy library (note: start a postgres docker container first)
2. Export the data to three different tables in the postgres database:

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
After the data is exported into a locally hosted Postgres database, it can easily be connected to a data visualization tool, such as Tableau or Power BI, to create insightful visualizations. For the specific questions for this project, `workbook.twb` includes the Tableau workbook structure of the  visualizations used to answer them.

## Main Results

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
clean_financial_data = pd.read_csv('financial_data.csv/data')
stocks_data = pd.read_csv('stock_data/data')
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
│   └── SP500.csv          ← Source CSV (do not modify)
|   └── financial_data_v3  ← Data Acquired in capstone.ipynb 
|   └── stock_data         ← Data Acquired in capstone.ipynb 
└── docker/
    └── docker-compose.yml ← Defines Postgres + Metabase containers
    └── .gitignore         ← Ignore local data 
```
