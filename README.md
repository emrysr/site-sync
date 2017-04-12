# sync
sync db and files with rsync and mysqldump
***
### SYNC DESTINATION WITH SRC FILES & DB
uses rsync and mysqldump to copy files to
destination system from source system
---

> # WARNING
> This will overwrite databases and files. you will loose work if you're not careful.
> Take backups before using my shonky code please!!

## Three bash scripts:
Run any of these to sync the way described. 
- STAGING-to-DEV - overwrites dev with staging
- DEV-to-STAGING - overwrites staging with dev
- DEV-to-PRODUCTION - overwrites production with dev
### Additional syncs
Create a copy of any of the above scrpts and change the SOURCE and DESTINATION variables to match corresponding environment in the settings.conf file.


## Install (example files below)
1. Clone this repo
2. Create an entry in your ~/.ssh/config file to handle the SSH connection. *Ensure your public key is on the server*
3. Save you mysql connection settings in a .cnf style file 
4. Save the settings.sample.conf as settings.conf
5. Edit the settings.conf file to match your settings
6. **BACKUP FIRST!!!**
7. Run the desired script. eg. ./STAGING-to-DEV
8. Press Y to continue or N to cancel
9. Wait for "Done" message.


## config
settings.conf - file paths for the 3 environments and mysql.cnf
[env].cnf - files referred to in settings.conf to store mysql connections

### NOTE
***"fake"* progress bar** - the pv progress bars expect a total size to calculate progress.
I've put the values into settings.conf to save me having to read the DB size every time (a little faster)

- $SIZE = the mysql dump file size (MB)
- $SIZE_COMPRESSED = the mysql dump file once gziped (MB)


---
## #REQUIRED
<pre>
commands used:    ssh,pv,mysql,mysqldump,printf,zcat and rsync
ssh config:       The variable SSH needs to match a section of ssh config to
                  to to enable rsync to connect without prompting.
rsync defaults:   Does not delete files
mysql connection: create .cnf files with the connection settings DB_SRC and DB_DEST
</pre>


---
### Example files

*includes.list:* - list of all paths to sync **(ignoring bases set in settings.conf)**
```
tmp/emrys/
private/system/ee/
web/
private/system/user/templates
private/system/user/addons
```

*excludes.list:* - list of all paths to ignore
```
tmp/emrys/online-only
```

*[env].mysql.cnf:* - same format as any .my.cnf config file
```
[client]
host=localhost 
user=root 
password=root
[mysql]
database=devirataorg
```

*includes.list:*
```bash
DEV_MYSQL_CONF=dev.mysql.cnf
DEV_DB_NAME="devdb"
DEV_SSH="dev"
DEV_PATH="/var/www/website23"
BASE_DEST="/vagrant"
```

*~/.ssh/config*
```
  Host test
  hostname 0.0.0.0
  user username
  port 22
  identityfile ~/.ssh/id_rsa
```