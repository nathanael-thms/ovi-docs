---
icon: lucide/box
---

# Models

Models are placed in the `project_root/models` directory

## Requirements

The model must be a raw OpenVINO IR model, containing an `openvino_model.xml`. The name of the model is the name used to run it. For example if you have `project_root/demo-model-int4/openvino_model.xml`, you would use `ovi run demo-model-int4` or use the menu to select it

## Pulling models

One place you can pull models from is HF hub, pre-converted models in raw IR format are available under the `OpenVINO Toolkit` organization

Here is an example of downloading qwen2.5-coder-7B-Instruct-Int4
```bash
# If you do not already have huggingface-hub installed, install it, if you have it installed, make sure you have the latest version:
# pip install huggingface-hub

# Swap project_root for the path of the project root

cd {project_root}/models

# You may customize the directory name, remember this is the name that will be used for the model menu

hf download OpenVINO/Qwen2.5-Coder-7B-Instruct-int4-ov --local-dir qwen2.5-coder:7B-int4
```

Ultimately you can download the models from anywhere, as long as they are in OpenVINO IR format

## Running models

In the next minor release, running via GPU, NPU etc will be supported. Currently it can only run via CPU

!!! warning
    Attempting to run a model without an IR graph(`openvino_model.xml`) will throw an error.

### Running via model menu

Open the model menu by either running `ovi` and selecting `Launch a model` or running `ovi run`

This will open a menu like the following:
```
↑/↓ navigate • ← back • enter launch • esc/q quit.
============================================================
 ▸ qwen2.5-coder:7B-int4
   qwen2.5-coder-1.5B-int4
```

navigate the menu and hit enter on the model you would like to launch, you will then see the following:
```
Connected to raw model 'qwen2.5-coder:7B-int4'. Type '/exit' to quit.
Use ↑/↓ to browse recent prompts.
------------------------------------------------------------

>>> 
```

You are now connected to your model!

### Running directly

Alternatively you can simply run `ovi run {model name}`, where model name is the name of the directory of the model you want to run.

This achieves the same outcome as [running via model menu](#running-via-model-menu)