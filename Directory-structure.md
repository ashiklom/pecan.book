# Directory structure

## Overview of PEcAn repository as of PEcAn 1.2

```
trunk/             # Overall repository
 +- all            # Dummy Package to load all PEcAn R packages
 +- common         # Core functions
 +- db             # Modules for querying the database
 +- depreciated    # code from PEcAn < 1.1 for reference or merging into new packages
 +- models         # Directory of wrapper scripts for each ecosystem model
    +- ed          # Wrapper scripts for running ED within PEcAn
    +- sipnet      # Wrapper scripts for running SIPNET within PEcAn
    +- ...         # Wrapper scripts for running [MODELX] within PEcAn
 +- modules        # Core modules
    +- allometry
    +- data.atmosphere
    +- data.hydrology
    +- data.land
    +- ensemble
    +- meta.analysis
    +- prospect
    +- uncertainty
 +- qaqc           # Database and model QAQC scripts
 +- scripts        # R and Shell scripts for use with PEcAn
    +- shell       # Shell scripts, e.g.. install.all.sh, check.all.sh
    +- R           # R scripts e.g. pft.add.spp.R, workflow.R, install.dependencies.R 
    +- fia         # FIA module
 +- utils          # Misc. utility functions
 +- visualization  # Advanced PEcAn visualization module
 +- web            # Main PEcAn website files
 +- documentation  # index_vm.html, references, other misc.
```