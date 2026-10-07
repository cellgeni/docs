Jupyter
=======

.. note::
    We no longer host our own JupyterHub (``jhub.cellgeni.sanger.ac.uk``) or Cloud GPU Notebooks. Both have been replaced by the **Central JupyterHub** run by Sanger IT directly on the farm. It supports CPU and GPU notebooks.

Central JupyterHub
------------------

The Central JupyterHub is available at https://jupyter.internal.sanger.ac.uk/hub/login. It is open to all Sanger staff, but **you need to be on the VPN or on site** to reach it. If you are off site and cannot use the VPN (for example on a personal laptop), use Teleport instead and log in at https://jupyter.portal.sanger.ac.uk (you need to ask the `Service Desk <https://sanger.freshservice.com/>`_ for Teleport access first). The service documentation is on the `Central JupyterHub service page <https://ssg-confluence.internal.sanger.ac.uk/spaces/FARM/pages/137073043/Central+JupyterHub+service>`_ (internal), and most of the documentation lives in the JupyterHub itself.

Because notebooks run on the farm, they have direct access to your farm home directory and Lustre, so there is no need to copy data around or mount the farm as with our old hub. Anything you do not store on the farm (such as a ``/tmp`` directory on the compute node) is lost when the notebook stops.

Starting a notebook
-------------------

#. Log in at https://jupyter.internal.sanger.ac.uk/hub/login.
#. On the **Server Options** page choose:

   * **Memory RAM (GB)** and **CPU (cores)**: the resources for your notebook.
   * **Use GPU** and **GPU Memory RAM (GB)**: tick this if you need a GPU. Select the minimum GPU memory you need, since jobs asking for 80GB can take a long time to be scheduled.
   * **Cluster**: the farm cluster to start the job on. Select ``General small farm 22.04 (gen22)`` if you haven't been on the farm course, as everyone can use it. The other options are ``General large farm 22.04 (farm22)``, ``Cub AI farm 22.04 (cub22)`` (Cub is the training cluster for accelerated computing, where you can get familiar with GPUs without affecting production workloads) and the ``ISG 22.04 test cluster``.
   * **Run environment**: ``Standard Jupyter Notebook`` uses the farm native software stack, or pick one of the pre-built Singularity containers (see below).

#. Click **Start** and wait for the server to start (this should take less than a minute).
#. A file browser is on the left and the launcher on the right. Use the launcher to open a notebook or a terminal (under ``Other``).

When you are finished, stop your server from ``File`` > ``Hub Control Panel`` > ``Stop My Server`` so the resources are freed for other users.

Singularity notebook environments
---------------------------------

Instead of the standard notebook you can run a pre-built Singularity image as your notebook. The following are available:

* CPU: `base <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-base-notebook>`__, `all-spark <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-all-spark-notebook>`__ and `datascience <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook>`__ notebooks
* GPU: `RAPIDS <https://developer.nvidia.com/topics/ai/data-science/cuda-x-for-data-science>`__ and `TensorFlow <https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-tensorflow-notebook>`__ notebooks

The container is granted access to any GPU you requested. Some things to keep in mind:

* A new directory named after the image is created under your home directory (e.g. ``~/Rapid_AI``) and used as the home of the container, so that the container cannot break your normal working environment.
* It is possible to mount ``/software`` in the container, and to get a merged Jupyter environment, but we do not recommend either: containers will not necessarily have what is needed to run the software in ``/software`` (for example ``module avail`` fails because ``tclsh`` is missing).
* The additional software in these images is installed by Sanger IT, and we are not experts in their use.

Getting started
---------------

Sanger IT provides an introductory tutorial. In a Jupyter terminal run:

.. code-block:: bash

    git clone https://gitlab.internal.sanger.ac.uk/ISG/_jupyter-intro.git

then double-click ``_jupyter-intro`` in the file browser and open ``Introduction to LSF``.

Accessing Lustre from the file browser
--------------------------------------

The JupyterHub file browser only shows your home directory, but your terminal and notebooks can use any path on the farm. To also browse a Lustre directory in the file browser, link it into your home directory:

.. code-block:: bash

    ln -s /lustre/scratch125/admin/team94/sp35 $HOME/lustre-125-sp35

GPUs
----

The Central JupyterHub gives everyone access to GPUs: tick **Use GPU** on the Server Options page and choose the minimum GPU memory you need. This is a shared "fractional GPU" service meant for exploring data and developing software, so you get a portion of a GPU rather than a whole one. For larger jobs use the GPU batch queues on the farm. The Sanger-supported frameworks are PyTorch, TensorFlow and scikit-learn. See `Introduction to GPU services at Sanger <https://ssg-confluence.internal.sanger.ac.uk/spaces/FARM/pages/216139396/Introduction+to+GPU+services+at+Sanger>`_ (internal) for details, and ask in the `Sanger machine learning community <https://sanger.enterprise.slack.com/archives/C05GHEE4Z36>`_ on Slack.

Installing packages
-------------------

The ``Jupyterhub Basic Container`` (Python 3.9, pip, virtualenv and CUDA 11.8) is built without conda, and ISG no longer supports conda inside it. Create a virtual environment on the farm (for example in your team's project space rather than your home directory) and register it as a kernel:

.. code-block:: bash

    cd /path/to/your/project/space
    virtualenv .env
    source .env/bin/activate
    pip install ipykernel
    python -m ipykernel install --user --name=.env

The environment then appears in the launcher. Install the packages you need with ``pip``. You can also use a conda environment you created yourself on the farm (see :doc:`conda`): activate it and run the last two commands above to add it as a kernel. For R, install the ``IRkernel`` package and register it with ``IRkernel::installspec()``.

Notebook templates
------------------

Our (older) example notebooks for a basic downstream scRNA-seq analysis (SoupX, Scrublet, Seurat, Scanpy, R-based integration and Monocle3) are available in the `cellgeni/notebooks <https://github.com/cellgeni/notebooks/>`_ GitHub repository, together with HTML renders at https://cellgeni.github.io/notebooks/. They are no longer maintained, so package versions may need adjusting.

How to get help
---------------

For questions about using Jupyter for your analysis you can ask in our `Slack channel <https://sanger-cellgen.slack.com/archives/C037CTA5WJD>`__. For problems with the service itself (login, notebooks not starting, missing software) raise a ticket with the `Service Desk <https://sanger.freshservice.com/>`_. If you have not attended the farm course, there is a `self-service version <https://isg.pages.internal.sanger.ac.uk/farm_course/>`_ (internal). The farm team also hold a weekly *Farmacy* drop-in hour on Tuesdays at 2pm, where you can get help with code, analyses that won't run or general computing questions.
