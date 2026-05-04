# DAT 400 Capstone: Inventory Analysis

## Project Summary
The objective of this project was to acquire, explore, clean, and export data pertaining to the inventory metrics and stock performance of S&P 500 companies to answer questions about how inventory metrics compare across sectors and companies and how they relate to a company's stock performance.

## Capstone Notebook

### Data Acquisition
This project collects financial data from the SEC EDGAR API database and from the yfinance python library. It also uses a .csv file called 'SP500.csv'* obtained from Kaggle that includes all S&P 500 companies and detail about them. This .csv file is used to help collect financial data and dates from the SEC EDGAR API. These dates along with the company ticker symbol from the SP500.csv to collect historical stock prices for each company on certain dates from the yfinance library. 

*The SP500.csv file is kept in the /data folder along with two other .csv files. The data in these two additional files come directly from the notebook after the data was acquired so that they could be quickly loaded back into the notebook, requiring seconds, not minutes.

### Data Exploration
Each time data was obtained, it was explored for two reasons. One is for the person working on this project to understand the data: its dimensions, datatypes, etc. The other reason was to understand what changes/transformations needed to be performed.

### Data Cleaning 
From the company data in the SP500.csv file, one ticker needed to be updated for the yfinance library. Additionally, the company names in this file needed to be added to the 2 final pandas dataframes.

From the SEC EDGAR API, the data came in json format and needed to be parsed correctly, split into separate columns, and much of this data was also dropped due to its irrelavence to the project. Additionally the date datatype needed to be changed, and an average inventory column needed to be created.

From the yfinance library, there were a few null values which were dropped due to how few there were. The date column also had to change datatypes and a return column was created.
### Data Exporting
After acquiring, exploring, and cleaning the data, there were ultimately 2 dataframes that contained data that was needed to export into a locally hosted postgres database. The first dataframe was called 'clean_financial_data' which created the companies table and financial_data tables. The second dataframe was called 'stocks_data' which created the stock_data table. It is important to note that a docker contained needed to be created for this section of the project. There is a docker-compose.yml file included in the /docker folder of this repository.

## Visualizations
After the data is exported into a locally hosted postgres database, it can easily be connected to a data visualization tool, such as Tableau or Power BI, to create insightful visualizations.
