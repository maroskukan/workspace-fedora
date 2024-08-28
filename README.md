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
