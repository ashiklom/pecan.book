The PEcAn system is configured using a xml file, often called settings.xml. The configuration file can be split in X seperate pieces:

1. [PEcAn folders](PEcAn-Configuration#PEcAn-folders)
1. [Database Access](PEcAn-Configuration#Database-Access)
1. bety configuration
1. Model setup
1. Run setup
1. PFTs
1. Meta Analysis
1. Ensemble/Sensitivity runs

## PEcAn folders

The following are the tags that can be used to configure the folders used by PEcAn. All of these are optional.

	<outdir>/home/pecan/runs/PEcAn_4/</outdir>

*outdir* specifies where PEcAn will write all outputs and create folders. If this is not specified the folder pecan in the current folder will be used.

## <a name="Database Access">Database Access</a>

The connection to the BETY database is configured using this section. In this section you will specify what driver to use to connect to the database (MySQL by default) and the connection parameters to connect to the database.

	<database>
		<dbname>bety</dbname>
		<username>bety</username>
		<password>bety</password>
		<location>localhost</location>
	</database>

For example for MySQL you can specify the following parameters:  
*dbname* : the name of the database (was name), default value is the username of the current user logged in.
*username* : the username to connect to the database (was userid), default value is the username of the current user logged in.
*password* : the password to connect to the database (was passwd), if not specified no password is used.
*location* : the name of the host to connect to, default value is localhost

For other database drivers these parameters will change. See the driver documentation in R for the right parameters.

## Bety Configuration



## Model Setup

	<model>
		<id>7</id>
		<name>ED2</name>
		<binary>/usr/local/bin/ed.r82</binary>
		<config.header>
			<radiation>
				<lai_min>0.01</lai_min>
			</radiation>
			<ed_misc>
				<output_month>12</output_month>      
			</ed_misc> 
		</config.header>
		<edin>/home/pecan/runs/PEcAn_4/ED2IN.template</edin>
		<veg>/home/pecan/oge2OLD/OGE2_</veg>
		<soil>/home/pecan/faoOLD/FAO_</soil>
		<psscss>/home/pecan/sites/niwot/</psscss>
		<inputs>/home/pecan/ed_inputs/</inputs>
		<phenol.scheme>0</phenol.scheme>
	</model>