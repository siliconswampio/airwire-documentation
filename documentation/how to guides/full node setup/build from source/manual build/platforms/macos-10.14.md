---
# MacOS 10.14
---

This section contains shell commands to manually download, build, install, test, and uninstall ALAIO and dependencies on MacOS 10.14.

> Building ALAIO is for Advanced Developers]]
| If you are new to ALAIO, it is recommended that you install the [ALAIO Prebuilt Binaries](../../../00_install-prebuilt-binaries.md) instead of building from source.

Select a task below, then copy/paste the shell commands to a Unix terminal to execute:

* [Download ALAIO Repository](#download-alaio-repository)
* [Install ALAIO Dependencies](#install-alaio-dependencies)
* [Build ALAIO](#build-alaio)
* [Install ALAIO](#install-alaio)
* [Test ALAIO](#test-alaio)
* [Uninstall ALAIO](#uninstall-alaio)

> Building ALAIO on another OS?]]
| Visit the [Build ALAIO from Source](../../index.md) section.

## Download ALAIO Repository
These commands set the ALAIO directories, install git, and clone the ALAIO repository.
```sh
# set ALAIO directories
export ALAIO_LOCATION=~/alaio/ala
export ALAIO_INSTALL_LOCATION=$ALAIO_LOCATION/../install
mkdir -p $ALAIO_INSTALL_LOCATION
# install git
brew update && brew install git
# clone ALAIO repository
git clone https://github.com/ALADINIO/ala.git $ALAIO_LOCATION
cd $ALAIO_LOCATION && git submodule update --init --recursive
```

## Install Dependencies
These commands install the ALAIO software dependencies. Make sure to [Download the ALAIO Repository](#download-alaio-repository) first and set the ALAIO directories.
```sh
# install dependencies
brew install cmake python libtool libusb graphviz automake wget gmp pkgconfig doxygen openssl@1.1 jq boost || :
export PATH=$ALAIO_INSTALL_LOCATION/bin:$PATH
# install mongodb
mkdir -p $ALAIO_INSTALL_LOCATION/bin
cd $ALAIO_INSTALL_LOCATION && curl -OL https://fastdl.mongodb.org/osx/mongodb-osx-ssl-x86_64-3.6.3.tgz
    tar -xzf mongodb-osx-ssl-x86_64-3.6.3.tgz && rm -f mongodb-osx-ssl-x86_64-3.6.3.tgz && \
    mv $ALAIO_INSTALL_LOCATION/mongodb-osx-x86_64-3.6.3/bin/* $ALAIO_INSTALL_LOCATION/bin/ && \
    rm -rf $ALAIO_INSTALL_LOCATION/mongodb-osx-x86_64-3.6.3 && rm -rf $ALAIO_INSTALL_LOCATION/mongodb-osx-ssl-x86_64-3.6.3.tgz
# install mongo-c-driver from source
cd $ALAIO_INSTALL_LOCATION && curl -LO https://github.com/mongodb/mongo-c-driver/releases/download/1.13.0/mongo-c-driver-1.13.0.tar.gz && \
    tar -xzf mongo-c-driver-1.13.0.tar.gz && cd mongo-c-driver-1.13.0 && \
    mkdir -p cmake-build && cd cmake-build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$ALAIO_INSTALL_LOCATION -DENABLE_BSON=ON -DENABLE_SSL=DARWIN -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF -DENABLE_STATIC=ON -DENABLE_ICU=OFF -DENABLE_SASL=OFF -DENABLE_SNAPPY=OFF .. && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    rm -rf $ALAIO_INSTALL_LOCATION/mongo-c-driver-1.13.0.tar.gz $ALAIO_INSTALL_LOCATION/mongo-c-driver-1.13.0
# install mongo-cxx-driver from source
cd $ALAIO_INSTALL_LOCATION && curl -L https://github.com/mongodb/mongo-cxx-driver/archive/r3.4.0.tar.gz -o mongo-cxx-driver-r3.4.0.tar.gz && \
    tar -xzf mongo-cxx-driver-r3.4.0.tar.gz && cd mongo-cxx-driver-r3.4.0/build && \
    cmake -DBUILD_SHARED_LIBS=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$ALAIO_INSTALL_LOCATION .. && \
    make -j $(getconf _NPROCESSORS_ONLN) VERBOSE=1 && \
    make install && \
    rm -rf $ALAIO_INSTALL_LOCATION/mongo-cxx-driver-r3.4.0.tar.gz $ALAIO_INSTALL_LOCATION/mongo-cxx-driver-r3.4.0
```

## Build ALAIO
These commands build the ALAIO software on the specified OS. Make sure to [Install ALAIO Dependencies](#install-alaio-dependencies) first.

[[caution | `ALAIO_BUILD_LOCATION` environment variable]]
| Do NOT change this variable. It is set for convenience only. It should always be set to the `build` folder within the cloned repository.

```sh
export ALAIO_BUILD_LOCATION=$ALAIO_LOCATION/build
mkdir -p $ALAIO_BUILD_LOCATION
cd $ALAIO_BUILD_LOCATION && cmake -DCMAKE_BUILD_TYPE='Release' -DCMAKE_INSTALL_PREFIX=$ALAIO_INSTALL_LOCATION -DBUILD_MONGO_DB_PLUGIN=true $ALAIO_LOCATION
cd $ALAIO_BUILD_LOCATION && make -j$(getconf _NPROCESSORS_ONLN)
```

## Install ALAIO
This command installs the ALAIO software on the specified OS. Make sure to [Build ALAIO](#build-alaio) first.
```sh
cd $ALAIO_BUILD_LOCATION && make install
```

## Test ALAIO
These commands validate the ALAIO software installation on the specified OS. This task is optional but recommended. Make sure to [Install ALAIO](#install-alaio) first.
```sh
$ALAIO_INSTALL_LOCATION/bin/mongod --fork --logpath $(pwd)/mongod.log --dbpath $(pwd)/mongodata
cd $ALAIO_BUILD_LOCATION && make test
```

## Uninstall ALAIO
These commands uninstall the ALAIO software from the specified OS.
```sh
xargs rm < $ALAIO_BUILD_LOCATION/install_manifest.txt
rm -rf $ALAIO_BUILD_LOCATION
```
