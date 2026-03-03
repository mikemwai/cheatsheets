# 🗄️ Databases 

## 🦉 Oracle
## Oracle Database Installation
- Add a swap space

```sh
  sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  echo '/swapfile none swap sw 0 0' >> /etc/fstab
  free -h
```

*`N/B:` This is for RAM that is less than 2GB since it prevents out-of-memory errors during installation.*

- Install Oracle Preinstall Package:

```sh
  dnf install -y oracle-database-preinstall-19c
```

*`N/B:` This installs all required RPM packages, sets kernel parameters, creates the oracle user and oinstall, dba, oper groups, and configures system limits in /etc/security/limits.conf*

- Assign the Oracle user to the required groups and set the password:

```sh
  usermod -g oinstall -G dba, oper oracle
  passwd oracle

  id oracle # Verify the groups have been assigned to the user
```

- Create the oracle directory structure and assign the correct ownership as root:

```sh
  mkdir -p /u01/app/oracle/product/19.0.0/dbhome_1
  mkdir -p /u01/app/oraInventory
  chown -R oracle:oinstall /u01
  chown -R 775 /u01
```

- Switch to the `oracle` user and configure the environment in `~/.bash_profile`:

```sh
  su - oracle
  vi ~/.bash_profile

  # Add the following content to the end of the file:
  export ORACLE_BASE=/u01/app/oracle
  export ORACLE_HOME=/u01/app/oracle/product/19.0.0/dbhome_1
  export ORACLE_SID=database_name
  export ORACLE_HOSTNAME=oracle8.local
  export ORA_INVENTORY=/u01/app/oraInventory
  export PATH=$ORACLE_HOME/bin:$PATH
  export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
  export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib

  # Apply the changes
  source ~/.bash_profile
  echo $ORACLE_HOME
```

*`N/B:` Edit the `database_name` parameter to the correct one.*

- Extract the Oracle software (This is after uploading it to your machine) into `ORACLE_HOME`:

```sh
  cd $ORACLE_HOME
  unzip Linux.x64_193000_db_home.zip

  # Ensure the correct ownership after extraction
  chown -R oracle:oinstall /u01/app/oracle/product/19.0.0/dbhome_1
  chmod -R 775 /u01/app/oracle/product/19.0.0/dbhome_1

  # Verify the runInstaller exists
  ls $ORACLE_HOME/runInstaller
```

- Run the silent installer as the oracle iser:

```sh
  export CV_ASSUME_DISTID=OEL8.0
  cd $ORACLE_HOME
  ./runInstaller -ignorePrereq -waitforcompletion -silent \
    -responseFile ${ORACLE_HOME}/install/response/db_install.rsp \
    oracle.install.option=INSTALL_DB_SWONLY \
    ORACLE_HOSTNAME=${ORACLE_HOSTNAME} \
    UNIX_GROUP_NAME=oinstall \
    INVENTORY_LOCATION=${ORA_INVENTORY} \
    SELECTED_LANGUAGES=en,en_GB \
    ORACLE_HOME=${ORACLE_HOME} \
    ORACLE_BASE=${ORACLE_BASE} \
    oracle.install.db.InstallEdition=EE \
    oracle.install.db.OSDBA_GROUP=dba \
    oracle.install.db.OSBACKUPDBA_GROUP=dba \
    oracle.install.db.OSDGDBA_GROUP=dba \
    oracle.install.db.OSKMDBA_GROUP=dba \
    oracle.install.db.OSRACDBA_GROUP=dba \
    SECURITY_UPDATES_VIA_MYORACLESUPPORT=false \
    DECLINE_SECURITY_UPDATES=true
```

- Switch to the root user and run the configuration scripts:

```sh
  /u01/app/oraInventory/orainstRoot.sh
  /u01/app/oracle/product/19.0.0/dbhome_1/root.sh
```

- Create the Database using DBCA as the oracle user:

