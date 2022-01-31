# Download ALAIO Source

To download the ALAIO source code, clone the `ala` repo and its submodules. It is adviced to create a home `alaio` folder first and download all the ALAIO related software there:

```sh
mkdir -p ~/alaio && cd ~/alaio
git clone --recursive https://github.com/ALADINIO/ala
```

## Update Submodules

If a repository is cloned without the `--recursive` flag, the submodules *must* be updated before starting the build process:

```sh
cd ~/alaio/ala
git submodule update --init --recursive
```

## Pull Changes

When pulling changes, especially after switching branches, the submodules *must* also be updated. This can be achieved with the `git submodule` command as above, or using `git pull` directly:

```sh
[git checkout <branch>]  (optional)
git pull --recurse-submodules
```

> What's Next? [Build ALAIO binaries](02_build-alaio-binaries.md)
