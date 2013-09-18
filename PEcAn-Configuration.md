The PEcAn system is configured using a xml file, often called settings.xml. The configuration file can be split in X seperate pieces:

1. [PEcAn Folders](#pecan_folders)
1. [Database Access](#database_access)
1. [BETY Configuration](#bety_database)
1. [PFT Selection](#pft_selection)
1. [Meta Analysis](#meta_analysis)
1. [Ensemble Runs](#ensemble_runs)
1. [Sensitivity Runs](#sensitivity_runs)
1. [Model Setup](#model_setup)
1. [Run Setup](#run_setup)

<a name="pecan_folders" />

## PEcAn folders

The following are the tags that can be used to configure the folders used by PEcAn. All of these are optional.

```{xml}
<outdir>/home/carya/testrun.pecan</outdir>
```

* **outdir** : [optional] specifies where PEcAn will write all outputs and create folders. If this is not specified the folder pecan in the current folder will be used.

<a name="database_access" />
## Database Access

The connection to the BETY database is configured using this section. In this section you will specify what driver to use to connect to the database (MySQL by default) and the connection parameters to connect to the database. This section is very picky and will only accept parameters that are passed into the connection function for each database driver, any other entries will result in an error by the database driver.

### functions: 

`db.open()`, and thus by `db.query()` and `db.check()`.

```{xml}
<database>
	<dbname>bety</dbname>
	<username>bety</username>
	<password>bety</password>
	<location>localhost</location>
</database>
```

* **driver** : [optional] the driver to use to connect to the database. Default value is MySQL  
* **dbname** : [optional] the name of the database (was name), default value is the username of the current user logged in.  
* **username** : [optional] the username to connect to the database (was userid), default value is the username of the current user logged in (PostgreSQL uses user for this field).
* **password** : [optional] the password to connect to the database (was passwd), if not specified no password is used.  
* **host** : [optional] the name of the host to connect to, default value is localhost.  

For other database drivers these parameters will change. See the driver documentation in R for the right parameters.

**This section might become a subsection of BETY Database Configuration in the future**

<a name="bety_database" />
## BETY Database Configuration

This section describes how to connect to the BETY Database.

### functions: 

`db.check()`, `write.configs()`, `run.models()`

```{xml}
<bety>
	<write>TRUE</write>
</bety>
```

* **write** : [optional] this can be TRUE/FALSE (the default is TRUE). If set to TRUE, runs, ensembles and workflows are written to the database.

<a name="pft_selection" />
## PFT Selection

The PEcAn system requires at least 1 PFT (Plant Functional Type) to be specified inside the `<pfts>` section. 

### functions:  

`get.traits()`


### PFT tags


```{xml}
<pfts>
	<pft>
		<name>sipnet.temperate.coniferous</name>
		<outdir>/home/carya/testrun.pecan/pft/1/</outdir>
		<constants>
			<num>1</num>
		</constants>
	</pft>
</pfts>
```

* **name** : [required] the pft as is found inside the BETY database, this needs to be an exact match.  
* **outdir**: [optional] path in which pft-specific output will be placed during meta-analysis and sensitivity analysis. If not specified it will be written into `<outdir>/<pftname>`.  
* **contants**: [optional] this section contains information that will be written directly into the model specific configuration files. PEcAn does not look at the information in this section.  

<a name="meta_analysis" />
## Meta Analysis

### meta analysis functions:

`run.meta.analysis()`

### meta analysis tags

```{xml}
<meta.analysis>
	<iter>1000</iter>
	<random.effects>FALSE</random.effects>
	<update>FALSE</update>
</meta.analysis>
```

The section meta.analysis needs to exists for a meta.analysis to be executed, even though all tags inside are optional.

* **iter** : [optional] [MCMC](http:/en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) (Markov Chain Monte Carlo) chain length, i.e. the total number of posterior samples in the meta-analysis, default is 3000. Smaller numbers will run faster but produce larger errors.  
* **random.effects** : [optional] Whether to include random effects (site, treatment) in meta-analysis model. Can be set to FALSE to work around convergence problems caused by an over parameterized model (e.g. too many sites, not enough data). The default value is TRUE.
* **update** : [optional] Should previous results of meta.analysis and get.traits be re-used. If set to TRUE the meta-analysis and get.trait.data will always be executed. Setting this to FALSE will try and reuse existing results. Future versions will allow for AUTO as well which will try and reuse if the PFT/traits have not changed. The default value is FALSE.

<a name="ensemble_runs" />
## Ensemble Runs

Only if this section is defined an ensemble analysis is done.

### ensemble functions: 

`write.configs()`, `run.ensemble.analysis()`

### ensemble tags

```{xml}
<ensemble>
	<size>5</size>
  <variable>GPP</variable>
  <variable>NPP</variable>
</ensemble>
```

* **size** : [required] the number of runs in the ensemble.
* **variable**: [optional] name of one (or more) variables the analysis should be run for. If not specified, sensitivity.analysis variable is used, otherwise default is GPP.

<a name="sensitivity_runs" />
## Sensitivity Runs

Only if this section is defined a sensitivity analysis is done. This section will have `<quantile>` or `<sigma>` nodes. If neither are given, the default is to use the median +/- [1 2 3] x sigma (e.g. the 0.00135 0.0228 0.159 0.5 0.841 0.977 0.999 quantiles); If the 0.5 (median) quantile is omitted, it will be added in the code.

### functions: 

`write.configs()`, `run.sensitivity.analysis()`

### sensitivity analysis tags

```{xml}
<sensitivity.analysis>
	<quantiles>
		<quantile></quantile>
		<sigma>-3</sigma>
		<sigma>-2</sigma>
		<sigma>-1</sigma>
		<sigma>1</sigma>
		<sigma>2</sigma>
		<sigma>3</sigma>
	</quantiles>
  <variable>GPP</variable>
	<start.year>2004</start.year>
	<end.year>2006</end.year>
</sensitivity.analysis>
```

* **quantiles** : [optional] Quantiles of parameter distributions at which the model should be evaluated when running sensitivity analysis. Values greater than 0 and less than 1 can be used.  
* **sigma** : [optional] Any real number can be used to indicate the quantiles to be used in units of normal probability.  
* **quantile** : [optional] Which quantile should be used.  
* **start.date** : [required?] start date of the sensitivity analysis (in YYYY/MM/DD format) 
* **end.date** : [required?] end date of the sensitivity analysis (in YYYY/MM/DD format)
* **_NOTE:_** start.date and end.date are distinct from values set in the run tag because this analysis can be done over a subset of the run.
* ** variable** : [optional] name of one (or more) variables the analysis should be run for. If not specified, sensitivity.analysis variable is used, otherwise default is GPP.

<a name="model_setup" />
## Model Setup

This section is required and tells PEcAn what model to run. This section should either specify `<id>` or both `<name>` and `<binary>`  of the model. If both id and name and/or binary are specified the id is used to check the specified name and/or binary.

### functions: 

`write.configs()`, `run.models()`

### model tags

```{xml}
<model>
	<id>7</id>
	<name>ED2</name>
	<binary>/usr/local/bin/ed2.r82</binary>
  <config.header>
    <!--...xml code passed directly to config file...-->
  </config.header>
</model>
```

* **id** : [optional/required] id in the models database table, see above.  
* **name** : [optional/required] name of the model, see above.  
* **binary** : [optional/required] path to the model executable, see above.  
* **config.headers** : [optional] XML that will appear at the start of generated config files.

### ED2 specific tags

Following variables are ED specific and are used in the [ED2 Configuration](ED2-Configuration).


```{xml}
   <config.header>
		<radiation>
			<lai_min>0.01</lai_min>
		</radiation>
		<ed_misc>
			<output_month>12</output_month>      
		</ed_misc> 
	</config.header>
	<edin>/home/carya/runs/PEcAn_4/ED2IN.template</edin>
	<veg>/home/carya/oge2OLD/OGE2_</veg>
	<soil>/home/carya/faoOLD/FAO_</soil>
	<psscss>/home/carya/sites/niwot/</psscss>
	<inputs>/home/carya/ed_inputs/</inputs>
	<phenol.scheme>0</phenol.scheme>
```

  
* **edin** : [required] template used to write ED2IN file  
* **veg** : [required] location of VEG database  
* **soil** : [required] location of soild database  
* **psscss** : [required] location of site inforation  
* **inputs** : [required] location of additional input files (e.g. data assimilation data)

<a name="run_setup" />
## Run Setup

### tags

```{xml}
<run>
	<start.date>2002-01-01 00:00:00</start.date>
	<end.date>2005-12-31 00:00:00</end.date>
	</dbfiles>/home/carya/.pecan/dbfiles</dbfiles>
	<site>
		<id>772</id>
		<name>Niwot Ridge Forest/LTER NWT1 (US-NR1)</name>
		<lat>40.032900</lat>
		<lon>-105.546000</lon>
		<met>/home/carya/sites/niwot/niwot.clim</met>
	</site>
	<host>
		<name>localhost</name>
		<rundir>/home/carya/testrun.pecan/run/</rundir>
		<outdir>/home/carya/testrun.pecan/out/</outdir>
		<scratchdir>/tmp/carya</scratchdir>
		<clearscratch>TRUE</clearscratch>
		<qsub>qsub -N @NAME@ -o @STDOUT@ -e @STDERR@</qsub>
		<qsub.jobid>lionxo_test ([0-9]+) .*</qsub.jobid>
		<qstat>qstat -j @JOBID@ 2>1 >/dev/null || echo DONE</qstat>
	</host>
</run>
```

* **start.date** : [required] the first day of the simulation  
* **end.date** : [required] the last day of the simulation
* **dbfiles** : [optional] location where pecan should write files that will be stored in the database. The default is store them in ${HOME}/.pecan/dbfiles

Site specific information is specified in the `<site>`subsection. Either `<id>` or `<name>`, `<lat>` and `<lon>` should be specified. If id and any of the others are specified the values will be compared with those from the bETY database.
 
* **id** : [optional/required] id of the site in the BETY database, see above.  
* **name** : [optional/required] site name, see above.  
* **lat** : [optional/required] site latitude, see above.  
* **lon** : [optional/required] site longitude, see above.  
* **met** : [required] location of met data header file used by the model (not required by BIOCRO).

Host on which the simulation will run is specified in the `<host>` subsection. If this section is not specified it is assumed the simulation will run on localhost. If qsub is specified (can be empty in which case the defaults will be used) the models will be executed using qsub.

* **name** : [optional] name of host server where model is located and executed, if not specified localhost is assumed.  
* **rundir** : [optional/required] location where all the configuration files are written. For localhost this is optional (`<outdir>/run` is the default), for any other host this is required.  
* **outdir** : [optional/required] location where all the outputs of the model are written. For localhost this is optional (`<outdir>/out` is the default), for any other host this is required.
* **scratchdir** : [optional] location where output is written. If specified the output from the model is written to this folder and copied to the outdir when the model is finished, this could significantly speed up the model execution (by using local or ram disk).
* **clearscratch** : [optional] if set to TRUE the scratchfolder is cleaned up after copying the results to the outdir, otherwise the folder will be left. The default is to clean up after copying.
* **qsub** : [optional] the command to submit a job to the queuing system. There are 3 parameters you can use when specifying the qsub command, you can add additional values for your specific setup (for example -l walltime to specify the walltime, etc). You can specify @NAME@ the pretty name, @STDOUT@ where to write stdout and @STDERR@, where to write stderr. The default value is "qsub -N @NAME@ -o @STDOUT@ -e @STDERR@".
* **qsub.jobid** : [optional] the regular expression used to find the jobid returned from qsub. The default value is "Your job ([0-9]+) .*"
* **qstat** : [optional] the command to execute to check if a job is finished, this should return DONE if the job is finished. There is one parameter this command should take @JOBID@ which is the id of the job as returned by qsub.jobid. The default value is "qstat -j @JOBID@ 2>1 >/dev/null || echo DONE"