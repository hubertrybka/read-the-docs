Quick Start
===========

.. _installation:

Installation
------------

ADAMET-XSpec can be set up with both ``uv`` and ``conda``.

First, clone the repository: 
.. code-block:: console

   git clone https://github.com/admet-xspec/admet-xspec.git
   cd admet-xspec

UV setup
--------

Follow the "Install uv" step at the [official uv docs](https://docs.astral.sh/uv/#__tabbed_1_1) to set up uv.
Then, have uv register a .venv within the current directory and install packages from the lockfile:

.. code-block:: console

   uv init .
   uv sync

Running example with uv:

.. code-block:: console

   uv run process.py --cfg configs/examples/train_lgbm.gin

Conda setup
-----------

.. code-block:: console
   conda create -n admet python=3.11.8
   conda activate admet_xspec
   conda install rdkit seaborn conda-forge::py-xgboost conda-forge::ray-all
   
   pip install -r requirements.txt
   
   # Dev dependencies
   pre-commit install

Run example with conda

.. code-block:: console
   # if you haven't already:
   conda activate admet_xspec
   
   python -m process --cfg configs/examples/train_lgbm.gin


Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']
