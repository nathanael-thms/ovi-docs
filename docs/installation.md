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

This file, located at `project_root/setup.sh`, is a bash script that does the following:

!!! warning
    **DO NOT** run this script as root.
!!! note
    This script must be run as a user with sudo privileges

1. Check for sudo privileges

2. Grant executable permissions to ovi

3. Install the `python3.14-venv` package if not present

4. Create the Python virtual environment and install required packages.

5. Create the global symlink into PATH. If an existing command named ovi is found and it is not an ovi installation, the script will abort for safety. If you are certain you want to override the command, run the setup script again with the `--force` flag.

This script is safe to run multiple times.

You are welcome to inspect the script before running it.

If you encounter any bugs/issues, open an issue on GitHub