```sh
  dbca -silent -createDatabase \
    -templateName General_Purpose.dbc \
    -gdbName database_name \
    -sid database_name \
    -responseFile NO_VALUE \
    -characterSet AL32UTF8 \
    -sysPassword password \
    -systemPassword password \
    -createAsContainerDatabase false \
    -databaseType MULTIPURPOSE \
    -memoryMgmtType AUTO_SGA \
    -totalMemory 800 \
    -storageType FS \
    -datafileDestination "${ORACLE_BASE}/oradata" \
    -redoLogFileSize 50 \
    -emConfiguration NONE \
    -ignorePreReqs
```

*`N/B:` Edit the `database_name` and `password` parameters to the correct ones.*

- Register the Database in oratab:

```sh
  echo "database_name:/u01/app/oracle/product/19.0.0/dbhome_1:Y" >> /etc/oratab
```

*`N/B:` Edit the `database_name` parameter to the correct one.*

- Verify the database instance is running:

```sh
  export ORACLE_SID=livedb
  sqlplus / as sysdba

  select instance_name, status, version from v$instance;
```

- Verify the database files:

```sh
  select name from v$datafile;
  select member from v$logfile;
  select tablespace_name, status from dba_tablespaces;
```

- Starting/ Stopping the database:

```sh
  export ORACLE_SID=database_name
  sqlplus / as sysdba

  startup # Start the DB
  shutdown immediate # Stop the DB
```

*`N/B:` Edit the `database_name` parameter to the correct one.*

- Checking the Database status:

```sh
  sqlplus / as sysdba

  select instance_name, status from v$instance;
```

- Connecting as a user:

```sh
  sqlplus user/password@database_name as sysdba
```

*`N/B:` Edit the `database_name` and `password` parameters to the correct ones.*
  
### Commands
- Check listener status:

```sh
  lsnrctl status
```

- Check where the cluster services are running from:
  
```sh
  crm status
```

- Switch to oracle:

```sh
  su - oracle
```

### OGG Command Interpreter
- Enter OGG command interpreter:

```sh
  g
```

- Check for OGG lag:

```sh
  info all
```

- View OGG lag info in detail:

```sh
  view report
```

- View more information about a specific process based on the group name:

```sh
  info group_name
```

## 🐬 SQL
### SQL Command Interpreter
- Connect to SQL:

```sh
  sqlplus / as sysdba
```

- Unlock a SQL account:

```sh
  alter user account_name account unlock;
```

- Change the password for a SQL account:

  ```sh
    alter user account_name identified by "new_password";
  ```
  
  - `N/B:` Ensure you follow the password policy.

- View the current hostname:

```sh
  !hostname
```

### Queries

---

## NoSQL (MongoDB)

### Commands
```sh
    show dbs;
```
```sh
   use dbname;
```
```sh
   use  mongotraining;
```
```sh
   db.createCollection('collectionname');
```
```sh
   show collections;
```

### Queries
#### Create
```sh
    db.collectionname.insertOne({"name": "Laptop", "rating": 4.5})
```
```sh
    db.collectionname.insertMany([{"name": "Laptop", "rating": 4.5},{"name": "Laptop", "rating": 4.5}]);
```

#### Find
```sh
    db.collectionname.find()
```
```sh
    db.collectionname.find({rating:{4.5})
```
```sh
    db.collectionname.find({rating:{$gt:4.5}})
```
```sh
    db.collectionname.find({category: 'Accessories'})
```
```sh
    db.collectionname.find({rating:{$gt:4.5}, category: 'Electronics'})
```
```sh
    db.collectionname.find().limit(3)
```

#### Update
```sh
    db.collectionname.updateOne({name: "Name1"}, {$set: {rating: 5.0}});
```
```sh
    db.collectionname.updateMany({category: "Electronics"}, {$set: {category: 'Devices'}});
```
```sh
    db.collectionname.updateMany({},{$set: {rating: 3.7}});
```

### Delete
```sh
    db.collectionname.deleteOne({price: 249.99})
```
```sh
    db.collectionname.deleteMany({price: {$lt: 650}})
```


