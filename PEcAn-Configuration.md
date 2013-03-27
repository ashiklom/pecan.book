The PEcAn system is configured using a xml file, often called settings.xml. The configuration file can be split in X seperate pieces:

1. PEcAn Folders
1. Database Access
1. BETY Configuration
1. PFT Selection
1. Meta Analysis
1. Ensemble/Sensitivity Runs
1. Model Setup
1. Run Setup

## PEcAn folders

The following are the tags that can be used to configure the folders used by PEcAn. All of these are optional.

	<outdir>/home/carya/testrun.pecan</outdir>

**outdir** : [optional] specifies where PEcAn will write all outputs and create folders. If this is not specified the folder pecan in the current folder will be used.

## Database Access

The connection to the BETY database is configured using this section. In this section you will specify what driver to use to connect to the database (MySQL by default) and the connection parameters to connect to the database.

The values used in this section are used by: db.open(), and thus by db.query() and db.check().

	<database>
		<dbname>bety</dbname>
		<username>bety</username>
		<password>bety</password>
		<location>localhost</location>
	</database>

For example for MySQL you can specify the following parameters:  
**dbname** : [optional] the name of the database (was name), default value is the username of the current user logged in.  
**username** : [optional] the username to connect to the database (was userid), default value is the username of the current user logged in.  
**password** : [optional] the password to connect to the database (was passwd), if not specified no password is used.  
**host** : [optional] the name of the host to connect to, default value is localhost.  

For other database drivers these parameters will change. See the driver documentation in R for the right parameters.

**This section might become a subsection of BETY Database Configuration in the future**

## BETY Database Configuration

This section describes how to connect to the BETY Database.

	<bety>
		<write>TRUE</write>
	</bety>

**write** : [optional] this can be TRUE/FALSE (the default is TRUE). If set to TRUE, runs, ensembles and workflows are written to the database.

## PFT Selection

The PEcAn system requires at least 1 PFT (Plant Functional Type) to be specified inside the <pfts> section. 

The values specified in this section are used in get.traits().

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
**outdir**: [optional] where to save output for each PFT. If not specified it will be written into <outdir>/pfts/<pftname>.
**contants**: [optional] this section contains information that will be written directly into the model specific configuration files. PEcAn does not look at the information in this section.

## Meta Analysis

	<meta.analysis>
		<iter>1000</iter>
		<random.effects>FALSE</random.effects>
	</meta.analysis>

## Ensemble Runs/Sensitivity Runs

	<ensemble>
		<size>5</size>
	</ensemble>

## Model Setup

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
