# moxa-image-uploader [![Build Status](https://www.travis-ci.com/imZack/moxa-image-uploader.svg?branch=master)](https://www.travis-ci.com/imZack/moxa-image-uploader) [![Docker Pulls](https://img.shields.io/docker/pulls/zack/moxa-image-uploader.svg)](https://hub.docker.com/r/zack/moxa-image-uploader)

Provides an api to to work with the serial server. The server watches to see what devices are hooked up to the serial cables, and provides some apis to discover the devices hooked up. Most importantly it allows you to re-image a device based on serial number.

Available APIs

* GET /devices/:serialNumber
* GET /devices
* GET /ports
* GET /ports/:portName
* POST /image

## Configuration

The application comes with default ports configured. They are located in `./lib/portDefaults.js`. The defaults are for a 4 port device. The ports are named `Port1` through `Port4`.

* Port defaults
  * Serial server ip: 192.168.127.254
  * Port1: 4001
  * Port2: 4002
  * Port3: 4003
  * Port4: 4004

You can also configure the ports using environment variables. To get it to work you need to use at least two environment variables, i.e. `SERVER_COUNT` and `SERVER_1`. You can increase the `SERVER_COUNT` and add the corresponding `SERVER_#` environment variables to add additional servers. The `SERVER_#` is a comma separated list of arguments. The port names are prefixed with `Server#`. To mirror the default config you would use the following:

The `SERVER_#` comma separated arguments are:

1. Serial server ip
1. First port number
1. Number of total ports. It increases by 1 from the first port number through the total number of ports.

```bash
SERVER_COUNT=1 SERVER_1="192.168.127.254,4001,4" npm start
```

This would result in the following config

* Generated ports
  * Serial server ip: 192.168.127.254
  * Server1Port1: 4001
  * Server1Port2: 4002
  * Server1Port3: 4003
  * Server1Port4: 4004

### Using docker

There is a Dockerfile available in this repo. You can use the docker image by running the following. You can remove `--env DEBUG=*` if you don't want it, and you change change the server count and port assignments as needed.

```bash
docker run -it --rm \
  --name moxa-image-uploader \
  -p 8080:8080 \
  --env DEBUG="*" \
  --env SERVER_COUNT="1" \
  --env SERVER_1="192.168.127.254,4001,4" \
  moxa-image-uploader
```

## Development

```bash
git clone git@github.com:imZack/moxa-image-uploader.git
cd moxa-image-uploader
nvm use # (optional to use common node version)
npm install
```

You can start the server by running:

```bash
npm start
```

Make sure you lint new code before committing.

```bash
npm run lint
```

You can build the docker image by using

```bash
docker build -t moxa-image-uploader .
```

## Testing

The following node scripts and their descriptions

* `test` runs the unit tests.
* `test:watch` runs the unit tests with the watch flag min report and bails on the first failed test.
