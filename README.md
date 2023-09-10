# Bank Market Cap Scraper

This Python script is designed to scrape data from a specific URL and extract information about the largest banks' market capitalization.

## Installation

Before running the script, make sure to install the required libraries:

```bash
pip install beautifulsoup4
pip install html5lib

## Usage
1. Import the necessary libraries:
```python3
from bs4 import BeautifulSoup as BS4
import requests
import pandas as pd

2. Define the URL you want to scrape:
```python3
url = 'https://web.archive.org/web/20200318083015/https://en.wikipedia.org/wiki/List_of_largest_banks'

3. Send a request to the URL and parse the HTML content:
```python3
html_data = requests.get(url)
if html_data.status_code == 200:
    soup = BS4(html_data.text, 'html.parser')

## Create an empty DataFrame to store the scraped data:
```python3
data = pd.DataFrame(columns=["Name", "Market Cap (US$ Billion)"])

## Find the relevant table in the HTML and extract the data:
```python3
table = soup.find_all('tbody')[2].find_all('tr')

for row in table:
    col = row.find_all('td')
    if len(col) >= 2:
        # Extract the bank name (first column)
        bank_name = col[1].text.strip()

        # Extract the market cap (second column)
        market_cap = col[0].text.strip()

        # Create a temporary DataFrame to concatenate with the empty data DataFrame
        df_temp = pd.DataFrame({'Name': [bank_name], 'Market Cap (US$ Billion)': [market_cap]})

        data = pd.concat([data, df_temp], ignore_index=True)

## Save the scraped data as a JSON file:
```python3
data.to_json(path_or_buf='./bank_market_cap.json', orient='columns')

This README provides an overview of how to use the script to scrape and store bank market capitalization data. You can customize it further based on your specific needs.

Happy coding!

# Credits
This script was created by Mete Turkan.
