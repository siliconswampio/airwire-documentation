---
# Install ALAIO Binaries
---

## ALAIO install script

For ease of contract development, content can be installed at the `/usr/local` folder using the `alaio_install.sh` script within the `ala/scripts` folder. Adequate permission is required to install on system folders:

```sh
cd ~/alaio/ala
sudo ./scripts/alaio_install.sh
```

## ALAIO manual install

In lieu of the `alaio_install.sh` script, you can install the ALAIO binaries directly by invoking `make install` within the `ala/build` folder. Again, adequate permission is required to install on system folders:

```sh
cd ~/alaio/ala/build
sudo make install
```

> What's Next?]]
| Configure and use [Nodala](../../../01_alanode/index.md), or optionally [Test the ALAIO binaries](04_test-alaio-binaries.md).
