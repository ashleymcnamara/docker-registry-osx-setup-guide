# Docker Registry OS X Setup Guide

This guide will walk you through running the new Go based [Docker registry](https://github.com/docker/distribution) on your local OS X machine.

## Checkout the Docker Distribution source tree

```
mkdir -p $GOPATH/src/github.com/docker
git clone https://github.com/docker/distribution.git $GOPATH/src/github.com/docker/distribution
cd $GOPATH/src/github.com/docker/distribution
```

## Build the registry binary

```
GOPATH=$(PWD)/Godeps/_workspace:$GOPATH make binaries
sudo cp bin/registry /usr/local/libexec/registry
```

## Setup

Copy the registry configuration file in place:

```
sudo mkdir /etc/registry
sudo cp config.yml /etc/registry/config.yml
```

Create the local image storage directory:

```
sudo mkdir /var/lib/registry
```

## Running the Docker Registry under launchd

Copy the Docker registry plist into place:

```
plutil -lint com.docker.registry.plist
cp com.docker.registry.plist ~/Library/LaunchAgents/
chmod 644 /Library/LaunchAgents/com.docker.registry.plist
```

Start the Docker registry:

```
launchctl load ~/Library/LaunchAgents/com.docker.registry.plist
```

### Restarting the docker registry service

```
launchctl stop com.docker.registry
launchctl start com.docker.registry
```

### Unloading the docker registry service

```
launchctl unload ~/Library/LaunchAgents/com.docker.registry.plist
```
