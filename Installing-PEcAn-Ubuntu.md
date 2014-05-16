# Ubuntu

These are specific notes for installing PEcAn on Ubuntu and will be referenced from the main [installing PEcAn](Installing-PEcAn) page. You will at least need to install the build environment and Postgres sections. If you want to access the database/PEcAn using a web browser you will need to install Apache. To access the database using the BETY interface, you will need to have Ruby installed.

This document also contains information on how to install the Rstudio server edition as well as any other packages that can be helpful.

## Install build environment

```bash
sudo -s

# point to latest R
echo "deb http://cran.rstudio.com/bin/linux/ubuntu `lsb_release -s -c`/" > /etc/apt/sources.list.d/R.list
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9

# update package list
apt-get -y update

# install all packages needed
apt-get -y install build-essential git gfortran openmpi-bin libhdf5-openmpi-dev r-base-core jags liblapack-dev libnetcdf-dev netcdf-bin bc libcurl4-openssl-dev curl udunits-bin libudunits2-dev libsqlite3-dev libgmp-dev 

# this needs to be done separately from previous command
apt-get -y install libgdal1-dev libproj-dev

# install packages for webserver
apt-get -y install apache2 libapache2-mod-php5 php5

# install packages to compile docs
apt-get -y install texinfo texlive-latex-base texlive-latex-extra texlive-fonts-recommended

# install devtools
echo 'install.packages("devtools", repos="http://cran.rstudio.com/")' | R --vanilla

# done as root
exit
```

## Install Postgres

```bash
# point to latest PostgreSQL
echo "deb http://apt.postgresql.org/pub/repos/apt `lsb_release -s -c`-pgdg main" > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

# update package list
apt-get -y update

# install packages for postgresql (using a newer version than default)
apt-get -y install libdbd-pgsql postgresql postgresql-client php5-pgsql libpq-dev 

# set ability to trust instead of peer for all
vi /etc/postgresql/9.3/main/pg_hba.conf 
/etc/init.d/postgresql restart
```

## Apache Configuration

```bash
# become root
sudo -s

# get index page
rm /var/www/index.html
ln -s ${HOME}/pecan/documentation/index_vm.html /var/www/index.html

# setup a redirect
cat > /etc/apache2/conf.d/pecan.conf << EOF
Alias /pecan ${HOME}/pecan/web
<Directory ${HOME}/pecan/web>
  DirectoryIndex index.php
  Options +All
  Order allow,deny
  Allow from all
</Directory>
EOF
/etc/init.d/apache2 restart

# done as root
exit
```

## Ruby

```bash
sudo apt-get -y install python-software-properties
sudo apt-add-repository -y ppa:brightbox/ruby-ng
sudo apt-get update

# install all ruby related packages
sudo apt-get -y install ruby1.9.3 rubygems ruby-switch passenger-common1.9.1 libapache2-mod-passenger 
```

## Rstudio-server
