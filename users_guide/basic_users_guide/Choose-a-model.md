1. On the **Select Host** webpage use the Host pull-down menu to select the server you want to run on. Note that at the moment only the LOCAL server will run through the web-based workflow.

2. Next, select the model you want to run under the Model pull-down menu

3. If the model you want to run is not listed login to BETY and navigate to Runs > Models.

4. If there are already entries for the model you want to run then most likely the PEcAn modules for that model have been implemented but PEcAn is unaware that the model has been installed on your server. Information on adding a model executable to the PEcAn database can be found [here](../../developers_guide/Adding-an-Ecosystem-Model.md#model). After this is done, your model should appear on the PEcAn **Select Host** page after your refresh the page.

5. If there a no entries for the model you want to run then most likely the PEcAn modules need to be implemented. See the instructions at Adding an Ecosystem Model

6. If selecting your model causes your site to disappear from the Google Map then the site exists but there are no drivers for that site registered in the database. See the instruction [here](Choose-a-site.md#my-site-shows-up-when-i-dont-have-any-model-selected-but-disappears-once-i-select-the-model-i-want-to-run) for more info on diagnosing what drivers are missing.

