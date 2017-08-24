MYSQL-MARIADB-SNAPSHOT
=============================

Backups MySQL/MariaDB tables and views separately with mysqldump. Each table/structure as single file within a tgz archive.

## Features ##

* Dumps Tables+Views separately (two files for each database)
* Suitable to use with small Databases < 1GB
* Exclude Databases from backup
* Backups are stored as compressed `.tar.bz2` archives
* Backup multiple database instances
* Cleanup of old backups using `find`
* Custom options
* Uses **debian-sys-maint** user or a dedicated backup account
* Suitable to use with MySQL or MariaDB (both shipped with mysqldump)

## Package Installation ##

### 1. Add Aenon-Dynamics Repository ###

See [AenonDynamics/CPR](https://github.com/AenonDynamics/CPR#debian-packages)

### 2. Install the Package ###

```
apt-get update
apt-get install mysql-mariadb-snapshot
```

### 3. Configure the Backup Destination ###

**Store the backups in /backup/mysql**

```bash
mkdir -p /backup/mysql
```

Edit `/etc/mysql-mariadb-snapshot/backup.conf`

```bash
# bin locations
MYSQL_BIN=/usr/bin/mysql
MYSQLDUMP_BIN=/usr/bin/mysqldump

# backup location
MYSQL_BACKUP_LOCATION=/backup/mysql

# lifetime in days
MYSQL_BACKUP_LIFETIME=200

# filename - $NOW is defined as $(date +"%d-%m-%Y")
MYSQL_BACKUPFILE=mysql.$NOW.tar.bz2

# tmp dir
MYSQL_TMP_DIR=/tmp/mysqldump
...
```

### 4. Setup cron for scheduled backups ###

Example: Run Backup every day at 4:15

Note: in case you want to use another account then **debian-sys-maint** it is **not required** to run this script as root!

```bash
echo "15  4  *  *  * root /usr/bin/mysql-mariadb-snapshot /etc/mysql-mariadb-snapshot/backup.conf" > /etc/cron.d/mysql-mariadb-snapshot
```

## Usage ##

```
mysql-mariadb-snapshot <config>
```

## Distributions ##

Tested with:

* Debian Jessie 8.7

## Contribution ##

The **.deb** package is automatically generated via a **Continuous Delivery Pipeline** - please do not build packages manually!

## License ##
MYSQL-MARIADB-SNAPSHOT is OpenSource and licensed under the Terms of [The MIT License (X11)](http://opensource.org/licenses/MIT) - your're welcome to contribute