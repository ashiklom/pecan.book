# Met Data

The standard met data inputs should be of the form:

Converters from a raw to standard format go in `/modules/data.atmosphere/R`; converters from standard to model-specific go in `models/<mymodel>/R`.

Examples:
* NARR:
* CRUNCEP:
* ISIMIP: 

Names should be `met2CF.<sourcename>` and `met2model.<modelname>`.

The variable names should be `standard_name`


| CF standard-name                          | units | bety         | isimip       | cruncep | narr  |
|:------------------------------------------|:------|:-------------|:-------------|:--------|:------|
| **air_temperature**                       | K     | airT         | tasAdjust    | tair    | air   |
| air_temperature_max                       | K     |              | tasmaxAdjust | NA      | tmax  |
| air_temperature_min                       | K     |              | tasminAdjust | NA      | tmin  |
| **air_pressure**                          | Pa    | air_pressure |              |         |       |
| relative_humidity                         | % | relative_humidity | rhurs       | NA      | rhum  |
| **specific_humidity**                     | 1 | specific_humidity | NA          | qair    | shum  |
| **surface_downwelling_longwave_flux_in_air** |       |           | rldsAdjust   | swdown  | dswrf |
| **surface_downwelling_shortwave_flux_in_air**|       |           | rsdsAdjust   | lwdown  | dlwrf |
| **precipitation_flux**                    |       |              | prAdjust     | rain    | acpc  |
| wind_speed                                | m/s   | Wspd         |              |         |       |
| **eastward_wind**                         | m/s   | eastward_wind |             |         |       |
| **northward_wind**                        | m/s   |              |              |         |       |

* preferred variables indicated in bold
* variable names are from [MsTMIP](http://nacp.ornl.gov/MsTMIP_variables.shtml), but lowercase to be consistent with the MsTMIP drivers.
* standard_name is CF-convention standard names
* units can be converted by udunits, so these can vary (e.g. the time denominator may change with time frequency of inputs)

For the standardized files, we are using CF standard names as variable names.

For example, in the [MsTMIP-CRUNCEP](https://www.betydb.org/inputs/280) data, the variable `rain` should be `precipitation_rate`.
We want to standardize the units as well as part of the `met2CF.<product>` step. I believe we want to use the CF "canonical" units but retain the MsTMIP units any time CF is ambiguous about the units.

The key is to process each type of met data (site, reanalysis, forecast, climate scenario, etc) to the exact same standard. This way every operation after that (extract, gap fill, downscale, convert to a model, etc) will always have the exact same inputs. This will make everything else much simpler to code and allow us to avoid a lot of unnecessary data checking, tests, etc being repeated in every downstream function.