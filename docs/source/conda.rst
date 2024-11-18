Conda
=====

.. warning:: **Do NOT install Anaconda or Miniconda distributions on Sanger systems**. Do not try to have your own install of conda. **You should use the cellgen/conda module**. Only use `miniforge <https://github.com/conda-forge/miniforge>`_ as a Conda installer if you require so outside the cluster. Avoid using the Anaconda or Miniconda installers. 

Conda is an open-source, language-agnostic package manager and environment management system. It was originally developed to deal with Python package management but now supports Python, R, and binary releases.

Initially, the Anaconda Python distribution was developed by Anaconda Inc., but it was later spun out as a separate package and released under the BSD license. Now, Conda is a NumFOCUS-affiliated project.


**Key Conda concepts**

- *Conda* is the package and environment manager.
- *Channels* are the sources where the conda client gets the packages from. Use ``bioconda`` and ``conda-forge`` as your default channels. 
- *Anaconda* is a commerical company that develops and supports conda installers and some channels.


Conda Module
------------

  .. code-block:: bash
    
    module load cellgen/conda
    
The **cellgen/conda module** is designed to make it easy for you to use Conda and create individual environments for your work. The Conda client is installed centrally, preventing unnecessary duplicate installations.

The module enforces the following items:

1. Ensures that you can install and organize tools, packages, and dependencies for your projects without affecting others on the same system.
2. Places newly created environments in the appropriate location for software within the cluster (at ``/software``).
3. Automatically sets up paths and configurations for Conda to work smoothly, such as defining where your environments and packages will be stored and ensuring you use the fastest and most reliable sources for downloading software.
4. Includes helpful features, such as automatically cleaning up unused files, creating personal directories for your settings, and enabling smooth switching between different project setups.


Using the module
----------------

  .. error:: **Remove any previous conda instructions** from your ``~/.bashrc`` before using the module. Exit and log back in to the farm to guarantee nothing from any previous install is loaded.
 

After activating the module the use should be fairly similar to your current workflow.

  .. code-block:: bash

    ssh farm22    
    module load cellgen/conda
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

    module load cellgen/conda
    conda activate yourFavouriteEnv

    echo "Added some conda magic 🧙‍♂️"


Load module automatically
^^^^^^^^^^^^^^^^^^^^^^^^^

You can add the module to your ``~/.bashrc`` if you'd like for it to be avaiable when you login to the farm.
To do this you need to add the following line:

  .. code-block:: bash

    ## load module
    { module load cellgen/conda } &>/dev/null




Create environments
-------------------

.. note::
    **Environment modification (creation/deletion) can only be done from head nodes**.
    The environments are located inside the ``/software`` area. This filesystem is only writable from head nodes.

To create an environment, first load the module, provide a name for your environment, and specify the package(s) you want to install, such as python=3.10:

  .. code-block:: bash
    
    module load cellgen/conda
    conda create --name myEnv python=3.10

By default, environments will be created in:

  .. code-block:: bash
    
    /software/cellgen/<teamNumber>/<userName>/envs

If your primary group does not match your current one, please notify the team so it can be fixed. However, if you want to control the location of your environments, you can set the ``CONDA_ENVS_PATH`` environment variable. **Don't use this unless you really have to.**

For example, to create new environments under a different team directory, export the variable **before loading the module**:

  .. code-block:: bash
    
    export CONDA_ENVS_PATH=/software/cellgen/team123/ob1/envs
    module load cellgen/conda
    conda create --name myEnv2 python=3.11
    # this will create myEnv2 at /software/cellgen/team123/ob1/envs/myEnv2


Migrate your environments
-------------------------

If you already had your own install of Miniconda or Miniforge you must move all the environments to the central location. 
To do so follow the next steps:

Before your start, **remove any previous conda instructions from your**  ``~/.bashrc``. Then exit the farm and re-connect.

1. Load the ``cellgen/conda`` module to guarantee the right folders are created:

  .. code-block:: bash
    
    module load cellgen/conda

2. Backup your existing conda environemnts:

  .. code-block:: bash
    
    conda export -n oldEnv > oldEnv.yaml

3. *Copy* or *move* all the environments from your local install to the central one. This will take some time because environemnts usually have thousands of files. 

    You can *copy* the environemnts using:

    .. code-block:: bash
        
        rsync -azhP /path/to/your/miniconda3/envs/* ${CONDA_ENVS_PATH}/envs/

    Or, directly *move* all your environments using:

    .. code-block:: bash
        
        mv /path/to/your/miniconda3/envs/* ${CONDA_ENVS_PATH}/envs/


4. Check your environments were successfully copied over and make sure you can see them when listing all your environments with conda:

  .. code-block:: bash
    
    conda env list

5. Once sure everything is in place and working remove your previous Conda installation:

  .. code-block:: bash
    
    rm -rf /path/to/your/miniconda3


Common issues
-------------

- **Don't know where env is**: Write down which is your primary group (use ``id -gn``) and then search for it ``/software/cellgen/<teamNmber>/<userName>``. Your primary group may not be the one you think.
- **Permissions denied**: you probably didn't activate the right environment. Trying to install to the base env (owned by ``cellgeni``) will produce this error.
- **Path not found**: it's possible that you are part of a new team that hasn't been setup in ``/software/cellgen``, please contact us to fix this.
- **No space left on device**: very rare but it's possible that ``/software`` has filled up, please contact us to fix this.

Conda Support
-------------

For any Conda related questions please use the ``#bioinformatics`` Slack channel. There are lots of users there who can quickly answer your questions.
