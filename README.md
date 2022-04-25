# The minios based on linux kernel and buildroot

### download linux and buildroot

```bash
$ cd ~/workplaces
$ git clone https://github.com/vernon2gh/linux.git
$ git clone https://github.com/vernon2gh/buildroot.git
```

diffrent linux version use diffrent buildroot version, as example:

* linux2.6.34 == buildroot2014.05
* linux3.16   == buildroot2014.11
* linux4.4    == buildroot2016.02
* linux5.4    == buildroot2020.05

as except linux2.6.34 test x86_64 architecture only, other linux version can test x86_64 and arm64 architecture

### compile linux and rootfs

```bash
# x86_64
$ cd /mnt/linux
$ make x86_64_defconfig
$ make

$ cd /mnt/buildroot
$ make qemu_x86_64_linuxx.x_defconfig
$ make

# arm64
$ cd /mnt/linux
$ make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig
$ make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-

$ cd /mnt/buildroot
$ make qemu_aarch64_virt_linuxx.x_defconfig
$ make
```

### running

```bash
# x86_64
$ qemu-system-x86_64 -nographic -M pc -kernel bzImage -drive file=rootfs.extx -append "root=/dev/sda console=ttyS0"

# arm64
$ qemu-system-aarch64 -nographic -M virt -cpu cortex-a57 -kernel Image -initrd rootfs.cpio -append "console=ttyAMA0"
```

### docker (option)

If your current environment cannot be compiled, please use docker.

1. download docker image
2. startup docker image and mount ubuntu ~/workplaces to docker /mnt
3. compile linux and buildroot

Now docker images, as below:

* vernon2dh/linux-2.x
* vernon2dh/linux-3.x
* vernon2dh/linux-4.x
* vernon2dh/linux-5.x

As example, compile linux5.4 and buildroot2020.05

```bash
$ docker pull vernon2dh/linux-5.x
$ docker run -itd --name linux5.x -v ~/workplaces:/mnt vernon2dh/linux-5.x bash
$ docker exec -it linux5.x bash
```
