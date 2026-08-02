---
icon: lucide/rocket
---

# Install/Update

This section covers installing and updating ovi, currently this must be done manually, as the project evolves a curl-to-bash installer/updater will be added

## Installation

1. Clone the git repo for latest changes, or download the .zip file from the release you would like and unzip it

3. Change to the directory containing the source code

4. Run the setup script, this script should install everything required and set up the virtual environment, install pip packages, etc. Please open an issue if you encounter any bugs:
```bash
bash setup.sh
```

The installation is now complete!

## Updating

1. If during installation you had cloned the git repo, simply run git pull, if you had downloaded the .zip file, download the new .zip file from the release you would like and unzip it

2. Change into the directory containing the new source code

3. Run the setup script again. Please open an issue if you encounter any bugs:
```bash
bash setup.sh
```

ovi has now been updated!