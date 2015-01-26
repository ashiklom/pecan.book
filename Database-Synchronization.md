The database synchronization consists of 2 parts:
- Getting the data from the remote servers to your server
- Sharing your data with everybody else

## How does the database synchronization work

Each server that runs the BETY database will have a unique machine_id and a sequence of ID's associated. Whenever the user creates a new record in BETY it will receive an ID in the sequence. This allows us to uniquely identify where a record came from. This is information is crucial for the code that works with the synchronization since we can now copy those records that have an ID in the sequence specified. If you have not asked for a unique ID your ID will be 99.

The synchronization code itself is split into two parts, load.bety.sh and dump.bey.sh. Unless you plan on sharing your data you will only use load.bety.sh to update your database.

## How to get the latest data

When logged into the machine you can fetch the latest data using the load.bety.sh script. 
how to get latest data from EBI/BU

run script load.bety.sh
how to get an ID

add server to EBI (hardcoded first, see #149)
send back ID and range (hardcoded first, see #149)
make sure to update when you run load.bety.sh
how to propegate your data

run dump.bey.sh
create issue to add to EBI remote url (hardcoded first, see #149)