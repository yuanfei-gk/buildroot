(All is tested on 64bit-ubuntu 16.04)

Quick Start for qemu run
========================
 echo "First download the files.";
 wget -nc https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/toolchain_<buildroot-config>_<buildroot-version>.tar.xz;
 wget -nc https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/Image;
 wget -nc https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/fw_jump.elf;

 echo "Now let's run on qemu";
 mkdir host;
 tar -Jxf toolchain_<buildroot-config>_<buildroot-version>.tar.xz -C host;
 qemu_start_cmd;

 (PS. Login with username "root", and no password)


Quick copy app into qemu
========================
 mkdir ./mydisk;
 sudo mount -t ext2 /dev/vda ./mydisk;
 -o loop ./mydisk.img ./mydisk;
 ./host/bin/riscv64-linux-gcc hello.c -o ./mydisk/hello.elf;
 sudo umount ./mydisk;

 echo "Boot qemu normally."
 qemu_start_cmd;


The rootfs is automatically mounted in qemu, run in qemu shell!
-------------------------------------
 ./hello.elf;


Enable qemu network
===================
 echo "Please use sudo privilege, becasue qemu will setup tap device in your host";
 sudo qemu_start_cmd -netdev tap,script=no,id=net0 -device virtio-net-device,netdev=net0;

 echo "Configure tap device in your host";
 sudo ifconfig tap0 192.168.101.200;

Configure eth0 in qemu, run in qemu shell!
------------------------------------------
 ifconfig eth0 192.168.101.23;
 ping 192.168.101.200;


Run with Jtag
=============
 minicom -D /dev/ttyUSB0
 cd ./host/csky-debug
 sudo ./DebugServerConsole.elf
 cd -
 ./host/bin/riscv64-linux-gdb -x ddrinit your_ddr_init.elf
 ./host/bin/riscv64-linux-gdb -x gdbinit vmlinux


Build linux kernel
==================
 echo "Download kernel source, csky patch and rootfs";
 wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-<kernel-version>.tar.xz;
 wget https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/linux-<kernel-version>.patch.xz;
 wget https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/rootfs.cpio.xz;

 echo "Now extra kernel source, patch it and compile";
 tar -Jxf linux-<kernel-version>.tar.xz;
 xz -d rootfs.cpio.xz;
 xz -d linux-<kernel-version>.patch.xz;
 cd linux-<kernel-version>;
 patch -p1 < ../linux-<kernel-version>.patch;
 BR_BINARIES_DIR=.. make ARCH=riscv CROSS_COMPILE=../host/bin/riscv64-linux- vmlinux;


Build buildroot
===============
 git clone https://gitlab.com/c-sky/buildroot.git;
 cd buildroot;
 git checkout <buildroot-version>;
 make CONF=<buildroot-config>;


Gitlab-CI url
=============
 https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/browse/output/images/


Versions:
