MYSQL-MARIADB-SNAPSHOT
=============================

Backups MySQL/MariaDB tables and views separately with mysqldump. Each table/structure as single file within a tgz archive.

## Features ##

* Dumps Tables+Views separately (two files for each database)
* Suitable to use with small Databases < 1GB
* Exclude Databases from backup
* Backups are stored as compressed `.tgz` archives
* Backup multiple database instances
* Cleanup of old backups using `find`
* Custom options
* Uses **debian-sys-maint** user or a dedicated backup account
* Suitable to use with MySQL or MariaDB (both shipped with mysqldump)
* Optional file encryption via `gpg` using a publickey file

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
MYSQL_BACKUPFILE=mysql.$NOW.tgz

# tmp dir
MYSQL_TMP_DIR=/tmp/mysqldump
...
```

## Schedule ##

### Setup cron for scheduled backups ###

Example: Run Backup every day at 4:15

Note: in case you want to use another account then **debian-sys-maint** it is **not required** to run this script as root!

```bash
echo "15  4  *  *  * root /usr/bin/mysql-mariadb-snapshot /etc/mysql-mariadb-snapshot/backup.conf" > /etc/cron.d/mysql-mariadb-snapshot
```

### Setup systemd for scheduled backups ###

mysql-mariadb-snapshot comes with a systemd service and timer. You can easily modify the scripts to match your needs (backup schedule, config path).

The service file is designed to work on per-instance base and uses the instance name as config file name `/etc/mysql-mariadb-snapshot/%I.conf`

**Defaults**

* `/lib/systemd/system/mysql-mariadb-snapshot.timer`
* `/lib/systemd/system/mysql-mariadb-snapshot.service`

**Example: Run Backup every day at 4:30**

File: `/etc/systemd/system/mysql-mariadb-snapshot.timer`

```ini
[Unit]
Description=mysql-mariadb-snapshot task daily

[Timer]
# Run at 04:30 am
OnCalendar=04:30

# do NOT start backup immediately if its missed (servers..)
Persistent=false

# linked unit instance -> use config file `/etc/mysql-mariadb-snapshot/backup.conf`
Unit=mysql-mariadb-snapshot@backup.service

[Install]
WantedBy=timers.target
```

## Usage ##

```
mysql-mariadb-snapshot <config>
```

## Backup Encryption ##

The backup - this means every single file within the tar archive - can optionally encrypted using a **gnupg** publickey. You should be familar with gnupg/openpgp before using this feature - otherwise the backups may become inaccesible. 

**Export a public key**

```
gpg \
    --no-options \
    --export \
    "246C BBDF 5D82 D7CC 786B  6D6D 3693 5688 EC2C 82E9" > publickey.gpg
```

**Configuration**

```bash
...

# optional gpg encryption
GPG_RECIPIENT="246C BBDF 5D82 D7CC 786B  6D6D 3693 5688 EC2C 82E9"
GPG_PUBLICKEY="/etc/mariadb/backupkey.gpg"
```

## Distributions ##

Tested with:

* Debian jessie
* Debian stretch
* Debian buster

## Contribution ##

The **.deb** package is automatically generated via a **Continuous Delivery Pipeline** - please do not build packages manually!

## License ##
MYSQL-MARIADB-SNAPSHOT is OpenSource and licensed under the Terms of [Mozilla Public License 2.0](https://opensource.org/licenses/MPL-2.0). You're welcome to [contribute](docs/CONTRIBUTING.md)