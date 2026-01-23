Conda
=====

.. warning:: **Do NOT install Anaconda or Miniconda distributions on Sanger systems**. Do not try to have your own install of conda. **You should use the ISG/conda module**. Only use `miniforge <https://github.com/conda-forge/miniforge>`_ as a Conda installer if you require so outside the cluster. Avoid using the Anaconda or Miniconda installers at all cost. 

Conda is an open-source, language-agnostic package manager and environment management system. It was originally developed to deal with Python package management but now supports Python, R, and binary releases.

Initially, the Anaconda Python distribution was developed by Anaconda Inc., but it was later spun out as a separate package and released under the BSD license. Now, Conda is a NumFOCUS-affiliated project.


**Key Conda concepts**

- *Conda* is the package and environment manager.
- *Channels* are the sources where the conda client gets the packages from. Use ``bioconda`` and ``conda-forge`` as your default channels. 
- *Anaconda* is a commerical company that develops and supports conda installers and some channels.


Conda Module
------------

  .. code-block:: bash
    
    module load ISG/conda
    
The **ISG/conda module** is designed to make it easy for you to use Conda and create individual environments for your work. The Conda client is installed centrally, preventing unnecessary duplicate installations.

The module enforces the following items:

1. Ensures that you can install and organize tools, packages, and dependencies for your projects without affecting others on the same system.
2. Places newly created environments in the appropriate location for software within the cluster (at ``/software``).
3. Automatically sets up paths and configurations for Conda to work smoothly, such as defining where your environments and packages will be stored and ensuring you use the fastest and most reliable sources for downloading software.
4. Includes helpful features, such as automatically cleaning up unused files, creating personal directories for your settings, and enabling smooth switching between different project setups.


Using the module
----------------

  .. warning:: **Remove any previous conda instructions** from your ``~/.bashrc`` before using the module. Exit and log back in to the farm to guarantee nothing from any previous install is loaded.
 

After activating the module the use should be fairly similar to your current workflow.

  .. code-block:: bash

    ssh farm22    
    module load ISG/conda
    conda activate yourFavouriteEnv


Using inside jobs
^^^^^^^^^^^^^^^^^

When using conda inside jobs you should activate the module, for example:

  .. code-block:: bash

    #!/bin/bash
    #BSUB -q normal
    #BSUB -M 8G
    #BSUB -R "select[mem>8G] rusage[mem=8G]"
    #BSUB -o %J.out
    #BSUB -e %J.err
    #BSUB -n 4
    #BSUB -R "span[hosts=1]"

    set -eo pipefail

    module load ISG/conda
    conda activate yourFavouriteEnv

    echo "Added some conda magic 🧙‍♂️"

if you prefer to use in-line bsub, activate the environment before submitting the job:

  .. code-block:: bash

    module load ISG/conda
    conda activate yourFavouriteEnv

    bsub -q normal -n 4 -M 8G -R "select[mem>8G] rusage[mem=8G] span[hosts=1]" \
         -o %J.out -e %J.err python yourScript.py


Load module automatically
^^^^^^^^^^^^^^^^^^^^^^^^^

You can add the module to your ``~/.bashrc`` if you'd like for it to be avaiable when you login to the farm.
To do this you need to add the following line:

  .. code-block:: bash

    ## load module
    module load ISG/conda




Create environments
-------------------

.. note::
    **Environment modification (creation/deletion) can only be done from head nodes**.
    The environments are located inside the ``/software`` area. This filesystem is only writable from head nodes.

To create an environment, first load the module, provide a name for your environment, and specify the package(s) you want to install, such as python=3.10:

  .. code-block:: bash
    
    module load ISG/conda
    conda create --name myEnv python=3.12

By default, environments will be created in:

  .. code-block:: bash
    
    /software/conda/users/<userName>


Migrate your environments
-------------------------


.. error:: **The method of "copying your old envs directly to the new path" is not recommended anymore because we've detected issues with runtime environments.** Please avoid it. You will have to recreate the envionment. If that's not possible please use instead `<https://conda.github.io/conda-pack/>`__


If you already had your own install of Miniconda or Miniforge you must move all the environments to the central location. 
To do so follow the next steps:

Before your start, **remove any previous conda instructions from your**  ``~/.bashrc``. Then exit the farm and re-connect.

1. Load the ``ISG/conda`` module to guarantee the right folders are created:

  .. code-block:: bash
    
    module load ISG/conda

2. Backup your existing conda environemnts:

  .. code-block:: bash
    
    conda env export --no-builds -p /path/to/current/envName > environ_backup.yml


3. Create the environment with the central conda module. 

  .. code-block:: bash
    
    conda env create -f environ_backup.yml -n envName

This will put the environment in the right place and guarantees no licensed packages are included. However, dev packages (installed from local sources) or licensed packages won't be able to install in the new environment. Alternatively you can use:

.. code-block:: bash

    conda create --name YourNewEnvName --clone /path/to/original_env_name

Both methods may be missing packages that were installed via pip or manually from GitHub.


4. Check your environments were successfully copied over and make sure you can see them when listing all your environments with conda:

  .. code-block:: bash
    
    conda env list



Common issues
-------------

- **Don't know where env is**: Search for it ``/software/conda/users/<userName>``.
- **Permissions denied**: you probably didn't activate the right environment. Trying to install to the base env will produce this error. Trying to install packages from nodes *other than head nodes* (farm22-head1/farm22-head2) will produce this error.
- **No space left on device**: very rare but it's possible that ``/software`` has filled up, please contact us to fix this.

Conda Support
-------------

For any Conda related questions please use the ``#bioinformatics`` Slack channel. There are lots of users there who can quickly answer your questions.
