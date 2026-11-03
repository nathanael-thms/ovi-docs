---
icon: lucide/terminal
---

# Usage

## Running with options/flags

Available flags and options for the `ovi` command, parameters in [] are optional:

| Flag/Option                               | Function                                                              |
|-------------------------------------------|-----------------------------------------------------------------------|
| `run [model_name]`                      | Launch a model directly                                               |
| `add hf [organization/repository] [name]` | Launch add model wizard directly using the Hugging Face Hub Method   |
| `add local [absolute_path] [name]`        | Launch add model wizard directly using the local directory Method    |
| `edit [model_name]`                       | Edit the Modelfile for the specified model                            |
| `--is-ovi-install`                        | Check if this is an ovi installation, used by setup.sh for validation |
| `help`, `--help`, `-h`                    | Show the help message and exit                                        |

## Running without flags/options

Running without any options or flags shows a menu in the terminal. This is navigated by arrow keys and enter, the menu will look like the following:
```
↑/↓ navigate • ← back • enter launch • esc/q quit.
============================================================

 ▸ Launch a model
   Add a model
   Edit a Modelfile
   Exit
```

More detailed info on each option follows.

!!! note
    The terminal must be at least 20 columns wide and 8 rows tall, for the menu to display properly. The minimum size for the model menu is 20 × (number of models + 6)

### Launch a model

When run without model name, this calls the model menu, as explained in [running models via model menu](models.md#running-via-model-menu) similar to the main menu, but with a list of models.
!!! warning
    This displays every directory in the `project_root/models` folder. After the model loader checks for `openvino_model.xml`, if not found an error will be thrown

There is also an exit option

When run with model name, it launches the model directly, as explained in [running directly](models.md#running-directly) 

### Add a model

This runs the add model wizard, as explained in [adding models](models.md#adding-models)

### Edit a Modelfile

This runs the edit Modelfile wizard, as explained in [editing the Modelfile](models.md#editing-the-modelfile)