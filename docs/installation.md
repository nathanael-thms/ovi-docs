---
icon: lucide/rocket
---

# Install/Update

This section covers installing and updating ovi. Currently this must be done manually. As the project evolves, a curl‑to‑bash installer/updater will be added.

## Installation

1. Clone the git repo for the latest changes, or download the .zip file from the release you want and extract it.
    
2. Change into the directory containing the source code

3. Run the [setup script](#setup-script):
```bash
bash setup.sh
```

The installation is now complete!

## Updating

1. If you cloned the git repo during installation, simply run `git pull`. If you downloaded a .zip file, download the new release and extract it.

2. Change into the directory containing the new source code

3. Run the [setup script](#setup-script) again:
```bash
bash setup.sh
```

ovi has now been updated!

## Setup script

This script supports `--verbose` for full command output and `--force` to allow overriding an existing non-ovi `ovi` command.

This file is located at `project_root/setup.sh`

## Requirements
- Should be run as a non-root user with sudo privileges. The script will prompt for sudo authentication.
- Must have sudo and bash installed on the system.
- Must use a glibc-based Linux distribution with one of the following package managers: `apt`, `dnf`, `yum`, `pacman`, or `zypper`.

### Limitations
- On RHEL enterprise environments(AlmaLinux, Rocky Linux, etc. - Excluding Fedora) the script will not automatically resolve dependencies for OpenVINO GPU acceleration.
- Supports only glibc-based Linux distributions, using one of the following package managers: `apt`, `dnf`, `yum`, `pacman`, or `zypper`.

### Testing
The installer has been tested on the following distributions:

- Ubuntu 26.04
- Debian 12 Bookworm
- Fedora 40
- Arch Linux
- openSUSE tumbleweed
- AlmaLinux 9
- Rocky Linux 9
- Alpine Linux is not supported, since the OpenVINO GPU stack it relies on requires a glibc-based distro.

### What the script does
!!! tip
    You may inspect the script yourself before running it

1. Checks that the system is not Alpine Linux.

2. Verifies sudo access and prompts for authentication if needed.

3. Grants executable permissions to the `ovi` launcher script.

4. Detects the available package manager (`apt`, `dnf`, `yum`, `pacman`, or `zypper`) and installs the OS-level OpenCL/GPU dependencies needed for OpenVINO acceleration.

5. Detects and fixes hardware device permissions for `/dev/dri` render and video nodes when present, and adds the current user to the `video`/`render` groups when needed.

6. Ensures a modern Python version is available (`3.11` through `3.14`), installing it automatically if the system is missing one. On Debian/Ubuntu it installs `uv` and uses it to provision Python 3.14 when needed.

7. Creates a local virtual environment (`ovi-env`) and installs the project requirements into it.

8. Updates the shebang in `ovi` to point at the best possible Python interpreter.

9. Installs the global `ovi` command into `/usr/local/bin` by creating a symlink. If an existing command named `ovi` is found and it is not already an ovi installation, the script aborts for safety. To override that protection, rerun the script with the `--force` flag.

This script is safe to run multiple times.

If you encounter any bugs/issues, open an issue on GitHub