---
icon: lucide/box
---

# Models

Models are placed in the `project_root/models` directory

## Requirements

The model must be a raw OpenVINO IR model, containing an `openvino_model.xml`. The name of the model is the name used to run it. For example if you have `project_root/demo-model-int4/openvino_model.xml`, you would use `ovi run demo-model-int4` or use the menu to select it

## Adding a model

### Pulling models

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
