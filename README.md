# gokrazy kernel: upstream Linux for arm64 (ARM64)

This repository holds a pre-built Linux kernel image for PCs and VMs, used by
the [gokrazy](https://gokrazy.org/) project.

The files in this repository are picked up automatically by
the `gok` tool, so you don’t need to interact with this repository
unless you want to update the kernel to a custom version.

## gokrazy kernel repository map

| repository             | source         | devices                       |
|------------------------|----------------|-------------------------------|
| [gokrazy/kernel.rpi]   | [Raspberry Pi] | Pi 3, Pi 4, Pi 5, Pi Zero 2 W |
| [gokrazy/kernel]       | [kernel.org]   | Pi 3, Pi 4, Pi Zero 2 W       |
| [gokrazy/kernel.amd64] | [kernel.org]   | PC x86_64, VMs                |
| [gokrazy/kernel.arm64] | [kernel.org]   | PC arm64, VMs                 |

[Raspberry Pi]: https://github.com/raspberrypi/linux
[kernel.org]: https://kernel.org/
[gokrazy/kernel.rpi]: https://github.com/gokrazy/kernel.rpi
[gokrazy/kernel]: https://github.com/gokrazy/kernel
[gokrazy/kernel.amd64]: https://github.com/gokrazy/kernel.amd64
[gokrazy/kernel.arm64]: https://github.com/gokrazy/kernel.arm64

## Cloning the kernel repository

This repository clocks in at over 3 GB of disk usage, so you might want to clone
it as a shallow clone:

```
git clone --depth=1 https://github.com/gokrazy/kernel.arm64
```

## Updating the kernel

First, follow the [gokrazy installation instructions](https://gokrazy.org/quickstart/).

We’re using docker to get a reproducible build environment for our
kernel images, so install docker if you haven’t already:

```
sudo apt install docker.io
sudo addgroup $USER docker
newgrp docker
```

Clone the kernel git repository:
```
git clone --depth=1 https://github.com/gokrazy/kernel.arm64
cd kernel.arm64
```

Install the kernel-related gokrazy tools into the `_build` directory:
```
GOBIN=$PWD/_build go install github.com/gokrazy/autoupdate/cmd/gokr-rebuild-kernel@latest
```

And build a new kernel (takes about 5 minutes):
```
(cd _build && ./gokr-rebuild-kernel -cross=arm64)
```

The new kernel is stored in the working directory. Use `gok add .` to
ensure the next `gok` build will pick up your changed files.
