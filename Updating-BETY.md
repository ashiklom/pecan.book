If you have an instance of BETY and you might want to update it to the latest version at certain points for the following reasons.
- security updates
- new functionality
- importing data and remote server has newer version

To update BETY you can use the following steps to update your system (this is assuming the VM, if you installed BETY in another location please change the path accordingly).
```bash
sudo -s
# change to BETY
cd /usr/local/bety
# update BETY to latest version
git pull
# install all required gems
bundle install --without test_js
# update database
rake db:migrate ﻿RAILS_ENV="production"
# restart BETY
touch tmp/restart.txt
```

At this point your database should have migrated to the latest version and the BETY application should have restarted.