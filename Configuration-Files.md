PEcAn has a single xml configuration file. This fill will dictate what models to run, where, with what input files and for how long. Each model has its own configuration file as well.

[PEcAn Configuration File](PEcAn-Configuration)<br>
This file is often called pecan.xml. The system will try to find this file in the following locations:
1. Passed in when calling R with --settings
2. Passed in to read.settings() function
3. File called pecan.xml in the current working folder.
Once any of these are loaded it will try to locate a pecan.xml in the outdir specified in the loaded file.

[ED2 Configuration File](ED2-Configuration)<br>
The system expects a standard ED2IN file (Fortran Namelist). This file should have a few special variables that will be replaced by PEcAn when writing the run configurations.

[SIPNET Configuration File](SIPNET-Configuration)<br>
sipnet.in

[BIOCRO Configuration File](BIOCRO-Configuration)<br>
pecan settings: models/biocro/inst/extdata/pecan.biocro.xml