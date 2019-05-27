# cloudscale.ch Docker machine driver

[![Go Report Card](https://goreportcard.com/badge/github.com/cloudscale-ch/docker-machine-driver-cloudscale)](https://goreportcard.com/report/github.com/cloudscale-ch/docker-machine-driver-cloudscale)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Build Status](https://api.travis-ci.com/cloudscale-ch/docker-machine-driver-cloudscale.svg?branch=master)](https://travis-ci.com/cloudscale-ch/docker-machine-driver-cloudscale)

> This library adds the support for creating [Docker machines](https://github.com/docker/machine) hosted on the [cloudscale.ch Cloud](https://www.cloudcale.ch).

Based on the Work of:
* https://github.com/JonasProgrammer/docker-machine-driver-hetzner
* https://github.com/splattner
* https://github.com/cloudscale-ch/machine

## Installation

You can find sources and pre-compiled binaries [here](https://github.com/cloudscale-ch/docker-machine-driver-cloudscale/releases).

```bash
# Download the binary (this example downloads the binary for linux amd64)
$ wget https://github.com/cloudscale-ch/docker-machine-driver-cloudscale/releases/download/v0.0.1/docker-machine-driver-cloudscale_v0.0.1_linux_amd64.tar.gz
$ tar -xvf docker-machine-driver-cloudscale_v0.0.1_linux_amd64.tar.gz

# Make it executable and copy the binary in a directory accessible with your $PATH
$ chmod +x docker-machine-driver-cloudscale
$ cp docker-machine-driver-cloudscale /usr/local/bin/
```

## Usage

```bash
$ docker-machine create \
  --driver cloudscale \
  --cloudscale-token=... \
  --cloudscale-image=ubuntu-18.04 \
  --cloudscale-flavor=flex-4
  some-machine
```

### Using environment variables

```bash
$ CLOUDSCALE_TOKEN=... \
  && CLOUDSCALE_IMAGE=ubuntu-18.04 \
  && CLOUDSCALE_FLAVOR=flex-4
  && docker-machine create \
     --driver cloudscale \
     some-machine
```   


### Using Cloud-init

```bash
$ CLOUD_INIT_USER_DATA=`cat <<EOF
#cloud-config
write_files:
  - path: /test.txt
    content: |
      Here is a line.
      Another line is here.
EOF
`

$ docker-machine create \
  --driver cloudscale \
  --cloudscale-token==... \
  --cloudscale-userdata="${CLOUD_INIT_USER_DATA}" \
  some-machine
```


## Options

- `--cloudscale-token`: **required**. Your project-specific access token for the cloudscale.ch API.
- `--cloudscale-image`: The slug of the cloudscale.ch image to use, see [Images API](https://www.cloudscale.ch/en/api/v1#images) for how to get a list (defaults to `ubuntu-18.04`).
- `--cloudscale-flavor`: The flavor of the cloudscale.ch server, see [Flavor API](https://www.cloudscale.ch/en/api/v1#flavors) for how to get a list (defaults to `flex-4`).
- `--cloudscale-volume-size-gb`: The size of the root volume in GB.
- `--cloudscale-ssh-user`: The SSH User (defaults to `root`)
- `--cloudscale-ssh-port`: The SSH Port (defaults to `22`)
- `--cloudscale-use-private-network`: Enables the Private network Interface (defaults to `false`)
- `--cloudscale-use-ipv6`: Enables IPv6 on Public Network Interface (defaults to `false`)
- `--cloudscale-server-groups`: the UUIDs of server groups identifying the [server groups](https://www.cloudscale.ch/en/api/v1#server-groups) to which the new server will be added.
- `--cloudscale-anti-affinity-with`: the UUID of another server to create an anti-affinity group with that server or add it to the same group as that server.



## Building from source

Use an up-to-date version of [Go](https://golang.org/dl) and [dep](https://github.com/golang/dep)

To use the driver, you can download the sources and build it locally:

```shell
# Get sources and build the binary at ~/go/bin/docker-machine-driver-cloudscale
$ go get github.com/cloudscale-ch/docker-machine-driver-cloudscale

# Make the binary accessible to docker-machine
$ export GOPATH=$(go env GOPATH)
$ export GOBIN=$GOPATH/bin
$ export PATH="$PATH:$GOBIN"
$ cd $GOPATH/src/cloudscale-ch/docker-machine-driver-cloudscale
$ dep ensure
$ go build -o docker-machine-driver-cloudscale
$ cp docker-machine-driver-cloudscale /usr/local/bin/docker-machine-driver-cloudscale
```

## Development

Fork this repository, yielding `github.com/<yourAccount>/docker-machine-driver-cloudscale`.

```shell
# Get the sources of your fork and build it locally
$ go get github.com/<yourAccount>/docker-machine-driver-cloudscale

# * This integrates your fork into the $GOPATH (typically pointing at ~/go)
# * Your sources are at $GOPATH/src/github.com/<yourAccount>/docker-machine-driver-cloudscale
# * That folder is a local Git repository. You can pull, commit and push from there.
# * The binary will typically be at $GOPATH/bin/docker-machine-driver-cloudscale
# * In the source directory $GOPATH/src/github.com/<yourAccount>/docker-machine-driver-cloudscale
#   you may use go get to re-build the binary.
# * Note: when you build the driver from different repositories, e.g. from your fork
#   as well as github.com/cloudscale-ch/docker-machine-driver-cloudscale,
#   the binary files generated by these builds are all called the same
#   and will hence override each other.

# Make the binary accessible to docker-machine
$ export GOPATH=$(go env GOPATH)
$ export GOBIN=$GOPATH/bin
$ export PATH="$PATH:$GOBIN"

# Make docker-machine output help including cloudscale-specific options
$ docker-machine create --driver cloudscale
```
