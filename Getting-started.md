### Getting Started: Background

The quickest way to get started is to download the virtual machine. The demo provides instructions to get started on your own machine. 
 * [Download Virtual Machine](http://isda.ncsa.illinois.edu/download/minimal.php?project=PEcAn&category=vm) and optionally convert VM to [[Desktop Machine | VM-Desktop-Conversion]]
 * [Work through Demo](http://pecanproject.github.io/tutorials.html)

### Site-level Runs

The most basic function of PEcAn is to make it easier to run models at different sites and evaluate their outputs. The [Demo](http://pecanproject.github.io/tutorials.html) describes the basics of running the model at an existing site. The following links walk through not only this, but also how to set up new sites and new model inputs so that you can extend PEcAn to your own research.

* [[Choose a site]]
* [[Choose a model]]
* [[Choosing meteorology]]
* [[Choosing PFTs]]
* [[Choosing initial vegetation]]
* [[Choosing soils]]

### Additional PEcAn Modules and Tools

R Package documentation for the PEcAn modules can be found [here](http://pecanproject.github.io/pecan/)

* [allometries Module](https://github.com/PecanProject/pecan/tree/master/modules/allometry) - Bayesian synthesis of allometric equations
* [assim.batch Module](https://github.com/PecanProject/pecan/tree/master/modules/assim.batch) - Batch data assimilation tools (e.g. MCMC)
* [assim.sequential Module](https://github.com/PecanProject/pecan/tree/master/modules/assim.sequential) - Sequential data assimilation tools (e.g. EnKF, particle filter)
* [benchmark Module](https://github.com/PecanProject/pecan/tree/master/modules/benchmark) - Model benchmarking
* [data.atmosphere Module](https://github.com/PecanProject/pecan/tree/master/modules/data.atmosphere) - Meteorology workflow
* [data.land Module](https://github.com/PecanProject/pecan/tree/master/modules/data.land/R) - Tools for land and vegetation data. At the moment predominantly Bayesian state-space tree-ring models.
* [meta.analysis Module](https://github.com/PecanProject/pecan/tree/master/modules/meta.analysis) - Trait assimilation workflow & hierarchical Bayes meta-analysis
* [photosynthesis Module](https://github.com/PecanProject/pecan/tree/master/modules/photosynthesis) - Hierarchical Bayes fitting of the Farquhar model
* [priors Module](https://github.com/PecanProject/pecan/tree/master/modules/priors) - tools for fitting priors to data
* [rtm Module](https://github.com/PecanProject/pecan/tree/master/modules/rtm) - Radiative Transfer Modeling tools. At the moment predominantly a hierarchical Bayes inversion of PROSPECT
* [uncertainty Module](https://github.com/PecanProject/pecan/tree/master/modules/uncertainty) - Sensitivity and Uncertainty Analysis workflow. Variance Decomposition

### Supported Models

The following is a list of supported models. See [[Adding an Ecosystem Model]] for info on how to add new models to the system and don't hesitate to contact us if you need help. That said, the PEcAn team is not experts in all the models linked to PEcAn, so if there are issues with models themselves you would be encouraged to contact the original modeling teams.

* [Ecosystem Demography model](https://github.com/EDmodel/ED2)
* [SIPNET](http://thesipnetmodel.blogspot.com/)
* [DALEC](http://www.geos.ed.ac.uk/homes/mwilliam/DALEC.html)
* [BioCro](https://github.com/dlebauer/biocro)
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
