# EIA Bulk Data Collector

**EIA Bulk Data Collector** is a Python library designed to interact with the U.S. Energy Information Administration (EIA) API to collect bulk energy-related data for analysis and research purposes. This library simplifies the process of fetching, organizing, and processing large datasets from the EIA API.

## Features

- Collect bulk data from the EIA API
- Automatically handle API requests and rate limits
- Parse and store data in JSON(limited), DataFrames, Databases.
- Support for all publicly available EIA data. More information about the EIA API found here: [https://www.eia.gov](https://www.eia.gov/opendata/).
- Easy-to-use interface for querying and downloading data
- Extensive error catching for ease of use.

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
```python
from eia import EIABulk as ebulk
```
### Getting route information

Example with 
```python
  eia = ebulk(write2db=False, route='electricity/rto/fuel-type-data', from_api=True, verbose=2)
  print(eia)
```
<details>
<summary>Click to view output.</summary>

```json
Warning: No frequency variable found. Will default to hourly.
Warning: No data variable found. Will select all available data variables.

This function will provide information about the available facets if you provide a facet type.
 If you only provide a valid route then it will give you information about the next available path
or if its an endpoint it will provide information about the available facet types, available data types, frequency types, and sort options.

By default will print result from API. You can change this by setting the form_api variable to False.
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/?&api_key=api_key_here_1234567890

{
  "response": {
    "id": "fuel-type-data",
    "name": "Hourly Generation by Energy Source",
    "description": "Hourly net generation by balancing authority and energy source.  
    Source: Form EIA-930
    Product: Hourly Electric Grid Monitor",
    "frequency": [
      {
        "id": "hourly",
        "alias": "hourly (UTC)",
        "description": "One data point for each hour in UTC time.",
        "query": "H",
        "format": "YYYY-MM-DD\"T\"HH24"
      },
      {
        "id": "local-hourly",
        "alias": "hourly (Local Time Zone)",
        "description": "One data point for each hour in local time.",
        "query": "LH",
        "format": "YYYY-MM-DD\"T\"HH24TZH"
      }
    ],
    "facets": [
      {
        "id": "respondent",
        "description": "Balancing Authority / Region"
      },
      {
        "id": "fueltype",
        "description": "Energy Source"
      }
    ],
    "data": {
      "value": {
        "aggregation-method": "SUM",
        "alias": "Net Generation",
        "units": "megawatthours"
      }
    },
    "startPeriod": "2019-01-01T00",
    "endPeriod": "2024-12-16T06",
    "defaultDateFormat": "YYYY-MM-DD\"T\"HH24",
    "defaultFrequency": "hourly"
  },
  "request": {
    "command": "/v2/electricity/rto/fuel-type-data/",
    "params": {
      "api_key": "api_key_here_1234567890"
    }
  },
  "apiVersion": "2.1.8",
  "ExcelAddInVersion": "2.1.0"
}
```
</details>

```python
  eia = ebulk(write2db=False, route='electricity/rto', facet_types=[('respondent', 'US48')], frequency='hourly', data_types=['value'], override=True, verbose=2, from_api=False)
  print(eia)
 ``` 
<details>
<summary>Click to view output.</summary>

```json
Warning: You have provided a partial route. You will be able to print route information with the route_endpoints function or by printing the class instance.

WARNING: The route you provided or the other search settings you have provided do not appear to be supported by the data collection API.
 If you would like to use the data API functions regardless it is likely to result in an invalid API call.
If you believe the route tree is outdated you can run the update_tree() function.

The following are the potential routes/features one can pick from given endpoint: electricity/rto
{
  "region-data": {
    "facets": {
      "respondent": [
        "GWA",
        "TEX",
        "BANC",
        "GVL",
        "FPL",
        "..."
      ],
      "type": [
        "CO2.IMPR",
        "CO2.EM",
        "TI",
        "CO2.EXPR",
        "NG",
        "..."
      ]
    },
    "frequency": [
      "hourly",
      "local-hourly"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "respondent",
      "type",
      "value"
    ]
  },
  "fuel-type-data": {
    "facets": {
      "respondent": [
        "CAR",
        "CAL",
        "NYIS",
        "NWMT",
        "PSEI",
        "..."
      ],
      "fueltype": [
        "UES",
        "PS",
        "NG",
        "OTH",
        "WND",
        "..."
      ]
    },
    "frequency": [
      "hourly",
      "local-hourly"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "respondent",
      "fueltype",
      "value"
    ]
  },
  "region-sub-ba-data": {
    "facets": {
      "subba": [
        "SPRM",
        "VEA",
        "PS",
        "4001",
        "4007",
        "..."
      ],
      "parent": [
        "PJM",
        "NYIS",
        "MISO",
        "SWPP",
        "CISO",
        "..."
      ]
    },
    "frequency": [
      "hourly",
      "local-hourly"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "subba",
      "parent",
      "value"
    ]
  },
  "interchange-data": {
    "facets": {
      "fromba": [
        "CAL",
        "GVL",
        "FPL",
        "TEX",
        "PJM",
        "..."
      ],
      "toba": [
        "IESO",
        "FPL",
        "GVL",
        "TEX",
        "PJM",
        "..."
      ]
    },
    "frequency": [
      "hourly",
      "local-hourly"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "fromba",
      "toba",
      "value"
    ]
  },
  "daily-region-data": {
    "facets": {
      "respondent": [
        "PJM",
        "FPL",
        "NY",
        "TEX",
        "EPE",
        "..."
      ],
      "type": [
        "NG",
        "CO2.IMPR",
        "CO2.CON",
        "CO2.EM",
        "CO2.GER",
        "..."
      ],
      "timezone": [
        "Eastern",
        "Mountain",
        "Central",
        "Pacific",
        "Arizona"
      ]
    },
    "frequency": [
      "daily"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "respondent",
      "type",
      "timezone",
      "value"
    ]
  },
  "daily-region-sub-ba-data": {
    "facets": {
      "subba": [
        "4001",
        "4003",
        "4005",
        "4006",
        "4008",
        "..."
      ],
      "parent": [
        "PJM",
        "MISO",
        "SWPP",
        "ISNE",
        "ERCO",
        "..."
      ],
      "timezone": [
        "Pacific",
        "Central",
        "Mountain",
        "Eastern",
        "Arizona"
      ]
    },
    "frequency": [
      "daily"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "subba",
      "parent",
      "timezone",
      "value"
    ]
  },
  "daily-fuel-type-data": {
    "facets": {
      "respondent": [
        "BANC",
        "FPL",
        "CAL",
        "PJM",
        "WWA",
        "..."
      ],
      "fueltype": [
        "UES",
        "PS",
        "NG",
        "NUC",
        "OTH",
        "..."
      ],
      "timezone": [
        "Mountain",
        "Central",
        "Arizona",
        "Eastern",
        "Pacific"
      ]
    },
    "frequency": [
      "daily"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "respondent",
      "fueltype",
      "timezone",
      "value"
    ]
  },
  "daily-interchange-data": {
    "facets": {
      "fromba": [
        "FPL",
        "GVL",
        "GWA",
        "TEX",
        "PJM",
        "..."
      ],
      "toba": [
        "CEN",
        "BANC",
        "FPL",
        "GVL",
        "GWA",
        "..."
      ],
      "timezone": [
        "Central",
        "Pacific",
        "Arizona",
        "Mountain",
        "Eastern"
      ]
    },
    "frequency": [
      "daily"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "fromba",
      "toba",
      "timezone",
      "value"
    ]
  },
  "emissions-data-by-fuel-type": {
    "facets": {
      "respondent": [
        "SPA",
        "IPCO",
        "NW",
        "MISO",
        "SOCO",
        "..."
      ],
      "datatype": [
        "CO2.EM"
      ],
      "fueltype": [
        "OTH",
        "OIL",
        "NG",
        "COL"
      ]
    },
    "frequency": [
      "hourly",
      "local-hourly"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "respondent",
      "datatype",
      "fueltype",
      "value"
    ]
  },
  "daily-emissions-by-fuel-type-data": {
    "facets": {
      "respondent": [
        "GVL",
        "FMPP",
        "CENT",
        "SWPP",
        "IPCO",
        "..."
      ],
      "datatype": [
        "CO2.EM"
      ],
      "fueltype": [
        "COL",
        "OTH",
        "OIL",
        "NG"
      ],
      "timezone": [
        "Central",
        "Eastern",
        "Pacific",
        "Mountain",
        "Arizona"
      ]
    },
    "frequency": [
      "daily"
    ],
    "data": [
      "value"
    ],
    "sort_by": [
      "period",
      "respondent",
      "datatype",
      "fueltype",
      "timezone",
      "..."
    ]
  }
}
```

</details>

```python
response = ebulk(write2db=False, route='electricity/rto/fuel-type-data', facet_types=[('respondent', 'US48')], frequency='hourly', data_types=['value'], override=True, verbose=2, from_api=False).route_endpoints()
print(response)
```
<details>
<summary>Click to view output.</summary>

```json
The following are the potential routes/features one can pick from given endpoint: electricity/rto/fuel-type-data
{
  "frequency": [
    "hourly",
    "local-hourly"
  ],
  "data": [
    "value"
  ],
  "facets": {
    "respondent": [
      "CAR",
      "CAL",
      "NYIS",
      "NWMT",
      "PSEI",
      "NY",
      "FMPP",
      "MIDA",
      "GVL",
      "TEPC",
      "LDWP",
      "SCEG",
      "CISO",
      "NEVP",
      "DUK",
      "SC",
      "WAUW",
      "GCPD",
      "IID",
      "PSCO",
      "SW",
      "JEA",
      "PNM",
      "PACE",
      "TVA",
      "FPL",
      "US48",
      "TIDC",
      "MISO",
      "CENT",
      "ERCO",
      "NE",
      "TEC",
      "PGE",
      "HST",
      "BANC",
      "TPWR",
      "PJM",
      "WALC",
      "PACW",
      "CHPD",
      "YAD",
      "EPE",
      "SEC",
      "DEAA",
      "FPC",
      "AVRN",
      "ISNE",
      "NW",
      "SWPP",
      "WWA",
      "FLA",
      "WACM",
      "CPLE",
      "MIDW",
      "GRID",
      "AECI",
      "SEPA",
      "TEN",
      "AVA",
      "TEX",
      "AZPS",
      "IPCO",
      "SPA",
      "AEC",
      "SE",
      "CPLW",
      "TAL",
      "GRIF",
      "SCL",
      "HGMA",
      "EEI",
      "GLHB",
      "SRP",
      "DOPD",
      "BPAT",
      "LGEE",
      "GWA",
      "NSB",
      "SOCO"
    ],
    "fueltype": [
      "UES",
      "PS",
      "NG",
      "OTH",
      "WND",
      "NUC",
      "BAT",
      "SUN",
      "OIL",
      "UES",
      "SNB",
      "WAT",
      "BAT",
      "COL",
      "UNK",
      "SNB",
      "PS"
    ]
  },
  "sort_by": [
    "period",
    "respondent",
    "fueltype",
    "value"
  ]
}
```
</details>

### Getting dataframe
```python
eia = ebulk(write2db=False, route='electricity/rto/fuel-type-data', frequency='hourly', facet_types = ('respondent', 'US48'), data_types='value', override=True, verbose=2, multiplier=1/2)
for df in eia.collect_months_data(2019, [1,2]):
    print(df.head(25).to_string(index=False))
```
<details>
<summary>Click to view output.</summary>

```json
Collecting from 2019-01-01 to 2019-01-16
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-01-01T00&end=2019-01-16T00&offset=0&length=5000&api_key=api_key_here_1234567890
Collecting from 2019-01-16 to 2019-02-01
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-01-16T00&end=2019-02-01T00&offset=0&length=5000&api_key=api_key_here_1234567890
Collecting from 2019-02-01 to 2019-02-15
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-02-01T00&end=2019-02-15T00&offset=0&length=5000&api_key=api_key_here_1234567890
Collecting from 2019-02-15 to 2019-03-01
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-02-15T00&end=2019-03-01T00&offset=0&length=5000&api_key=api_key_here_1234567890
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>period</th>
      <th>respondent</th>
      <th>respondent-name</th>
      <th>fueltype</th>
      <th>type-name</th>
      <th>value</th>
      <th>value-units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>151102</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NG</td>
      <td>Natural Gas</td>
      <td>171401</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NUC</td>
      <td>Nuclear</td>
      <td>103615</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OIL</td>
      <td>Petroleum</td>
      <td>363</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OTH</td>
      <td>Other</td>
      <td>9439</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>1424</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WAT</td>
      <td>Hydro</td>
      <td>41590</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WND</td>
      <td>Wind</td>
      <td>24323</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>146397</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NG</td>
      <td>Natural Gas</td>
      <td>165141</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NUC</td>
      <td>Nuclear</td>
      <td>103696</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OIL</td>
      <td>Petroleum</td>
      <td>370</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OTH</td>
      <td>Other</td>
      <td>9250</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>3577</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WAT</td>
      <td>Hydro</td>
      <td>37472</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WND</td>
      <td>Wind</td>
      <td>23474</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>141832</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NG</td>
      <td>Natural Gas</td>
      <td>158337</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NUC</td>
      <td>Nuclear</td>
      <td>103679</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OIL</td>
      <td>Petroleum</td>
      <td>362</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OTH</td>
      <td>Other</td>
      <td>9119</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>5945</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WAT</td>
      <td>Hydro</td>
      <td>35440</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WND</td>
      <td>Wind</td>
      <td>23511</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T21</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>140873</td>
      <td>megawatthours</td>
    </tr>
  </tbody>
</table>
</details>

```python
eia = ebulk(write2db=False, route='electricity/operating-generator-capacity', facet_types=[('balancing_authority_code', 'CPLE'),('balancing_authority_code', 'AEC')], data_types=['nameplate-capacity-mw'], override=True, verbose=0, multiplier=4)

for df in eia.collect_years_data(start_year=2019, end_year=2020):
    print(df.head(25).to_string(index=False))
```
<details>
<summary>Click to view output.</summary>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>period</th>
      <th>stateid</th>
      <th>stateName</th>
      <th>sector</th>
      <th>sectorName</th>
      <th>entityid</th>
      <th>entityName</th>
      <th>plantid</th>
      <th>plantName</th>
      <th>generatorid</th>
      <th>technology</th>
      <th>energy_source_code</th>
      <th>energy-source-desc</th>
      <th>prime_mover_code</th>
      <th>balancing_authority_code</th>
      <th>balancing-authority-name</th>
      <th>status</th>
      <th>statusDescription</th>
      <th>nameplate-capacity-mw</th>
      <th>unit</th>
      <th>nameplate-capacity-mw-units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>commercial-non-chp</td>
      <td>Commercial Non-CHP</td>
      <td>12374</td>
      <td>Metropolitan Sewerage District</td>
      <td>10181</td>
      <td>Metropolitan Sewerage District</td>
      <td>GEN1</td>
      <td>Conventional Hydroelectric</td>
      <td>WAT</td>
      <td>Water</td>
      <td>HY</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>SB</td>
      <td>Standby/Backup: available for service but not normally used</td>
      <td>.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>commercial-non-chp</td>
      <td>Commercial Non-CHP</td>
      <td>12374</td>
      <td>Metropolitan Sewerage District</td>
      <td>10181</td>
      <td>Metropolitan Sewerage District</td>
      <td>GEN2</td>
      <td>Conventional Hydroelectric</td>
      <td>WAT</td>
      <td>Water</td>
      <td>HY</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>commercial-non-chp</td>
      <td>Commercial Non-CHP</td>
      <td>12374</td>
      <td>Metropolitan Sewerage District</td>
      <td>10181</td>
      <td>Metropolitan Sewerage District</td>
      <td>GEN3</td>
      <td>Conventional Hydroelectric</td>
      <td>WAT</td>
      <td>Water</td>
      <td>HY</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN1</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN2</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN3</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN4</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN5</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN6</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61494</td>
      <td>Radian Generation</td>
      <td>62168</td>
      <td>Church Road Solar LLC</td>
      <td>CHU01</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61494</td>
      <td>Radian Generation</td>
      <td>62167</td>
      <td>County Home Solar LLC</td>
      <td>COU01</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>58325</td>
      <td>New Bern Farm LLC</td>
      <td>58339</td>
      <td>New Bern Farm</td>
      <td>1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61528</td>
      <td>Bayboro Solar Farm</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61351</td>
      <td>Bear Creek Solar</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>2</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61561</td>
      <td>Bladen Solar</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>50</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61283</td>
      <td>Bladenboro Solar, LLC</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>4.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61352</td>
      <td>Bondi Solar</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>58807</td>
      <td>Boseman Solar Center LLC</td>
      <td>BSC1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61693</td>
      <td>Buckleberry Solar</td>
      <td>GEN</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>52.1</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>59673</td>
      <td>Choco Solar LLC</td>
      <td>59899</td>
      <td>Choco Solar, LLC</td>
      <td>CHOCO</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>58819</td>
      <td>Graham Solar Center LLC</td>
      <td>58957</td>
      <td>Graham Solar Center LLC</td>
      <td>GRAH</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>4.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GEN8</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GEN9</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GN10</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GN11</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
  </tbody>
</table>

</details>

### Writing to database
```python
eia = ebulk(write2db=True, route='electricity/rto/fuel-type-data', frequency='hourly', facet_types = ('respondent', 'US48'), data_types='value', override=True, verbose=2, multiplier=1/2)
for df in eia.collect_months_data(2019, [1,2]):
    print(df.head(25).to_string(index=False))
```
<details>
<summary>Click to view output.</summary>

```json
Collecting from 2019-01-01 to 2019-01-16
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-01-01T00&end=2019-01-16T00&offset=0&length=5000&api_key=api_key_here_1234567890
Collecting from 2019-01-16 to 2019-02-01
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-01-16T00&end=2019-02-01T00&offset=0&length=5000&api_key=api_key_here_1234567890
Collecting from 2019-02-01 to 2019-02-15
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-02-01T00&end=2019-02-15T00&offset=0&length=5000&api_key=api_key_here_1234567890
Collecting from 2019-02-15 to 2019-03-01
API URL:https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/?frequency=hourly&data[0]=value&facets[respondent][]=US48&start=2019-02-15T00&end=2019-03-01T00&offset=0&length=5000&api_key=api_key_here_1234567890
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>period</th>
      <th>respondent</th>
      <th>respondent-name</th>
      <th>fueltype</th>
      <th>type-name</th>
      <th>value</th>
      <th>value-units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>151102</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NG</td>
      <td>Natural Gas</td>
      <td>171401</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NUC</td>
      <td>Nuclear</td>
      <td>103615</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OIL</td>
      <td>Petroleum</td>
      <td>363</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OTH</td>
      <td>Other</td>
      <td>9439</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>1424</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WAT</td>
      <td>Hydro</td>
      <td>41590</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-16T00</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WND</td>
      <td>Wind</td>
      <td>24323</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>146397</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NG</td>
      <td>Natural Gas</td>
      <td>165141</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NUC</td>
      <td>Nuclear</td>
      <td>103696</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OIL</td>
      <td>Petroleum</td>
      <td>370</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OTH</td>
      <td>Other</td>
      <td>9250</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>3577</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WAT</td>
      <td>Hydro</td>
      <td>37472</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T23</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WND</td>
      <td>Wind</td>
      <td>23474</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>141832</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NG</td>
      <td>Natural Gas</td>
      <td>158337</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>NUC</td>
      <td>Nuclear</td>
      <td>103679</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OIL</td>
      <td>Petroleum</td>
      <td>362</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>OTH</td>
      <td>Other</td>
      <td>9119</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>5945</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WAT</td>
      <td>Hydro</td>
      <td>35440</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T22</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>WND</td>
      <td>Wind</td>
      <td>23511</td>
      <td>megawatthours</td>
    </tr>
    <tr>
      <td>2019-01-15T21</td>
      <td>US48</td>
      <td>United States Lower 48</td>
      <td>COL</td>
      <td>Coal</td>
      <td>140873</td>
      <td>megawatthours</td>
    </tr>
  </tbody>
</table>
</details>

```python
eia = ebulk(route='electricity/operating-generator-capacity', facet_types=[('balancing_authority_code', 'CPLE'),('balancing_authority_code', 'AEC')], data_types=['nameplate-capacity-mw'], override=True, verbose=0, multiplier=4)

for df in eia.collect_years_data(start_year=2019, end_year=2020):
    print(df.head(25).to_string(index=False))
```
<details>
<summary>Click to view output.</summary>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>period</th>
      <th>stateid</th>
      <th>stateName</th>
      <th>sector</th>
      <th>sectorName</th>
      <th>entityid</th>
      <th>entityName</th>
      <th>plantid</th>
      <th>plantName</th>
      <th>generatorid</th>
      <th>technology</th>
      <th>energy_source_code</th>
      <th>energy-source-desc</th>
      <th>prime_mover_code</th>
      <th>balancing_authority_code</th>
      <th>balancing-authority-name</th>
      <th>status</th>
      <th>statusDescription</th>
      <th>nameplate-capacity-mw</th>
      <th>unit</th>
      <th>nameplate-capacity-mw-units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>commercial-non-chp</td>
      <td>Commercial Non-CHP</td>
      <td>12374</td>
      <td>Metropolitan Sewerage District</td>
      <td>10181</td>
      <td>Metropolitan Sewerage District</td>
      <td>GEN1</td>
      <td>Conventional Hydroelectric</td>
      <td>WAT</td>
      <td>Water</td>
      <td>HY</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>SB</td>
      <td>Standby/Backup: available for service but not normally used</td>
      <td>.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>commercial-non-chp</td>
      <td>Commercial Non-CHP</td>
      <td>12374</td>
      <td>Metropolitan Sewerage District</td>
      <td>10181</td>
      <td>Metropolitan Sewerage District</td>
      <td>GEN2</td>
      <td>Conventional Hydroelectric</td>
      <td>WAT</td>
      <td>Water</td>
      <td>HY</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>commercial-non-chp</td>
      <td>Commercial Non-CHP</td>
      <td>12374</td>
      <td>Metropolitan Sewerage District</td>
      <td>10181</td>
      <td>Metropolitan Sewerage District</td>
      <td>GEN3</td>
      <td>Conventional Hydroelectric</td>
      <td>WAT</td>
      <td>Water</td>
      <td>HY</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN1</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN2</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN3</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN4</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN5</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>56814</td>
      <td>Black Creek Renewable Energy LLC</td>
      <td>57492</td>
      <td>Sampson County Disposal</td>
      <td>GEN6</td>
      <td>Landfill Gas</td>
      <td>LFG</td>
      <td>Landfill Gas</td>
      <td>IC</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>1.6</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61494</td>
      <td>Radian Generation</td>
      <td>62168</td>
      <td>Church Road Solar LLC</td>
      <td>CHU01</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61494</td>
      <td>Radian Generation</td>
      <td>62167</td>
      <td>County Home Solar LLC</td>
      <td>COU01</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2019-05</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>58325</td>
      <td>New Bern Farm LLC</td>
      <td>58339</td>
      <td>New Bern Farm</td>
      <td>1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61528</td>
      <td>Bayboro Solar Farm</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61351</td>
      <td>Bear Creek Solar</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>2</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61561</td>
      <td>Bladen Solar</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>50</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61283</td>
      <td>Bladenboro Solar, LLC</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>4.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61352</td>
      <td>Bondi Solar</td>
      <td>GEN1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>58807</td>
      <td>Boseman Solar Center LLC</td>
      <td>BSC1</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>61060</td>
      <td>Cypress Creek Renewables</td>
      <td>61693</td>
      <td>Buckleberry Solar</td>
      <td>GEN</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>52.1</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>59673</td>
      <td>Choco Solar LLC</td>
      <td>59899</td>
      <td>Choco Solar, LLC</td>
      <td>CHOCO</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2019-04</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>ipp-non-chp</td>
      <td>IPP Non-CHP</td>
      <td>58819</td>
      <td>Graham Solar Center LLC</td>
      <td>58957</td>
      <td>Graham Solar Center LLC</td>
      <td>GRAH</td>
      <td>Solar Photovoltaic</td>
      <td>SUN</td>
      <td>Solar</td>
      <td>PV</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>4.8</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GEN8</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GEN9</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GN10</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2019-01</td>
      <td>NC</td>
      <td>North Carolina</td>
      <td>industrial-chp</td>
      <td>Industrial CHP</td>
      <td>23815</td>
      <td>Blue Ridge Paper Products Inc</td>
      <td>50244</td>
      <td>Canton North Carolina</td>
      <td>GN11</td>
      <td>Conventional Steam Coal</td>
      <td>BIT</td>
      <td>Bituminous Coal</td>
      <td>ST</td>
      <td>CPLE</td>
      <td>Duke Energy Progress East</td>
      <td>OP</td>
      <td>Operating</td>
      <td>7.5</td>
      <td>None</td>
      <td>MW</td>
    </tr>
  </tbody>
</table>
</details>

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
