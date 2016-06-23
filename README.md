
# travisCItest_silexDatabaseExample

This is an example project to illustrate PHPUnit MySQL database testing with TravisCI YAML settings for it all to work properlt

based on notes from:
https://docs.travis-ci.com/user/database-setup/#MySQL

Note - replace ```databaseName``` with the name of your database

## Step 1 - write your Travis YAML file: .travis.yml

```

    services:
       - mysql
      
    before_install:
       - mysql -e "create database IF NOT EXISTS databaseName;" -uroot
    
    before_script:
       - mysql -u root database_name < databaseName.sql
```

Each part explained:

```
    services:
       - mysql
```

services - this starts running the mysql server
(this will create a DB runing at 127.0.0.01 - which I assume is 'localhost' as usual)
2 users - 'root' (no password) with full priviedges and 'travis' (no password) less privilidges


```
    before_install:
       - mysql -e "create database IF NOT EXISTS databaseName;" -uroot
```

before install - this creates new empty DB



```
    before_script:
       - mysql -u root database_name < databaseName.sql
```


before script - this sets up DB table structures
(data should come from your seeds ...)

## Step 2 - edit your code's DB credentials to work with Travis

Set the following:

DB HOST = 'localhost' 
DB USER = 'root' 
DB PASS = '' empty string (no password) 
DB NAME = databaseName

## Step 3 - export the structure of your database as an SQL file (to the root of your project)

(these are steps for PHPMyAdmin)

- select your database (make sure it's the database that is selected and not one of the tables)
- click on Export on the menu bar, select the custom option and ensure format is SQL
- in the Tables section uncheck everything in the data column (we just want the structure)
-- make sure 'Save output to a file' is selected
-- in the Object Creation Options I turn off everything except the AUTO_INCREMENT and 'Enclose table and column names in backquotes' options (don't know if this really matters, the only one that could be a problem is Events since you may not have permission to create them on the server)
-- click Go

You should now have a .sql file that creates your tables, indexes and any constraints you set up.  

Copy this file into the root of your project (and add to git repo)

