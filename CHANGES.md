### 1.0.0 ###
* Added: support for tcp connections
* Added: custom transport settings via `MYSQL_TRANSPORT`
* Added: optional asymmetric gpg encryption via publickey file
* Changed: added file check to ensure that existing backup files are not overwritten
* Changed: compression to `gzip` (20% larger files, up to 10 times faster)
* Changed: license to MPL-2.0 https://mozilla.org/MPL/2.0/

### 0.4.0 ###
* Added: systemd service and timer

### 0.3.0 ###
* Added: Separate trigger/routines dump
* Changed: Extended create table statements are used (autoincrement values, charsets)
* Changed: Moved config files to `config/`

### 0.2.0 ###
* Added: Section/Priority Entries
* Changed: General Description

### 0.1.1 ###
* Added: Changelog

### 0.1.0 ###
* Initial Release