# Trainite

Trainite generates self-contained PyTorch training projects from a small set of
tested building blocks. Choose a model, dataset, and trainer, then use the
generated project as a readable starting point for your experiment.

The generated project includes its model, data pipeline, trainer, configuration,
and declared dependencies. It does not need Trainite at runtime. Starter choices
include basic and rotary-position Transformers, string-reversal, counting, and
Hugging Face datasets, with decoder training powered by PyTorch-Ignite.

## Getting Started

This guide creates a small local training project with Trainite's default
components: the rotary-position Transformer, string-reversal dataset, and
decoder trainer.

### Requirements

- Python 3.10 or newer
- [`uv`](https://docs.astral.sh/uv/) (recommended) or `pip`

### Install Trainite

Install the latest release from PyPI:

```bash
pip install trainite
```

To work from a source checkout instead, install its dependencies with `uv`:

```bash
git clone https://github.com/pytorch-ignite/trainite.git
cd trainite
uv sync
```

Prefix the commands below with `uv run` when using the source checkout, for
example `uv run trainite init`.

### Create a project

Run the interactive setup and answer each prompt:

```bash
trainite init
```

For a reproducible, non-interactive setup, pass the same choices explicitly:

```bash
trainite init my-experiment \
  --model rope-transformer \
  --dataset string-reverse \
  --trainer decoder-trainer
```

Trainite creates `my-experiment/` with:

- `config.yaml` for model, data, training, and output settings
- `main.py` as the training entry point
- local `models/`, `dataset_impl/`, and `trainer.py` implementations
- `pyproject.toml` with the generated project's runtime dependencies
- `README.md` with the selected components and recreation command

### Run the experiment

Install the generated project's dependencies and start training:

#### With uv

```bash
cd my-experiment
uv sync
uv run python main.py config.yaml
```

#### With pip

Install the default project's runtime dependencies directly. The generated
project runs from its source directory; it is not configured as an installable
Python package. For other component choices, use the dependency list in the
generated `pyproject.toml`.

```bash
cd my-experiment
pip install "pydantic>=2" PyYAML "omegaconf>=2.3.0" torch pytorch-ignite tensorboard clearml
python main.py config.yaml
```

The run writes logs, checkpoints, and TensorBoard data beneath
`outputs/rope_transformer__string_reverse/` in a timestamped directory.

You now have a standalone project. Change `config.yaml` to tune the experiment,
or edit the generated Python modules to replace the starter implementation.
