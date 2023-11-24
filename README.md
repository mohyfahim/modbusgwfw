# How to Fetch

- First, `$ git pull https://github.com/mohyfahim/modbusgwfw openwrt` in your workspace directory.
- Then `$ cd /path/to/openwrt` and fetch the submodule by `$ git submodule update --init --recursive`
- Install the prerequisite from the [openwrt doc](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem) based on your os type.
- Then build the dependecies (toolchains and the firmware...) from the [openwrt doc](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem) :

for short:

```bash
$ cd /path/to/openwrt
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
$ make -j$(nproc) defconfig
$ make -j$(nproc) download
$ make -j$(nproc) clean
$ make -j$(nproc) world
```

by this, the entire of os is going to build. to build just one package, after ruunig the feeds scripts, you just need to run:

```bash
$ make package/<package-name>/compile V=s
```

for the example, our backend package is `modbusgwbcknd` so to compile only this one:

```bash
$  make package/modbusgwbcknd/compile V=s
```

the output of the compiled package is located at the `bin/packages` directory by the `ipk` format. push these packages to the device and install them. if a package needs some other package as its dependency, please intall that package before your pakcage. ofcouse, if you build the entire os and flash it to the device, all dependencies, are installed by defaul.

for the example, to install our backend package, we need `zlib, sqlite3, libulfius, jsoncpp`.

```bash
$ scp bin/packages/mipsel_24kc/base/zlib_1.2.11-6_mipsel_24kc.ipk root@192.168.2.1:/tmp
$ ssh root@192.168.2.1
# cd /tmp
# opkg intall ./zlib_1.2.11-6_mipsel_24kc.ipk
```

and go on for the others.

finally our backend can run by the `mgbcknd` command.
