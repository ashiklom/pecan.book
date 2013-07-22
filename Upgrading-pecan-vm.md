To upgrade to the latest version of PEcAn/VM you will need to do each of the intermediate steps, you can not skip to the latest version.

## Version 1.3.1 to Version 1.3.2

### Changes

* [PEcAn/BETY] support for postgresql
* [PEcAn] store results from get.traits and run.meta.analysis for reuse
* [PEcAn] ED/biocro templates have moved to models/inst folders from web folder
* [PEcAn] better checks of pecan.xml file
* [PEcAn] separate variables can be supplied for ensembles and sensitivity analysis
* [PEcAn] fixes to mstmip output
* [BETY] complete redesign of interface

### Updating PEcAn

First install some new packages
```bash
sudo apt-get install udunits-bin libudunits2-dev
```

Update PEcAn
```bash
cd pecan
git pull
R --vanilla < scripts/install.dependencies.R
./scripts/build.sh
```

### Updating BETY

BETY now requires RUBY 1.9 following are the instructions to get RUBY on an Ubuntu server
```bash
rm /etc/apt/sources.list.d/brightbox-passenger-precise.list 
apt-get install python-software-properties
apt-add-repository ppa:brightbox/ruby-ng
apt-get update

apt-get install ruby1.9.3 rubygems ruby-switch passenger-common1.9.1 bundler
apt-get dist-upgrade
ruby-switch --set ruby1.9.1
```

Now we can update BETY
```bash
cd bety
sudo git pull
sudo bundle install
```

Finally either download the latest version of the database using the updatedb.sh script or run the following command to keep the current installed database and update it to the latest version
```bash
sudo rake db:migrate RAILS_ENV="production"
```