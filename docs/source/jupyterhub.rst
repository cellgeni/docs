Jupyter
=======

.. note::
    We no longer host our own JupyterHub (``jhub.cellgeni.sanger.ac.uk``) nor ``Cloud GPU Notebooks``. Both have been replaced by the **Central JupyterHub** run by Sanger IT directly on the farm. It supports CPU and GPU notebooks.

Central JupyterHub
------------------

The Central JupyterHub is available at https://jupyter.internal.sanger.ac.uk. 
It is open to all Sanger staff, but **you need to be on the VPN or on site** to reach it. 
If you are off site and cannot use the VPN (for example on a personal laptop), use Teleport instead and log in at https://jupyter.portal.sanger.ac.uk (you need to ask the `Service Desk <https://sanger.freshservice.com/>`_ for Teleport access first). 
The service documentation is on the `Central JupyterHub service page <https://ssg-confluence.internal.sanger.ac.uk/spaces/FARM/pages/137073043/Central+JupyterHub+service>`_ (internal), and most of the documentation lives in the JupyterHub itself.

Because notebooks run on the farm, they have direct access to your farm home directory and Lustre, so there is no need to copy data around or mount the farm as with our old hub. 
Anything you do not store on the farm (such as a ``/tmp`` directory on the compute node) is lost when the notebook stops.

Starting a notebook
-------------------

#. Log in at https://jupyter.internal.sanger.ac.uk/
#. On the **Server Options** page choose:

   * **Memory RAM (GB)** and **CPU (cores)**: the resources for your notebook.
   * **Use GPU** and **GPU Memory RAM (GB)**: tick this if you need a GPU. Select the minimum GPU memory you need, since jobs asking for 80GB can take a long time to be scheduled. GPUs are not available on ``gen22``, so choose a different cluster (see below).
   * **Cluster**: the farm cluster to start the job on. Select ``General small farm 22.04 (gen22)`` if you haven't been on the farm course, as everyone can use it. The other options are ``General large farm 22.04 (farm22)``, ``Cub AI farm 22.04 (cub22)`` (Cub is the training cluster for accelerated computing, where you can get familiar with GPUs without affecting production workloads).
   * **Run environment**: ``Standard Jupyter Notebook`` uses the farm native software stack, or pick one of the pre-built Singularity containers (see below).

#. Click **Start** and wait for the server to start (this should take less than a minute).
#. A file browser is on the left and the launcher on the right. Use the launcher to open a notebook or a terminal (under ``Other``).

When you are finished, stop your server from ``File`` > ``Hub Control Panel`` > ``Stop My Server`` so the resources are freed for other users.


Accessing Lustre from the file browser
--------------------------------------

The JupyterHub file browser only shows your home directory, but your terminal and notebooks can use any path on the farm. 
To also browse NFS and Lustre directories in the file browser, link them into your home directory:

.. code-block:: bash

    ln -s /lustre/scratch126 $HOME/lustre126
    ln -s /nfs/team123 $HOME/nfs_team123


GPUs
----

The Central JupyterHub gives everyone access to GPUs: tick **Use GPU** on the Server Options page, choose the minimum GPU memory you need and pick a cluster other than ``gen22``, which has no GPUs. 
This is a shared "fractional GPU" (MIG) service meant for exploring data and developing software, so you get a portion of a GPU rather than a whole one. 
For larger jobs use the GPU batch queues on the farm. 
The Sanger-supported frameworks are PyTorch, TensorFlow and scikit-learn. 
See `Introduction to GPU services at Sanger <https://ssg-confluence.internal.sanger.ac.uk/spaces/FARM/pages/216139396/Introduction+to+GPU+services+at+Sanger>`_ (internal) for details, and ask in the `Sanger machine learning community <https://sanger.enterprise.slack.com/archives/C05GHEE4Z36>`_ on Slack.


Installing packages
-------------------

