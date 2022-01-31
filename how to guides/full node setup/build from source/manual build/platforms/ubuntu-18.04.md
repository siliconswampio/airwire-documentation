---
# Ubuntu 18.04
---

This section contains shell commands to manually download, build, install, test, and uninstall ALAIO and dependencies on Ubuntu 18.04.

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
apt-get update && apt-get upgrade -y && DEBIAN_FRONTEND=noninteractive apt-get install -y git
# clone ALAIO repository
git clone https://github.com/ALADINIO/ala.git $ALAIO_LOCATION
cd $ALAIO_LOCATION && git submodule update --init --recursive
```

## Install ALAIO Dependencies
These commands install the ALAIO software dependencies. Make sure to [Download the ALAIO Repository](#download-alaio-repository) first and set the ALAIO directories.
```sh
# install dependencies
apt-get install -y make bzip2 automake libbz2-dev libssl-dev doxygen graphviz libgmp3-dev \
    autotools-dev libicu-dev python2.7 python2.7-dev python3 python3-dev \
    autoconf libtool curl zlib1g-dev sudo ruby libusb-1.0-0-dev \
    libcurl4-gnutls-dev pkg-config patch llvm-7-dev clang-7 vim-common jq
# build cmake
export PATH=$ALAIO_INSTALL_LOCATION/bin:$PATH
cd $ALAIO_INSTALL_LOCATION && curl -LO https://cmake.org/files/v3.13/cmake-3.13.2.tar.gz && \
    tar -xzf cmake-3.13.2.tar.gz && \
    cd cmake-3.13.2 && \
    ./bootstrap --prefix=$ALAIO_INSTALL_LOCATION && \
    make -j$(nproc) && \
    make install && \
    rm -rf $ALAIO_INSTALL_LOCATION/cmake-3.13.2.tar.gz $ALAIO_INSTALL_LOCATION/cmake-3.13.2
# build boost
cd $ALAIO_INSTALL_LOCATION && curl -LO https://dl.bintray.com/boostorg/release/1.71.0/source/boost_1_71_0.tar.bz2 && \
    tar -xjf boost_1_71_0.tar.bz2 && \
    cd boost_1_71_0 && \
    ./bootstrap.sh --prefix=$ALAIO_INSTALL_LOCATION && \
    ./b2 --with-iostreams --with-date_time --with-filesystem --with-system --with-program_options --with-chrono --with-test -q -j$(nproc) install && \
    rm -rf $ALAIO_INSTALL_LOCATION/boost_1_71_0.tar.bz2 $ALAIO_INSTALL_LOCATION/boost_1_71_0
# build mongodb
cd $ALAIO_INSTALL_LOCATION && curl -LO https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1804-4.1.1.tgz && \
    tar -xzf mongodb-linux-x86_64-ubuntu1804-4.1.1.tgz && rm -f mongodb-linux-x86_64-ubuntu1804-4.1.1.tgz && \
    mv $ALAIO_INSTALL_LOCATION/mongodb-linux-x86_64-ubuntu1804-4.1.1/bin/* $ALAIO_INSTALL_LOCATION/bin/ && \
    rm -rf $ALAIO_INSTALL_LOCATION/mongodb-linux-x86_64-ubuntu1804-4.1.1
# build mongodb c driver
cd $ALAIO_INSTALL_LOCATION && curl -LO https://github.com/mongodb/mongo-c-driver/releases/download/1.13.0/mongo-c-driver-1.13.0.tar.gz && \
    tar -xzf mongo-c-driver-1.13.0.tar.gz && cd mongo-c-driver-1.13.0 && \
    mkdir -p build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$ALAIO_INSTALL_LOCATION -DENABLE_BSON=ON -DENABLE_SSL=OPENSSL -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF -DENABLE_STATIC=ON -DENABLE_ICU=OFF -DENABLE_SNAPPY=OFF .. && \
    make -j$(nproc) && \
    make install && \
    rm -rf $ALAIO_INSTALL_LOCATION/mongo-c-driver-1.13.0.tar.gz $ALAIO_INSTALL_LOCATION/mongo-c-driver-1.13.0
# build mongodb cxx driver
cd $ALAIO_INSTALL_LOCATION && curl -L https://github.com/mongodb/mongo-cxx-driver/archive/r3.4.0.tar.gz -o mongo-cxx-driver-r3.4.0.tar.gz && \
    tar -xzf mongo-cxx-driver-r3.4.0.tar.gz && cd mongo-cxx-driver-r3.4.0 && \
    sed -i 's/\"maxAwaitTimeMS\", count/\"maxAwaitTimeMS\", static_cast<int64_t>(count)/' src/mongocxx/options/change_stream.cpp && \
    sed -i 's/add_subdirectory(test)//' src/mongocxx/CMakeLists.txt src/bsoncxx/CMakeLists.txt && \
    mkdir -p build && cd build && \
    cmake -DBUILD_SHARED_LIBS=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$ALAIO_INSTALL_LOCATION .. && \
    make -j$(nproc) && \
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
cd $ALAIO_BUILD_LOCATION && cmake -DCMAKE_BUILD_TYPE='Release' -DCMAKE_CXX_COMPILER='clang++-7' -DCMAKE_C_COMPILER='clang-7' -DLLVM_DIR='/usr/lib/llvm-7/lib/cmake/llvm' -DCMAKE_INSTALL_PREFIX=$ALAIO_INSTALL_LOCATION -DBUILD_MONGO_DB_PLUGIN=true $ALAIO_LOCATION
cd $ALAIO_BUILD_LOCATION && make -j$(nproc)
```

## Install ALAIO
This command installs the ALAIO software on the specified OS. Make sure to [Build ALAIO](#build-alaio) first.
```sh
cd $ALAIO_BUILD_LOCATION && make install
```

## Test ALAIO
These commands validate the ALAIO software installation on the specified OS. Make sure to [Install ALAIO](#install-alaio) first. (**Note**: This task is optional but recommended.)
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
