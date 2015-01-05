Once a Machine, Model, and Site have been selected, PEcAn will take you to the Input selection page. From this page you will select what Plant Functional Type (PFT) you want to run at a site, the start and end dates of the run, and various Input selections. The most common of these across all models is the need to specify meteorological forcing data. The exact name of the menu item for meteorology will vary by model because all of the Input requirements are generated individually for each model based on the MODEL_TYPE table.  In general there are 3 possible cases for meteorology

* PEcAn already has driver files in its database
* PEcAn does not have drivers, but can generate them from publicly available data
* You need (or want) to upload your own drivers

The first two cases will appear automatically in the the pull down menu. For meteorological files that already exist you will see the date range that's available. By contrast, met that can be generated will appear as "Use <source>", where <source> is the origin of the data (e.g. "Use Ameriflux" will use the micromet from an Ameriflux eddy covariance tower, if one is present at the site).

If you want to upload your own met data this can be done in three ways. The standard way is to upload data in PEcAn's standard meteorological format (netCDF files, CF metadata). See [here](https://github.com/PecanProject/pecan/wiki/Adding-an-Input-Converter#met-data) for details about variables and units. From this standard, PEcAn can then convert the file to the model-specific format required by the model you have chosen. This approach is preferred because PEcAn will also be able to convert the file into the format required by any other model as well. The second option for adding met data is to add it in a model-specific format, which is often easiest if you've already been running your model at a site and are just switching to using PEcAn. The final way to add met data is to incorporate it into the overall meteorological processing workflow. This is preferred if you are working with a common meteorological data product that is not yet in PEcAn's workflow. Details are [here](https://github.com/PecanProject/pecan/wiki/Adding-an-Input-Converter#met-data), though at this stage you would also be strongly encouraged to contact the PEcAn development team.

In all cases, visit [[How to insert new Input data]] for information about how to register the files with the PEcAn database.

## Met workflow

In a nutshell, the PEcAn met workflow is designed to reduce the problem of converting *n* possible met inputs into *m* possible model formats, which requires *n x m* conversion functions as well as numerous custom functions for downscaling, gap filling, etc. Instead, PEcAn works with a single met standard, and thus requires *n* conversion functions, one for converting each data source into the PEcAn standard, and then *m* conversion functions for converting from that standard to what an individual model requires. For a new model joining the PEcAn system the burden in particularly low -- writing one conversion function provides access to *n* inputs. Similarly, PEcAn performs all other operations/manipulations (extracting a site, downscaling, gap filling, etc) within the PEcAn standard, which means these operations only need be implemented once.

Consider a generic met data product named MET for simplicity. PEcAn will use a function, download.MET, to pull data for the selected year from a public data source (e.g. Ameriflux, North American Regional Reanalysis, etc). Next, PEcAn will use a function, met2CF.MET, to convert the data into the PEcAn standard. If the data is already at the site scale it will then gapfill the data. If the data is a regional or global data product, PEcAn will then permute the data to allow easier site-level extraction, then it will extract data for the requested site and data range. Modules to address the temporal and spatial downscaling of meteorological data products, as well as their uncertainties, are in development but not yet part of the operational workflow. All of these functions are located within the data.atmosphere module.

Once data is in the standard format and processed, it will be converted to the model-specific format using a met2model.MODEL function (located in that MODEL's module).

## Troubleshooting meteorological conversions

At the current moment (PEcAn 1.4.0), most of the issues below address possible errors that the Ameriflux meteorology workflow might report

### 


