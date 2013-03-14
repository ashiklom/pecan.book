# Example Settings Files

* ED2
 * ED2IN template (Fortran Namelist): [models/ed/inst/ED2IN.r82](https://github.com/PecanProject/pecan/blob/master/models/ed/inst/ED2IN.r82), [models/ed/inst/ED2IN.r46](https://github.com/PecanProject/pecan/blob/master/models/ed/inst/ED2IN.r46)
 * example settings file (see below)
* SIPNET: 
 * [sipnet.in](https://github.com/PecanProject/pecan/blob/master/models/sipnet/inst/sipnet.in)
 * pecan settings: [utils/inst/extdata/test.settings.xml](https://github.com/PecanProject/pecan/blob/master/utils/inst/extdata/test.settings.xml] (this is the 'standard' used for testing)
* BIOCRO: 
 * pecan settings: [models/biocro/inst/extdata/pecan.biocro.xml](https://github.com/PecanProject/pecan/blob/master/models/biocro/inst/extdata/pecan.biocro.xml)

## Node definitions

### Settings File
An example of a xml settings file can be found in template.settings.xml, under the PEcAn root folder.

The following is a list of commonly used tags within xml settings files.
<table>
  <tr>
    <td><strong>node</strong></td>
    <td> <strong>definition</strong></td>
  </tr>
  <tr>
    <td> <strong><code>&lt;pfts&gt;</code></strong> </td>
    <td> precedes a list of pft </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;pfts&gt;&lt;pft&gt;</code></strong> </td>
    <td> defines a single pft </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;pfts&gt;&lt;pft&gt;&lt;name&gt;</code></strong> </td>
    <td> The plant functional type that the meta-analysis will be performed on. A list of options may be found under the "name" column of the pfts table in the "Biofuel Ecophysioligical Traits and Yields database (BETYdb)":http:/ebi-forecast.igb.uiuc.edu/bety  </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;pfts&gt;&lt;pft&gt;&lt;outdir&gt;</code></strong> </td>
    <td> The file path to which pft-specific output will be placed during meta-analysis and sensitivity analysis </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;pfts&gt;&lt;pft&gt;&lt;contants&gt;</code></strong> </td>
    <td> ED specific. Xml that will appear within generated config files for that specific pft file. Parameters given a constant will be excluded from analyses.</td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td></td>
  </tr>
  <tr>
    <td> <strong><code>&lt;meta.analysis&gt;&lt;iter&gt;</code></strong> </td>
    <td> "MCMC":http:/en.wikipedia.org/wiki/Markov_chain_Monte_Carlo  chain length, i.e. the total number of posterior samples in the meta-analysis</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;meta.analysis&gt;&lt;random.effects&gt;</code></strong> </td>
    <td> TRUE/FALSE. Whether to include random effects (site, treatment) in meta-analysis model. Can be set to FALSE to work around convergence problems caused by  an over parameterized model (e.g. too many sites, not enough data)  </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;ensemble&gt;&lt;size&gt;</code></strong> </td>
    <td> The number of runs in the ensemble</td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td></td>
  </tr>
  <tr>
    <td><strong><code>&lt;pecanDir&gt;</code></strong></td>
    <td> pecan package home directory.</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;Rlibdir&gt;</code></strong> </td>
    <td> location of R pecan library</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;outdir&gt;</code></strong> </td>
    <td> The folder to which output will be written.</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;runnames&gt;</code></strong></td>
    <td> currently supports  'prior' and posterior('post') runs listed in &lt;runname&gt; nodes.</td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td></td>
  </tr>
  <tr>
    <td> <strong><code>&lt;sensitivity.analysis&gt;</code></strong> </td>
    <td> Either be empty or have &lt;quantile&gt; or &lt;sigma&gt; nodes. If neither are given, the default is to use the median +/- [1 2 3] x sigma (e.g. the  0.00135 0.0228 0.159 0.5 0.841 0.977 0.999 quantiles);  If the 0.5 (median) quantile is omitted, it will be added in the code. </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;sensitivity.analysis&gt;&lt;quantiles&gt;</code></strong> </td>
    <td> Quantiles of parameter distributions at whic which model should be evaluated when running sensitivity analysis. Values greater than 0 and less than 1 can be used.   </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;sensitivity.analysis&gt;&lt;sigma&gt;</code></strong> </td>
    <td> Any real number can be used to indicate the quantiles to be used in units of normal probability.  </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;sensitivity.analysis&gt;&lt;start.date&gt;</code></strong> </td>
    <td> YYYY/MM/DD format</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;sensitivity.analysis&gt;&lt;end.date&gt;</code></strong> </td>
    <td> YYYY/MM/DD format</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;sensitivity.analysis&gt;&lt;variable&gt;</code></strong> </td>
    <td> name of the variable the analysis should be run for </td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td></td>
  </tr>
  <tr>
    <td> <strong><code>&lt;database&gt;</code></strong> </td>
    <td> </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;database&gt;&lt;userid&gt;</code></strong> </td>
    <td> database login </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;database&gt;&lt;passwd&gt;</code></strong> </td>
    <td> database password </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;database&gt;&lt;location&gt;</code></strong> </td>
    <td> database server (localhost for a the local machine)</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;database&gt;&lt;name&gt;</code></strong> </td>
    <td> name of the database </td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td></td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;</code></strong> </td>
    <td>  </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;name&gt;</code></strong> </td>
    <td> name of the model </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;binary&gt;</code></strong> </td>
    <td> path to the model executable </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;id&gt;</code></strong> </td>
    <td> id in the models database table </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;config.headers&gt;</code></strong> </td>
    <td> ED specific. Xml that will appear at the start of generated config files. </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;edin&gt;</code></strong> </td>
    <td> template used to write ED2IN file</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;veg&gt;</code></strong> </td>
    <td> location of VEG database</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;soil&gt;</code></strong> </td>
    <td> location of soild database</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;psscss&gt;</code></strong> </td>
    <td> location of site inforation</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;model&gt;&lt;inputs&gt;</code></strong> </td>
    <td> location of additional input files (e.g. data assimilation data)</td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td></td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;</code></strong> </td>
    <td> </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;site&gt;</code></strong> </td>
    <td> </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;site&gt;&lt;name&gt;</code></strong> </td>
    <td> site name</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;site&gt;&lt;lat&gt;</code></strong> </td>
    <td> site latitude</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;site&gt;&lt;lon&gt;</code></strong> </td>
    <td> site longitude</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;site&gt;&lt;met&gt;</code></strong> </td>
    <td> location of met data header file </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;start.date&gt;</code></strong> </td>
    <td> </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;end.date&gt;</code></strong> </td>
    <td> </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;host&gt;</code></strong> </td>
    <td> </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;host&gt;&lt;name&gt;</code></strong> </td>
    <td> name of host server where model is located  </td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;host&gt;&lt;rundir&gt;</code></strong> </td>
    <td> directory including model executable</td>
  </tr>
  <tr>
    <td> <strong><code>&lt;run&gt;&lt;host&gt;&lt;outdir&gt;</code></strong> </td>
    <td> location of model output files</td>
  </tr>
</table>




 

## Example file

```
<?xml version="1.0"?>
<pecan>
  <!-- directories -->
  <pecanDir>/home/pecan/pecan/</pecanDir>
  <Rlibdir>/home/pecan/lib/R/</Rlibdir>
  <outdir>/home/pecan/runs/PEcAn_4/</outdir>

  <!-- how to connect to pecan database -->
  <database>
    <name>bety</name>
    <userid>bety</userid>
    <passwd>bety</passwd>
    <location>localhost</location>
  </database>

  <!-- a list of pfts -->  
  <pfts>
    <pft>
      <name>temperate.Early_Hardwood</name>
      <outdir>/home/pecan/runs/PEcAn_4/pft/1/</outdir>
      <constants>
        <num>15</num>
        <!-- an example of how to overwrite traits -->
	<mort2>27.9</mort2>
	<Vm_low_temp>10.0</Vm_low_temp>
	<root_respiration_rate>3.76</root_respiration_rate>
	<seedling_mortality>0.97</seedling_mortality>
	<nonlocal_dispersal>0.214</nonlocal_dispersal>
	<stomatal_slope>3.45</stomatal_slope>
      </constants>
    </pft>
  </pfts>
  
  <!-- configuration of meta analysis phase in workflow -->
  <meta.analysis>
    <iter>50000</iter>
    <random.effects>TRUE</random.effects>
  </meta.analysis>
  
  <!-- number of runs to do -->
  <ensemble>
    <size>100</size>
  </ensemble>

  <!-- configuration of sensitivity phase in workflow -->
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

  <!-- configuration of data assimilation phase in workflow -->
  <data.assimilation>
    <nmcmc>1000</nmcmc>
  </data.assimilation>

  <!-- model specific information -->
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

  <!-- what site to run and where -->
  <run>
    <site>
      <id>772</id>
      <name>Niwot Ridge Forest/LTER NWT1 (US-NR1)</name>
      <lat>40.032900</lat>
      <lon>-105.546000</lon>
      <met>/home/pecan/sites/niwot/ED_MET_DRIVER_HEADER</met>
      <met.start>2006/06/01</met.start>
      <met.end>2006/07/31</met.end>
    </site>
    <start.date>2000/01/01</start.date>
    <end.date>2002/12/31</end.date>
    <host>
      <name>ebi-cluster.igb.uiuc.edu</name>
      <rundir>/home/dlebauer/EDBRAMS/ED/run/</rundir>
      <outdir>/home/scratch/dlebauer/pecan/RUNDIRNAME/out/</outdir>
    </host>
  </run>
</pecan>
```