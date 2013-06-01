* We recommend that new users download the PEcAn ["pecan.ova"](http://isda.ncsa.illinois.edu/download/index.php?project=PEcAn&sort=version) instead of creating from one from scratch. These instructions are provided to document steps used to create the PEcAn Virtual Machines.

## Creating a Virtual Machine

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

For the virtual machines created the following routes are created:
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
```

Install Virtual Box additions for better integration

```bash
sudo mount /dev/cdrom /mnt
sudo /mnt/VBoxLinuxAdditions.run
sudo umount /mnt
sudo addgroup carya vboxsf
```

## Install PEcAn software

See generic notes [[Installing PEcAn]].

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
echo "r-libs-user=~/R/library" >> /etc/rstudio/rsession.conf
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
echo "r-libs-user=~/R/library" >> /etc/rstudio/rsession.conf
rstudio-server restart

# all done, exit root
exit
```

### Install additional packages

HDF5 Tools, netcdf, GDB and emacs
```bash
sudo apt-get -y install hdf5-tools cdo nco netcdf-bin ncview gdb emacs ess nedit
```

## Additional datasets from Harvard

Add datasets and runs

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
PEcAn, ED, SIPNET and BETY.

You can access this system using a webbrowser at
 http://<hosting machine>:6480/
or using SSH at
 ssh -l carya -p 6422 <hosting machine>
where <hosting machine> is the machine where the VM runs on.

For more information about:
Pecan  - http://pecanproject.org
SIPNET - http://thesipnetmodel.blogspot.com
BETY   - http://www.betydb.org
ED     - http://www.esm.harvard.edu
EOF
exit
```

or in case of 32 bit machine

```bash
sudo -s
cat > /etc/motd.tail << EOF
This system allows you to experiment and create simulations using
PEcAn, ED, SIPNET and BETY.

You can access this system using a webbrowser at
 http://<hosting machine>:3280/
or using SSH at
 ssh -l carya -p 3222 <hosting machine>
where <hosting machine> is the machine where the VM runs on.

For more information about:
Pecan  - http://pecanproject.org
SIPNET - http://thesipnetmodel.blogspot.com
BETY   - http://www.betydb.org
ED     - http://www.esm.harvard.edu
EOF
exit
```

## Helper Scripts

Script to clean the VM and remove as much as possible history [cleanvm.sh](http://isda.ncsa.uiuc.edu/~kooper/EBI/cleanvm.sh)

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

## Convert VM to Desktop machine

```bash
sudo apt-get update
sudo apt-get install xfce4 xorg
```

For a more refined desktop environment, try 

```bash
sudo apt-get install --no-install-recommends xubuntu-desktop 
```
* replace `xubuntu-` with `ubuntu-`, `lubuntu-`, or other preferred desktop enviornment
* the `--no-install-recommends` eliminates additional applications, removing it will add a word processor, a browser, and lots of other applications included in the default operating system.

Reinstall Virtual Box additions for better integration adding X/mouse support

```bash
sudo mount /dev/cdrom /mnt
sudo /mnt/VBoxLinuxAdditions.run
sudo umount /mnt
```

### Install RStudio Desktop

```bash
wget http://download1.rstudio.org/rstudio-0.97.551-amd64.deb
apt-get install libjpeg621
dpkg -i rstudio-*
rm rstudio-*
```
