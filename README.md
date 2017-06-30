# dcrdocker

Dockerfiles for Decred.

## Available Images


| Dockerfile | Image Name | Link | Description | pull command |
| --- | --- | --- | --- | --- |
|Dockerfile-dcrd|dcrd-mainnet|[docker hub](https://hub.docker.com/r/decred/dcrd-mainnet/)|Mainnet dcrd for insight use.|docker pull decred/dcrd-mainnet|
|Dockerfile-insight|decred-insight|[docker hub](https://hub.docker.com/r/decred/decred-insight/)|Decred insight on mainnet.|docker pull decred/decred-insight|
|Dockerfile-dcrd-testnet|dcrd-testnet|[docker hub](https://hub.docker.com/r/decred/dcrd-testnet/)|Testnet dcrd for insight use.|docker pull decred/dcrd-testnet|
|Dockerfile-insight|decred-insight-testnet|[docker hub](https://hub.docker.com/r/decred/decred-insight/)|Decred insight on testnet.|docker pull decred/decred-insigh-testnet|
|Dockerfile-1.7|decred-golang-builder-1.7|[docker hub](https://hub.docker.com/r/decred/decred-golang-builder-1.7/)|Build/test with go1.7.|docker pull decred/decred-golang-builder-1.7|
|Dockerfile-1.8|decred-golang-builder-1.8|[docker hub](https://hub.docker.com/r/decred/decred-golang-builder-1.8/)|Build/test with go1.8.|docker pull decred/decred-golang-builder-1.8|
|Dockerfile-decrediton|decrediton-builder|[docker hub](https://hub.docker.com/r/decred/decrediton-builder/)|Builder image for decrediton|docker pull decred/decrediton-builder|

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
