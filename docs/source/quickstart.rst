Quick Start
===========

Installation
------------

ADAMET-XSpec can be set up with both ``uv`` and ``conda``.

First, clone the repository:

.. code-block:: shell

   git clone https://github.com/admet-xspec/admet-xspec.git
   cd admet-xspec

UV setup
--------

Follow the "Install uv" step at the [official uv docs](https://docs.astral.sh/uv/#__tabbed_1_1) to set up uv.
Then, have uv register a .venv within the current directory and install packages from the lockfile:

.. code-block:: shell
   
   uv init .
   uv sync

Running example with uv:

.. code-block:: shell
   
   uv run process.py --cfg configs/examples/train_lgbm.gin

Conda setup
-----------
 
.. code-block:: shell

   conda create -n admet python=3.11.8
   conda activate admet_xspec
   conda install rdkit seaborn conda-forge::py-xgboost conda-forge::ray-all
   
   pip install -r requirements.txt
   
   # dev dependencies
   pre-commit install

Run example with conda
 
.. code-block:: shell

   # if you haven't already:
   conda activate admet_xspec
   
   python -m process --cfg configs/examples/train_lgbm.gin


Three experiments you can run right away
----------------------------------------

1. I want to test how the inclusion of datapoints from *Mus musculus* and *Rattus norvegicus*-based assays into a *Homo sapiens* dataset influences the performance of LightGBM classifiers trained to identify human acetylcholinesterase (AChE) inhibitors among small molecules.

I want to input pre-optimized hyperparameters for the model, utilize scaffold-based train-test split strategy, represent the molecules using ECFP4 fingerprints and employ a single-end LightGBM classifier alogrithm.
 
.. code-block:: shell

   uv run process.py --cfg configs/examples/train_lgbm.gin
   # or
   python -m process --cfg configs/examples/train_lgbm.gin

Take a look at ''configs/examples/train_lgbm.gin'', as it's structure corresponds to the exact setup described above. 
**This is the general config for an ADMET-Xspec experiment in which ML models would be trained and/or evaluated.** The build process for a config file describing our desired experiment will be discussed in next chapters.

2. I want to optimize the hyperaparameters and train an RF regressor - tasked with the prediction of human AChE inhibitory activity of small molecules (as IC50 values). In my pipeline I want to include a scaffold splitter, a count ECFP featurizer and a single-end RF regression model. I want to train on 'Homo sapiens'-derived data only.
 
.. code-block:: shell

   uv run process.py --cfg configs/examples/train_rf_optimize.gin
   # or
   python -m process --cfg configs/examples/train_rf_optimize.gin

3. I want to test if training multiend predictor

.. note::
   In ADMET-Xspec, there is a possibility to train multiend predictors on heterogenous data. Each data point, represented by a vector 

Let :math:`\mathcal{D} = \{(\mathbf{x}^{(k)}, y^{(k)})\}_{k=1}^{m}` be a dataset of
   :math:`m` samples with features :math:`\mathbf{x}^{(k)} \in \mathbb{R}^d` and labels
   :math:`y^{(k)} \in \{1, \dots, n\}`.


Each label is one-hot encoded via :math:`\mathbf{e}_y \in \{0,1\}^n`, where
:math:`(\mathbf{e}_y)_i = \delta_{iy}`. The augmented input is the concatenation

.. math::

   \tilde{\mathbf{x}}^{(k)} = \mathbf{x}^{(k)} \oplus \mathbf{e}_{y^{(k))}}
   = \begin{bmatrix} \mathbf{x}^{(k)} \\ \mathbf{e}_{y^{(k)}} \end{bmatrix}
   \in \mathbb{R}^{d+n}.
