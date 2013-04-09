PEcAn has a single xml configuration file. This fill will dictate what models to run, where, with what input files and for how long. Each model has its own configuration file as well.

### [PEcAn Configuration Files](PEcAn-Configuration)  

This file is often called pecan.xml. The system will try to find this file in the following locations:  
* Passed in when calling R with --settings  
* Passed in to read.settings() function  
* File called pecan.xml in the current working folder  
Once any of these are loaded it will try to locate a pecan.xml in the outdir specified in the loaded file.


### Model-specific configuration files

* [ED2](ED2-Configuration)  
* [SIPNET](SIPNET-Configuration)  
* [BIOCRO](BIOCRO-Configuration) 