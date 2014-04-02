### If the remote machine supports ssh keys

Before the first time, run `scripts/sshkey.sh`

### If the remote machine does not support ssh keys

Add the following to `~/.ssh/config`

```
Host <hostname goes here>
 ControlMaster auto
 ControlPath /tmp/%r@%h:%p
```

Next create a single ssh connection to the host, any ssh connection
afterwards will use the same connection and should not require a password.

Before running the PEcAn workflow, open (and leave open) a single ssh connection from the local machine to the remote machine

# Steps for setting for setting up a remote model run

1. Make a copy of an existing pecan settings file, preferably for the same site, using cleansettings.R

2. Open the copy new settings file (e.g. in RStudio)

2. Remote machine settings

    Under `<host>` specify

    * the name of the remote server and your login, e.g. `<name>dietze@geo.bu.edu</name>`

    * the remote run directory `<rundir>`

    * the remote output directory `<outdir>`

3. Remote file directories

    Under `<model>` specify 

    * the location of the model executable on the remote machine, e.g. `<binary>/usr2/faculty/dietze/ED.r82/ED/build/ed_2.1-opt</binary>`

    * for ED2, also specify remote databases: `<veg>`, `<soil>`, `<psscss>`, `<inputs>`

      `<veg>/projectnb/cheas/EDI/oge2OLD/OGE2_</veg>`
      `<soil>/projectnb/cheas/EDI/faoOLD/FAO_</soil>`
      `<inputs>/projectnb/cheas/EDI/ed_inputs/</inputs>`

    note that `<psscss>` is not required for a bare-ground run

    Under <run> specify

    * the remote location of the meteorology file <met>

    Note: for ED2, if you've copied files make sure the ED_MET_DRIVER_HEADER sets it's internal path correctly

4. Remote queue specification (if applicable)



5. The highest-level `<outdir>` is where output is copied to on the LOCAL machine
