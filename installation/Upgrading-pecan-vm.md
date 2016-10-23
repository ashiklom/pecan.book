Release notes for all releases can be found [here](https://github.com/PecanProject/pecan/releases).

This page will only list any steps you have to do to upgrade an existing system. When updating PEcAn it is highly encouraged to update BETY. You can find instructions on how to do this, as well on how to update the database in the [Updating BETYdb](https://pecan.gitbooks.io/betydb-documentation/content/updating_betydb_when_new_versions_are_released.html) gitbook page.

To upgrade to the latest version of PEcAn\/VM you will need to do each of the intermediate steps, you can not skip to the latest version.

### Updating PEcAn

```bash
cd pecan
git pull
R --vanilla < scripts/install.dependencies.R
make
```

### Updating BETY

Instructions on how to update bety can be found at the wiki page of the bety project [here](https://github.com/PecanProject/bety/wiki/Updating-BETY)

