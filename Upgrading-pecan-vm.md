Release notes for all releases can be found at: https://github.com/PecanProject/pecan/releases

This page will only list any steps you have to do to upgrade an existing system. When updating PEcAn it is highly encouraged to update BETY. You can find instructions on how to do this, as well on how to update the database in the [Updating BETYdb](https://github.com/PecanProject/bety/wiki/Updating-BETY) wiki page.

To upgrade to the latest version of PEcAn/VM you will need to do each of the intermediate steps, you can not skip to the latest version.

## Version 1.3.5 to 1.3.6

- update pecan/web/config.php, see config.example.php for all options
- pecan.xml has changed the database section, read.settings will take care of this automatically.
- models now uses dbfiles to store path to binaries, please add binaries to database. For example see pecan/scripts/addmodels.sh on how this is done in the VM.

## Version 1.3.4 to 1.3.5

- switch to postgreSQL, see [Installing-PEcAn](Installing-PEcAn) on how to load the database the first time
- sites/addsites.sh was incorrect, download sites.tgz
- web interface now uses config.php instead of system.php, simply copy pecan/web/config.example.php to pecan/web/config.php
- added a script to create hooks to prevent people from committing files as carya
- massive cleanup of repository. This breaks old repositories. Everybody will need to fork and clone a clean copy of pecan.

## Version 1.3.3 to 1.3.4

- To use HPC clusters you need to have a <qsub/> under run/host in pecan.xml
- The web needs to have the following added to system.php
```php
# List of allowed hosts
$hostlist=array(gethostname());
```

- PEcAn now uses PostgreSQL by default. To switch do the following:
```bash
# install postgresql
sudo -s
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -s -c`-pgdg main" > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
apt-get -y update
apt-get -y upgrade
apt-get -y install libdbd-pgsql postgresql postgresql-client php5-pgsql libpq-dev 
exit

# set ability to trust instead of peer for all
vi /etc/postgresql/9.3/main/pg_hba.conf 
/etc/init.d/postgresql restart

# create user
sudo -u postgres createuser -D -P -R -S bety
sudo -u postgres createdb -O bety bety 

# download database
pecan/scripts/update.psql.sh 
```

To force pecan to use a specific database add <driver> to the database section. For MySQL add <driver>MySQL</driver> and for PostgreSQL add <driver>PostgreSQL</driver>

### Changes

* Support for HPC
* Use PostgreSQL as default

## Version 1.3.2 to 1.3.3 

```bash
sudo apt-get install libgdal1-dev libgdal-dev libgeos-c1 libspatialite3
```

```r
install.packages("rgdal")
```

## Version 1.3.1 to Version 1.3.2

d### Changes

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

apt-get install ruby1.9.3 rubygems ruby-switch passenger-common1.9.1
apt-get dist-upgrade
ruby-switch --set ruby1.9.1
```

Now we can update BETY
```bash
cd bety
sudo gem install bundler
sudo git pull
sudo bundle install
```

Finally either download the latest version of the database using the updatedb.sh script or run the following command to keep the current installed database and update it to the latest version
```bash
sudo rake db:migrate RAILS_ENV="production"
```