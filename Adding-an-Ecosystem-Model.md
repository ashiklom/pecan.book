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



## model2netcdf.MODEL (required)

## met2model.MODEL

Converts meteorology input files from the PEcAn standard (netCDF, CF metadata) to the format required by the model.

# PEcAn Database