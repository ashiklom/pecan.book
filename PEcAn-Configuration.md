The PEcAn system is configured using a xml file, often called settings.xml. The configuration file can be split in X seperate pieces:

1. PEcAn Folders
1. Database Access
1. BETY Configuration
1. PFT Selection
1. Meta Analysis
1. Ensemble Runs
1. Sensitivity Runs
1. Model Setup
1. Run Setup

## PEcAn folders

The following are the tags that can be used to configure the folders used by PEcAn. All of these are optional.

	<outdir>/home/carya/testrun.pecan</outdir>

**outdir** : [optional] specifies where PEcAn will write all outputs and create folders. If this is not specified the folder pecan in the current folder will be used.

## Database Access

The connection to the BETY database is configured using this section. In this section you will specify what driver to use to connect to the database (MySQL by default) and the connection parameters to connect to the database.

*functions* : db.open(), and thus by db.query() and db.check().

	<database>
		<dbname>bety</dbname>
		<username>bety</username>
		<password>bety</password>
		<location>localhost</location>
	</database>

**driver** : [optional] the driver to use to connect to the database. Default value is MySQL  
**dbname** : [optional] the name of the database (was name), default value is the username of the current user logged in.  
**username** : [optional] the username to connect to the database (was userid), default value is the username of the current user logged in.  
**password** : [optional] the password to connect to the database (was passwd), if not specified no password is used.  
**host** : [optional] the name of the host to connect to, default value is localhost.  

For other database drivers these parameters will change. See the driver documentation in R for the right parameters.

**This section might become a subsection of BETY Database Configuration in the future**

## BETY Database Configuration

This section describes how to connect to the BETY Database.

*functions* : db.check(), write.configs(), run.models()

	<bety>
		<write>TRUE</write>
	</bety>

**write** : [optional] this can be TRUE/FALSE (the default is TRUE). If set to TRUE, runs, ensembles and workflows are written to the database.

## PFT Selection

The PEcAn system requires at least 1 PFT (Plant Functional Type) to be specified inside the <pfts> section. 

*functions* :  get.traits().

	<pfts>
		<pft>
			<name>sipnet.temperate.coniferous</name>
			<outdir>/home/carya/testrun.pecan/pft/1/</outdir>
			<constants>
				<num>1</num>
			</constants>
		</pft>
	</pfts>

**name** : [required] the pft as is found inside the BETY database, this needs to be an exact match.  
**outdir**: [optional] path in which pft-specific output will be placed during meta-analysis and sensitivity analysis. If not specified it will be written into \<outdir\>/pfts/\<pftname\>.  
**contants**: [optional] this section contains information that will be written directly into the model specific configuration files. PEcAn does not look at the information in this section.  

## Meta Analysis

*functions* : run.meta.analysis()

	<meta.analysis>
		<iter>1000</iter>
		<random.effects>FALSE</random.effects>
	</meta.analysis>

**iter** : [optional] [MCMC](http:/en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) (Markov Chain Monte Carlo) chain length, i.e. the total number of posterior samples in the meta-analysis, default is 3000. Smaller numbers will run faster but produce larger errors.  
**random.effects** : [optional] Whether to include random effects (site, treatment) in meta-analysis model. Can be set to FALSE to work around convergence problems caused by an over parameterized model (e.g. too many sites, not enough data). The default value is TRUE.

## Ensemble Runs

Only if this section is defined an ensemble analysis is done.

*functions* : write.configs(), run.ensemble.analysis()

	<ensemble>
		<size>5</size>
	</ensemble>

**size** : [required] the number of runs in the ensemble.

## Sensitivity Runs

Only if this section is defined a sensitivity analysis is done. This section will have \<quantile\> or \<sigma\> nodes. If neither are given, the default is to use the median +/- [1 2 3] x sigma (e.g. the 0.00135 0.0228 0.159 0.5 0.841 0.977 0.999 quantiles); If the 0.5 (median) quantile is omitted, it will be added in the code.

*functions* : write.configs(), run.sensitivity.analysis()

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
		<start.year>2004</start.year>
		<end.year>2006</end.year>
	</sensitivity.analysis>

**quantiles** : [optional] Quantiles of parameter distributions at which the model should be evaluated when running sensitivity analysis. Values greater than 0 and less than 1 can be used.  
**sigma** : [optional] Any real number can be used to indicate the quantiles to be used in units of normal probability.  
**quantile** : [optional] Which quantile should be used.  
**start.date** : [required?] start date of the sensitivity analysis (in YYYY/MM/DD format) **NOTE why is not the same as start/end date in run?**
**end.date** : [required?] end date of the sensitivity analysis (in YYYY/MM/DD format)  
**variable** : [optional] name of the variable the analysis should be run for, if not specified GPP will be used.


## Model Setup

This section is required and tells PEcAn what model to run. This section should either specify \<id\> or both \<name\> and \<binary\>  of the model. If both id and name and/or binary are specified the id is used to check the specified name and/or binary.

*functions* : write.configs(), run.models()

	<model>
		<id>7</id>
		<name>ED2</name>
		<binary>/usr/local/bin/ed2.r82</binary>
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
	</model>

**id** : [optional] id in the models database table, see above.  
**name** : [optional] name of the model.  
**binary** : [optional] path to the model executable, see above.  

Following variables are ED specific and are used in the [ED2 Configuration](ED2-Configuration).

**config.headers** : [optional] XML that will appear at the start of generated config files.  
**edin** : [required] template used to write ED2IN file  
**veg** : [required] location of VEG database  
**soil** : [required] location of soild database  
**psscss** : [required] location of site inforation  
**inputs** : [required] location of additional input files (e.g. data assimilation data)

## Run Setup

	<run>
		<site>
			<id>772</id>
			<name>Niwot Ridge Forest/LTER NWT1 (US-NR1)</name>
			<lat>40.032900</lat>
			<lon>-105.546000</lon>
			<met>/home/carya/sites/niwot/niwot.clim</met>
		</site>
		<start.date>2002-01-01 00:00:00</start.date>
		<end.date>2005-12-31 00:00:00</end.date>
		<host>
			<name>localhost</name>
			<rundir>/home/carya/testrun.pecan/run/</rundir>
			<outdir>/home/carya/testrun.pecan/out/</outdir>
		</host>
	</run>

Site specific information is specified in the \<site\> subsection. Either \<id\> or 
**id** : [optional] id of the site in the BETY database.
**name** : [optional] site name.
<run><site><lat>	 site latitude
<run><site><lon>	 site longitude
<run><site><met>	 location of met data header file
<run><start.date>	
<run><end.date>	
<run><host>	
<run><host><name>	 name of host server where model is located
<run><host><rundir>	 directory including model executable
<run><host><outdir>	 location of model output files.
