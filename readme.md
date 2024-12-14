#Key Instance Variables

api_key: Stores the EIA API authentication key
route: Constructed API route for data retrieval
facets: Filtered categories or facets
data: Specific data types to retrieve
frequency: Data frequency (hourly, daily, monthly, etc.)
write2db: Boolean to determine if data should be written to database
verbose: Controls the level of logging and output
multiplier: Adjusts time range for data retrieval based on frequency

#Key Methods and Their Purposes
###1. request_data(start, end, as_df=True)

Retrieves data from the EIA API for a specified date range
Parameters:

start: Start date for data retrieval
end: End date for data retrieval
as_df: Whether to return data as a pandas DataFrame

Purpose: Fetch data from EIA API with flexible output options

###2. collect_year_small_data(year, engine, months=None)

Collects data for a specific year, typically for hourly or daily frequencies
Parameters:

year: Year to collect data for
engine: Database connection engine
months(Optional): specific months to collect

Purpose: Retrieve and optionally store small-granularity time series data

###3. collect_year_large_data(the_range, engine)

Collects data for larger time frequencies (monthly, quarterly, yearly)
Parameters:
the_range: Range of years to collect
engine: Database connection engine

Purpose: Retrieve and store data for less frequent time series



###4. collect_years_data(years=None, start_year=None, end_year=None)

Flexible method to collect data across multiple years
Can specify exact years or a range
Handles different data frequencies
Purpose: Comprehensive data collection across multiple years

###5. update_tree()

Updates the internal route and API structure information
Builds a comprehensive dictionary of available API routes, facets, and data types
Purpose: Maintain an up-to-date understanding of the EIA API structure

###6. route_endpoints() or __str__()

Provides information about available routes, facets, and data types
Can fetch information from API or from local route tree
Purpose: Explore and understand available API endpoints

Other Relevant Variables: 

from_api(Default True): If True retrieves infromation from the API, if false retrieves data from one of the tree json files.


###7. url_constructor(start, end)

Builds the URL for API data retrieval
Incorporates route, frequency, data types, facets, and date range
Purpose: Construct precise API request URLs

###8. collect_month_of_small_data(year, month)

Purpose: Collects the data for exactly one month generally with smaller time frequencies.

###9. collect_months_of_large(dates)

Parameters:

dates: list or touple that contains two date strings 

Purpose: Collects larger time frequency data within the given date range.
