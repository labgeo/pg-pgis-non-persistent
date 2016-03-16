pg-pgis-non-persistent
=========================

Build a non persistent docker image containing PostgreSQL 9.5 + PostGIS 2.1.8. This may be particularly useful when you need to commit a running docker with attached data.

This Docker image is a container is based on Ubuntu 14.04 and features:

* PostgreSQL 9.5 (from PGDG packages)
* PostGIS 2.1.8 and GEOS

It won't create any database, but you can use a labgeo superuser (password labgeo), with postgis. This Docker is aimed at tests and development. Do not use it for production purposes. It lacks security and is not micro-service oriented as should a Docker stack be. Use at your own risk.


Build and/or run the container
------------------------------

You can build this dockerfile directly from github:

```
docker build https://github.com/labgeo/pg-pgis-non-persistent.git
```

Or pull it directly compiled from Docker Hub:

```
docker pull benizar/pg-pgis-non-persistent
```

Then, run the image using an appropiate tag:

```
docker run -d -p 5433:5432 --name pgis-non-persistent_test benizar/pg-pgis-non-persistent:9.5-2.1.8
```

Finally, test if postgresql is running with psql. Create a new geodatabase with a basic setup:

```
PGPASSWORD=labgeo psql -h localhost -p 5436 -U labgeo -d postgres -w <<EOSQL
CREATE DATABASE siose2005
WITH OWNER "labgeo"
ENCODING 'UTF8'
TEMPLATE template0;

\c siose2005
CREATE EXTENSION postgis;

CREATE SCHEMA IF NOT EXISTS relational;
CREATE SCHEMA IF NOT EXISTS jsonb;
CREATE SCHEMA IF NOT EXISTS grids;
CREATE SCHEMA IF NOT EXISTS reports;
\q
EOSQL
```

