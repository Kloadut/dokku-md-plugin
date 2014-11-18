MariaDB plugin for Dokku
------------------------

Project: https://github.com/progrium/dokku


Installation
------------
```
cd /var/lib/dokku/plugins
git clone --recursive https://github.com/Kloadut/dokku-md-plugin mariadb
cd mariadb/dockerfiles
git checkout master
dokku plugins-install
```


Commands
--------
```
$ dokku help
     mariadb:create <app>      Create a MariaDB container
     mariadb:delete <app>      Delete specified MariaDB container
     mariadb:info <app>        Display database informations
     mariadb:link <app> <db>   Link an app to a MariaDB database
     mariadb:console <app>     Open mysql-console to MariaDB container
     mariadb:dump <app> <file> Dump default db database into file <file> is optional. 
     mariadb:dumpraw <app>     Dump default db database to std out
     mariadb:logs <app>        Display last logs from MariaDB container
```

Info
--------
This plugin adds following envvars to your project automatically:

* DATABASE_URL
* DB_HOST
* DB_PORT
* DB_NAME
* DB_USER
* DB_PASSWORD

Simple usage
------------

Create a new DB:
```
$ dokku mariadb:create foo            # Server side
$ ssh dokku@server mariadb:create foo # Client side

-----> MariaDB container created: mariadb/foo

       Host: 172.16.0.104
       User: 'root'
       Password: 'RDSBYlUrOYMtndKb'
       Database: 'db'
       Public port: 49187
```

Deploy your app with the same name (client side):
```
$ git remote add dokku git@server:foo
$ git push dokku master
Counting objects: 155, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (70/70), done.
Writing objects: 100% (155/155), 22.44 KiB | 0 bytes/s, done.
Total 155 (delta 92), reused 131 (delta 80)
remote: -----> Building foo ...
remote:        Ruby/Rack app detected
remote: -----> Using Ruby version: ruby-2.0.0

... blah blah blah ...

remote: -----> Deploying foo ...
remote: 
remote: -----> App foo linked to mariadb/foo database
remote:        DATABASE_URL=mysql://root:RDSBYlUrOYMtndKb@172.16.0.104/db
remote: 
remote: -----> Deploy complete!
remote: -----> Cleaning up ...
remote: -----> Cleanup complete!
remote: =====> Application deployed:
remote:        http://foo.server
```


Advanced usage
--------------

Inititalize the database with SQL statements:
```
cat init.sql | dokku mariadb:create foo
```

Deleting databases:
```
dokku mariadb:delete foo
```

Linking an app to a specific database:
```
dokku mariadb:link foo bar
```

MariaDB logs (per database):
```
dokku mariadb:logs foo
```

Database informations:
```
dokku mariadb:info foo
```

Login to mariadb console
```
dokku mariadb:console
```

Import to existing database
```
dokku mariadb:console < import.sql
```

Copy database from one container to another
```
ssh -t dokku@SERVER1.COM mariadb:dumpraw foo | ssh -t dokku@SERVER2.COM mariadb:console foo
```
