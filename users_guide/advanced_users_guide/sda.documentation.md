# State Data Assimilation 

## This function is housed within: **/pecan/modules/assim.sequential/R**

### **sda.enkf.R**
This is the main ensemble Kalman filter and generalized filter code. Originally, this was just ensemble Kalman filter code. Mike Dietze and Ann Raiho added a generalized filter to avoid filter divergence.

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
  <state.variable>AGB.pft</state.variable>
  <state.variable>TotSoilCarb</state.variable>
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
