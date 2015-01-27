The database synchronization consists of 2 parts:
- Getting the data from the remote servers to your server
- Sharing your data with everybody else

## How does it work?

Each server that runs the BETY database will have a unique machine_id and a sequence of ID's associated. Whenever the user creates a new record in BETY it will receive an ID in the sequence. This allows us to uniquely identify where a record came from. This is information is crucial for the code that works with the synchronization since we can now copy those records that have an ID in the sequence specified. If you have not asked for a unique ID your ID will be 99.

The synchronization code itself is split into two parts, load.bety.sh and dump.bey.sh. Unless you plan on sharing your data you will only use load.bety.sh to update your database.

## Fetch latest data

When logged into the machine you can fetch the latest data using the load.bety.sh script. The script will check what site you want to get the data for and will remove all data in the database associated with that id. It will then reinsert all the data from the remote database.

The script is configured using environment variables, the following variables are recognized:
- DATABASE: database where the script should write the results, default is bety
- OWNER: owner of the database if it is to be created, default is bety.
- PG_OPT: additional options to be added to psql, default is nothing.
- MYSITE: ID of your site, if you have none requested use 99 which is used for all sites that do not want to share their data (i.e. VM), default is 99.
- REMOTESITE: ID of the site you want to fetch the data from, default is 0, EBI.
- CREATE: should existing database be removed, **THIS WILL REMOVE ALL DATA**, set to YES (in caps) to remove the database, default is NO.
- KEEPTMP: should the file downloaded be preserved, set to YES (in caps) to keep downloaded files, default is NO.
- USERS: should default users be created, set to YES (in caps) to create default users with default passwords, default is NO unless you are downloading REMOTESITE=0 and MYSITE=99 in which case it will create the default users.

## Sharing data

To share your data requires a few steps. First before entering any data you will need to request an ID from the pecan developers. Simply open an issue at github and we will generate an ID for you. If possible add the URL of where you will host your data.

You will now need to synchronize the database again and use your ID, for example if you are given ID=42 you can use the following command: `MYID=42 REMOTEID=0 ./scripts/load.bety.sh`. This will load the EBI database and set the ID's such that any data you insert will have the right ID.

To share your data you can now run the dump.bey.sh. The script is configured using environment variables, the following variables are recognized:
- DATABASE: database where the script should write the results, default is bety
- PG_OPT: additional options to be added to psql, default is nothing.
- MYSITE: ID of your site, if you have none requested use 99 which is used for all sites that do not want to share their data (i.e. VM), default is 99.
- LEVEL: minimum level of the data to be dumped (0=private, 4=public), default is level 3.
- UNCHECKED: should all unchecked traits and yields be dumped, set to YES (all caps) to dump unchecked data, default is NO.
- ANONYMOUS: should all users be anonymized, set to YES (all caps) to keep the original users (**INCLUDING PASSWORD**) in the dump file, default is NO.
- OUTPUT: location on disk where to write the result file, default is ${PWD}/dump.

## Tasks

Following is a list of tasks we plan on working on to improve these scripts:
- #149 : add server to EBI, currently we will assign ID by hand and the range is based on this ID and is computed in the scripts. This information should be stored in the database, as well as the URL where to get the data from. This will allow a user to update the URL.
- #341 : currently scripts are configured with environment variables, it would be good to use command line arguments instead
- #342 : users should not automatically be generated even on VM.