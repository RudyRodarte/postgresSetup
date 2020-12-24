# Installing Postgres on Linux Mint
The following are the steps I took in installing Postgres on Linux Mint

## Download and Install
Followed the directions from the offical postgres site here: https://www.postgresql.org/download/linux/ubuntu/  

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo apt-get update

sudo apt-get -y install postgresql
```
# Connecting to the Newly Created Database Server
It looks like by default, there is a user, which they call a **role,** created named postgres. With this default user, we make the initial connection. 
## Making the First Connection
We first switch to the postgres user in the terminal.  
```
sudo -i -u postgres
```
Next, you run the psql interpreter. 
```
psql
``` 
It seems that running the **psql** command assumes that:  
1. The database is local
2. The database us running on the default psql port: **5432**
3. The current user has a login (role) on the target database server.
   1.Could use **-U** to specify a username.
4. The user is already authenticated.
   1. Could use **-W** to force a password prompt.  
## Inside of postgres
Once you are connected, the terminal prompt changes to the postgres prompt.
```
postgres=#
```
You can quit and exit back to the terminal at anytime using the quit command.
```
postgres=# \q
```
At this point, there are no databases on the server. There are also no users, besides the postgres user. 

Followed most of the steps from here.
https://www.postgresqltutorial.com/install-postgresql-linux/

# Using a Sample Database
At this point I dont know much about what sample databases exist for Postgres. In the SQL Server world, we have Northwind, AdventureWorks, and WideWorldImporters. But I haven't yet found an equivalent for Postgres. The link above has a **dvdrental** database that I'll install for testing.
## Downloading the Sample Database
We'll download a backup of the dvd rental database using the Linux **curl** command.
```
curl -O https://sp.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip
```  
Next, we have to unzip the file. Notice, this will unzip into whatever directory you're currently in.  
```
unzip dvdrental.zip
```
The zipped file contains the database backup, **dvdrental.tar**.  

## Creating the Sample Database
The next step is to create the sample database inside of postgres and restore the backup. 

From the terminal, log back into psql and create the database named **dvdrental**, and exit psql.
```
psql
postgres=# create database dvdrental;
postgres=# \q
```  

Next, restore the backup using the pg_restore command.
```
pg_restore --dbname=dvdrental --verbose dvdrental.tar
```
## File Locations

After the restore, the database now now exists on the server. 
With my current, default settings the datafile resides here:
```
/var/lib/postgresql/12/main/base
```  
From what I can tell, the sub folders are database ids, similar to *db_id()* in SQL Server.  I don't yet know how to find the database id 

The WAL file, similar to the SQL Server log files, reside here:
```
/var/lib/postgresql/12/main/pg_wal
```
I'll have to look and see where to set this in the configuration files. 