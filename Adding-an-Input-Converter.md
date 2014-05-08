# Met Data

The standard met data inputs should be of the form:

Converters from a raw to standard format go in `/modules/data.atmosphere/R`; converters from standard to model-specific go in `models/<mymodel>/R`.

Examples:
* NARR:
* CRUNCEP:
* ISIMIP: 

Names should be `met2CF.<sourcename>` and `met2model.<modelname>`.

The variable names should be `standard_name`


| CF standard-name                          | isimip       | cruncep | narr  |
|:-------------------------------------------|:--------------|:---------|:-------|
| air_temperature                           | tasAdjust    | tair    | air   |
| air_temperature_max                       | tasmaxAdjust | NA      | tmax  |
| air_temperature_min                       | tasminAdjust | NA      | tmin  |
| relative_humidity                         | rhurs        | NA      | rhum  |
| specific_humidity                         | NA           | qair    | shum  |
| surface_downwelling_longwave_flux_in_air  | rldsAdjust   | swdown  | dswrf |
| surface_downwelling_shortwave_flux_in_air | rsdsAdjust   | lwdown  | dlwrf |
| precipitation_flux                        | prAdjust     | rain    | acpc  |


* variable names are from [MsTMIP](http://nacp.ornl.gov/MsTMIP_variables.shtml), but lowercase to be consistent with the MsTMIP drivers.
* standard_name is CF-convention standard names
* units can be converted by udunits, so these can vary (e.g. the time denominator may change with time frequency of inputs)