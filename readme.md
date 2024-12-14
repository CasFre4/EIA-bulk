# EIA Bulk Data Collector

**EIA Bulk Data Collector** is a Python library designed to interact with the U.S. Energy Information Administration (EIA) API to collect bulk energy-related data for analysis and research purposes. This library simplifies the process of fetching, organizing, and processing large datasets from the EIA API.

## Features

- Collect bulk data from the EIA API
- Automatically handle API requests and rate limits
- Parse and store data in JSON(limited), DataFrames, Databases.
- Support for all publicly available EIA data. More information about the EIA API found here: [https://www.eia.gov](https://www.eia.gov/opendata/).
- Easy-to-use interface for querying and downloading data

## Installation

You can install the `eia-bulk` package using `pip`:

```bash
pip install eia-bulk
```
# Getting Started
## 1. Set Up API Key and Database Information (Note: The database set up is not required)

To interact with the EIA API, you will need an API key. You can obtain an API key from the EIA website.

Once you have your API key, store it in a .env file in your project directory:
```make
EIA_KEY=your_api_key_here
HOST=db_host_here 
DBNAME=db_name_here
USERNAME=db_username_here
PASSWORD=db_password_here
TABLENAME=db_tablename_here
```
## 2. Example Usage
  ### Getting route information
```python
  eia = ebulk(write2db=False, route='electricity/rto/fuel-type-data', from_api=True, verbose=2)
  print(eia)
  
  eia = ebulk(write2db=False, route='electricity/rto', facet_types=[('respondent', 'US48')], frequency='hourly', data_types=['value'], override=True, verbose=2, from_api=False)
  print(eia)
  
  
  print(ebulk(write2db=False, route='electricity/rto/fuel-type-data', facet_types=[('respondent', 'US48')], frequency='hourly', data_types=['value'], override=True, verbose=2, from_api=False).route_endpoints())
```
### Getting dataframe
``` python
from eia_bulk import EIABulk as ebulk

eia = ebulk(write2db=False, route='electricity/rto/fuel-type-data', frequency='hourly', facet_types = ('respondent', 'US48'), data_types='value', override=True, verbose=2, multiplier=1/5)
for item in eia.collect_months_data(2019, [1,2]):
    for i in item:
        print(i.head(25).to_string())
print('\n\n\n\n\n')
eia = ebulk(write2db=False, route='electricity/operating-generator-capacity', facet_types=[('balancing_authority_code', 'CPLE'),('balancing_authority_code', 'AEC')], data_types=['nameplate-capacity-mw'], override=True, verbose=0, multiplier=4)

for item in eia.collect_years_data(start_year=2019, end_year=2020):
    for i in item:
        print(i.head(25).to_string())
```
### Writing to database
```python
from eia_bulk import EIABulk as ebulk
eia = ebulk(write2db=True, route='electricity/rto/fuel-type-data', frequency='hourly', facet_types = ('respondent', 'US48'), data_types='value', override=True, verbose=2, multiplier=1/5)
for item in eia.collect_months_data(2019, [1,2]):
    for i in item:
        print(i.head(25).to_string())
```
# Key Instance Variables

api_key: Stores the EIA API authentication key
route: Constructed API route for data retrieval
facets: Filtered categories or facets
data: Specific data types to retrieve
frequency: Data frequency (hourly, daily, monthly, etc.)
write2db: Boolean to determine if data should be written to database
verbose: Controls the level of logging and output
multiplier: Adjusts time range for data retrieval based on frequency

# Key Methods and Their Purposes
### 1. route_endpoints() or __str__()

Provides information about available routes, facets, and data types
Can fetch information from API or from local route tree
Purpose: Explore and understand available API endpoints

Other Relevant Variables: 

from_api(Default True): If True retrieves infromation from the API, if false retrieves data from one of the tree json files.


### 2. request_data(start, end, as_df=True)

Retrieves data from the EIA API for a specified date range
Parameters:

start: Start date for data retrieval
end: End date for data retrieval
as_df: Whether to return data as a pandas DataFrame

Purpose: Fetch data from EIA API with flexible output options


### 3. collect_year_small_data(year, engine, months=None)

Collects data for a specific year, typically for hourly or daily frequencies
Parameters:

year: Year to collect data for
engine: Database connection engine
months(Optional): specific months to collect

Purpose: Retrieve and optionally store small-granularity time series data


### 4. collect_year_large_data(the_range, engine)

Collects data for larger time frequencies (monthly, quarterly, yearly)
Parameters:
the_range: Range of years to collect
engine: Database connection engine

Purpose: Retrieve and store data for less frequent time series


### 5. collect_years_data(years=None, start_year=None, end_year=None)

Flexible method to collect data across multiple years
Can specify exact years or a range
Handles different data frequencies
Purpose: Comprehensive data collection across multiple years


### 6. update_tree()

Updates the internal route and API structure information
Builds a comprehensive dictionary of available API routes, facets, and data types
Purpose: Maintain an up-to-date understanding of the EIA API structure


### 7. url_constructor(start, end)

Builds the URL for API data retrieval
Incorporates route, frequency, data types, facets, and date range
Purpose: Construct precise API request URLs

### 8. collect_month_of_small_data(year, month)

Purpose: Collects the data for exactly one month generally with smaller time frequencies.

### 9. collect_months_of_large(dates)

Parameters:

dates: list or touple that contains two date strings 

Purpose: Collects larger time frequency data within the given date range.
