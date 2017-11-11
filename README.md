### Change Log - November 2017 Release
- Postcode display boundaries are now also created - note: these postcodes are approximations from GNAF addresses and are very close to the real thing, but they are not authoritative
### Change Log - August 2017 Release
- Locality boundaries are now trimmed to the coastline using ABS Census 2016 SA4 boundaries
- To use the ABS Census 2011 SA4 table as per before - supply the following argument: `--sa4-boundary-table abs_2011_sa4`
### Change Log - February, May 2017 Releases
- No changes
### Change Log - November 2016 Release
- Logging is now written to locality-clean.log in your local repo directory as well as to the console 
- Added `--psma-version` to the parameters. Represents the PSMA version number in YYYYMM format and is used to add a suffix to the default schema names. Defaults to current year and latest release month. e.g. `201611`. Valid values are `<year>02` `<year>05` `<year>08` `<year>11`, and is based on the PSMA quarterly release months 
- All default schema names are now suffixed with `--psma-version` to avoid clashes with previous versions. e.g. `gnaf_201705`
- locality-clean.py now works with Python 2.7 and Python 3.5
- locality-clean.py has been successfully tested on Postgres 9.6 and PostGIS 2.3
    - Note: Limited performance testing on Postgres 9.6 has shown setting the maximum number of parallel processes `--max-processes` to 3 is the most efficient value on non-SSD machines
- Code has been refactored to simplify it a bit and move some common functions to a new psma.py file

# psma-admin-bdys
Some utils to make it easier to use the PSMA's Administrative Boundaries

## locality-clean
A Python script for creating a clean version of the Suburb-Locality boundaries for presentation or visualisation.

Trims the boundaries to the coastline; fixes state border overlaps and gaps; and thins the boundaries for faster display performance in desktop GIS tools and in browsers.

This process takes ~30-45 mins.

![aus.png](https://github.com/iag-geo/psma-admin-bdys/blob/master/sample-images/aus.png "clean vs original localities")

Coastline improvements

![border-comparison](https://github.com/iag-geo/psma-admin-bdys/blob/master/sample-images/border-comparison.png "clean vs original borders")

State border improvements

### Important

The cleaned localities are not well suited to data processing as they have been deliberately thinned to improve display performance.

A better dataset for processing is the admin_bdys.locality_bdy_analysis table that gets created in the [gnaf-loader](https://github.com/minus34/gnaf-loader) process

### I Just Want the Data!

You can run the script to get the result or just download the data from here:
- [Shapefile](https://github.com/iag-geo/psma-admin-bdys/releases/download/201705/locality-bdys-display-201705-shapefile.zip) (~40Mb) 
- [GeoJSON](https://github.com/iag-geo/psma-admin-bdys/releases/download/201705/locality-bdys-display-201705.geojson.zip) (~25Mb) 

#### Data License

Incorporates or developed using Administrative Boundaries ©PSMA Australia Limited licensed by the Commonwealth of Australia under [Creative Commons Attribution 4.0 International licence (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

### Script Pre-requisites

- You will need to run the [gnaf-loader](https://github.com/minus34/gnaf-loader) script to load the required Admin Bdy tables into Postgres
- Postgres 9.x (tested on 9.3, 9.4 & 9.5 on Windows and 9.5 & 9.6 on OSX)
- PostGIS 2.1+
- Python 2.7 or 3.5 with Psycopg2 2.6+

### Missing localities
Trimming the boundaries to the coastline removes a small number of bay or estuary based localities.  These have very few G-NAF addresses.

These localities are:

| locality_pid | name | postcode | state | addresses | streets |
| ------------- | ------------- | ------------- | ------------- | -------------: | -------------: |
| NSW524 | BOTANY BAY | 2019 | NSW | 1 | 12 |
| NSW2046 | JERVIS BAY |  | NSW | 0 | 5 |
| NSW2275 | LAKE MACQUARIE |  | NSW | 1 | 67 |
| NSW2627 | MIDDLE HARBOUR | 2087 | NSW | 4 | 23 |
| NSW3019 | NORTH HARBOUR |  | NSW | 0 | 11 |
| NSW3255 | PITTWATER | 2106 | NSW | 5 | 31 |
| NT26 | BEAGLE GULF |  | NT | 0 | 0 |
| NT75 | DARWIN HARBOUR |  | NT | 0 | 0 |
| QLD1351 | HERVEY BAY |  | QLD | 0 | 2 |
