# 🧐 duvida

![GitHub Workflow Status (with branch)](https://img.shields.io/github/actions/workflow/status/scbirlab/py-template/python-publish.yml)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/py-template)
![PyPI](https://img.shields.io/pypi/v/py-template)

**py-template** is a template python package. 

- [Installation](#installation)
- [Command-line interface](#command-line-interface)
- [Issues, problems, suggestions](#issues-problems-suggestions)
- [Documentation](#documentation)

## Installation

### The easy way

You can install the precompiled version directly using `pip`. You need to specify the machine learning framework
that you want to use:

```bash
$ pip install py-template
```

### From source

Clone the repository, then `cd` into it. Then run:

```bash
$ pip install -e .
```

## Python API

### Neural networks

The core of **duvida** is the `ModelBox`, which is a container for a trainable model and its training data.
These are connected because measures of confidence and information gain depend directly on the information
or evidence already seen by the model.

There are several `ModelBox` classes for specific deep learning architechtures in pytorch. 

```python
>>> from duvida.base.modelbox_registry import MODELBOX_REGISTRY
>>> from pprint import pprint
>>> pprint(MODELBOX_REGISTRY)
{'chemprop': <class 'duvida.torch.chem.ChempropModelBox'>,
 'fingerprint': <class 'duvida.torch.chem.FPMLPModelBox'>,
 'fp': <class 'duvida.torch.chem.FPMLPModelBox'>,
 'mlp': <class 'duvida.torch.models.mlp.MLPModelBox'>}
```

The modelboxes `chemprop` and `fingerprint` (alias `fp`) featurize SMILES representations of chemical 
structures. The modelbox `mlp` is a general purpose multilayer perceptron.

You can set up your model with various training parameters.

```python
from duvida.autoclass import AutoClass
modelbox = AutoClass(
    "fingerprint",
    n_units=16,
    n_hidden=2,
    ensemble_size=10,
)
```

The internal neural network is instantiated on loading training data.

```python
modelbox.load_training_data(
    filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:train",
    inputs="smiles",
    labels="clogp",
)
```

The `filename` can be a Huggingface dataset, in which case it is automatically downloaded. The `"@"`
indicates the dataset configuration, and the `":"` indicates the specific data split.

Alternatively, the training data can be a local CSV or TSV file. In-memory Pandas dataframes 
or dictionaries can be supplied through the `data` argument.

With training data loaded, the model can be trained!

```python
modelbox.train(
    val_filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:test",
    epochs=10,
    batch_size=128,
)
```

The `ModelBox.train()` method uses pytorch Lightning under the hood, so other options such as callbacks
for this framework should be accepted.

#### Saving and sharing a trained model

**duvida** provides a basic checkpointing mechanism to save model weights and training data to later reload.

```python
modelbox.save_checkpoint("checkpoint.dv")
modelbox.load_checkpoint("checkpoint.dv")
```

#### Evaluating and predicting on new data

**duvida** `ModelBox`es provide methods for evaluating predictions on new data.

```python
from duvida.evaluation import rmse, pearson_r, spearman_r
predictions, metrics = modelbox.evaluate(
    filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:test",
    metrics={
        "RMSE": rmse, 
        "Pearson r": pearson_r, 
        "Spearman rho": spearman_r
    },
)
```
## Python API

### Neural networks

The core of **duvida** is the `ModelBox`, which is a container for a trainable model and its training data.
These are connected because measures of confidence and information gain depend directly on the information
or evidence already seen by the model.

There are several `ModelBox` classes for specific deep learning architechtures in pytorch. 

```python
>>> from duvida.base.modelbox_registry import MODELBOX_REGISTRY
>>> from pprint import pprint
>>> pprint(MODELBOX_REGISTRY)
{'chemprop': <class 'duvida.torch.chem.ChempropModelBox'>,
 'fingerprint': <class 'duvida.torch.chem.FPMLPModelBox'>,
 'fp': <class 'duvida.torch.chem.FPMLPModelBox'>,
 'mlp': <class 'duvida.torch.models.mlp.MLPModelBox'>}
```

The modelboxes `chemprop` and `fingerprint` (alias `fp`) featurize SMILES representations of chemical 
structures. The modelbox `mlp` is a general purpose multilayer perceptron.

You can set up your model with various training parameters.

```python
from duvida.autoclass import AutoClass
modelbox = AutoClass(
    "fingerprint",
    n_units=16,
    n_hidden=2,
    ensemble_size=10,
)
```

The internal neural network is instantiated on loading training data.

```python
modelbox.load_training_data(
    filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:train",
    inputs="smiles",
    labels="clogp",
)
```

The `filename` can be a Huggingface dataset, in which case it is automatically downloaded. The `"@"`
indicates the dataset configuration, and the `":"` indicates the specific data split.

Alternatively, the training data can be a local CSV or TSV file. In-memory Pandas dataframes 
or dictionaries can be supplied through the `data` argument.

With training data loaded, the model can be trained!

```python
modelbox.train(
    val_filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:test",
    epochs=10,
    batch_size=128,
)
```

The `ModelBox.train()` method uses pytorch Lightning under the hood, so other options such as callbacks
for this framework should be accepted.

#### Saving and sharing a trained model

**duvida** provides a basic checkpointing mechanism to save model weights and training data to later reload.

```python
modelbox.save_checkpoint("checkpoint.dv")
modelbox.load_checkpoint("checkpoint.dv")
```

#### Evaluating and predicting on new data

**duvida** `ModelBox`es provide methods for evaluating predictions on new data.

```python
from duvida.evaluation import rmse, pearson_r, spearman_r
predictions, metrics = modelbox.evaluate(
    filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:test",
    metrics={
        "RMSE": rmse, 
        "Pearson r": pearson_r, 
        "Spearman rho": spearman_r
    },
)
```
## Python API

### Neural networks

The core of **duvida** is the `ModelBox`, which is a container for a trainable model and its training data.
These are connected because measures of confidence and information gain depend directly on the information
or evidence already seen by the model.

There are several `ModelBox` classes for specific deep learning architechtures in pytorch. 

```python
>>> from duvida.base.modelbox_registry import MODELBOX_REGISTRY
>>> from pprint import pprint
>>> pprint(MODELBOX_REGISTRY)
{'chemprop': <class 'duvida.torch.chem.ChempropModelBox'>,
 'fingerprint': <class 'duvida.torch.chem.FPMLPModelBox'>,
 'fp': <class 'duvida.torch.chem.FPMLPModelBox'>,
 'mlp': <class 'duvida.torch.models.mlp.MLPModelBox'>}
```

The modelboxes `chemprop` and `fingerprint` (alias `fp`) featurize SMILES representations of chemical 
structures. The modelbox `mlp` is a general purpose multilayer perceptron.

You can set up your model with various training parameters.

```python
from duvida.autoclass import AutoClass
modelbox = AutoClass(
    "fingerprint",
    n_units=16,
    n_hidden=2,
    ensemble_size=10,
)
```

The internal neural network is instantiated on loading training data.

```python
modelbox.load_training_data(
    filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:train",
    inputs="smiles",
    labels="clogp",
)
```

The `filename` can be a Huggingface dataset, in which case it is automatically downloaded. The `"@"`
indicates the dataset configuration, and the `":"` indicates the specific data split.

Alternatively, the training data can be a local CSV or TSV file. In-memory Pandas dataframes 
or dictionaries can be supplied through the `data` argument.

With training data loaded, the model can be trained!

```python
modelbox.train(
    val_filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:test",
    epochs=10,
    batch_size=128,
)
```

The `ModelBox.train()` method uses pytorch Lightning under the hood, so other options such as callbacks
for this framework should be accepted.

#### Saving and sharing a trained model

**duvida** provides a basic checkpointing mechanism to save model weights and training data to later reload.

```python
modelbox.save_checkpoint("checkpoint.dv")
modelbox.load_checkpoint("checkpoint.dv")
```

#### Evaluating and predicting on new data

**duvida** `ModelBox`es provide methods for evaluating predictions on new data.

```python
from duvida.evaluation import rmse, pearson_r, spearman_r
predictions, metrics = modelbox.evaluate(
    filename="hf://scbirlab/fang-2023-biogen-adme@scaffold-split:test",
    metrics={
        "RMSE": rmse, 
        "Pearson r": pearson_r, 
        "Spearman rho": spearman_r
    },
)
```

## Command-line interface

**py-template** has a command-line interface. 

```bash
$ pytemplate
Running app version 0.0.1
```

Edit `cli.py` to develop the CLI.

## Issues, problems, suggestions

Add to the [issue tracker](https://www.github.com/scbirlab/py-template/issues).

## Documentation

(To come at [ReadTheDocs](https://py-template.readthedocs.org).)