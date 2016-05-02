#### PEcAn in a nutshell

PEcAn provides an interface to a variety of ecosystem models and attempts to standardize and automate the processes of model parameterization, execution, and analysis. First, you choose an ecosystem model, then the time and location of interest (a site), the plant community (or crop) that you are interested in simulating, and a source of atmospheric data from the BETY database (LeBauer et al, 2010). These are set in a "settings" file, commonly named `pecan.xml` which can be edited manually if desired. From here, PEcAn will take over and set up and execute the selected model using your settings. The key is that PEcAn uses models as-is, and all of the translation steps are done within PEcAn so no modifications are required of the model itself. Once the model is finished it will allow you to create graphs with the results of the simulation as well as download the results. It is also possible to see all past experiments and simulations.

<!--can add figures of the web interface and workflow here-->

### Quick Start

There are two ways of using PEcAn, via the web interface and directly within R. Even for users familiar with R, using the web interface is a good place to start because it provides a high level overview of the PEcAn workflow.The quickest way to get started is to download the virtual machine. The demo provides instructions to get started on your own machine. 
 * [Download Virtual Machine](http://opensource.ncsa.illinois.edu/projects/artifacts.php?key=PECAN) 
 * [Work through Demo](http://pecanproject.github.io/tutorials.html)

### Using PEcAn

#### Site-level Runs
The most basic function of PEcAn is run a model at the site level. The [Demo](http://pecanproject.github.io/tutorials.html) describes the basics of running the model at an existing sit and the following links will advise you on your options, diagnosis of potential issues, and will give you more in depth information about the interface.

* [Choose a model](Choose-a-model.md)
* [Choose a site](Choose-a-site.md)
* [Choosing PFTs](Choosing-PFTs.md)
* [Choosing meteorology](Choosing-meteorlogy.md)
* [Choosing initial vegetation](Coosing-initial-vegetation.md)
* [Choosing soils](Choosing-soils.md)


### Additional PEcAn Modules and Tools
The packages listed below were developed by the PEcAn team. Some are used within the workflow itself and others stand alone for now. Please take a look through their documentation to get a better feel for the utilities of PEcAn.

| Package name | documentation | source code | description |
|:---|:---|:---|:---|
|**Utilities**| | |
|PEcAn.all| [all](https://pecanproject.github.io/pecan//all/inst/web/index.html) | [all](https://github.com/PecanProject/pecan/tree/master/all)  | Wrapper to facilitate loading of core packages|
| PEcAn.DB| [db](https://pecanproject.github.io/pecan//db/inst/web/index.html)|[db](https://github.com/PecanProject/pecan/tree/master/db)  |Interface to database containing data, provenance, and results|
|PEcAn.settings| [settings](https://pecanproject.github.io/pecan//settings/inst/web/index.html)| [settings](https://github.com/PecanProject/pecan/tree/master/settings)  | Package to read PEcAn settings files|
| PEcAn.utils | [utils](https://pecanproject.github.io/pecan//utils/inst/web/index.html)| [utils](https://github.com/PecanProject/pecan/tree/master/utils)  | Utility functions used by many PEcAn packages|
|PEcAn.visualization| [visualization](https://pecanproject.github.io/pecan//visualization/inst/web/index.html)| ||
|**Workflow Modules**| | |
|PEcAn.allometry | [allometry](https://pecanproject.github.io/pecan//modules/allometry/inst/web/index.html) |  [allometries ](https://github.com/PecanProject/pecan/tree/master/modules/allometry)  | Bayesian synthesis of allometric equations|
|PEcAn.assim.batch| [assim.batch](https://pecanproject.github.io/pecan//modules/assim.batch/inst/web/index.html) | [assim.batch ](https://github.com/PecanProject/pecan/tree/master/modules/assim.batch) | Batch data assimilation tools (e.g. MCMC)
|PEcAn.assim.sequential| [assim.sequential](https://pecanproject.github.io/pecan//modules/assim.sequential/inst/web/index.html)| [assim.sequential ](https://github.com/PecanProject/pecan/tree/master/modules/assim.sequential) | Sequential data assimilation tools (e.g. EnKF, particle filter)
|PEcAn.benchmark| |  [benchmark ](https://github.com/PecanProject/pecan/tree/master/modules/benchmark)  | Model benchmarking
|PEcAn.data.atmosphere| [data.atmosphere](https://pecanproject.github.io/pecan//modules/data.atmosphere/inst/web/index.html)| [data.atmosphere ](https://github.com/PecanProject/pecan/tree/master/modules/data.atmosphere) | Meteorology workflow|
|PEcAn.data.land| [data.land](https://pecanproject.github.io/pecan//modules/data.land/inst/web/index.html)| [data.land ](https://github.com/PecanProject/pecan/tree/master/modules/data.land/R) | Tools for land and vegetation data. At the moment predominantly Bayesian state-space tree-ring models.|
|PEcAn.data.remote| [data.remote](https://pecanproject.github.io/pecan//modules/data.remote/inst/web/index.html)| [data.remote ](https://github.com/PecanProject/pecan/tree/master/modules/data.remote/R) | Tools for remotely sensed data|
|PEcAn.emulator| [emulator](https://pecanproject.github.io/pecan//modules/emulator/inst/web/index.html)| [emulator ](https://github.com/PecanProject/pecan/tree/master/modules/emulator/R) | Tools for emulating models. Used in parameter data assimilation|
|PEcAn.MA | [meta.analysis](https://pecanproject.github.io/pecan//modules/meta.analysis/inst/web/index.html)| [meta.analysis ](https://github.com/PecanProject/pecan/tree/master/modules/meta.analysis) | Trait assimilation workflow & hierarchical Bayes meta-analysis|
|PEcAn.photosynthesis | [photosynthesis](https://pecanproject.github.io/pecan//modules/photosynthesis/inst/web/index.html)|  [photosynthesis ](https://github.com/PecanProject/pecan/tree/master/modules/photosynthesis) | Hierarchical Bayes fitting of the Farquhar model |
|PEcAn.priors | [priors](https://pecanproject.github.io/pecan//modules/priors/inst/web/index.html) | [priors ](https://github.com/PecanProject/pecan/tree/master/modules/priors)  | tools for fitting priors to data and expert constraint|
|PEcAnRTM | [rtm](https://pecanproject.github.io/pecan//modules/rtm/inst/web/index.html)| [rtm ](https://github.com/PecanProject/pecan/tree/master/modules/rtm)   | Radiative Transfer Modeling tools; hierarchical Bayes inversion of PROSPECT |
|PEcAn.uncertainty | [uncertainty](https://pecanproject.github.io/pecan//modules/uncertainty/inst/web/index.html) | [uncertainty ](https://github.com/PecanProject/pecan/tree/master/modules/uncertainty) | Sensitivity and Uncertainty Analysis workflow. Variance Decomposition|
|**Model Wrappers** | | | 
|PEcAn.BIOCRO| [biocro](https://pecanproject.github.io/pecan//models/biocro/inst/web/index.html) | [biocro](https://github.com/PecanProject/pecan/tree/master/models/biocro)|
|PEcAn.CLM| [clm45](https://pecanproject.github.io/pecan//models/clm45/inst/web/index.html) |[clm45](https://github.com/PecanProject/pecan/tree/master/models/clm45) |
|PEcAn.DALEC| [dalec](https://pecanproject.github.io/pecan//models/dalec/inst/web/index.html) |[dalec](https://github.com/PecanProject/pecan/tree/master/models/dalec) |
|PEcAn.ED| [ed](https://pecanproject.github.io/pecan//models/ed/inst/web/index.html) | [ed](https://github.com/PecanProject/pecan/tree/master/models/ed)|
|PEcAn.GDAY| [gday](https://pecanproject.github.io/pecan//models/gday/inst/web/index.html) |[gday](https://github.com/PecanProject/pecan/tree/master/models/gday) |
|PEcAn.LINKAGES| [linkagers](https://pecanproject.github.io/pecan//models/linkages/inst/web/index.html) |[linkages](https://github.com/PecanProject/pecan/tree/master/models/linkages) |
|PEcAn.LPJGUESS| [lpjguess](https://pecanproject.github.io/pecan//models/lpjguess/inst/web/index.html) |[lpjguess](https://github.com/PecanProject/pecan/tree/master/models/lpjguess) |
|PEcAn.MAAT| [maat](https://pecanproject.github.io/pecan//models/maat/inst/web/index.html) |[maat](https://github.com/PecanProject/pecan/tree/master/models/maat) |
|PEcAn.MAESPA| [maespa](https://pecanproject.github.io/pecan//models/maespa/inst/web/index.html) |[maespa](https://github.com/PecanProject/pecan/tree/master/models/maespa) |
|PEcAn.PRELES| [preles](https://pecanproject.github.io/pecan//models/preles/inst/web/index.html) |[preles](https://github.com/PecanProject/pecan/tree/master/models/preles) |
|PEcAn.SIPNET| [sipnet](https://pecanproject.github.io/pecan//models/sipnet/inst/web/index.html) |[sipnet](https://github.com/PecanProject/pecan/tree/master/models/sipnet) |




### Beyond the Basics

If you are interested in learning more about configuring PEcAn to fit your usage needs, continue on to the [Advanced user section](../advanced_users_guide.md)


