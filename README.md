# buildroot
Buildroot for THEAD CPU ISA

## Introduction
Buildroot is a tool that simplifies and automates the process of building a complete Linux system for an embedded system, using cross-compilation.
In order to achieve this, Buildroot is able to generate a cross-compilation toolchain, a root filesystem, a Linux kernel image and a bootloader for your target. Buildroot can be used for any combination of these options, independently (you can for example use an existing cross-compilation toolchain, and build only your root filesystem with Buildroot).

Thead buildroot is based on Buildroot and make some project-specific customizations.

## Get Started
    1. prepare a project work directory just like 'Project'
    2. cd Project
    3. git clone https://github.com/c-sky/buildroot.git or git@github.com:c-sky/buildroot.git

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
        |--Makefile          
        |--README.md
     //below folder will be generated after compilation by 'make CONF=<buildroot-config>'
        |--buildroot-ded3f9954f158b5d9cd08ae76749eade72fcca3a
        |--dl
        |--<buildroot-config>
              |--build
              |--host
              |--images
              |--staging
              |--target

## For below questions, Please refer to "buildroot-config/images/readme.txt" for details.       
* How to quickly Start for qemu run
* How to quickly copy app into qemu
* How to enable qemu network
* How to build linux kernel
* How to build buildroot
* How to run with Jtag

## How to get the reference board
    We will provide *** as reference board. Before you apply please simulate the case pass in qemu and send the log or screenshot to us.
    Everyone can apply from the url: ***

## Contact us
    If you want to discuss about this project，please contact us by email ***@alibaba-inc.com
