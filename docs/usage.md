---
icon: lucide/terminal
---

# Usage

## Running with options/flags

Available flags and options for the `ovi` command:

| Flag/Option | Function |
| ---| ---|
| `run` | Launch the model menu directly, identical functionality to [Launch a model](#launch-a-model) |
| `run {model_name}` | Launch the specified model |
| `--is-ovi-install` | Check if this is an ovi installation, used by setup.sh for validation |
| `help`, `--help`, `-h` | Show the help message and exit|

## Running without flags/options

Running without any options or flags shows a menu in the terminal this is navigated by arrow keys and enter, the menu will look like the following:
```
↑/↓ navigate • ← back • enter launch • esc/q quit.
============================================================

 ▸ Launch a model
   Exit
```

More detailed info on each option follows.

!!! note
    The terminal must be at least 20 columns wide and 8 rows tall, for the menu to display properly. the minimum size for the model menu is 20 × (number of models + 6)

### Launch a model

This calls the model menu, as explained in [running models via model menu](models.md#running-via-model-menu) similar to the main menu, but with a list of models.
!!! warning
    This displays every directory in the `project_root/models` folder. After the model loader checks for `openvino_model.xml`, if not found an error will be thrown

There is also an exit option