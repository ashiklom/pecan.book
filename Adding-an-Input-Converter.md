# Met Data

The standard met data inputs should be of the form:

| variable names| standard_name | units | 
| :--| :-- | :--|
| tair | air_temperature|C|
| qair | specific_humidity|g/g|
| press | air_pressure |Pa|
|rain | precipitation_flux|mm/_time_|
|swdown| surface_downwelling_shortwave_flux_in_air|W/m2|
|lwdown|surface_downwelling_longwave_flux_in_air|W/m2|
|uwind |eastward_wind|m/s|
|vwind| northward_wind|m/s|

* variable names are from [MsTMIP](http://nacp.ornl.gov/MsTMIP_variables.shtml), but lowercase to be consistent with the MsTMIP drivers.
* standard_name is CF-convention standard names
* units can be converted by udunits, so these can vary (e.g. the time denominator may change with time frequency of inputs)