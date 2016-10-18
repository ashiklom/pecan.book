# State Data Assimilation 

## This function is housed within: **/pecan/modules/assim.sequential/R**

### **sda.enkf.R**
This is the main ensemble Kalman filter and generalized filter code. Originally, this was just ensemble Kalman filter code. Mike Dietze and Ann Raiho added a generalized ensemble filter to avoid filter divergence.

The main parts of the function are:

Setting up run:

* read settings
  
* load climate data

* open database connection

* get new workflow ids
  
* create ensemble ids

* set up model specific functions

Performing the initial set of runs (spinup)

Set up for data assimilation

Loop over time

* read.restart - read netcdfs corresponding to year that you want to assimilate data into
  
* analysis - if observation is available run either kalman filter or generalized ensemble filter
  
* write.restart - write model specific write.configs to run model with state variables from analysis
  
* run model
  
Save outputs

Create diagnostics

### State Data Assimilation Tags

```
<state.data.assimilation>
	<process.variance>FALSE</process.variance>
  <sample.parameters>FALSE</sample.parameters>
  <state.variables>
   <variable>AGB.pft</variable>
   <variable>TotSoilCarb</variable>
  </state.variables>
  <spin.up>
  	<start.date>2004/01/01</start.date>
	  <end.date>2006/12/31</end.date>
  </spin.up>
  <forecast.time.step>1</forecast.time.step>
	<start.date>2004/01/01</start.date>
	<end.date>2006/12/31</end.date>
</state.data.assimilation>
```

* **process.variance** : [optional] TRUE/FLASE flag for if process variance should be estimated (TRUE) or not (FALSE). If TRUE, a generalized ensemble filter will be used. If FALSE, an ensemble Kalman filter will be used. Default is FALSE.
* **sample.parameters** : [optional] TRUE/FLASE flag for if parameters should be sampled for each ensemble member or not. This allows for more spread in the intial conditions of the forecast.
* **_NOTE:_** If TRUE, you must also assign a vector of trait names to pick.trait.params within the sda.enkf function.
* **state.variable** : [required] State variable that is to be assimilated (in PEcAn standard format). Default is "AGB" - Above Ground Biomass.
* **spin.up** : [required] start.date and end.date for model spin up.
* **_NOTE:_** start.date and end.date are distinct from values set in the run tag because spin up can be done over a subset of the run.
* **forecast.time.step** : [optional] start.date and end.date for model spin up.
* **start.date** : [required?] start date of the state data assimilation (in YYYY/MM/DD format) 
* **end.date** : [required?] end date of the state data assimilation (in YYYY/MM/DD format)
* **_NOTE:_** start.date and end.date are distinct from values set in the run tag because this analysis can be done over a subset of the run.

### **read.restart.MODEL.R**
This model specific function reads the netcdfs of a specific year. Takes out the applicable state variable for state data assimilation, and give it to a matrix in sda.enkf.R

### **write.restart.MODEL.R**
This model specific function takes in an analysis matrix from sda.enkf.R and translates the state variables back to the model variables. Then, writes a write.config.MODEL to restart the model.

### The Generalized Ensemble Filter
An ensemble filter is a sequential data assimilation algorithm with two procedures at every time step: a forecast followed by an analysis. The forecast ensembles arise from a model while the analysis makes an adjustment of the forecasts ensembles from the model towards the data. An ensemble Kalman filter is typically suggested for this type of analysis because of its computationally efficient analytical solution and its ability to update states based on an estimate of covariance structure. But, in some cases, the ensemble Kalman filter fails because of filter divergence. Filter divergence occurs when forecast variability is too small, which causes the analysis to favor the forecast and diverge from the data. Models often produce low forecast variability because there is little internal stochasticity. Our ensemble filter overcomes this problem in a Bayesian framework by including an estimation of model process variance. This methodology also maintains the benefits of the ensemble Kalman filter by updating the state vector based on the estimated covariance structure.

This process begins after the model is spun up to equilibrium.

The likelihood function uses the data vector $\left(\boldsymbol{y_{t}}\right)$ conditional on the estimated state vector $\left(\boldsymbol{x_{t}}\right)$ such that
  
 $\boldsymbol{y}_{t}\sim\mathrm{multivariate\:normal}(\boldsymbol{x}_{t},\boldsymbol{R}_{t})$
 
where $\boldsymbol{R}_{t}=\boldsymbol{\sigma}_{t}^{2}\boldsymbol{I}$ and $\boldsymbol{\sigma}_{t}^{2}$ is a vector of data variances. To obtain an estimate of the state vector $\left(\boldsymbol{x}_{t}\right)$, we use a process model that incorporates a process covariance matrix $\left(\boldsymbol{Q}_{t}\right)$. This process covariance matrix differentiates our methods from past ensemble filters. Our process model contains the following equations

$\boldsymbol{x}_{t}	\sim	\mathrm{multivariate\: normal}(\boldsymbol{x}_{model_{t}},\boldsymbol{Q}_{t})$

$\boldsymbol{x}_{model_{t}}	\sim	\mathrm{multivariate\: normal}(\boldsymbol{\mu}_{forecast_{t}},\boldsymbol{P}_{forecast_{t}})$

where $\boldsymbol{\mu}_{forecast_{t}}$ is a vector of means from the ensemble forecasts and $\boldsymbol{P}_{forecast_{t}}$ is a covariance matrix calculated from the ensemble forecasts. The prior for our process covariance matrix is $\boldsymbol{Q}_{t}\sim\mathrm{Wishart}(\boldsymbol{V}_{t},n_{t})$ where $\boldsymbol{V}_{t}$ is a scale matrix and $n_{t}$ is the degrees of freedom. The prior shape parameters are updated at each time step through moment matching such that

$\boldsymbol{V}_{t+1}	=	n_{t}\bar{\boldsymbol{Q}}_{t}$

$n_{t+1}	=	\frac{\sum_{i=1}^{I}\sum_{j=1}^{J}\frac{v_{ijt}^{2}+v_{iit}v_{jjt}}{Var(\boldsymbol{\bar{Q}}_{t})}}{I\times J}$

where we calculate the mean of the process covariance matrix $\left(\bar{\boldsymbol{Q}_{t}}\right)$ from the posterior samples at time t. Degrees of freedom for the Wishart are typically calculated element by element where $v_{ij}$ are the elements of $\boldsymbol{V}_{t}$. $I$ and $J$ index rows and columns of $\boldsymbol{V}$. Here, we calculate a mean number of degrees of freedom for $t+1$ by summing over all the elements of the scale matrix $\left(\boldsymbol{V}\right)$ and dividing by the count of those elements $\left(I\times J\right)$. We fit this model sequentially through time in the R computing environment using R package 'rjags.'
 
 
