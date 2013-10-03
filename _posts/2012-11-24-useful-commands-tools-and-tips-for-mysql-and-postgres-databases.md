---
layout: post
title: Useful commands, tools and tips for MySQL and Postgres databases
tags:
- mysql
- postgres
- databases
- optimization
---
There are some quite useful commands I had to collect from many different
sources which can come in handy when working with SQL databases like MySQL and
Postgres.


## Postgres

### Useful SQL statements

Show the maximum number of possible connections: SELECT name, setting FROM
pg_settings WHERE name = 'max_connections';

Show the active connections: SELECT * from pg_stat_activity;

Show the number of active connections: SELECT COUNT(\*) from pg_stat_activity;

Log in to database via terminal (with password): psql -h HOST -p 5432 -d
DATABASE_NAME -U USER_NAME -W


### Heroku Postgres App

The Mac OS X Postgres app from Heroku saves you a lot of hassle. Just download
the app and drop it into your Applications folder and you are ready to go ->
[http://postgresapp.com/](http://postgresapp.com/)

When using this app, the postgresql.conf can be found in
~/Library/Application\ Support/Postgres/var/postgresql.conf


### Postgres - Bumping up the maximum number of connections on nix-systems

The value for the maximum number of connections can be found in the
aforementioned postgresql.conf file. It's called "max_connections". If you
want to bump it up, you must also consider increasing the "shared_buffers"
value. I currently have 200MB of shared buffers and 70 maximum allowed
connections. You will have to restart the Postgres app then. If it refuses to
start, it's because your system's kernel values for shared memory is not high
enough. In order to adjust that, use the following commands in your terminal:

**sudo sysctl -w kern.sysv.shmall=1073741824**

**sudo sysctl -w kern.sysv.shmmax=1073741824**

This will bump up the shared memory to a maximum of 1024MB. SHMALL refers to
the total amount of shared memory pages that can be used system wide. If you
want to dig deeper, then read this: [http://www.puschitz.com/TuningLinuxForOra
cle.shtml#SettingSharedMemory](http://www.puschitz.com/TuningLinuxForOracle.sh
tml#SettingSharedMemory)


### **Use Postgres as an in-memory database**

The following changes to your Postgres config are especially useful for local
development. All those settings are related to the WAL (Write-Ahead-Log).
Write-Ahead-Logging consists of a couple of techniques that provide atomicity
and durability which are two of the ACID properties.


Go back to your postgresql.conf and change those values:

fsync=off

full_page_writes=off

synchronous_commit=off

bgwriter_lru_maxpages=0

**fsync **usually tries to make sure that updates are written to disk. This enables to recover to a consistent state after a system/hardware crash. **ATTENTION: If you turn it off it can result in unrecoverable data corruption!**

**full_page_writes **will write the entire content of each disk page to the WAL (Write-Ahead-Log) during the first modification of that page after a checkpoint. It's needed because a page write that is in process during an operating system crash might be only partially completed. **AGAIN: It might speed up the database to turn this off, but if you do, it could lead to unrecoverable data corruption after a system/hardware crash!**

**synchronous_commit **specifies whether a transaction commit will wait for WAL records to be written to disk before the command returns a "success" indication to the client. It's not too critical if you turn this off and the system crashes. It would just result in some committed transactions to be lost.

**bgwriter_lru_maxpages **is related to the background writer. This is a separate server process which issues writes of dirty (which means new or modified) shared buffers. This value adjusts how many buffers will be written by the aforementioned background writer. Setting it to zero will disable it completely.

Have a look at this article, if you want to populate your database with a
large amount of data: [http://www.postgresql.org/docs/9.2/static/populate.html
](http://www.postgresql.org/docs/9.2/static/populate.html)


## Mysql

### Useful SQL statements

Show the maximum number of possible connections: SHOW variables LIKE '%conn%';

Show the active connections: SHOW process list;

Show the number of active connections: SELECT COUNT(id) FROM
information_schema.processlist;

Log in to database via terminal (with password): mysql -u USER_NAME -p -h HOST



#### Sources

  * [http://www.postgresql.org/docs/9.2/static/populate.html](http://www.postgresql.org/docs/9.2/static/populate.html)
  * [http://www.postgresql.org/docs/9.2/static/runtime-config-wal.html#GUC-FULL-PAGE-WRITES](http://www.postgresql.org/docs/9.2/static/runtime-config-wal.html#GUC-FULL-PAGE-WRITES)
  * [http://www.postgresql.org/docs/9.2/static/wal-internals.html](http://www.postgresql.org/docs/9.2/static/wal-internals.html)
  * [http://www.postgresql.org/docs/9.2/static/runtime-config-resource.html](http://www.postgresql.org/docs/9.2/static/runtime-config-resource.html)
  * [http://en.wikipedia.org/wiki/Write-ahead_logging](http://en.wikipedia.org/wiki/Write-ahead_logging) 


