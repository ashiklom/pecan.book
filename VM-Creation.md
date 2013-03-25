# VM Creation

_*Note: This document provided for informational purposes*_

We recommend that users download the PEcAn ["pecan.ova"](http://isda.ncsa.illinois.edu/download/index.php?project=PEcAn&sort=version) instead of creating from one from scratch. 

## Create Virtual Machine

First create  virtual machine

```bash
# ----------------------------------------------------------------------
# CREATE VM USING FOLLOWING:
# - VM NAME  = PEcAn 32bit | PEcAn 64bit
# - CPU      = 1
# - MEMORY   = 1GB 
# - DISK     = 100GB
# - HOSTNAME = pecan32/64
# - FULLNAME = PEcAn Demo User
# - USERNAME = xxxxxxx
# - PASSWORD = yyyyyyy
# - PACKAGE  = openssh
# ----------------------------------------------------------------------
```

To enable tunnels run the following on the host machine, I used xx=64 for the 64 bit version of the VM and xx=32 for the 32 bit version of the VM:
```bash
VBoxManage modifyvm "<virtual machine name>" --natpf1 "ssh,tcp,,xx22,,22"
VBoxManage modifyvm "<virtual machine name>" --natpf1 "www,tcp,,xx80,,80"
```

```bash
VBoxManage modifyvm "PEcAn 32bit" --natpf1 "ssh,tcp,,3222,,22"
VBoxManage modifyvm "PEcAn 32bit" --natpf1 "www,tcp,,3280,,80"
VBoxManage modifyvm "PEcAn 64bit" --natpf1 "ssh,tcp,,6422,,22"
VBoxManage modifyvm "PEcAn 64bit" --natpf1 "www,tcp,,6480,,80"
```

Make sure machine is up to date.

```bash
sudo apt-get update
sudo apt-get -y dist-upgrade
sudo reboot
```

Install compiler and other packages needed and install the tools.

```bash
sudo apt-get -y install build-essential linux-headers-server dkms

sudo mount /dev/cdrom /mnt
sudo /mnt/VBoxLinuxAdditions.run
sudo umount /mnt
sudo addgroup carya vboxsf
```

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
apt-get -y install build-essential git gfortran openmpi-bin libhdf5-openmpi-dev r-base-core default-jre libdbd-mysql libmysqlclient-dev mysql-server mysql-client jags r-cran-rjags r-cran-xml r-cran-hdf5 r-cran-mass r-cran-rmysql liblapack-dev libnetcdf-dev netcdf-bin texlive-latex-base texlive-latex-extra texlive-fonts-recommended bc libcurl4-openssl-dev texinfo curl apache2 libapache2-mod-php5 php5 php5-mysql

# install devtools
echo 'install.packages("devtools", repos="http://cran.rstudio.com/")' | R --vanilla

# done as root
exit
```

### Mac OSX

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
curl -o hdf5-1.8.10-patc#tar.gz http://www.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.8.10-patc#tar.gz
tar zxf hdf5-1.8.10-patc#tar.gz
cd hdf5-1.8.10-patch1
sed -i -e 's/-O3/-O0/g' config/gnu-flags 
./configure --prefix=/usr/local/hdf5 --enable-fortran --enable-cxx --with-szlib=/usr/local/szip
make
# make check
sudo make install
# sudo make check-install
cd ..
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
wget http://isda.ncsa.illinois.edu/~kooper/EBI/inputs.tgz
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
wget -O ED2IN http://isda.ncsa.illinois.edu/~kooper/EBI/ED2IN.r46
sed -i -e "s#\$HOME#$HOME#" ED2IN
wget -O config.xml  http://isda.ncsa.illinois.edu/~kooper/EBI/config.r46.xml
# execute test run
time ed2.r46
```

### ED 2.2 r82

```bash
wget -o ED.r82.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/ED.r82.tgz
tar zxf ED.r82.tgz
rm ED.r82.tgz

cd ED.r82
wget http://isda.ncsa.illinois.edu/~kooper/EBI/ED.r82.patch
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
wget -O ED2IN http://isda.ncsa.illinois.edu/~kooper/EBI/ED2IN.r82
sed -i -e "s#\$HOME#$HOME#" ED2IN
wget -O config.xml  http://isda.ncsa.illinois.edu/~kooper/EBI/config.r82.xml
# execute test run
time ed2.r82
```

### SIPNET Installation

```bash
cd
wget http://isda.ncsa.illinois.edu/~kooper/EBI/sipnet_unk.tar.gz
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
wget -O ${HOME}/updatedb.sh http://isda.ncsa.illinois.edu/~kooper/EBI/updatedb.sh
chmod 755 ${HOME}/updatedb.sh

# download and update/install database
${HOME}/updatedb.sh
```

### PHP version

The php version comes with PEcAn and should be accessible from http://<host>:<port>/pecan/db/.

### RUBY version

The RUBY version requires a few extra packages to be installed first.

```bash
# passenger 3 repository
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

### Configuring PEcAn

First install web server and setup pecan and the web server
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
</Directory>
EOF
/etc/init.d/apache2 restart

# done as root
exit

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
```

all done you can now visit the server http://<hostname>:<port>/pecan and you can interact with the database using http://<hostname>:<port>/pecan/db/


### PEcAn Testrun

Do the run
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


## FIA database

FIA database is not installed by default, this would add an extra 10GB to the VM. Add a text file describing how to install the FIA database.

```bash
cat > FIA.txt << EOF
The following command will downaload and install the FIA database
for use with PEcAn. This database will require a 10GB download.

# needs to be done only once
mysql -u root -p -e "grant all on fia5data.* to bety@localhost identified by 'bety';" 

# download and update/install database
wget http://isda.ncsa.illinois.edu/~kooper/EBI/fiadb.sql
mysql -u bety -p"bety" -e 'drop database if exists fia5data; create database fia5data;'
mysql -u bety -p"bety" fia5data < fiadb.sql
rm fiadb.sql
EOF
```

## Installing additional software

### Installing rstudio

*NOTE This will allow anybody to login to the machine through the rstudio interface and run any arbitrary code. The login used however is the same as the system login/password.*

```bash
# bceome root
sudo -s

# install required packages
apt-get -y install libapparmor1 apparmor-utils libssl0.9.8

# setup rstudio forwarding in apache
a2enmod proxy_http
cat > /etc/apache2/conf.d/rstudio.conf << EOF
ProxyPass        /rstudio/ http://localhost:8787/
ProxyPassReverse /rstudio/ http://localhost:8787/
RedirectMatch permanent ^/rstudio$ /rstudio/
EOF
/etc/init.d/apache2 restart
```

Based on version of ubuntu 32/64 use either of the following

*32bit only*
```bash
wget http://download2.rstudio.org/rstudio-server-0.97.336-i386.deb
dpkg -i rstudio-server-*
rm rstudio-server-*
echo "www-address=127.0.0.1" >> /etc/rstudio/rserver.conf
rstudio-server restart

# all done, exit root
exit
```

*64bit only*
```bash
wget http://download2.rstudio.org/rstudio-server-0.97.336-amd64.deb
dpkg -i rstudio-server-*
rm rstudio-server-*
echo "www-address=127.0.0.1" >> /etc/rstudio/rserver.conf
rstudio-server restart

# all done, exit root
exit
```

### Install additional packages

HDF5 Tools, netcdf, GDB and emacs
```bash
sudo apt-get -y install hdf5-tools cdo nco netcdf-bin ncview gdb emacs ess nedit
```

## Addiotional datasets

### Flux Camp

Following will install the data for flux camp
```bash
cd
wget -O plot.tgz http://isda.ncsa.illinois.edu/~kooper/EBI/plot.tgz
tar zxf plot.tgz
rm plot.tgz
```

### Harvard additions

Add additional datasets and runs

```bash
wget http://isda.ncsa.illinois.edu/~kooper/EBI/Santarem_Km83.zip
unzip -d sites Santarem_Km83.zip
sed -i -e "s#/home/pecan#${HOME}#" sites/Santarem_Km83/ED_MET_DRIVER_HEADER
rm Santarem_Km83.zip

wget http://isda.ncsa.illinois.edu/~kooper/EBI/testrun.s83.zip
unzip testrun.s83.zip
sed -i -e "s#/home/pecan#${HOME}#" testrun.s83/ED2IN
rm testrun.s83.zip

wget http://isda.ncsa.illinois.edu/~kooper/EBI/ed2ws.harvard.tgz
tar zxf ed2ws.harvard.tgz
mkdir ed2ws.harvard/analy ed2ws.harvard/histo
sed -i -e "s#/home/pecan#${HOME}#g" ed2ws.harvard/input_harvard/met_driver/HF_MET_HEADER ed2ws.harvard/ED2IN ed2ws.harvard/*.r
rm ed2ws.harvard.tgz

wget http://isda.ncsa.illinois.edu/~kooper/EBI/testrun.PDG.zip
unzip testrun.PDG.zip
sed -i -e "s#/home/pecan#${HOME}#" testrun.PDG/Met/PDG_MET_DRIVER testrun.PDG/Template/ED2IN
sed -i -e 's#/n/scratch2/moorcroft_lab/kzhang/PDG/WFire_Pecan/##' testrun.PDG/Template/ED2IN
rm testrun.PDG.zip

wget http://isda.ncsa.illinois.edu/~kooper/EBI/create_met_driver.tar.gz
tar zxf create_met_driver.tar.gz
rm create_met_driver.tar.gz
```

## Finishing up the machine

### Add a message to the login:

In case of 64 bit machine

```bash
sudo -s
cat > /etc/motd.tail << EOF
This system allows you to experiment and create simulations using
PEcAn, ED and BETY.

You can access this system using a webbrowser at 
 http://<hosting machine>:6480/
or using SSH at 
 ssh -l pecan -p 6422 <hosting machine>
where <hosting machine> is the machine where the VM runs on.

For more information about:
Pecan - http://pecanproject.org
BETY  - http://www.betydb.org
ED    - http://www.esm.harvard.edu
EOF
exit
```

or in case of 32 bit machine

```bash
sudo -s
cat > /etc/motd.tail << EOF
This system allows you to experiment and create simulations using
PEcAn, ED and BETY.

You can access this system using a webbrowser at 
 http://<hosting machine>:3280/
or using SSH at 
 ssh -l pecan -p 3222 <hosting machine>
where <hosting machine> is the machine where the VM runs on.

For more information about:
Pecan - http://pecanproject.org
BETY  - http://www.betydb.org
ED    - http://www.esm.harvard.edu
EOF
exit
```

### Helper Scripts

Script to clean the VM and remove as much as possible history ("cleanvm.sh":http://isda.ncsa.uiuc.edu/~kooper/EBI/cleanvm.sh)

```bash
wget -O ~/cleanvm.sh http://isda.ncsa.uiuc.edu/~kooper/EBI/cleanvm.sh
chmod 755 ~/cleanvm.sh
```

Make sure machine has SSH keys ("rc.local":http://isda.ncsa.illinois.edu/~kooper/EBI/rc.local)

```bash
sudo wget -O /etc/rc.local http://isda.ncsa.illinois.edu/~kooper/EBI/rc.local
```

Change the resolution of the console

```bash
sudo sed -i -e 's/#GRUB_GFXMODE=640x480/GRUB_GFXMODE=1024x768/' /etc/default/grub
sudo update-grub
```

Once all done, stop the virtual machine
```bash
history -c && ${HOME}/cleanvm.sh
```
