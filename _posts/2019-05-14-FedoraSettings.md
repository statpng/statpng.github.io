---
layout: post
title:  "Linux :: Setup and issues"
date:   2019-05-14
categories: Linux
permalink: Linux
tags: linux fedora
use_math: true

# author
author: Kipoong Kim
---

<!-- more -->

## Issues

- **Problem1:**
  When we run a R script, the linux system would forcely shutdown. The issue code is as follows.

  ```bash
  [385.313611] mce: [Hardware Error]: CPU 14: Machine Check Exception: 5 Bank 0: f200004000000005
  [385.313624] mce: [Hardware Error]: RIP !INEXACT! 33:<00007f491fce658e>
  [385.313628] mce: [Hardware Error]: TSC 1371dff9010
  [385.313632] mce: [Hardware Error]: PROCESSOR 0:50654 TIME 1557723392 SOCKET 0 APIC 30 microcode 200004d
  [385.313636] mce: [Hardware Error]: Run the above through 'mcelog --ascii'
  [385.313640] mce: [Hardware Error]: Machine check: Processor context corrupt
  [385.313644] Kernel panic - not syncing: Fatal machine check
  [385.313676] Kernel Offset: 0x3d000000 from 0xffffffff81000000 (relocation range: 0xffffffff00000000-0xffffffffbfffffff)
  [385.313693] Rebooting in 30 seconds..
  ```

- **The Answer:** 
  According to the following answer, I disabled the options of speedsteps, TurboMode, Virtualization, Hyper-Threading in “CPU features” submenu of BIOS.

- **Reference:**
  <https://askubuntu.com/questions/146469/kernel-panic-machine-check-processor-context-corrupt-after-install>



## Install Fedora

- **Requirements:**
  1. USB with storage more than 8GB
  2. Program “refus” to make booting USB
  3. ISO image file of Fedora desktop/workstation
- **Notification**
  - Make the booting USB with DD image mode not ISO image mode.



## Start ssh-server service

- #### Install

  ```bash
  sudo dnf install -y openssh-server;
  ```

- #### Start

  ```bash
  sudo systemctl start sshd.service;
  ```

- #### Stop

  ```bash
  sudo systemctl stop sshd.service;
  ```

- #### Enable

  ```bash
  sudo systemctl enable sshd.service;
  ```

- #### Disable

  ```bash
  sudo systemctl disable sshd.service;
  ```

  

## Install R <Fedora>

- #### Install

  ```bash
  sudo dnf install R
  ```

- 







