Following is information on how to update the software and data to the latest version.

## Updating BETY database

A new system is in place which will allow you to update the BETY database without loosing any local changes (this is still BETA though). To update the datbase and keep your changes, all you have to do is run the following:

```
cd
pecan/scripts/load.bety.sh
```

For more information see the [Updating BETYdb](https://github.com/PecanProject/bety/wiki/Updating-BETY) documentation.

## Update Build and Check PEcAn

The [`build.sh`](https://github.com/PecanProject/pecan/blob/master/scripts/build.sh) script has options that make it easy to to update PEcAn, compile your local changes, and use `R CMD check` on all of the packages  the most recent versions. 

Here are the options (see `./scripts/build.sh -h`)

```
./scripts/build.sh <options>
 -c, --check         : check the R packages before install
 -d, --documentation : (re)generates all Rd files
 -f, --force         : force a build
 -g, --git           : do a git pull
 -h, --help          : this help text
 -i, --install       : install all R packages (default=yes)
 -n, --noinstall     : do not install all R packages
 -t, --test          : run tests
```

The best method to update PEcAn is to use `./scripts/build.sh -t -g`. This will get the latest version from GitHub, compile and run the local test cases.
