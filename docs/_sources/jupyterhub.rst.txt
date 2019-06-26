Jupyter Hub
===========

We support a Jupyter Hub server running on Sanger Cloud. Jupyter allows you to run your analysis in multiple environments (``R``, ``python``, ``Julia``, etc.) and also to create and share notebooks containing your analysis, code, equations and visualizations. We think this is an ideal environment for any kind of downstream analysis. For more details please refer to `Jupyter Hub documentation <http://jupyter.org/hub>`_.

How to get access
-----------------

Our Jupyter Hub service is available in a web browser on any computer anywhere in the world. You will need to provide us with your GitHub ID to be able to login. Once we notify you that your account is created you can login using your GitHub credentials.

Before logging in to JupyterHub please read `our instructions
<https://github.com/cellgeni/notebooks#user-instructions>`_.

Flavours
--------

There are three flavours of Jupyter Hub that we provide. Please choose the one required for your needs and access ti via links below.

.. note:: Please only use **Large** and **Extra Large** flavours when this much resources is really required because this might impact the availability of large instances to other users.

Default
^^^^^^^
https://jupyter.cellgeni.sanger.ac.uk

| RAM: 33 Gb
| vCPUs: 2-4
| Disk: 30 Gb

Large
^^^^^
https://jupyter-large.cellgeni.sanger.ac.uk

| RAM: 100 Gb
| vCPUs: 4-16
| Disk: 100 Gb

Extra Large
^^^^^^^^^^^
https://jupyter-xl.cellgeni.sanger.ac.uk

| RAM: 150 Gb
| vCPUs: 4-16
| Disk: 150 Gb

Notebooks
---------

We provide some notebook templates with the pre-installed software. These are located in the ``notebooks`` folder. Corresponding example data is located in the ``data`` folder. Before running your analysis, please make a copy of a notebook template, save it to your home folder and work with the copy. At the moment we have the following notebook templates:

1. `scanpy notebook for analysis of 10X data <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-scanpy.ipynb>`_
2. `Seurat notebook for analysis of 10X data <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-Seurat.Rmd>`_
3. `Batch correction notebook 1 <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-batch-correction-bbknn-scanorama.ipynb>`_
4. `Batch correction notebook 2 <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-batch-correction-harmony-mnn-cca-other.Rmd>`_

How to get help
^^^^^^^^^^^^^^^
For any Jupyter Hub related questions please use our `MatterMost channel <https://mattermost.sanger.ac.uk/cellgeninf/channels/jupyterhub>`_. There are lots of users there who can quickly answer your questions.