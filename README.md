# Docker GeoServer image

This docker GeoServer image is based on the following proposal:
 
https://github.com/geoserver/geoserver/wiki/GSIP-192

Work is still in progress!

## How it works

1. The [Dockerfile](Dockerfile)
    1. installs dependencies
    1. installs the GeoServer by downloading and extracting the war file
    1. defines defaults for environment variables
1. The [entrypoint.sh](scripts/entrypoint.sh) startup script (in a running container)
    1. executes [install-extensions.sh](scripts/install-extensions.sh) to download and install GeoServer extensions based on the `STABLE_EXTENSIONS` environment variable.
    1. handles the `GEOSERVER_OPTS`
    1. starts the tomcat

## Building

`docker build -t geoserver:test .`

## Running

`docker run -it -e STABLE_EXTENSIONS='wps,csw' -p 8080:8080 geoserver:test`

The extensions will be downloaded on startup of the image (before starting the tomcat).

## Configuration

Pass as environment variables. If not passed, the default values will be used.

* `GEOSERVER_DATA_DIR` (default: /opt/geoserver_data)
* `INITIAL_MEMORY` (default: 2G)
* `MAXIMUM_MEMORY` (default: 4G)
* `JAIEXT_ENABLED` (default: true)
* `STABLE_EXTENSIONS` as comma separated list, will be downloaded and installed on startup (default: "")

## TODOs

* CORS
* configuration of JNDI connections in the tomcat/custom tomcat configuration in general
* default data for gs datadir?
* possibility to add custom java dependencies (for example the oracle jdbc driver)
  * in this context: mount `EXTENSION_DOWNLOAD_DIR` and implement checks to cache extensions that have already been downloaded before?
* log4j properties
* add possibility to add custom fonts
* starting from which version we want to provide geoserver images (maybe 2.15.x?)/backwards compatability
* add a docker-compose demo environment?
