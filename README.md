# Front End tools in a Docker Container (gulp, grunt, etc.)

## Build Status

[![Build Status](https://travis-ci.org/promet/fetools.svg?branch=develop)](https://travis-ci.org/promet/fetools)

## Overview

This project contains the source files for building the Docker image prometsource/fetools.

FETools is a container that contains gulp and grunt, and can be used for front end development on any Drupal development project.

The container is located on Docker Hub at:  https://hub.docker.com/r/prometsource/fetools

## Adding this toolset to your project

To add this toolset to your project, modify your docker-compose file to include the following:

```
version: '3.4'
...
services:
  fetools:
    build: docker/fetools
    environment:
      TZ: 'EST5EDT'
    depends_on:
      - <place the service name of your PHP container here>
    volumes:
      - node_modules:/data/themesrc/node_modules
      - ./themesrc/config:/data/themesrc/config
      - ./themesrc/gulpfile.js:/data/themesrc/gulpfile.js
      - ./themesrc/themes:/data/themesrc/themes
      - ./docroot:/data/docroot
...
volumes:
  ## persistent data volume for node_modules
  node_modules:
    driver: local
```

### Explanation of the 'volumes' used by the project

The volume mounts located in the /themesrc directory are explained in detail in the documents contained in the /docs folder.

The following volumes are used by this project:

#### node_modules

```
   - node_modules:/data/themesrc/node_modules
```
This volume is a local volume mount and is used to build your node_modules directory inside the
container.

#### ./themesrc/config

```
   - ./themesrc/config:/data/themesrc/config
```
This volume is a host volume mount, and contains the configuration folder of your project's /themesrc directory.

#### ./themesrc/gulpfile.js
```
   - ./themesrc/gulpfile.js:/data/themesrc/gulpfile.js
```

This script defines the "build" and "watch" tasks for your project.  This script is designed to be project
agnostic, but depends on project specific files located in the /themesrc subdirectory.  See the documentation in the /docs folder for more detail.

#### ./themesrc/themes
```
   - ./themesrc/themes:/data/themesrc/themes
```

This host directory should container your project specific files,a nd should mirror the `./docroot/themes` directory for any Drupal theme you want to build.  See the documentation in the /docs folder for more detail.

Please note that "./docroot" may change based on your project's directory structure.

#### ./docroot:/data/docroot
```
   - ./docroot:/data/docroot
```
This volume is the location of your drupal root installation.  The host mount can be changed to suite your project's specific directory structure, but the mapping inside the container should always map to `/data/docroot`.
