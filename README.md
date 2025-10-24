# Postgresql Install Guide
Documentation on Postgresql Installation

Ubuntu Noble 24.04
Postgresql Version 17

17 needs to be used because at the time of writing TimescaleDB has not produced an extension built for PG18 - appears to be in development. 

Disclaimer
    Remember all extensions need to be made on the applied database but never on the Postgres Database itself. 

If working on a container - remember to add a User with sudo permissions rather than use root. 



## Ubuntu Prepare 

`apt get update`

`apt list --upgradable`

## Postgresql Install

`sudo apt install gnupg postgresql-common apt-transport-https lsb-release wget`

`sudo apt install postgresql-17`

`sudo systemctl start postgresql`

`sudo systemctl enable postgresql`

If you need to open Postgresql for connections outside of localhost the conf file needs to be edited to permit access. 

`sudo nano /etc/postgresql/17/main/postgresql.conf`

Change 

`listen_addresses='localhost'` to  `listen_addresses = '*'`

`sudo ufw allow 5432/tcp`

`reboot`


## TimeScaleDB Installation 
Is from their site only
We need to add their repo's and pull from those directly. 
Review this page - 
https://docs.tigerdata.com/self-hosted/latest/install/installation-linux/ 
https://packagecloud.io/timescale/timescaledb - contains scripts for each type 


`sudo apt install timescaledb-2-postgresql-17 postgresql-client-17`

`sudo timescaledb-tune`

## PostGis - available under generic Ubuntu Repo's

`apt search postgresql-17 | grep postgis`

`apt install postgresql-17-postgis-3/noble-pgdg`

`sudo systemctl restart postgresql`

## PGRouting - available under generic Ubuntu Repo's

`apt search postgresql-17 | grep pgrouting`

`apt install postgresql-17-pgrouting/noble-pgdg`

`sudo systemctl restart postgresql`




#### Useful Links

https://dev.to/johndotowl/postgresql-17-installation-on-ubuntu-2404-5bfi

https://docs.tigerdata.com/self-hosted/latest/install/installation-linux/

https://www.prisma.io/docs/orm/more/help-and-troubleshooting/dataguide/connecting-to-postgresql-databases

https://harish2k01.in/create-and-delete-users-in-proxmox-lxc/

https://www.tigerdata.com/blog/top-8-postgresql-extensions

https://trac.osgeo.org/postgis/wiki/UsersWikiPostGIS3UbuntuPGSQLApt

https://packagecloud.io/timescale/timescaledb

Adminer Setup