You can install additional Python packages by creating your own environment on the farm. We recommend using ``conda`` (see :doc:`conda`) or `uv <https://docs.astral.sh/uv/>`_. Keep environments in your project or team space rather than installing packages into the shared JupyterHub environment.

After creating an environment, register it as a Jupyter kernel. You can then select that kernel from the JupyterLab launcher or the notebook kernel selector.

Using Conda
~~~~~~~~~~~

Create an environment with Python 3.12 and the packages you need, activate it, and install a Jupyter kernel:

.. code-block:: bash

    # Choose a location in your project or team space
    conda create my-conda-env python=3.12 ipykernel
    conda activate my-conda-env

    # Give the kernel a recognisable name in JupyterHub
    python -m ipykernel install --user --name my-conda-env --display-name "Python (my-conda-env)"

Add more packages later using ``conda install package-name`` while the environment is active.
If needed, ``pip install package-name`` can install packages that are not available through Conda. 
Prefer Conda packages where possible and avoid mixing package managers unnecessarily.

If ``conda activate`` is not available, see :doc:`conda` for how to initialise Conda in your shell.

Using uv
~~~~~~~~

Alternatively, create a Python virtual environment using ``uv``:

.. code-block:: bash

    module load cellgen/uv
    uv venv my-env --python 3.12
    source my-env/bin/activate
    uv pip install ipykernel scanpy pandas
    python -m ipykernel install --user --name my-env --display-name "Python (my-env)"

Install additional packages with ``uv pip install package-name`` after activating the environment.

Managing kernels
~~~~~~~~~~~~~~~~

Restart your JupyterLab session if the new kernel does not appear. To see registered kernels, run ``jupyter kernelspec list``. To remove a kernel registration you no longer need, run ``jupyter kernelspec uninstall my-conda-env`` (or the kernel name you registered). Removing a kernel registration does **not** delete its environment.

For R environments, install ``IRkernel`` and register the kernel using ``IRkernel::installspec()``.


Notebook templates
------------------

Our (older) example notebooks for a basic downstream scRNA-seq analysis (SoupX, Scrublet, Seurat, Scanpy, R-based integration and Monocle3) are available in the `cellgeni/notebooks <https://github.com/cellgeni/notebooks/>`_ GitHub repository, together with HTML renders at https://cellgeni.github.io/notebooks/. They are no longer maintained, so package versions may need adjusting.


Singularity notebook environments
---------------------------------

We highly advise you use ``Standard Jupyter Notebook`` by default. 
However, if you need to you can run a pre-built Singularity image as your notebook. 
The following are available:

* CPU: `base <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-base-notebook>`__, `all-spark <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-all-spark-notebook>`__ and `datascience <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook>`__ notebooks
* GPU: `RAPIDS <https://developer.nvidia.com/topics/ai/data-science/cuda-x-for-data-science>`__ and `TensorFlow <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-tensorflow-notebook>`__ notebooks

The container is granted access to any GPU you requested. Some things to keep in mind:

* A new directory named after the image is created under your home directory (e.g. ``~/Rapid_AI``) and used as the home of the container, so that the container cannot break your normal working environment.
* It is possible to mount ``/software`` in the container, and to get a merged Jupyter environment, but we do not recommend either: containers will not necessarily have what is needed to run the software in ``/software`` (for example ``module avail`` fails because ``tclsh`` is missing).
* The additional software in these images is installed by Sanger IT, and we are not experts in their use.


How to get help
---------------

For questions about using Jupyter for your analysis you can ask in the `#bioinformatcs Slack channel <https://sanger-cellgen.slack.com/>`__. For problems with the service itself (login, notebooks not starting, missing software) raise a ticket with the `Service Desk <https://sanger.freshservice.com/>`_. If you have not attended the farm course, there is a `self-service version <https://isg.pages.internal.sanger.ac.uk/farm_course/>`_ (internal). The farm team also hold a weekly *Farmacy* drop-in hour on Tuesdays at 2pm, where you can get help with code, analyses that won't run or general computing questions.
