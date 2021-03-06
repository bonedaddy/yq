# yq

[![Build Status](https://travis-ci.com/mikefarah/yq.svg?branch=master)](https://travis-ci.com/mikefarah/yq) ![Docker Pulls](https://img.shields.io/docker/pulls/mikefarah/yq.svg) ![Github Releases (by Release)](https://img.shields.io/github/downloads/mikefarah/yq/total.svg) ![Go Report](https://goreportcard.com/badge/github.com/mikefarah/yq)


a lightweight and portable command-line YAML processor

The aim of the project is to be the [jq](https://github.com/stedolan/jq) or sed of yaml files.


## Major upgrade - V3 beta is out! 

This addresses a number of features requests and issues that have been raised :)

Currently only available only available as a [binary release here](https://github.com/mikefarah/yq/releases/tag/3.0.0-beta) or via docker mikefarah/yq:3.0.0-beta!

It does have a few breaking changes listed on the [release page](https://github.com/mikefarah/yq/releases/tag/3.0.0-beta)

Looking forward to feedback - once this is out of beta it will be added to the remaining package managers, and be the default version downloaded (and merged into master).

V2 will no longer have any new features added, and will be moved to a branch (v2). It will have limited maintenance for bugs for a few months.

## Install

### On MacOS:
```
brew install yq
```
### On Ubuntu and other Linux distros supporting `snap` packages:
```
snap install yq
```

#### Snap notes
`yq` installs with with [_strict confinement_](https://docs.snapcraft.io/snap-confinement/6233) in snap, this means it doesn't have direct access to root files. To read root files you can:

```
sudo cat /etc/myfile | yq r - a.path
```

And to write to a root file you can either use [sponge](https://linux.die.net/man/1/sponge):
```
sudo cat /etc/myfile | yq w - a.path value | sudo sponge /etc/myfile
```
or write to a temporary file:
```
sudo cat /etc/myfile | yq w - a.path value | sudo tee /etc/myfile.tmp
sudo mv /etc/myfile.tmp /etc/myfile
rm /etc/myfile.tmp
```

### On Ubuntu 16.04 or higher from Debian package:
```
sudo add-apt-repository ppa:rmescandon/yq
sudo apt update
sudo apt install yq -y
```
### or, [Download latest binary](https://github.com/mikefarah/yq/releases/latest) or alternatively:
```
GO111MODULE=on go get github.com/mikefarah/yq/v2
```

## Run with Docker

Oneshot use:

```bash
docker run --rm -v ${PWD}:/workdir mikefarah/yq yq [flags] <command> FILE...
```

Run commands interactively:

```bash
docker run --rm -it -v ${PWD}:/workdir mikefarah/yq sh
```

It can be useful to have a bash function to avoid typing the whole docker command:

```bash
yq() {
  docker run --rm -i -v ${PWD}:/workdir mikefarah/yq yq $@
}
```

## Features
- Written in portable go, so you can download a lovely dependency free binary
- Deep read a yaml file with a given path
- Update a yaml file given a path
- Update a yaml file given a script file
- Update creates any missing entries in the path on the fly
- Create a yaml file given a deep path and value
- Create a yaml file given a script file
- Prefix a path to a yaml file
- Convert from json to yaml
- Convert from yaml to json
- Pipe data in by using '-'
- Merge multiple yaml files where each additional file sets values for missing or null value keys.
- Merge multiple yaml files and override previous values.
- Merge multiple yaml files and append array values.
- Supports multiple documents in a single yaml file

## [Usage](http://mikefarah.github.io/yq/)

Check out the [documentation](http://mikefarah.github.io/yq/) for more detailed and advanced usage.

```
yq is a lightweight and portable command-line YAML processor. It aims to be the jq or sed of yaml files.

Usage:
  yq [flags]
  yq [command]

Available Commands:
  delete      yq d [--inplace/-i] [--doc/-d index] sample.yaml a.b.c
  help        Help about any command
  merge       yq m [--inplace/-i] [--doc/-d index] [--overwrite/-x] [--append/-a] sample.yaml sample2.yaml
  new         yq n [--script/-s script_file] a.b.c newValue
  prefix      yq p [--inplace/-i] [--doc/-d index] sample.yaml a.b.c
  read        yq r [--doc/-d index] sample.yaml a.b.c
  write       yq w [--inplace/-i] [--script/-s script_file] [--doc/-d index] sample.yaml a.b.c newValue

Flags:
  -h, --help      help for yq
  -t, --trim      trim yaml output (default true)
  -v, --verbose   verbose mode
  -V, --version   Print version information and quit

Use "yq [command] --help" for more information about a command.
```

## Contribute

**Note: v3 is currently in progress - for the moment I won't be accepting new feature PRs until v3 is ready :)**

1. `scripts/devtools.sh`
2. `make [local] vendor`
3. add unit tests
4. apply changes to go.mod
5. `make [local] build`
6. If required, update the user documentation
    - Update README.md and/or documentation under the mkdocs folder
    - `make [local] build-docs`
    - browse to docs/index.html and check your changes
7. profit
