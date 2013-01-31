The PEcAn system is configured using a xml file, often called settings.xml. The configuration file can be split in X seperate pieces:

1. PEcAn folders
1. database access
1. Model setup
1. Run setup
1. PFTs
1. Meta Analysis
1. Ensemble/Sensitivity runs

## PEcAn folders

The following are the tags that can be used to configure the folders used by PEcAn. All of these are optional.

	<!-- directories -->
	<outdir>/home/pecan/runs/PEcAn_4/</outdir>
	<pecanDir>/home/pecan/pecan/</pecanDir>
	<Rlibdir>/home/pecan/lib/R/</Rlibdir>

*outdir* specifies where PEcAn will write all outputs and create folders. If this is not specified the current folder will be used.
*Rlibdir* specifies any additional R paths that are needed.
*pecanDir* this folder will specify where the pecan source is. This argument is deprecated and not needed anymore.

## Database Access

The connection to the BETY database is configured using this section. This section is likely to change in the near future to allow for access to the FIA database as well.

	<database>
		<name>bety</name>
		<userid>bety</userid>
		<passwd>bety</passwd>
		<location>localhost</location>
	</database>

## Model Setup

	<model>
		<name>ED2</name>
		<binary>/usr/local/bin/ed.r82</binary>
		<id>7</id>
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
