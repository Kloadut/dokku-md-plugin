This fork adds DB_HOST,DB_PASS,DB_NAME,DB_PORT envvars.
It also adds command for connecting into MariaDB container console. So much easier to use than checking the url :)

MariaDB plugin for Dokku
------------------------

Project: https://github.com/progrium/dokku


Installation
------------
```
cd /var/lib/dokku/plugins
git clone https://github.com/onnimonni/dokku-md-plugin mariadb
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
     mariadb:dump <app> <file> Dump default db database into file
     mariadb:logs <app>        Display last logs from MariaDB container
```

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
