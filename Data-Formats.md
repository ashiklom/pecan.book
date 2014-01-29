## Inputs

* Use ALMA standard (this is the format used by NACP and MsTMIP) 

Input variables (for BioCro, not sufficient for all models)

* Specific Humidity (provides daily mean, min, max)
 * shum:long_name = "mean Daily Specific Humidity at 2 m" ;
 * shum:units = "kg/kg" ;
* Relative Humidity
 * rhum:long_name = "mean Daily relative humidity at sigma level 995" ;
 * rhum:units = "%" ;
* Precipitation
 * prate:long_name = "mean Daily Precipitation Rate at surface" ;
 * prate:units = "Kg/m^2/s" ;
* Wind
 * uwnd:long_name = "mean Daily u-wind at 10 m" ;
 * uwnd:units = "m/s" ;
 * vwnd:long_name = "mean Daily v-wind at 10 m" ;
 * vwnd:units = "m/s" ;
 * wind = sqrt(vwnd^2 + uwnd^2)
* Temperature
 * air:long_name = "mean Daily Air temperature at 2 m" ;
 * air:units = "degK" ;
* Solar Radiation
 * dswrf:long_name = "mean Daily Downward Solar Radiation Flux at surface" ;
 * dswrf:units = "W/m^2" ;

## Outputs

* created by `model2netcdf` functions
* based on format used by [MsTMIP](http://nacp.ornl.gov/MsTMIP_variables.shtml)

## Defining new inputs

* New inputs can be defined on the ['formats' page of BETYdb](http://betydb.org/formats)
* The contents should be defined using EML