We recommend that new users download the PEcAn ["pecan.ova"](http://isda.ncsa.illinois.edu/download/index.php?project=PEcAn&sort=version) instead of creating from one from scratch. These instructions are provided to document how to install PEcAn on different Operating Systems.

## Install build environment


### Set `R_LIBS_USER` 


(All platforms) [CRAN Reference](http://cran.r-project.org/doc/manuals/r-devel/R-admin.html#Managing-libraries)

```bash
# point R to personal lib folder
echo 'export R_LIBS_USER=${HOME}/R/library' >> ~/.bashrc
source ~/.bashrc
mkdir -p ${R_LIBS_USER}
```

### Linux Ubuntu

```bash

sudo -s

# point to latest R
echo "deb http://cran.rstudio.com/bin/linux/ubuntu `lsb_release -s -c`/" > /etc/apt/sources.list.d/R.list
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
apt-get -y update

# install all packages needed
apt-get -y install build-essential git gfortran openmpi-bin libhdf5-openmpi-dev r-base-core jags liblapack-dev libnetcdf-dev netcdf-bin bc libcurl4-openssl-dev curl udunits-bin libudunits2-dev libmysqlclient-dev 

# this needs to be done separately from previous command
apt-get -y install libgdal1-dev libproj-dev

# install packages for webserver
apt-get -y install apache2 libapache2-mod-php5 php5

# install packages for mysql
apt-get -y install libdbd-mysql mysql-server mysql-client php5-mysql 

# install packages for postgresql (experimental, commands might be wrong)
apt-get -y install libdbd-postgresql postgresql php5-postgresql

# install packages to compile docs
apt-get -y install texinfo texlive-latex-base texlive-latex-extra texlive-fonts-recommended

# install devtools
echo 'install.packages("devtools", repos="http://cran.rstudio.com/")' | R --vanilla

# done as root
exit
```

### CentOS / RHEL

#### Install and configure MySQL, udunits2, NetCDF

[Reference: centoshelp.org](http://centoshelp.org/servers/database/installing-configuring-mysql-server/)

```bash
yum -y install git R mysql mysql-server udunits2 netcdf
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

#### Install and configure Rstudio-server

based on [Rstudio Server documentation](http://www.rstudio.com/ide/docs/server/getting_started)

1. add `PATH=$PATH:/usr/sbin:/sbin` to `/etc/profile`
   ```bash
   cat "PATH=$PATH:/usr/sbin:/sbin; export PATH" >> /etc/profile
   ```
2. add [rstudio.conf](https://gist.github.com/dlebauer/6921889) to /etc/httpd/conf.d/ 
   ```bash
   wget https://gist.github.com/dlebauer/6921889/raw/d1e0f945228e5519afa6223d6f49d6e0617262bd/rstudio.conf
   sudo mv rstudio.conf /httpd/conf.d
   ```
3. download and install server:
   ```bash
   wget http://download2.rstudio.org/rstudio-server-0.97.551-i686.rpm
   sudo yum install --nogpgcheck rstudio-server-0.97.551-i686.rpm
   ```
4. restart server `sudo httpd restart`
5. now you should be able to access `http://<server>/rstudio`

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

curl -o hdf5-1.8.11.tar.gz http://www.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.8.11.tar.gz
tar zxf hdf5-1.8.11.tar.gz
cd hdf5-1.8.11
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

#### Install Postgres

For those on a Mac I use the following app for postgresql which has
postgis already installed (just make sure you have 9.3)
http://postgresapp.com/

To get postgis run the following commands in psql:
```
## Enable PostGIS (includes raster)
CREATE EXTENSION postgis;
## Enable Topology
CREATE EXTENSION postgis_topology;
## fuzzy matching needed for Tiger
CREATE EXTENSION fuzzystrmatch;
## Enable US Tiger Geocoder
CREATE EXTENSION postgis_tiger_geocoder;
```
To check your postgis run the following command again in psql:
```
SELECT PostGIS_full_version();
```
#### Install JAGS

For more instructions see http://martynplummer.wordpress.com/2011/11/04/rjags-3-for-mac-os-x/



#### Install udunits

Installing udunits-2 on MacOSX is done from source.

* download most recent [version of Udunits here](http://www.unidata.ucar.edu/downloads/udunits/index.jsp)
* instructions for [compiling from source](http://www.unidata.ucar.edu/software/udunits/udunits-2/udunits2.html#Obtain)


```bash
wget ftp://ftp.unidata.ucar.edu/pub/udunits/udunits-2.1.24.tar.gz
tar -xvf udunits-2.1.24.tar.gz
cd udunits-2.1.24
./configure; make; make check; make clean

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

**Developers: Install BioCro from Source** 

```bash
git clone ebi-forecast.igb.illinois.edu:/home/share/source/biocro.git
R CMD INSTALL biocro
```

**Public: Install BioCro Binary**

```bash
wget http://file-server.igb.illinois.edu/~dlebauer/BioCro_0.91.tar.gz
R CMD INSTALL BioCro_0.91.tar.gz
```

## Installing BETY

There are two flavors of BETY, PHP and RUBY. The PHP version allows for a minimal interaction with the database while the RUBY version allows for full interaction with the database. Both however require the database to be created and installed

### Database creation

The following creates the user and database.
```bash
# needs to be done only once
mysql -u root -p -e "grant all,super on bety.* to bety@localhost identified by 'bety';"
mysql -u root -p -e "grant super on *.* to bety@localhost identified by 'bety';"
mysql -u bety -pbety -e "create database bety;"
```

If you plan on installing PEcAn you can skip the following section. Next we populate the database with the latest version from Illinois. Once PEcAn is installed you can use the updatedb.sh script that is bundled with PEcAn to update the database.

```bash
# needs to be done only once
curl -o ${HOME}/updatedb.sh http://isda.ncsa.illinois.edu/~kooper/EBI/updatedb.sh
chmod 755 ${HOME}/updatedb.sh

# download and update/install database
${HOME}/updatedb.sh
```

### PHP version

The php version comes with PEcAn and should be accessible from http://<host>:<port>/pecan/db/ once PEcAn is installed.

### RUBY version

The RUBY version requires a few extra packages to be installed first.

```bash
sudo apt-get -y install python-software-properties
sudo apt-add-repository -y ppa:brightbox/ruby-ng
sudo apt-get update

# install all ruby related packages
sudo apt-get -y install ruby1.9.3 rubygems ruby-switch passenger-common1.9.1 libapache2-mod-passenger 
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
RackEnv production
RackBaseURI /bety
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
./scripts/install.dependencies.R

# install mysql driver
echo "install.packages('RMySQL', repos='http://cran.rstudio.com/')" | R --vanilla

# compile pecan
./scripts/build.sh

# update/install database
./scripts/updatedb.sh
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

# List of allowed hosts
\$hostlist=array(gethostname());

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


## Update Build and Check PEcAn

The [`build.sh`](https://github.com/PecanProject/pecan/blob/master/scripts/build.sh) script has options that make it easy to to update PEcAn, compile your local changes, and use `R CMD check` on all of the packages  the most recent versions. To use the email option, bsd mail must be installed (on Ubuntu / CentOS: `apt-get install bsd-mailx` / `yum install bsd-mailx`).

Here are the options (see `./scripts/build.sh -h`)

```
./scripts/build.sh <options>
 -h, --help      : this help text
 -f, --force     : force a build
 -g, --git       : do a git pull
 -i, --install   : install all R packages (default=yes)
 -n, --noinstall : do not install all R packages
 -c, --check     : check the R packages before install
 -e, --email     : send email to following people on success
```
