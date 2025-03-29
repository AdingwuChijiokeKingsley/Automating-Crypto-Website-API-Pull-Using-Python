

# README for "Automating Crypto Website API Pull Using Python"

---

# Automating Crypto Website API Pull Using Python**  
A Python script that automates cryptocurrency price data extraction from CoinMarketCap API, stores it in a CSV file, and continuously updates the data at regular intervals.

# Overview  
This project fetches real-time cryptocurrency data using the CoinMarketCap API. It retrieves the latest listings, normalizes the data into a structured format using pandas, and saves it into a CSV file for further analysis. The script runs in a loop, appending new data at fixed time intervals.

# Features  
- Uses CoinMarketCap API to fetch real-time cryptocurrency prices.  
- Extracts and normalizes data into a pandas DataFrame.  
- Saves data to a CSV file and appends new data at regular intervals.  
- Handles API connection errors gracefully.  
- Automates data collection for further analysis.

# Technologies Used 
- Python  
- Requests (for API calls)  
- Pandas (for data manipulation)  
- JSON (for parsing API response)  
- OS (for file handling)  
- Time (for scheduling requests)  

# Installation & Setup  
# Prerequisites  
Ensure you have the following installed:  
- Python 3.x  
- pip (Python package manager)  

# Installation Steps  
1. Clone this repository:  
   ```bash
   git clone https://github.com/yourusername/Automating-Crypto-API.git
   cd Automating-Crypto-API
   ```
2. Install dependencies:  
   ```bash
   pip install requests pandas
   ```
3. Obtain a CoinMarketCap API key from [CoinMarketCap Developer Portal](https://pro.coinmarketcap.com/signup).  

4. Replace `'X-CMC_PRO_API_KEY'` with your actual API key in the script.

# Usage  
Run the script to start collecting data:  
```bash
python crypto_api_runner.py
```
The script will continuously fetch and store cryptocurrency data every 60 seconds.

# Code Explanation  
# API Request & Data Processing  
```python
from requests import Request, Session
from requests.exceptions import ConnectionError, Timeout, TooManyRedirects
import json
import pandas as pd

url = 'https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest'
parameters = {
    'start': '1',
    'limit': '15',
    'convert': 'USD'
}
headers = {
    'Accepts': 'application/json',
    'X-CMC_PRO_API_KEY': 'your_api_key_here',
}

session = Session()
session.headers.update(headers)

try:
    response = session.get(url, params=parameters)
    data = json.loads(response.text)
except (ConnectionError, Timeout, TooManyRedirects) as e:
    print(e)

df = pd.json_normalize(data['data'])
df['timestamp'] = pd.to_datetime('now')
```
- Establishes a session with the API.  
- Retrieves and processes cryptocurrency data.  
- Converts JSON response into a pandas DataFrame.

# Automating API Calls & Data Storage  
```python
import os
from time import sleep

def api_runner():
    global df
    try:
        response = session.get(url, params=parameters)
        data = json.loads(response.text)
        df = pd.json_normalize(data['data'])
        df['timestamp'] = pd.to_datetime('now')

        # Save to CSV
        file_path = r'C:\Users\HP\Desktop\API Script\API.csv'
        if not os.path.isfile(file_path):
            df.to_csv(file_path, header=True)
        else:
            df.to_csv(file_path, mode='a', header=False)
    except Exception as e:
        print(e)

for i in range(333):
    api_runner()
    print('API Runner completed')
    sleep(60)  # Fetch data every 1 minute
```
- Calls the API every 60 seconds to fetch new data.  
- Stores data in a CSV file, appending new records.  
- Runs for 333 iterations (~5.5 hours of continuous data collection).  
