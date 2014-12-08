## Creating a Virtual Machine

First create  virtual machine

```bash
# ----------------------------------------------------------------------
# CREATE VM USING FOLLOWING:
# - VM NAME  = PEcAn 32bit | PEcAn 64bit
# - CPU      = 1
# - MEMORY   = 2GB 
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

## Finishing up the machine

### Add a message to the login:

```bash
sudo -s
export PORT=$( hostname | sed 's/pecan//' )
cat > /etc/motd.tail << EOF
PEcAn version 1.3.7

This system allows you to experiment and create simulations using
PEcAn, ED, SIPNET and BETY.

You can access this system using a webbrowser at
 http://<hosting machine>:${PORT}80/
or using SSH at
 ssh -l carya -p ${PORT}22 <hosting machine>
where <hosting machine> is the machine where the VM runs on.

For more information about:
Pecan  - http://pecanproject.org
SIPNET - http://thesipnetmodel.blogspot.com
BETY   - http://www.betydb.org
ED     - http://www.esm.harvard.edu
EOF
exit
```

## Finishing up

Script to clean the VM and remove as much as possible history [cleanvm.sh](http://isda.ncsa.uiuc.edu/~kooper/EBI/cleanvm.sh)

```bash
wget -O ~/cleanvm.sh http://isda.ncsa.uiuc.edu/~kooper/EBI/cleanvm.sh
chmod 755 ~/cleanvm.sh
```

Make sure machine has SSH keys [rc.local](http://isda.ncsa.illinois.edu/~kooper/EBI/rc.local)

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
