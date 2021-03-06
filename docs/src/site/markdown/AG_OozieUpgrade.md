

[::Go back to Oozie Documentation Index::](index.html)

# Oozie Upgrade

<!-- MACRO{toc|fromDepth=1|toDepth=4} -->

## Preparation

Make sure there are not Workflows in RUNNING or SUSPENDED status, otherwise the database upgrade will fail.

Shutdown Oozie and backup the Oozie database.

Copy the oozie-site.xml from your current setup.

## Oozie Server Upgrade

Expand the new Oozie tarball in a new location.

Edit the new `oozie-site.xml` setting all custom properties values from the old `oozie-site.xml`

IMPORTANT: From Oozie 2.x to Oozie 3.x the names of the database configuration properties have
changed. Their prefix has changed from `oozie.service.StoreService.*` to `oozie.service.JPAService.*`.
Make sure you are using the new prefix.

After upgrading the Oozie server, the `oozie-setup.sh` MUST be rerun before starting the
upgraded Oozie server.

Oozie database migration is required when there Oozie database schema changes, like
upgrading from Oozie 2.x to Oozie 3.x.

Configure the oozie-site.xml with the correct database configuration properties as
explained in the 'Database Configuration' section  in [Oozie Install](AG_Install.html).

Once `oozie-site.xml` has been configured with the database configuration execute the `ooziedb.sh`
command line tool to upgrade the database:


```
$ bin/ooziedb.sh upgrade -run

Validate DB Connection.
DONE
Check DB schema exists
DONE
Check OOZIE_SYS table does not exist
DONE
Verify there are not active Workflow Jobs
DONE
Create SQL schema
DONE
DONE
Create OOZIE_SYS table
DONE
Upgrade COORD_JOBS new columns default values.
DONE
Upgrade COORD_JOBS & COORD_ACTIONS status values.
DONE
Table 'WF_ACTIONS' column 'execution_path', length changed to 1024
DONE

Oozie DB has been upgraded to Oozie version '3.2.0'

The SQL commands have been written to: /tmp/ooziedb-5737263881793872034.sql

$
```

The new version of the Oozie server is ready to be started.

NOTE: If using MySQL or Oracle, copy the corresponding JDBC driver JAR file to the `libext/` directory before running
the `ooziedb.sh` command line tool.

NOTE: If instead using the '-run' option, the `-sqlfile <FILE>` option is used, then all the
database changes will be written to the specified file and the database won't be modified.

## Oozie Client Upgrade

While older Oozie clients work with newer Oozie server, to have access to all the
functionality of the Oozie server the same version of Oozie client should be installed
and used by users.

[::Go back to Oozie Documentation Index::](index.html)


