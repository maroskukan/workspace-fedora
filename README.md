# Fedora Development Machine Workspace

## Introduction

This repository contains wrapper scripts and Ansible playbooks that ensures that a fresh Fedora Core minimal installation has required software and configuration to support common development environment.

This project is based on [Ubuntu Development Machine Workspace](https://github.com/maroskukan/workspace-ubuntu) which focuses on setting up the environment on Ubuntu.


## Install

Clone or download this repository into your local machine. The wrapper scripts are located in `./bin/` directory. As always, before execution I encourage you to explore content of any script.

The `./bin/bootstrap` script will ensure that:

1. Existing installed packages are up to date
2. Ansible is present
3. Ansible playbook `main.yml` is executed

When ready, navigate to repository root and execute the bootstrap script.

```bash
./bin/bootstrap
```

After first run the bootstrap script, you can use the `./bin/apply` shell script. This will skip the first two steps and just execute the playbook. This is useful when you want to reapply the playbook later.

```bash
./bin/apply
```


## Configuration

The `main.yml` playbook imports individual tasks files from `./tasks/` folder. Using variables defined in `./config.yml` it is possible to easily include or exclude a given category.

For example, since hardware may vary from machine to machine I have decided that the driver installation portion is excluded by default.

It is up to you to customize the list of any drivers that you machine might require.

Other, more generic packages are included by default. Again, you can customize the list of applications in the `./config.yml` file.
 
### Default

These UI changes will be installed or applied
- Wayland display server protocol
- Gnome desktop environment

These tools and applications will be installed:

These VScode extensions will be installed

These build tools applications will be installed for OCaml development.

## Tips

### Changing TTYs

From GUI you can use the key combination `Ctrl+Alt+Fx` and from console you can use `Alt+Fx`. Where `x` is the TTY number `1` to `6`

### Setting up default target

To graphical:

```bash
sudo systemctl set-default graphical.target
```

To multi-user

```bash
sudo systemctl set-default multi-user.target
```

You can also use `isolate` instead of `set-default` to switch to given target immediately.

### Adjusting filesystem

The default storage layout for Fedora minimal installation creates logical volume for root paritition with size of 15G with the remaining disk space being unused.

```bash
sudo lvdisplay
  --- Logical volume ---
  LV Path                /dev/fedora_slayer/root
  LV Name                root
  VG Name                fedora_slayer
  LV UUID                KxhpQv-PYU1-5JAH-jbWE-0cnS-NvMf-H6h37b
  LV Write Access        read/write
  LV Creation host, time slayer, 2024-08-23 16:40:12 -0400
  LV Status              available
  # open                 1
  LV Size                15.00 GiB
  Current LE             3840
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1

sudo pvdisplay
  --- Physical volume ---
  PV Name               /dev/mapper/luks-a08c4c3e-e45c-4819-80ff-d71a124e8a17
  VG Name               fedora_slayer
  PV Size               1.86 TiB / not usable 0   
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              487968
  Free PE               484128
  Allocated PE          3840
  PV UUID               MUIWlm-9xJt-tQsq-PITW-Q6Vf-OO8E-UkgScu
   
sudo vgdisplay
  --- Volume group ---
  VG Name               fedora_slayer
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               1.86 TiB
  PE Size               4.00 MiB
  Total PE              487968
  Alloc PE / Size       3840 / 15.00 GiB
  Free  PE / Size       484128 / <1.85 TiB
  VG UUID               Im8m1R-n009-OHU6-VgHR-vwBf-i3nm-h29vDv
```

In order to increase the LV size for root partition, extend the logical volume and then resize the filesystem.

```bash
# Extend the logical volume
sudo lvextend -L +15G /dev/mapper/fedora_slayer-root

# Resize the filesystem
sudo xfs_growfs /dev/mapper/fedora_slayer-root
```
