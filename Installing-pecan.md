We recommend that new users download the PEcAn ["pecan.ova"](http://isda.ncsa.illinois.edu/download/index.php?project=PEcAn&sort=version) instead of creating from one from scratch. These instructions are provided to document how to install PEcAn on different Operating Systems.

## Install build environment

### Linux Ubuntu

```bash
# point R to personal lib folder
echo 'export R_LIBS_USER=${HOME}/R/library' >> ~/.bashrc
source ~/.bashrc
mkdir -p ${R_LIBS_USER}

sudo -s

# point to latest R
echo "deb http://cran.rstudio.com/bin/linux/ubuntu `lsb_release -s -c`/" > /etc/apt/sources.list.d/R.list
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
apt-get -y update

# install all packages needed
apt-get -y install build-essential git gfortran openmpi-bin libhdf5-openmpi-dev r-base-core default-jre libdbd-mysql libmysqlclient-dev mysql-server mysql-client jags r-cran-rjags r-cran-xml r-cran-hdf5 r-cran-mass r-cran-rmysql liblapack-dev libnetcdf-dev netcdf-bin texlive-latex-base texlive-latex-extra texlive-fonts-recommended bc libcurl4-openssl-dev texinfo curl apache2 libapache2-mod-php5 php5 php5-mysql libgdal1-dev libproj-dev udunits-bin libudunits2-dev
# install devtools
echo 'install.packages("devtools", repos="http://cran.rstudio.com/")' | R --vanilla

# done as root
exit
```

### CentOS / RHEL

#### Install and configure MySQL

[Reference: centoshelp.org](http://centoshelp.org/servers/database/installing-configuring-mysql-server/)

```bash
yum -y install git R mysql mysql-server
chkconfig --level 2345 mysqld on 
service mysqld start
```

#### Install and start Apache

```bash
yum -y install httpd
sudo service httpd start
```

#### Install PHP

```bash
sudo yum install php php-mysql
```

### Mac OSX

#### Install R, Fortran, OpenMPI and HDF5

```bash
# install R
# download from http://cran.r-project.org/bin/macosx/

# install gfortran 
# download from http://hpc.sourceforge.net
sudo tar -xvf ~/Downloads/gcc-mlion.tar -C /

# install OpenMPI
curl -o openmpi-1.6.3.tar.gz http://www.open-mpi.org/software/ompi/v1.6/downloads/openmpi-1.6.3.tar.gz
tar zxf openmpi-1.6.3.tar.gz
cd openmpi-1.6.3
./configure --prefix=/usr/local
make all
sudo make install
cd ..

# install szip
curl -o szip-2.1-MacOSX-intel.tar.gz ftp://ftp.hdfgroup.org/lib-external/szip/2.1/bin/szip-2.1-MacOSX-intel.tar.gz
tar zxf szip-2.1-MacOSX-intel.tar.gz
sudo mv szip-2.1-MacOSX-intel /usr/local/szip

# install HDF5
curl -o hdf5-1.8.10-patch1tar.gz http://www.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.8.10-patch1tar.gz
tar zxf hdf5-1.8.10-patch1tar.gz
cd hdf5-1.8.10-patch1
sed -i -e 's/-O3/-O0/g' config/gnu-flags 
./configure --prefix=/usr/local/hdf5 --enable-fortran --enable-cxx --with-szlib=/usr/local/szip
make
# make check
sudo make install
# sudo make check-install
cd ..
```

#### Install MySQL

I use the package that comes from mysql (http://dev.mysql.com/downloads/mysql/) and install the package. And add the mysql/bin folder to your path

```bash
sudo bash -c 'echo "/usr/local/mysql/bin" > /etc/paths.d/mysql'
```

#### Install JAGS

For more instructions see http://martynplummer.wordpress.com/2011/11/04/rjags-3-for-mac-os-x/

#### Install tgp

**This is not needed anymore but kept for reference**

This package is source only and needs to be installed first.

```bash
echo 'install.packages("tgp", type="both", repos="http://cran.rstudio.com/")' | R --vanilla
```

## Install models

### Model Testrun data

These are large-ish files that contain data used with ED2 and SIPNET

```bash
rm -rf sites
curl -o sites.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/sites.tgz
tar zxf sites.tgz
sed -i -e "s#/home/kooper/projects/EBI#${PWD}#" sites/*/ED_MET_DRIVER_HEADER
rm sites.tgz

rm -rf inputs
curl -o inputs.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/inputs.tgz
tar zxf inputs.tgz
rm inputs.tgz
```

### ED 2.2 r46 (used in PEcAn manuscript)

```bash
# ----------------------------------------------------------------------
# Get version r46 with a few patches for ubuntu
curl -o ED.r46.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/ED.r46.tgz
tar zxf ED.r46.tgz
rm ED.r46.tgz
# ----------------------------------------------------------------------
# configure and compile ed
cd ~/ED.r46/ED/build/bin
curl -o include.mk.opt http://isda.ncsa.illinois.edu/~kooper/EBI/include.mk.opt.`uname -s`
./install.sh
sudo cp ../ed_2.1-opt /usr/local/bin/ed2.r46
cd
```

Perform a test run using pre configured ED settings for ED2.2 r46

```bash
# ----------------------------------------------------------------------
# Create sample run
cd
mkdir testrun.ed.r46
cd testrun.ed.r46
curl -o ED2IN http://isda.ncsa.illinois.edu/~kooper/EBI/ED2IN.r46
sed -i -e "s#\$HOME#$HOME#" ED2IN
curl -o config.xml  http://isda.ncsa.illinois.edu/~kooper/EBI/config.r46.xml
# execute test run
time ed2.r46
```

### ED 2.2 r82

```bash
curl -o ED.r82.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/ED.r82.tgz
tar zxf ED.r82.tgz
rm ED.r82.tgz

cd ED.r82
curl -o ED.r82.patch http://isda.ncsa.illinois.edu/~kooper/EBI/ED.r82.patch
patch -p1 < ED.r82.patch
cd ED/build/bin
curl -o include.mk.opt http://isda.ncsa.illinois.edu/~kooper/EBI/include.mk.opt.`uname -s`.r82
./install.sh
sudo cp ../ed_2.1-opt /usr/local/bin/ed2.r82
cd
```

Perform a test run using pre configured ED settings for ED2.2 r82

```bash
cd
mkdir testrun.ed.r82
cd testrun.ed.r82
curl -o ED2IN http://isda.ncsa.illinois.edu/~kooper/EBI/ED2IN.r82
sed -i -e "s#\$HOME#$HOME#" ED2IN
curl -o config.xml  http://isda.ncsa.illinois.edu/~kooper/EBI/config.r82.xml
# execute test run
time ed2.r82
```

### SIPNET Installation

```bash
cd
curl -o sipnet_unk.tar.gz http://isda.ncsa.illinois.edu/~kooper/EBI/sipnet_unk.tar.gz
tar zxf sipnet_unk.tar.gz
rm sipnet_unk.tar.gz

cd sipnet_unk
make
sudo cp sipnet /usr/local/bin/sipnet.runk
cd
```

SIPNET testrun

```bash
cd
curl -o testrun.sipnet.tar.gz http://isda.ncsa.illinois.edu/~kooper/EBI/testrun.sipnet.tar.gz
tar zxf testrun.sipnet.tar.gz
rm testrun.sipnet.tar.gz
cd testrun.sipnet
sipnet.runk
```

### Installing BioCro

```bash
echo 'devtools::install_github("biocro", username = "dlebauer")' | R --vanilla
```


## Installing BETY

There are two flavors of BETY, PHP and RUBY. The PHP version allows for a minimal interaction with the database while the RUBY version allows for full interaction with the database. Both however require the database to be created and installed

### Database creation

The following creates the user, database and populates the database with the latest version from Illinois.

```bash
# needs to be done only once
mysql -u root -p -e "grant all on bety.* to bety@localhost identified by 'bety';"
curl -o ${HOME}/updatedb.sh http://isda.ncsa.illinois.edu/~kooper/EBI/updatedb.sh
chmod 755 ${HOME}/updatedb.sh

# download and update/install database
${HOME}/updatedb.sh
```

### PHP version

The php version comes with PEcAn and should be accessible from http://<host>:<port>/pecan/db/ once PEcAn is installed.

### RUBY version

**THESE INSTRUCTIONS ARE OUT OF DATE! BETY NOW REQUIRES RUBY 1.9**

The RUBY version requires a few extra packages to be installed first.

```bash
# passenger 3 repository
apt-get install python-software-properties
apt-add-repository ppa:brightbox/passenger
apt-get update

# install all ruby related packages
apt-get -y install ruby1.8 ruby1.8-dev rubygems1.8 librmagick-ruby1.8 libmysql-ruby1.8 libapache2-mod-passenger imagemagick libmagickwand-dev libmagic-dev libxslt1-dev libmysqlclient-dev libnetcdf-dev libsqlite3-dev
```

Next we install the web app.

```bash
sudo -s

# install bety
cd /usr/local
git clone https://github.com/PecanProject/bety.git

# install gems
cd bety
gem install bundler
bundle install
exit
```

and configure BETY

```bash
sudo -s
cd /usr/local/bety

# create folders for upload folders
mkdir paperclip/files paperclip/file_names
chmod 777 paperclip/files paperclip/file_names

# create folder for log files
mkdir log
touch log/production.log
chmod 0666 log/production.log

# fix configuration for vm
cp config/additional_environment_vm.rb config/additional_environment.rb

# setup bety database configuration
cat > config/database.yml << EOF
production:
  adapter: mysql2
  encoding: latin1
  reconnect: false
  database: bety
  pool: 5
  username: bety
  password: bety
EOF

# setup login tokens
cat > config/initializers/site_keys.rb << EOF
REST_AUTH_SITE_KEY         = 'thisisnotasecret'
REST_AUTH_DIGEST_STRETCHES = 10
EOF

# configure apache
ln -s /usr/local/bety/public /var/www/bety

cat > /etc/apache2/conf.d/bety << EOF
RailsEnv production
RailsBaseURI /bety
<Directory /var/www/bety>
   Options FollowSymLinks
   AllowOverride None
   Order allow,deny
   Allow from all
</Directory>
EOF
/etc/init.d/apache2 restart

# drop out of root
exit
```

## PEcAn Installation

PEcAn is the software package that ties all the pieces together.

## Installing PEcAn

Download, compile and install PEcAn

```bash
# download pecan
cd
git clone https://github.com/PecanProject/pecan.git

# install PEcAn packages in R
cd pecan
R --vanilla < scripts/install.dependencies.R

# compile pecan
./scripts/build.sh
```

### PEcAn Testrun

Do the run, this assumes you have installed the BETY database, sites tar file and sipnet.

```bash
# create folder
cd
mkdir testrun.pecan
cd testrun.pecan

# download example of pecan workflow and configuration file
wget -O pecan.xml http://isda.ncsa.uiuc.edu/~kooper/EBI/pecan.xml
wget -O workflow.R http://isda.ncsa.uiuc.edu/~kooper/EBI/workflow.R

# exectute workflow
rm -rf pecan
R --vanilla < workflow.R
```
NB: pecan.xml is configured for the virtual machine, you will need to change the <met> field from '/home/carya/' to wherever you installed your 'sites', usually $HOME

### Configuring PEcAn Web Interface

#### Apache Configuration for Linux Ubuntu 

```bash
# bceome root
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

#### Apache Configuration for Mac OSX

```bash
sudo sed -i '' 's/^#LoadModule php5_module/LoadModule php5_module/' /etc/apache2/httpd.conf

sudo mkdir /var/mysql
sudo ln -s /tmp/mysql.sock /var/mysql/mysql.sock

cat > /etc/apache2/users/pecan.conf << EOF
Alias /pecan ${PWD}/pecan/web
<Directory ${PWD}/pecan/web>
  DirectoryIndex index.php
  Options +All
  Order allow,deny
  Allow from all
</Directory>
EOF
```

#### Instructions for all OS

```bash
# configure for web app
cat > ${HOME}/pecan/web/system.php << EOF
<?php

# Information to connect to the database
\$db_hostname="localhost";
\$db_username="bety";
\$db_password="bety";
\$db_database="bety";

# Folder where PEcAn is installed
\$pecan_install="${R_LIBS_USER}";

# Location where PEcAn is installed, not really needed anymore
\$pecan_home="${HOME}/pecan/";

# Folder where the runs are stored
\$output_folder="${HOME}/output/";

# ED specific inputs, should come from database
\$ed_veg="${HOME}/oge2OLD/OGE2_";
\$ed_soil="${HOME}/faoOLD/FAO_";
\$ed_inputs="${HOME}/ed_inputs/";
?>
EOF

cp ${HOME}/pecan/web/db/config_example.php ${HOME}/pecan/web/db/config.php
```

All done you can now visit the server http://\<hostname>:\<port>/pecan' and you can interact with the database using 'http://\<hostname>:\<port>/pecan/db/'


## Addiotional datasets

### FIA database

FIA database is large and will add an extra 10GB to the installation.

```bash
# needs to be done only once
mysql -u root -p -e "grant all on fia5data.* to bety@localhost identified by 'bety';" 

# download and update/install database
wget http://isda.ncsa.illinois.edu/~kooper/EBI/fiadb.sql
mysql -u bety -p"bety" -e 'drop database if exists fia5data; create database fia5data;'
mysql -u bety -p"bety" fia5data < fiadb.sql
rm fiadb.sql
```

### Flux Camp

Following will install the data for flux camp (as well as the demo script for PEcAn).
```bash
cd
wget -O plot.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/plot.tgz
tar zxf plot.tgz
rm plot.tgz
```
