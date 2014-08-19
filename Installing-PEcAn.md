We recommend that new users download the PEcAn ["pecan.ova"](http://isda.ncsa.illinois.edu/download/index.php?project=PEcAn&sort=version) instead of creating from one from scratch. These instructions are provided to document how to install PEcAn on different Operating Systems.

## Installing Prerequisites

Check specific notes in:

- [VM Creation](VM-Creation)
- [Installing PEcAn Data](Installing-PEcAn-Data)
- [Enabling Remote Execution](Enabling-Remote-Execution)

OS specific installation instructions:

 - [Installing PEcAn Ubuntu](Installing-PEcAn-Ubuntu)
 - [Installing PEcAn OSX](Installing-PEcAn-OSX)
 - [Installing PEcAn RedHat](Installing-PEcAn-RedHat)

To update PEcAn see:

- [Updating PEcAn](Updating-PEcAn)
- [Updating BETY](https://github.com/PecanProject/bety/wiki/Updating-BETY)

The rest of the instructions assumes you have done the appropriate steps in each of the above guides.

### Set `R_LIBS_USER` 

[CRAN Reference](http://cran.r-project.org/doc/manuals/r-devel/R-admin.html#Managing-libraries)

```bash
# point R to personal lib folder
echo 'export R_LIBS_USER=${HOME}/R/library' >> ~/.bashrc
source ~/.bashrc
mkdir -p ${R_LIBS_USER}
```

## Install models

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

### ED 2.2 bleeding edge

**Needs updating**

```bash
cd
git clone ...

cd ED2
...
sudo cp ../ed_2.1-opt /usr/local/bin/ed2.git
cd
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

### SIPNET testrun

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

# compile pecan
./scripts/build.sh
```


Following will run a small script to setup some hooks to prevent people from using the pecan demo user account to check in any code.

```bash
# prevent pecan user from checking in code
./scripts/create-hooks.sh
```

## Installing BETY

### Install Database+Data

```
# install database (code assumes password is bety)
sudo -u postgres createuser -d -l -P -R -S bety
sudo -u postgres createdb -O bety bety
sudo -u postgres CREATE=YES scripts/load.bety.sh

# configure for web app (change passowrd if needed)
cp web/config.example.php web/config.php 

# add models to database
./scripts/add.models.sh

# add data to database
./scripts/add.data.sh

# create outputs folder
mkdir ~/output
chmod 777 ~/output
```

### Installing BETYdb Web Application

There are two flavors of BETY, PHP and RUBY. The PHP version allows for a minimal interaction with the database while the RUBY version allows for full interaction with the database.

#### PHP version

The php version comes with PEcAn and is already configured.

#### RUBY version

The RUBY version requires a few extra packages to be installed first.

Next we install the web app.

```bash
sudo -s

# install bety
cd /usr/local
git clone https://github.com/PecanProject/bety.git

# install gems
cd bety
gem install bundler
bundle install --without test_js
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
chmod go+w /usr/local/bety/public/javascripts/cache/

# setup bety database configuration
cat > config/database.yml << EOF
production:
#  adapter: mysql2
  adapter: postgresql
  encoding: utf-8
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
PassengerRuby /usr/bin/ruby1.9.1
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


### PEcAn Testrun

Do the run, this assumes you have installed the BETY database, sites tar file and sipnet.

```bash
# create folder
cd
mkdir testrun.pecan
cd testrun.pecan

# copy example of pecan workflow and configuration file
cp ../pecan/tests/pecan.sipnet.xml pecan.xml
cp ../pecan/scripts/workflow.R workflow.R

# exectute workflow
rm -rf pecan
./workflow.R
```
NB: pecan.xml is configured for the virtual machine, you will need to change the <met> field from '/home/carya/' to wherever you installed your 'sites', usually $HOME
