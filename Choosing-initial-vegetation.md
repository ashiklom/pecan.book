On the Input Selection webpage, in addition to selecting PFTs, start & end dates, and meteorology, many models also require some way of specifying the initial conditions for the vegetation, which may range from setting the aboveground biomass and LAI up to detailed inventory-like data on species composition and stand structure.

At the moment, PEcAn has two cases for initial conditions and they only exist as required Inputs for the ED2 model:

If files already exist in the database, they can simply be selected from the menu. For ED2, there are 3 different veg files (site, pss, css) and it is important that you select a complete set, not mix and match.

If files don't exist they can be uploaded following the instructions on [[How to insert new Input data]]. Information on the ED2-specific format is located [here](https://github.com/EDmodel/ED2/wiki/Initial-conditions)

Two additional options are in development:

* Model spin-up
* Automated workflows

## Spin up

A number of ecosystem models are typically initialized by spinning up to steady state. At the moment PEcAn doesn't handle spin up automatically (e.g. looping met, checking for stability), but there are various ways to achieve a spin-up within the system. First, if there are model-specific settings in a model's settings/config file, then that file can be accessed by clicking on the **Edit model config** check box. If this box is selected then PEcAn will pause the site run workflow after it has generated your model config file, but before it runs the model, and give you an opportunity to edit the file 

## Veg workflow

