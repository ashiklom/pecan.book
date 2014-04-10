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

# Loading modules on a remote server

If running on the BU Geo cluster add `module load hdf5 netcdf nco` to your `.cshrc` or `.bashrc` file (depending upon which shell you use by default)

# Steps for setting up a remote model run

1. Before doing an individual run you will need to make sure that the PEcAn code is installed (and preferably up to date) in your home directory on the remote machine. Note that you just need to install and build the code, not the database.  See the Using Github instructions on how to clone PEcAn.

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

    Under `<run>` specify

    * the remote location of the meteorology file `<met>`

    Note: for ED2, if you've copied files make sure the ED_MET_DRIVER_HEADER sets it's internal path correctly

4. Remote queue specification (if applicable)

    [see run config details](https://github.com/PecanProject/pecan/wiki/PEcAn-Configuration#run-setup)

5. The highest-level `<outdir>` is where output is copied to on the LOCAL machine

    For more general info on setting up the pecan settings file see [PEcAn-Configuration](https://github.com/PecanProject/pecan/wiki/PEcAn-Configuration)

6. If you remote server doesn't support ssh keys, ssh to the server from the VM command line

7. At this point you should be able to use workflow.R to run the model, as described in Demo 2 of the tutorial.
