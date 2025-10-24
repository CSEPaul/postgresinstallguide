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

Edit the pg_hba.conf file - Log postgres

`sudo -u postgres psql`

Ask postgres where the file is

`SHOW hba_file;`

CD to that directory i.e.

`cd /etc/postgresql/17/main/ `

`nano pg_hba.conf`

Edit the file as follows. 

Add the following line as the first line of pg_hba.conf. It allows access to all databases for all users with an encrypted password:

`# TYPE DATABASE USER CIDR-ADDRESS  METHOD`

`host  all  all 0.0.0.0/0 scram-sha-256`

`service postgresql restart`


Edit the firewall

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



## Create Database and User
`sudo -u postgres psql`

`CREATE DATABASE mydb;`

`CREATE USER myuser WITH ENCRYPTED PASSWORD 'mypass';`

`ALTER USER myuser WITH SUPERUSER;`

`GRANT ALL PRIVILEGESS ON DATABASE mydb TO myuser;`


## Install extension to mydb
Log in as new user

`psql -d "postgres://myuser:myuserpassword@127.0.0.1:5432/mydb"`

`CREATE EXTENSION IF NOT EXISTS timescaledb;`

`ALTER DATABASE mydb SET search_path=public,postgis,contrib;`

`\connect mydb;`

`CREATE SCHEMA postgis;`

`CREATE EXTENSION postgis SCHEMA postgis;`

`SELECT postgis_full_version();`

`CREATE  EXTENSION pgrouting SCHEMA postgis;`

`SELECT * FROM pgr_version();`


#### Useful Links

https://dev.to/johndotowl/postgresql-17-installation-on-ubuntu-2404-5bfi

https://docs.tigerdata.com/self-hosted/latest/install/installation-linux/

https://www.prisma.io/docs/orm/more/help-and-troubleshooting/dataguide/connecting-to-postgresql-databases

https://harish2k01.in/create-and-delete-users-in-proxmox-lxc/

https://www.tigerdata.com/blog/top-8-postgresql-extensions

https://trac.osgeo.org/postgis/wiki/UsersWikiPostGIS3UbuntuPGSQLApt

https://packagecloud.io/timescale/timescaledb

# Adminer Setup
A very useful database UI tool
Best off setting it up on its own container 
2GB
Ubuntu Latest 

## PHP Install 

`apt install apache2 php php-fpm php-curl libapache2-mod-php php-cli php-mysql php-gd -y`


## Apache Install

`systemctl enable --now apache2`

`systemctl status apache2`

## Adminer Install

`apt install adminer -y`

`a2enconf php*-fpm`

`a2enconf adminer`

`systemctl restart apache2`



