# Elastic/Kibana Template Project

## Table of Contents
* [Prerequisites](#Prerequisites)
* [Setup your environment](#Setup-your-environment)
* [Tests](#Tests)
* [Deploy](#Deploy)
* [Issues](#Issues)
* [Roadmap](#Roadmap)

## Prerequisites

Run
```shell
sysctl vm.max_map_count=262144
```

and the following at the end of `/etc/sysctl.conf`:
```unit file (systemd)
vm.max_map_count=262144
```

## Setup your environment

```shell
docker-compose up
```

## Tests

*

## Deploy

Take a look at [README.md](./helm/README.md)

## Issues

Report any issue to this
[Github project](https://github.com/ursais/template/issues).

## Roadmap

*
