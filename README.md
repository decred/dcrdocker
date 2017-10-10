# dcrdocker

Dockerfiles for Decred.

## General info

This repo contains a number of docker files used by decred for various purposes.

The main goal is to keep the images simple and understandable.  Although we would like to keep images small when possible, simplicity/readability is to be preferred over size in all cases.  For example, this means it is better to use env variables for settings to be in one place even if it requires more than one `RUN` directive in the Dockerfile.  Whenever possible images should import from each other so we have as close to a known base as possible.  When possible, the images strive to be self-contained with as much data as possible set at build time (and persistant data mounted from host only).

## Available Images

The table below gives link to all images but a brief description is useful first.  There is a mainnet and testnet dcrd image with no special settings (these can be set with a local dcrd.conf file).  There are also images based on those that have the settings needed for insight already included.  Mainnet and a testnet insight images are provided and are meant to be run on the same machine as the dcrd image they use for blockchain data.

Images for running a full build and test of dcrd, dcrwallet, or any of the go projects are provided.  These are generally run in travis but this way the setup can be run identically locally and from travis.  Unlike the other images which are based on a recent Ubuntu, these are based on the offical go images.  They must be modified if a new minor version of go is needed and a new one added for a new major version.  Both opperations are described below.

The decrediton builder image allows us to check the build and lint in travis and allows for an easy build during development although we do not use the artifacts it creats for releases.  It would be a nice improvement if the node/npm dependancies could be included in the built image (would speed up builds and make them more reproducable).  It is possible something like npm-cache-install could be useful there.


| Dockerfile | Image Name | Link | Description | pull command |
| --- | --- | --- | --- | --- |
|Dockerfile-1.7|decred-golang-builder-1.7|[docker hub](https://hub.docker.com/r/decred/decred-golang-builder-1.7/)|Build/test with go1.7.|docker pull decred/decred-golang-builder-1.7|
|Dockerfile-1.8|decred-golang-builder-1.8|[docker hub](https://hub.docker.com/r/decred/decred-golang-builder-1.8/)|Build/test with go1.8.|docker pull decred/decred-golang-builder-1.8|
|Dockerfile-1.9|decred-golang-builder-1.9|[docker hub](https://hub.docker.com/r/decred/decred-golang-builder-1.9/)|Build/test with go1.9.|docker pull decred/decred-golang-builder-1.9|
|Dockerfile-dcrd|dcrd-mainnet|[docker hub](https://hub.docker.com/r/decred/dcrd-mainnet/)|Mainnet dcrd.|docker pull decred/dcrd-mainnet|
|Dockerfile-dcrd-testnet|dcrd-testnet|[docker hub](https://hub.docker.com/r/decred/dcrd-testnet/)|Testnet dcrd.|docker pull decred/dcrd-testnet|
|Dockerfile-dcrd-insight|dcrd-mainnet-insight|[docker hub](https://hub.docker.com/r/decred/dcrd-mainnet-insihght/)|Mainnet dcrd for insight use.|docker pull decred/dcrd-mainnet-insight|
|Dockerfile-dcrd-testnet-insight|dcrd-testnet-insight|[docker hub](https://hub.docker.com/r/decred/dcrd-testnet-insight/)|Testnet dcrd for insight use.|docker pull decred/dcrd-testnet-insight|
|Dockerfile-decrediton|decrediton-builder|[docker hub](https://hub.docker.com/r/decred/decrediton-builder/)|Builder image for decrediton|docker pull decred/decrediton-builder|
|Dockerfile-insight|decred-insight|[docker hub](https://hub.docker.com/r/decred/decred-insight/)|Decred insight on mainnet.|docker pull decred/decred-insight|
|Dockerfile-insight-testnet|decred-insight-testnet|[docker hub](https://hub.docker.com/r/decred/decred-insight-testnet/)|Decred insight on testnet.|docker pull decred/decred-insight-testnet|
|Dockerfile-slackinviter|decred-slackinviter|N/A|Slack Invitation Gateway| N/A |

## Building images

The images are built and pushed to the docker hub repo as follows (the names can be taken from the table above):
```
DOCKER_IMAGE_TAG=dcrd-mainnet
docker build -t $DOCKER_IMAGE_TAG -f Dockerfile-dcrd .
docker tag $DOCKER_IMAGE_TAG decred/$DOCKER_IMAGE_TAG
docker push decred/$DOCKER_IMAGE_TAG
```

## Using the images

All of the images have usage instructions at the top of the dockerfile.  Just pull the image:
```
docker pull decred/$DOCKER_IMAGE_TAG
```
then follow the instructions in the dockerfile.  When necessary, a script to run it is included in the repo where it is needed.

### Updating docker go versions

When a new minor version of go is released, the docker images must be updated and rebuilt to support it.  For example, when go1.8.3 was released, the file `Dockerfile-1.8` was edited by changing `FROM golang:1.8.2` to `FROM golang:1.8.3`.  Then the image was rebuilt and pushed to docker hub:
```
GOVERSION=1.8
DOCKER_IMAGE_TAG=decred-golang-builder-$GOVERSION
docker build -t $DOCKER_IMAGE_TAG -f ./Dockerfile-$GOVERSION .
docker tag $DOCKER_IMAGE_TAG decred/$DOCKER_IMAGE_TAG
docker push decred/$DOCKER_IMAGE_TAG
```

For a new major version, a new file `Dockerfile-VERSION` must be created and then the previous steps can be followed.  That new version must be added to the travis build matrix to run the tests in travis.
