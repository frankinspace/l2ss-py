# Level 2 Subsetter Service Docker Image

This directory contains the `Dockerfile` used to build the Docker image capable of running the L2 Subsetter service.

## Building

The docker image is setup to install the l2ss-py project into userspace using pip. It will look
in both PyPi and TestPyPi indexes unless building from a local wheel file.

In order to build the image the following build arguments are needed

- `SOURCE` : The value of this build arg will be used in the `pip install` command to install the l2ss-py package 
- `DIST_PATH` (optional): The value of this build arg should be the path (relative to the context) to the directory containing a locally built wheel file 

> **Note**: By default the image runs `l2ss_harmony` which requires that the `[harmony]` extra is installed. If
> the `[harmony]` extra is not installed, the resulting image 

### Building from PyPi or TestPyPi

If the version of the l2ss-py package has already been uploaded to PyPi, all that is needed is to supply
the `SOURCE` build argument with the package specification.  

Example:

```shell script
docker build -f docker/Dockerfile --build-arg SOURCE="l2ss-py[harmony]==1.1.0-alpha.9" .
```

### Building from local code

First build the project with Poetry.

```
poetry build
```

That will create a folder `dist/` and a wheel file that is named with the version of the software that was built. 

In order to use the local wheel file, the `DIST_PATH` build arg must be provided to the `docker build` command
and the `SOURCE` build arg should be set to the path to the wheel file.

Example:

```shell script
docker build -f docker/Dockerfile --build-arg SOURCE="dist/l2ss_py-1.1.0a1-py3-none-any.whl[harmony]" --build-arg DIST_PATH="dist/" .
```

## Running

By default, the docker image `ENTRYPOINT` is set to `l2ss_harmony`. This will execute the 
l2ss-py package as a [Harmony service](https://github.com/nasa/harmony-service-lib-py).


