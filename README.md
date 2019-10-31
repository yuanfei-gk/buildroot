# buildroot
Buildroot for THEAD CPU ISA

## Introduction
Buildroot is a tool that simplifies and automates the process of building a complete Linux system for an embedded system, using cross-compilation.
In order to achieve this, Buildroot is able to generate a cross-compilation toolchain, a root filesystem, a Linux kernel image and a bootloader for your target. Buildroot can be used for any combination of these options, independently (you can for example use an existing cross-compilation toolchain, and build only your root filesystem with Buildroot).

Thead buildroot is based on Buildroot and make some project-specific customizations.

## Directory Structure
When customizing Buildroot, one or more project-specific files need to be stored somewhere. Thead buildroot follows recommendation from the Buildroot developers, as described in this section.
        
        |--board
              |--csky-ci
                  |--defconfigs
        |--boot
              |--opensbi
        |--configs
        |--package
              |--csky-arch
              |--csky-ci
              |--csky-readme
              |--csky-tar-host
              |--hw-c610
              |--hw-c810
              |--hw-c910
              |--linux-patch-c910
              |--misc-download-c860
              |--misc-download-c910
              |--ntfs3g-ci
              |--perf-ci
              |--qemu
              |--riscv-pk-c910
              |--riscv-readme
              |--thead-linux-patch
        |--patches            
        |--.gitlab-ci.yml     
        |--Makefile          
        |--README.md
 
 
 

## Quick Start for qemu run
  1. get toolchain
      * wget -nc https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/toolchain_<buildroot-config>_<buildroot-version>.tar.xz;
  
  2. get vmlinux
      * wget -nc https://gitlab.com/c-sky/buildroot/-/jobs/<buildroot-job_id>/artifacts/raw/output/images/vmlinux.xz;
  
  3. run in Qemu shell
      * echo "Now let's run on qemu";
      * xz -d vmlinux.xz;
      * mkdir host;
      * tar -Jxf toolchain_<buildroot-config>_<buildroot-version>.tar.xz -C host;
      * qemu_start_cmd;
      (PS. Login with username "root", and no password)

  4. Enable qemu network
     * Please use sudo privilege, becasue qemu will setup tap device in your host
     > sudo qemu_start_cmd -netdev tap,script=no,id=net0 -device virtio-net-device,netdev=net0;

     * Configure tap device in your host
     > sudo ifconfig tap0 192.168.101.200;

     * Configure eth0 in qemu, run in qemu shell!
     > ifconfig eth0 192.168.101.23;
     > ping 192.168.101.200;


## Run with Jtag on board
   Preparation: 
        Board's serail interface is supposed to be connected to PC/laptop;
        Board's jtag interface is supposed to be connected to PC/laptop by jtag emulator(CKLink Lite).
           
   1. start minicom to connect board's serial port
      - minicom -D /dev/ttyUSB0
   2. start DebugServerConsole
      - cd ./host/csky-debug
      - sudo ./DebugServerConsole.elf
   3. load vmlinux and run
      - ./host/bin/csky-linux-gdb -x gdbinit vmlinux


## How to build
   1. git clone https://gitlab.com/c-sky/buildroot.git; 
   2. cd buildroot;
   3. make CONF=buildroot-config;  // buildroot-config can be found in 'configs' folder.

## How to get the reference board
    We will provide C610 as reference board. Before you apply please simulate the case pass in qemu and send the log or screenshot to us.
    Everyone can apply from the url: ***

## Contact us
    If you want to discuss about this project，please contact us by email ***@alibaba-inc.com
