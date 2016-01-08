### Getting Started: Background

The quickest way to get started is to download the virtual machine. The demo provides instructions to get started on your own machine. 
 * [Download Virtual Machine](http://opensource.ncsa.illinois.edu/projects/artifacts.php?key=PECAN) and optionally convert VM to [[Desktop Machine | VM-Desktop-Conversion]]
 * [Work through Demo](http://pecanproject.github.io/tutorials.html)

### Using PEcAn

#### Site-level Runs
The most basic function of PEcAn is to make it easier to run models at different sites and evaluate their outputs. The [Demo](http://pecanproject.github.io/tutorials.html) describes the basics of running the model at an existing site. The following links walk through not only this, but also how to set up new sites and new model inputs so that you can extend PEcAn to your own research.

* [[Choose a site]]
* [[Choose a model]]
* [[Choosing meteorology]]
* [[Choosing PFTs]]
* [[Choosing initial vegetation]]
* [[Choosing soils]]

### Additional PEcAn Modules and Tools
| Package name | documentation | source code | description |
|:---|:---|:---|:---|
|**Utilities**| | |
|PEcAn.all| [all](https://pecanproject.github.io/pecan//all/inst/web/index.html) | [all](https://github.com/PecanProject/pecan/tree/master/all)  | Wrapper to facilitate loading of core packages|
| PEcAn.utils | [utils](https://pecanproject.github.io/pecan//utils/inst/web/index.html)| [utils](https://github.com/PecanProject/pecan/tree/master/utils)  | Utility functions used by many PEcAn packages|
| PEcAn.DB| [db](https://pecanproject.github.io/pecan//db/inst/web/index.html)|[db](https://github.com/PecanProject/pecan/tree/master/db)  |Interface to database containing data, provenance, and results|
|PEcAn.settings| [settings](https://pecanproject.github.io/pecan//settings/inst/web/index.html)| [settings](https://github.com/PecanProject/pecan/tree/master/settings)  | Package to read PEcAn settings files|
|**Workflow Modules**| | |
|PEcAn.visualization| [visualization](https://pecanproject.github.io/pecan//visualization/inst/web/index.html)| ||
|PEcAn.priors | [priors](https://pecanproject.github.io/pecan//modules/priors/inst/web/index.html) | [priors ](https://github.com/PecanProject/pecan/tree/master/modules/priors)  | tools for fitting priors to data and expert constraint|
|PEcAn.MA | [meta.analysis](https://pecanproject.github.io/pecan//modules/meta.analysis/inst/web/index.html)| [meta.analysis ](https://github.com/PecanProject/pecan/tree/master/modules/meta.analysis) | Trait assimilation workflow & hierarchical Bayes meta-analysis|
|PEcAn.allometry | |  [allometries ](https://github.com/PecanProject/pecan/tree/master/modules/allometry)  | Bayesian synthesis of allometric equations|
|PEcAn.benchmark| |  [benchmark ](https://github.com/PecanProject/pecan/tree/master/modules/benchmark)  | Model benchmarking
|PEcAn.uncertainty | [uncertainty](https://pecanproject.github.io/pecan//modules/uncertainty/inst/web/index.html) | [uncertainty ](https://github.com/PecanProject/pecan/tree/master/modules/uncertainty) | Sensitivity and Uncertainty Analysis workflow. Variance Decomposition|
|PEcAn.data.land| [data.land](https://pecanproject.github.io/pecan//modules/data.land/inst/web/index.html)| [data.land ](https://github.com/PecanProject/pecan/tree/master/modules/data.land/R) | Tools for land and vegetation data. At the moment predominantly Bayesian state-space tree-ring models.|
|PEcAn.data.atmosphere| [data.atmosphere](https://pecanproject.github.io/pecan//modules/data.atmosphere/inst/web/index.html)| [data.atmosphere ](https://github.com/PecanProject/pecan/tree/master/modules/data.atmosphere) | Meteorology workflow|
|PEcAn.assim.batch| [assim.batch](https://pecanproject.github.io/pecan//modules/assim.batch/inst/web/index.html) | [assim.batch ](https://github.com/PecanProject/pecan/tree/master/modules/assim.batch) | Batch data assimilation tools (e.g. MCMC)
|PEcAn.assim.sequential| [assim.sequential](https://pecanproject.github.io/pecan//modules/assim.sequential/inst/web/index.html)| [assim.sequential ](https://github.com/PecanProject/pecan/tree/master/modules/assim.sequential) | Sequential data assimilation tools (e.g. EnKF, particle filter)
|PEcAn.photosynthesis | |  [photosynthesis ](https://github.com/PecanProject/pecan/tree/master/modules/photosynthesis) | Hierarchical Bayes fitting of the Farquhar model |
|PEcAnRTM | | [rtm ](https://github.com/PecanProject/pecan/tree/master/modules/rtm)   | Radiative Transfer Modeling tools; hierarchical Bayes inversion of PROSPECT |
|**Model Wrappers** | | | 
|PEcAn.ED| [ed](https://pecanproject.github.io/pecan//models/ed/inst/web/index.html) | |
|PEcAn.SIPNET| [sipnet](https://pecanproject.github.io/pecan//models/sipnet/inst/web/index.html) | |
|PEcAn.BIOCRO| [biocro](https://pecanproject.github.io/pecan//models/biocro/inst/web/index.html) | |

### Supported Models

The following is a list of supported models. See [[Adding an Ecosystem Model]] for info on how to add new models to the system and don't hesitate to contact us if you need help. That said, the PEcAn team is not experts in all the models linked to PEcAn, so if there are issues with models themselves you would be encouraged to contact the original modeling teams.

* [Ecosystem Demography model](https://github.com/EDmodel/ED2)
* [SIPNET](http://thesipnetmodel.blogspot.com/)
* [DALEC](http://www.geos.ed.ac.uk/homes/mwilliam/DALEC.html)
* [BioCro](https://github.com/ebimodeling/biocro)
* [LINKAGES](http://daac.ornl.gov/MODELS/guides/LINKAGES.html)

### Beyond the Basics

The following links describe more of the details of how PEcAn works

* [[Settings Files| PEcAn-Configuration]]: what goes in the PEcAn settings files (e.g. `pecan.xml`) and how to gain additional control over more advanced PEcAn features and analyses.
* [[Connecting PEcAn to a remote server]]
* [[Troubleshooting PEcAn]]: solutions to known gotchas 
* [[Upgrading PEcAn/VM]] and/or installing it on your own server
* [[Database Synchronization]] Retrieving latest updates and sharing your changes
* [[Updating BETY]]: handling database schema updates 
* [[Workflow Modules]]

### Reporting Bugs and Requesting Features

* GitHub
 * [[Reporting Bugs | GitHub Issues]]
 * [[Requesting Features | GitHub Issues ]]
* HipChat
 * [PEcAn Chatroom](https://hipchat.ncsa.illinois.edu/gW51EFhtT)