# m1n1: an experimentation playground for Apple Silicon

(And to some extent a Linux bootloader)

## Building

You need an `aarch64-linux-gnu-gcc` cross-compiler toolchain (or a native one, if running on ARM64).

```shell
$ git clone --recursive https://github.com/AsahiLinux/m1n1.git
$ cd m1n1
$ make
```

The output will be in build/m1n1.macho.

To build on a native arm64 machine, use `make ARCH=`.

To build verbosely, use `make V=1`.

Building on ARM64 macOS is supported with clang and LLVM; you need to use Homebrew to
install the required dependencies:

```shell
$ brew install llvm
```

After that, just type `make`.

### Building using the container setup

If you have a container runtime installed, like Podman or Docker, you can make use of the compose setup, which contains all build dependencies.

```shell
$ git clone --recursive https://github.com/AsahiLinux/m1n1.git
$ cd m1n1
$ podman-compose run m1n1 make
$ # or
$ docker-compose run m1n1 make
```

## Usage

Our [wiki](https://github.com/AsahiLinux/docs/wiki/m1n1%3AUser-Guide) has more information on how to
use m1n1.

To install on an OS container based on macOS <12.1, use `m1n1.macho`:

```shell
kmutil configure-boot -c m1n1.macho -v <path to your OS volume>
```

To install on an OS container based on macOS >=12.1, use `m1n1.bin`:

```shell
kmutil configure-boot -c m1n1.bin --raw --entry-point 2048 --lowest-virtual-address 0 -v <path to your OS volume>
```

## Payloads

m1n1 supports running payloads by simple concatenation:

```shell
$ cat build/m1n1.macho Image.gz build/dtb/apple-j274.dtb initramfs.cpio.gz > m1n1-payload.macho
$ cat build/m1n1.bin Image.gz build/dtb/apple-j274.dtb initramfs.cpio.gz > m1n1-payload.bin
```

Supported payload file formats:

* Kernel images (or compatible). Must be compressed or last payload.
* Devicetree blobs (FDT). May be uncompressed or compressed.
* Initramfs cpio images. Must be compressed.

Supported compression formats:

* gzip
* xz

## License

m1n1 is licensed under the MIT license, as included in the [LICENSE](LICENSE) file.

m1n1 embeds portions taken from
[arm-trusted-firmware](https://github.com/ARM-software/arm-trusted-firmware), which is
[BSD](3rdparty_licenses/LICENSE.BSD-3.arm) licensed and copyright:

* Copyright (c) 2013-2020, ARM Limited and Contributors. All rights reserved.

m1n1 embeds [Doug Lea's malloc](ftp://gee.cs.oswego.edu/pub/misc/malloc.c) (dlmalloc), which is in
the public domain ([CC0](3rdparty_licenses/LICENSE.CC0)).

m1n1 embeds portions of [PDCLib](https://github.com/DevSolar/pdclib), which is in the public
domain ([CC0](3rdparty_licenses/LICENSE.CC0)).

m1n1 embeds the [Source Code Pro](https://github.com/adobe-fonts/source-code-pro) font, which is
licensed under the [OFL-1.1](3rdparty_licenses/LICENSE.OFL-1.1) license and copyright:

* Copyright 2010-2019 Adobe (http://www.adobe.com/), with Reserved Font Name 'Source'. All Rights Reserved. Source is a trademark of Adobe in the United States and/or other countries.
* This Font Software is licensed under the SIL Open Font License, Version 1.1.

m1n1 embeds portions of the [dwc3 usb linux driver](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/usb/dwc3/core.h?id=7bc5a6ba369217e0137833f5955cf0b0f08b0712), which was [BSD-or-GPLv2 dual-licensed](3rdparty_licenses/LICENSE.BSD-3.dwc3) and copyright
* Copyright (C) 2010-2011 Texas Instruments Incorporated - http://www.ti.com

m1n1 embeds portions of [musl-libc](https://musl.libc.org/)'s floating point library, which are MIT licensed and copyright
* Copyright (c) 2017-2018, Arm Limited.

m1n1 embeds some rust crates. Licenses can be found in the vendor directory for every crate.
