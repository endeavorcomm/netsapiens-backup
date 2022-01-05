# netsapiens-backup
Backup NetSapiens data to a local directory, AWS S3 or Google Cloud Storage.  Script is based on recommendations found [here](https://help.netsapiens.com/hc/en-us/articles/205235690-What-Commands-Should-I-Execute-For-Scheduled-Backups-).  Forked from [Sean Cheesman](https://github.com/scheesman) 

Output logging is enabled for the console and to syslog.  You can grep NSBACKUP and see all entries for script exection.  `grep NSBACKUP /var/log/syslog`

File structure in the cloud buckets will be organized by hostname and service type: 
```
. bucketname
+-- hostname
|  +-- Service_date.gz
```
## Instructions
Copy the script to the location of your choice.  You can download the files from github, or `git clone` it.
  
Copy the `nsbackup.conf.sample` to `nsbackup.conf`, and change relevant options in it - such as backup_path, user, password, bucket name, .s3cfg location, storage type, etc.  

Run script manually or via [crontab](#crontab).

## Local Backup Requirements
* backup_path - defaults to /tmp. Change to your desired path

## Cloud Backup Requirements
* s3cmd - install via `sudo apt-get install s3cmd`
or
* gsutil - install via instructions [below](#gsutil-installation)
* Amazon S3 or Google Cloud Storage bucket with appropriate permissions
* Properly configured .s3cfg file for AWS S3 usage.  Should just require setting the access_key and secret_key.  Can be completed with `s3cmd --configure`.  Should output file into `/opt/.s3cfg`
* gsutil configuration.  Can be completed via `gcloud init`

## Usage
The script takes up to 9 parameters.  You can specify anywhere from 1 to 6, depending on your needs.

Options: `audiofiles`, `conference`, `core`, `cdr`, `cdr2`, `cdr2last`, `messaging`, `ndp`, `ndpfiles`, `recording`

### audiofiles
`audiofiles` backup greetings and voicemails. These could use a lot of backup disk space.

### conference
`conference` backup the Conferencing module.

### core
`core` backup the Core module configuration without CDRs.

### cdr
`cdr` backup the Core module CDRs.  This option only backs up the last 25 hours, so you will want to run this option once per day.

### cdr2
`cdr2` backup current month's CDR2 files.  This should be run every day as it only backs up the current month's tables.

### cdr2last
`cdr2last` backup the previous month's CDR2 tables.  This should only be run once a month as these files are huge and they do not change.

### messaging
`messaging` backup the MessagingDomain db (Chat/SMS) and all included tables.  Should be run once a day.

### ndp
`ndp` backup the Endpoints module.

### ndpfiles
`ndpfiles` backup the /frm folder and all of its contents.  This option was added separate from the `ndp` option as you probably don't want to back this up every night.

### recording
`recording` backup the Recording module.

## Examples

Backup all services, if on a single box:

`nsbackup.sh audiofiles conference core cdr messaging ndp ndpfiles recording`

Backup Core and Conferencing Modules:

`nsbackup.sh audiofiles conference core cdr messaging`

Backup Endpoints Module:

`nsbackup.sh ndp ndpfiles`

Backup Recording Module:

`nsbackup.sh recording`

## gsutil installation

gsutil isn't included in the default Ubuntu repositories so you will have to add it yourself.

`echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list`

`apt-get install apt-transport-https ca-certificates`

`curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -`

`sudo apt-get update && sudo apt-get install google-cloud-sdk`

`gcloud init`

## crontab
You will probably want to run these via crontab.  Below are example crontabs, depending on the roles installed.  Just `sudo crontab -e` and insert what's relevant for you.  If you need help with crontab schedules, checkout https://crontab-generator.org/.

`0 3 * * * /usr/local/scripts/nsbackup.sh core cdr conference ndp recording > /var/log/backups.log`

`30 0 * * 0 /usr/local/scripts/nsbackup.sh ndpfiles > /var/log/backups.log`
