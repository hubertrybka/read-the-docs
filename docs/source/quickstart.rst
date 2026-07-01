Quick Start
===========

Installation
------------

ADAMET-XSpec can be set up with both ``uv`` and ``conda``. Clone the repository and navigate to its directory.


.. code:: bash
   git clone https://github.com/admet-xspec/admet-xspec.git
   cd admet-xspec


UV setup
--------


.. _uv docs: https://docs.astral.sh/uv/getting-started/


Follow the *Install uv* guide at the `uv docs`_ to set up uv.
Then, have uv register a .venv within the current directory and install packages from the lockfile:


.. code:: bash
   uv init .
   uv sync


Running demo experiment with uv:


.. code:: bash
   uv run process.py --cfg configs/examples/train_lgbm.gin


Conda setup
-----------

.. _miniconda: https://www.anaconda.com/download/success?reg=skipped

Install `miniconda`_ following the instructions for your operating system.

 
.. code:: bash
   
   conda create -n admet python=3.11.8
   conda activate xspec
   conda install rdkit seaborn conda-forge\:\:py-xgboost conda-forge\:\:ray-all
   
   pip install -r requirements.txt
   
   # dev dependencies
   pre-commit install


Run demo experiment with Conda:

 
.. code:: bash
   
   # if you haven't already:
   conda activate xspec
   
   python -m process --cfg configs/examples/train_lgbm.gin


Three examples to run now
-------------------------

1. I want to test how the inclusion of datapoints from *Mus musculus* and *Rattus norvegicus*-based assays into a *Homo sapiens* dataset influences the performance of LightGBM classifiers trained to identify human acetylcholinesterase (AChE) inhibitors among small molecules.

   I want to input pre-optimized hyperparameters for the model, utilize scaffold-based train-test split strategy, represent the molecules using ECFP4 fingerprints and employ a single-end LightGBM classifier alogrithm.
 
 
   .. code:: bash
      
      uv run process.py --cfg configs/examples/train_lgbm.gin
      # or
      python -m process --cfg configs/examples/train_lgbm.gin


   Take a look at ''configs/examples/train_lgbm.gin'', as it's structure corresponds to the exact setup described above. 
**This is the general config for an ADMET-Xspec experiment in which ML models would be trained and/or evaluated.** The build process for a config file describing our desired experiment will be discussed in next chapters.

2. I want to optimize the hyperaparameters and train an RF regressor - tasked with the prediction of human AChE inhibitory activity of small molecules (as IC50 values). In my pipeline I want to include a scaffold splitter, a count ECFP featurizer and a single-end RF regression model. I want to train on 'Homo sapiens'-derived data only.
 
   .. code:: bash
   
      uv run process.py --cfg configs/examples/train_rf_optimize.gin
      # or
      python -m process --cfg configs/examples/train_rf_optimize.gin

3. I want to explore how the simultaneous prediction of 

   .. note::
      In ADMET-Xspec, there is a possibility to train multiend predictors on heterogenous data. 
      Each data point, represented by a vector
      :math:`\mathcal{D} = \{(\mathbf{x}^{(k)}, y^{(k)})\}_{k=1}^{m}` be a dataset of

