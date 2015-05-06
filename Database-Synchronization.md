The database synchronization consists of 2 parts:
- Getting the data from the remote servers to your server
- Sharing your data with everybody else

## How does it work?

Each server that runs the BETY database will have a unique machine_id and a sequence of ID's associated. Whenever the user creates a new row in BETY it will receive an ID in the sequence. This allows us to uniquely identify where a row came from. This is information is crucial for the code that works with the synchronization since we can now copy those rows that have an ID in the sequence specified. If you have not asked for a unique ID your ID will be 99.

The synchronization code itself is split into two parts, load.bety.sh and dump.bety.sh. Unless you plan on sharing your data you will only use load.bety.sh to update your database.

## Fetch latest data

When logged into the machine you can fetch the latest data using the load.bety.sh script. The script will check what site you want to get the data for and will remove all data in the database associated with that id. It will then reinsert all the data from the remote database.

The script is configured using environment variables.  The following variables are recognized:
- DATABASE: the database where the script should write the results.  The default is `bety`.
- OWNER: the owner of the database (if it is to be created).  The default is `bety`.
- PG_OPT: additional options to be added to psql (default is nothing).
- MYSITE: the (numerical) ID of your site.  If you have not requested an ID, use 99; this is used for all sites that do not want to share their data (i.e. VM). 99 is in fact the default.
- REMOTESITE: the ID of the site you want to fetch the data from.  The default is 0 (EBI).
- CREATE: If 'YES', this indicates that the existing database (`bety`, or the one specified by DATABASE) should be removed. Set to YES (in caps) to remove the database.  **THIS WILL REMOVE ALL DATA** in DATABASE.  The default is NO.
- KEEPTMP: indicates whether the downloaded file should be preserved.  Set to YES (in caps) to keep downloaded files; the default is NO.
- USERS: determines if default users should be created.  Set to YES (in caps) to create default users with default passwords.  The default is NO.

## Sharing data

Sharing your data requires a few steps. First, before entering any data, you will need to request an ID from the PEcAn developers. Simply open an issue at github and we will generate an ID for you.  If possible, add the URL of your data host.

You will now need to synchronize the database again and use your ID.  For example if you are given ID=42 you can use the following command: `MYID=42 REMOTEID=0 ./scripts/load.bety.sh`. This will load the EBI database and set the ID's such that any data you insert will have the right ID.

To share your data you can now run the dump.bey.sh. The script is configured using environment variables, the following variables are recognized:
- DATABASE: the database where the script should write the results.  The default is `bety`.
- PG_OPT: additional options to be added to psql (default is nothing).
- MYSITE: the ID of your site.  If you have not requested an ID, use 99, which is used for all sites that do not want to share their data (i.e. VM).  99 is the default.
- LEVEL: the minimum access-protection level of the data to be dumped (0=private, 4=public).  The default is level 3.
- UNCHECKED: specifies whether unchecked traits and yields be dumped.  Set to YES (all caps) to dump unchecked data.  The default is NO.
- ANONYMOUS: specifies whether all users be anonymized.  Set to YES (all caps) to keep the original users (**INCLUDING PASSWORD**) in the dump file.  The default is NO.
- OUTPUT: the location of where on disk to write the result file.  The default is ${PWD}/dump.

## Tasks

Following is a list of tasks we plan on working on to improve these scripts:
- #149 : add server to EBI, currently we will assign ID by hand and the range is based on this ID and is computed in the scripts. This information should be stored in the database, as well as the URL where to get the data from. This will allow a user to update the URL.
- #341 : currently scripts are configured with environment variables, it would be good to use command line arguments instead
- #342 : users should not automatically be generated even on VM.