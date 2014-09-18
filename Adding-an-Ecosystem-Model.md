Under development.

The [models/template](https://github.com/PecanProject/pecan/tree/master/models/template) package should provide a generic instruction for adding a new template, with example functions. An out-of-date draft of these instructions is located in the model template package [README](https://github.com/PecanProject/pecan/blob/master/models/template/README.Rmd) file.

Adding a model to PEcAn involves two activities:

1) Writing the interface modules between the model and PEcAn
2) Updating the PEcAn database to register the model

# Interface Modules

## Setting up the module directory (required)

PEcAn assumes that the interface modules are available as an R package in the models directory named after the model in question. The simplest way to get started on that R package is to make a copy the _template_ directory and re-name it to the name of your model. In the code, filenames, and examples below you will want to substitute the word *MODEL* for the name of your model. If you do not want to write the interface modules in R then it is fairly simple to set up the functions describe below to just call the script you want to run using R's _system_ command. Scripts that are not R functions can be placed in the _inst_ folder and R can look up the location of these files using the function _system.file_ which takes as arguments the _local_ path of the file within the package folder and the name of the package (typically PEcAn.MODEL).

Within the module folder open the *DESCRIPTION* file and change the package name to PEcAn.MODEL. Fill out other fields such as Title, Author, Maintainer, and Date.

Open the *NAMESPACE* file and change all instances of MODEL to the name of your model. If you are not going to implement one of the optional modules (described below) at this time then you will want to comment those out using the pound sign `#`. For a complete description of R NAMESPACE files [see here](http://cran.r-project.org/doc/manuals/r-devel/R-exts.html#Package-namespaces).

Once the package is defined you will then need to add it to the PEcAn build scripts.  From the root of the pecan directory, within _scripts_ folder open the file _build.sh_ and within the section of code that includes PACKAGES= add model/MODEL to the list of packages to compile. If, in writing your module, you add any other R packages to the system you will want to make sure those are listed in the DESCRIPTION and in the script *install.dependencies.R*. Next, from the root pecan directory open all/DESCRIPTION and add your model package to the *Suggests:* list

## write.config.MODEL (required)

This module performs two primary tasks. The first is to take the list of parameter values and model input files that it receives as inputs and write those out in whatever format(s) the MODEL reads (e.g. a settings file). The second is to write out a shell script, jobs.sh, which, when run, will start your model run and convert its output to the PEcAn standard (netCDF with metadata currently equivalent to the [MsTMIP standard](http://nacp.ornl.gov/MsTMIP_variables.shtml)). Within the MODEL directory take a close look at inst/template.job and the example write.config.MODEL to see an example of how this is done. It is important that this script writes or moves outputs to the correct location so that PEcAn can find them. The example function also shows an example of writing a model-specific settings/config file, also by using a template.

## model2netcdf.MODEL

This module converts model output into the PEcAn standard (netCDF with metadata currently equivalent to the [MsTMIP standard](http://nacp.ornl.gov/MsTMIP_variables.shtml)). This file was previously required, but now that the conversion is called within jobs.sh it is much easier to convert outputs using other approaches.

## met2model.MODEL

Converts meteorology input files from the PEcAn standard (netCDF, CF metadata) to the format required by the model. This file is optional if you want to specify input files explicitly, which is often the easiest way to get up and running quickly. However, this function is required if you want to benefit from PEcAn's meteorology workflows and model run cloning.

# PEcAn Database