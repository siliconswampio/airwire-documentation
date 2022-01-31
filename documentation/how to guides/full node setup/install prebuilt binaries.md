---
# Install Prebuilt Binaries
---

> Previous Builds]]
| If you have previously installed ALAIO from source using shell scripts, you must first run the [Uninstall Script](01_build-from-source/01_shell-scripts/05_uninstall-alaio.md) before installing any prebuilt binaries on the same OS.

## Prebuilt Binaries

Prebuilt ALAIO software packages are available for the operating systems below. Find and follow the instructions for your OS:

### Mac OS X:

#### Mac OS X Brew Install
```sh
brew tap alaio/alaio
brew install alaio
```
#### Mac OS X Brew Uninstall
```sh
brew remove alaio
```

### Ubuntu Linux:

#### Ubuntu 18.04 Package Install
```sh
wget https://github.com/ALADINIO/ala/releases/download/v2.0.7/alaio_2.0.7-1-ubuntu-18.04_amd64.deb
sudo apt install ./alaio_2.0.7-1-ubuntu-18.04_amd64.deb
```
#### Ubuntu 16.04 Package Install
```sh
wget https://github.com/ALADINIO/ala/releases/download/v2.0.7/alaio_2.0.7-1-ubuntu-16.04_amd64.deb
sudo apt install ./alaio_2.0.7-1-ubuntu-16.04_amd64.deb
```
#### Ubuntu Package Uninstall
```sh
sudo apt remove alaio
```

### RPM-based (CentOS, Amazon Linux, etc.):

#### RPM Package Install
```sh
wget https://github.com/ALADINIO/ala/releases/download/v2.0.7/alaio-2.0.7-1.el7.x86_64.rpm
sudo yum install ./alaio-2.0.7-1.el7.x86_64.rpm
```
#### RPM Package Uninstall
```sh
sudo yum remove alaio
```

## Location of ALAIO binaries

After installing the prebuilt packages, the actual ALAIO binaries will be located under:
* `/usr/opt/alaio/<version-string>/bin` (Linux-based); or
* `/usr/local/Cellar/alaio/<version-string>/bin` (MacOS)

where `version-string` is the ALAIO version that was installed.

Also, soft links for each ALAIO program (`alanode`, `clala`, `kalad`, etc.) will be created under `usr/bin` or `usr/local/bin` to allow them to be executed from any directory.

## Previous Versions

To install previous versions of the ALAIO prebuilt binaries:

1. Browse to https://github.com/ALADINIO/ala/tags and select the actual version of the ALAIO software you need to install.

2. Scroll down past the `Release Notes` and download the package or archive that you need for your OS.

3. Follow the instructions on the first paragraph above to install the selected prebuilt binaries on the given OS.
