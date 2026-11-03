---
icon: lucide/box
---

# Models

Models are placed in the `project_root/models` directory

## Requirements

- The model must be a raw OpenVINO IR model, containing an `openvino_model.xml`. The name of the model is the name used to run it. For example if you have `project_root/demo-model-int4/openvino_model.xml`, you would use `ovi run demo-model-int4` or use the menu to select it

- The model must not be named `Back` or `Exit`

## Adding models
Below are the two methods for adding a model, followed by detailed behaviour for both the Hugging Face (HF) and Local approaches.

### Adding via wizard
To add a model, either run `ovi` and select `Add a Model`, or run `ovi add`, this will open a menu like this:
```
↑/↓ navigate • ← back • enter launch • esc/q quit.
============================================================

 ▸ Add a model from a local directory
   Add a model from Hugging Face Hub
   Exit
```
Select the option you would like

### Adding directly
Depending on whether you would like to add models from a local directory or from the Hugging Face Hub, you may use this syntax to do it directly:

Local: `ovi add {absolute path to directory containing model} {name to identify the model by}`
HF Hub: `ovi add {organization/repository} {name to identify the model by}`

An example of a valid model under HF Hub would be `OpenVINO/Qwen2.5-Coder-1.5B-Instruct-int4-ov`

### Hugging Face Method
When run via the menu, a search menu will pop up where you can search all models under the OpenVINO organisation, you must use up/down arrow keys to highlight and enter to select.
You will then be prompted to enter a name to identify the model by locally, this must not already be used by another model, and must not be `Back` or `Exit`

!!! tip
    In the search menu, if for whatever reason you want to pull a model under a different organisation, you can find it on HF hub, and paste the name directly from there. Search will not be supported for models not under the OpenVINO organisation.

When run directly, the same outcome will be achieved, but without the search menu.

### Local Method
When run via the menu, you will be prompted to enter the **absolute** path to the directory containing the model, and then a name to identify the model by locally, this must not already be used by another model, and must not be `Back` or `Exit`

When run directly, the same outcome will be achieved, but without the prompts.

### Other Methods
The two listed are the only methods that can be done automatically by a wizard. You can also add models manually by placing them in the `project_root/models` directory, but you must ensure that the model is a raw OpenVINO IR model, containing an `openvino_model.xml`, and that the name of the model is not `Back` or `Exit`

## Running models

The model can be run via any OpenVINO‑supported targets, such as CPU, NPU and GPU, target selection is explained in [Device settings](#device-settings). The model can be run via the model menu or directly via the command line.

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
- System prompts use `SYSTEM <prompt>` and support single-line strings, triple-quoted blocks, and heredocs.
- Generation settings use `PARAMETER <name> <value>`.
- Aliases are accepted for compatibility, but the canonical names below are preferred.
- Values may be quoted when needed, for example `PARAMETER stop_strings "END"`.

Example:

```text
# Select the target device
DEVICE GPU

# System prompt
SYSTEM "You are a helpful coding assistant."

# Generation settings
PARAMETER max_new_tokens 256
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER stop_strings "END"
PARAMETER stop_token_ids 12,13,99
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

### System prompt

The `SYSTEM` key sets the system prompt for the model. It can be specified as a string or using triple quotes for multi-line prompts.
Here are below examples of valid system prompts:

```text
SYSTEM "You are a helpful coding assistant."
```

```text
SYSTEM 'You are a helpful coding assistant.'
```

```text
SYSTEM """
You are a helpful digital assistant.
Answer questions in a concise and clear manner.
"""
```

```text
SYSTEM <<EOF
You are a helpful digital assistant.
Answer questions in a concise and clear manner.
EOF
```

!!! note
    The system prompt allows any label, such as this:
    ```text
    SYSTEM <<PROMPT
    You are a helpful digital assistant.
    Answer questions in a concise and clear manner.
    PROMPT
    ```

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
- Known parameters with invalid values cause Modelfile validation to fail.

These rules are enforced to avoid invalid generation combinations; for example, beam search does not allow sampling penalties or sampling probabilities to be active at the same time.