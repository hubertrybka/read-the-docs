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
   In ADMET-Xspec, there is a possibility to train multiend predictors of heterogenous data. Each data point, represented by a vector 

.. math::

   Let $\mathcal{D} = \{(\mathbf{x}^{(k)}, y^{(k)})\}_{k=1}^{m}$ denote a dataset of $m$ samples, where $\mathbf{x}^{(k)} \in \mathbb{R}^d$ is a $d$-dimensional feature vector and $y^{(k)} \in \mathcal{Y} = \{1, \dots, n\}$ is its associated categorical label drawn from a set of $n$ classes.

   We encode each label $y \in \mathcal{Y}$ using the standard one-hot encoding map $\phi: \mathcal{Y} \to \{0,1\}^n$,

   $$\phi(y) = \mathbf{e}_y = \big(\delta_{1y}, \delta_{2y}, \dots, \delta_{ny}\big)^\top,$$

   where $\delta_{iy}$ denotes the Kronecker delta, so that $\mathbf{e}_y$ has a $1$ in its $y$-th entry and $0$ elsewhere.

   For each sample, we construct an augmented representation $\tilde{\mathbf{x}} \in \mathbb{R}^{d+n}$ by concatenating the feature vector with the one-hot label encoding:

   $$\tilde{\mathbf{x}}^{(k)} = \mathbf{x}^{(k)} \oplus \mathbf{e}_{y^{(k)}} = \begin{bmatrix} \mathbf{x}^{(k)} \\ \mathbf{e}_{y^{(k)}} \end{bmatrix}, \qquad k = 1, \dots, m,$$

   where $\oplus$ denotes vector concatenation. The resulting dataset $\tilde{\mathcal{D}} = \{\tilde{\mathbf{x}}^{(k)}\}_{k=1}^m \subset \mathbb{R}^{d+n}$ embeds label information directly into the input representation, which we use as input to [your model/method] in the following sections.


   **Notes on conventions used:**
   - Bold lowercase ($\mathbf{x}$, $\mathbf{e}_y$) for vectors — standard in ML papers (vs. plain italics for scalars).
   - Calligraphic font ($\mathcal{D}$, $\mathcal{Y}$) for sets/datasets.
   - Superscript $(k)$ for sample indexing, to avoid clashing with subscripts used for vector components.
   - $\phi$ as an explicit named map — useful if you reference one-hot encoding again later, or want to contrast it with a learned embedding $\phi_\theta$.

   If you'd like, I can also write this assuming a *batched/matrix* formulation (i.e., stacking all $\tilde{\mathbf{x}}^{(k)}$ into a design matrix $\tilde{\mathbf{X}}$), which is common if your paper later discusses model architecture in matrix form.
