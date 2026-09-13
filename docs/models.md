---
icon: lucide/box
---

# Models

Models are placed in the `project_root/models` directory

## Requirements

- The model must be a raw OpenVINO IR model, containing an `openvino_model.xml`. The name of the model is the name used to run it. For example if you have `project_root/demo-model-int4/openvino_model.xml`, you would use `ovi run demo-model-int4` or use the menu to select it

- The model must not be named `Back` or `Exit`

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

The model can be run via any OpenVINO‑supported targets, such as CPU, NPU and GPU, target selection is explained in [available settings](#available-settings)

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

## Modelfiles

Each model directory can contain a `Modelfile` used to configure the runtime device and generation parameters.

### Syntax

- Lines beginning with `#` are ignored.
- Device lines use `DEVICE <target>`.
- Generation settings use `PARAMETER <name> <value>`.
- Aliases are accepted for compatibility, but the canonical names below are preferred.
- Values may be quoted when needed, for example `PARAMETER stop_strings "END"`.

Example:

```text
# Select the target device
DEVICE GPU

# Generation settings
PARAMETER max_new_tokens 256
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER stop_strings "END"
PARAMETER stop_token_ids 12, 13, 99
```

### Available parameters and aliases

The parser accepts the following canonical generation parameters:

- `max_new_tokens`
- `temperature`
- `top_k`
- `top_p`
- `repetition_penalty`
- `presence_penalty`
- `frequency_penalty`
- `num_beams`
- `no_repeat_ngram_size`
- `max_length`
- `min_new_tokens`
- `max_ngram_size`
- `min_p`
- `diversity_penalty`
- `length_penalty`
- `ignore_eos`
- `echo`
- `logprobs`
- `stop_strings`
- `stop_token_ids`

Aliases accepted by the parser:

- `temp` -> `temperature`
- `stop` / `stop_sequence` / `stop_sequences` / `stop_string` -> `stop_strings`
- `stop_token` / `stop_tokens` -> `stop_token_ids`
- `num_beam_groups` / `beam_width` / `beam_size` -> `num_beams`
- `no_repeat_ngram` -> `no_repeat_ngram_size`
- `min_tokens` -> `min_new_tokens`
- `ngram_size` -> `max_ngram_size`
- `min_probability` -> `min_p`
- `diversity` -> `diversity_penalty`
- `length` -> `length_penalty`
- `ignore_end_of_sequence` -> `ignore_eos`
- `echo_prompt` -> `echo`
- `log_probabilities` -> `logprobs`
- `presence` -> `presence_penalty`
- `frequency` -> `frequency_penalty`

### Device settings

`DEVICE` is a special Modelfile key. Supported values are:

- `CPU`
- `GPU`
- `NPU`
- `AUTO`

If omitted, ovi defaults to `CPU`.

### Rules and validation

The parser enforces these rules when loading a Modelfile:

- `num_beams` must be a positive integer between `1` and `16`.
- If `num_beams > 1`, beam search mode is enabled and the following values must be exactly `0`, `0.0`, or `1.0` as required:
  - `top_k = 0`
  - `top_p = 0.0`
  - `min_p = 0.0`
  - `presence_penalty = 0.0`
  - `frequency_penalty = 0.0`
  - `diversity_penalty = 0.0`
  - `repetition_penalty = 1.0`
  - `temperature = 1.0`
  - `max_ngram_size = 0`
- `length_penalty` must be non-negative.
- `logprobs` must be an integer between `0` and `10`.
- `max_length` cannot be less than `min_new_tokens`.
- `max_new_tokens` cannot exceed `max_length` when both are set.
- `stop_token_ids` must be non-negative integers.
- Unknown parameters are ignored.
- Invalid parameter values are rejected rather than partially applied.

These rules are enforced to avoid invalid generation combinations; for example, beam search does not allow sampling penalties or sampling probabilities to be active at the same time.