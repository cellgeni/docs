Jupyter Hub
===========

We support a Jupyter Hub server running on Sanger Cloud. Jupyter allows you to run your analysis in multiple environments (``R``, ``python``, ``Julia``, etc.) and also to create and share notebooks containing your analysis, code, equations and visualizations. We think this is an ideal environment for any kind of downstream analysis. For more details please refer to `Jupyter Hub documentation <http://jupyter.org/hub>`_.

How to get access
-----------------

Our Jupyter Hub service is available in a web browser on any computer anywhere in the world. You will need to provide us with your GitHub ID to be able to login. Once we notify you that your account is created you can login using your GitHub credentials. Please note only Sanger employees are eligible for access.


Resources
---------

+-------------+------------+-----------+
| RAM         | vCPUs      | Storage   |
+=============+============+===========+
| up to 200 Gb| up to 16   | 100 Gb    |
+-------------+------------+-----------+


We provide open usage metrics of our Jupyter cluster using `Graphana Dashboard <https://metrics.cellgeni.sanger.ac.uk>`_.

Quick Start Guide
-----------------
JupyterHub website is public, so you don't need to turn on VPN to use it. However, it is only available to users who messaged us their Github usernames and have been whitelisted. 

#. In your browser go to https://jhub.cellgeni.sanger.ac.uk
#. Use your Github credentials for authentication. It may take some time to load first time.
#. Select your CPU number, RAM number and Image you would like to spawn your instance with.
#. Now you are ready to run your notebooks! 
#. **RStudio** is also available on JupyterHub. A new R session can be started from the Launcher or change the word `lab` in your adress bar to the word `rstudio`: ```https://jhub.cellgeni.sanger.ac.uk/user/<your-username>/rstudio```
#. You can switch to a classic Jupyter interface by change the word `lab` in your adress bar to the word `tree`: ```https://jhub.cellgeni.sanger.ac.uk/user/<your-username>/tree```


.. warning:: **JupyterHub environment and storage are not backed up**. Please only use for computations and download your results (and notebooks) afterwards. You've been warned!


.. warning:: **Keep your notebooks light**. Notebooks over 100MB *will* give you unexpected errors.


Notebook templates
------------------

We provide some notebook templates with the pre-installed software. These are located in the ``notebooks`` folder. Corresponding example data is located in the ``data`` folder. Before running your analysis, please make a copy of a notebook template, save it to your home folder and work with the copy. At the moment we have the following notebook templates:

#. `scanpy notebook for analysis of 10X data <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-scanpy.ipynb>`_
#. `Seurat notebook for analysis of 10X data <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-Seurat.Rmd>`_
#. `Batch correction notebook 1 <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-batch-correction-bbknn-scanorama.ipynb>`_
#. `Batch correction notebook 2 <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-batch-correction-harmony-mnn-cca-other.Rmd>`_



Installing packages
-------------------

Conda
^^^^^

The default conda environment is not persistent across Jupyter sessions - you can install additional packages, but it will not be there next time you start Jupyter.
To have a persistent conda environment create one inside ``/home/jovyan/`` folder *(if you've already got a conda environment activated jump to step 4)*:

1. Open a new terminal (click on the ``Terminal`` icon in the Launcher)
2. Create the environment and activate it (replace ``myenv`` with your environment name):

  .. code-block:: bash

    conda create --name myenv python=3.8
    conda activate myenv

3. Install ``ipython kernel`` to use as a python kernel inside your jupyter environment:

  .. code-block:: bash

    python -m ipykernel install --user --name myenv --display-name "Python (MyEnv)"

4. Install all the packages you need, for example:

  .. code-block:: bash

    conda install numpy pandas matplotlib scipy scikit-learn

5. Reload the main page. Now you will see your new environment in the Launcher. If you don't see it at first, try restarting your instance.


**Alternative**

Instead of creating a new environment, you can also clone an existing one this will eliminate the need to install repeated packages:

.. code-block:: bash

    conda create --clone old_name --name new_name


pip
^^^
``pip`` defaults to installing Python packages to a system directory, to make sure your packages persist they need to be installed in your home directory use the ``--user`` option to do this or **install them inside an active conda environment**.


R
^^^
Packages can be installed with the ``install.packages()`` function in an RStudio console:

.. code-block:: r

    install.packages("packageName")

or multiple packages at once:

.. code-block:: r
    install.packages(c("packageOne", "packageTwo", "packageThree"))

From a terminal ``RScript`` can be used to install pacakges **(don't install packages as sudo)**:

.. code-block:: bash

    Rscript -e 'install.packages("packageName")'


.. warning:: **Try not to mix conda r-* packages with R CRAN pacakges**. For example, if you've installed your own R using conda like this ``conda install r-recommended r-irkernel``, install packages using conda ``conda install r-hdf5r`` instead of ``install.packages("hdf5r")``.



Kernels
-------

Kernels are programming language specific processes that run independently and interact with Jupyter and their user interfaces. 
Kernels can be changed using the ``Kernel`` > ``Changer kernel`` menu.


Python Kernel
^^^^^^^^^^^^^
When the kernel list is located outside your home directory it can be reseted. If that happens, run this one-line command from your terminal to add **every conda environment** on your profile to the kernel list.

.. code-block:: bash

    pip install -U ipykernel; ENVS=$(conda info --envs | grep '^\w' | cut -d' ' -f1); for env in $ENVS; do source activate $env; python -m ipykernel install --user --name $env; echo "$env"; conda deactivate; done


R Kernel
^^^^^^^^^
If you want to run R code straight from JupyterLab without using RStudio you can use the ``R`` kernel. If you don't see it on the select list, you need to install the ``iRkernel`` package. 
Install the package and the spec:

.. code-block:: r

    install.packages('IRkernel')
    IRkernel::installspec() 


Mangaing your data
------------------

.. note:: Any data outside ``/home/jovyan`` will be lost when the environment is restarted. Make sure you keep the files you don't want to lose somewhere inside the home folder.


Upload using GUI
^^^^^^^^^^^^^^^^
You can copy files to and from Jupyter directly in a web interface (Menu and a button in the interface).


Copying data to/from other hosts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You can also copy data to/from other hosts, like the farm, using a terminal (click on the ``Terminal`` icon in the Launcher).

**Using rsync**

Copy from the farm to the local environment:

.. code-block:: bash

    rsync -avzh USER@farm5-login:/nfs/users/nfs_u/USER/<some-path>/ farm/

Copy from the local environment to the farm:

.. code-block:: bash

    rsync -avzh <some-path> USER@farm5-login:/nfs/users/nfs_u/USER/

**Using scp**

Copy from the farm to the local environment:

.. code-block:: bash

      scp -r USER@farm5-login:/nfs/users/nfs_u/USER/<some-path>/ farm/

Copy from the local environment to the farm:

.. code-block:: bash

    scp -r farm/ USER@farm5-login:/nfs/users/nfs_u/USER/<some-path>/ 


Mounting the farm on jupyter (sshfs)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To mount the farm's base paths (``/nfs``, ``/lustre`` and ``/warehouse``) on your jupyter instance:

#. Open a new terminal on your Jupyter.

#. Type ``mount-farm``, then press Enter.

#. When prompted for your username and password input them.


The three folders will be mounted on the root folder of your instance. 
Try opening a new terminal and change directory to your farm home ``cd /nfs/users/nfs_u/usr99`` or your team's lustre ``cd /lustre/scratch11X/team999`` and then type ``ls`` to see the files. You can use the same paths in your notebooks.

.. note:: You will not see these folders in Jupyter's File Browser because it only shows ``/home/jovyan``, if you really want to see them on your File Browser you need to **create symlinks** from the mounted folders to your home folder.
    For example: ``ln -s /nfs /home/jovyan/nfs``

.. warning:: Mounting folders with many files/folders inside them may affect Jupyter. We redommend to only link particular folders and not the whole mounting point.

.. Mounting NFS storages
.. ^^^^^^^^^^^^^^^^^^^^^

.. 1. Create a folder where to mount the share: ``mkdir -p ~/home/jovyan/shared``

.. 2. Mount the storage:

.. .. code-block:: bash

..     sudo mount.cifs //network/path/to/share/ /home/jovyan/shared -o rw,file_mode=0777,dir_mode=0777,credentials=/root/.cifs


Downloading data
^^^^^^^^^^^^^^^^

By default, JupyterHub does not provide an ability to download folders, but you can create an archive:

.. code-block:: bash

    tar cvfz <some-archive-name.tar> <target-directory>/

and download the resulting file with the right click ``Download`` option.


Exporting notebooks
^^^^^^^^^^^^^^^^


Export as PDF
"""""""""""""

To export a notebook as PDF, install the following pre-requisite software:

.. code-block:: bash

    sudo apt update && sudo apt-get install -y texlive-generic-recommended texlive-generic-recommended

Now you can export a notebook through ``File`` > ``Export notebook as...`` menu.


Knit to PDF
"""""""""""

To export an Rnotebook as PDF, install the following pre-requisite software:

.. code-block:: bash

    wget -qO- "https://yihui.org/gh/tinytex/tools/install-unx.sh" | bash


If that it is not enough, the easiest way is to install the whole texlive package, the downside is that it is **4.5G**:

.. code-block:: bash

    sudo apt update && sudo apt-get install -y texlive-full


Sharing notebooks
-----------------

#. Go to your `API Tokens page <https://jhub.cellgeni.sanger.ac.uk/hub/token>`_ or go to `hub/home <https://jhub.cellgeni.sanger.ac.uk/hub/home>`_ and then click  **"Token"**  on the top menu.
#. Type in a note like **"Shared with collaborator X"**
#. Click the orange button **"Request new API token"**
#. Copy the token that shows up under **"Your new API Token"**. (i.e. ``ba5eba11b01dfaceca55e77ecacaca11``)
#. Go to your jupyter instance, but using the **"tree"** view instead of the "lab" view:  ``https://jhub.cellgeni.sanger.ac.uk/user/<your username>/tree``
#. Find your notebook and open it. You should be on a link that looks like:  ``https://jhub.cellgeni.sanger.ac.uk/user/<your username>/notebooks/some_notebook.ipynb``
#. Add this to the end of the link: ``?token=<your API token>`` and copy that link. (i.e.: ``?token=ba5eba11b01dfaceca55e77ecacaca11``)
#. Share what you have copied. It should be something like: ``https://jhub.cellgeni.sanger.ac.uk/user/<your username>/notebooks/some_notebook.ipynb?token=<your API token>``
#. Once you have finished the collaboration. Go to your `API Tokens page <https://jhub.cellgeni.sanger.ac.uk/hub/token>`_ and click **"Revoke"** to delete that access token.


iRODS
-----------------

iRODS support is provided using a wrapper script and a singularity image already copied to your home profile. 
Before start using iRODS, you'll need to copy your environment file from the farm to your jupyter. Open a Terminal and please follow this steps:

1. Use ``mount-farm`` and input your credentials when promted.
 
2. Copy ``irods_environment.json`` from your home directory on the farm to your Jupyter instance:

.. code-block:: bash

    cp /nfs/users/nfs_u/USER/.irods/* ~/.irods/

3. Run ``irods iinit``, it will ask for your PAM password *(Sanger password, same as the one you use for the farm).*

4. Run all `icommands avaiable <https://docs.irods.org/master/icommands/user/>`_ using ``irods <icommand_name>``. For example: ``irods ils`` or ``irods ihelp``.

.. note:: **"irods iinit" also asked for iRODS password?** Go to the farm and type: ``head -1 ~/.irods/irods_password``, the output is your password.

.. warning:: These instructions asume you already have an iRODS account setup on the farm, if you don't please contact ServiceDesk.

Running containers
------------------

The jupyter environment includes **Singularity**, a container platform that allows creating and running tools in a portable and reproducible way. You can build a container using Singularity on your Jupyter instance, and then run it the farm. Your container is a single file, and you don’t have to worry about how to install all the software you need on each different operating system. Read more about building and running singularity containers on the `official docs <https://sylabs.io/docs/>`_.


Troubleshooting
---------------


Restart your instance
^^^^^^^^^^^^^^^^^^^^^

Sometimes, a server restart might solve an issue. For that:

#. Go to the menu "File" > "Hub Control Panel" or browse to your `Hub Home <https://jhub.cellgeni.sanger.ac.uk/hub/home>`_

#. Click ``Stop My Server``

#. Wait 2 minutes and reload the page.

#. Access `https://jhub.cellgeni.sanger.ac.uk/ <https://jhub.cellgeni.sanger.ac.uk/>`_ to get your instance up and running again.


Check storage usage
^^^^^^^^^^^^^^^^^^^

- Check your disk usage from a terminal using ``df -h /home/jovyan/`` or ``du -ha -d 1 ~``

- Find large files in your instance. Check files larger than 1GB from a terminal using: ``find /home/jovyan -size +1G -ls``. 

- Get usage of general folders under your home directory from a terminal ``du -h --max-depth=1 /home/jovyan/``


RStudio errors
^^^^^^^^^^^^^^

- ``[Errno 111] Connection refused`` error, try restarting the server.

- ``Rsession did not start in time`` or ``Error 500`` , go to the `lab` interface, start terminal, and delete the last R session and then reload RStudio:

.. code-block:: bash

    ls -a .rstudio/sessions/active  # see all active sessions
    rm -r ./rstudio/sessions/active/<session-name>  # note the name of the last active session and delete it

- ``Could not start RStudio in time`` error, it might be because you ran out of disk space. delete some files, move them to the farm or request more storage.




How to get help
---------------
For any Jupyter Hub related questions please use our `MatterMost channel <https://mattermost.sanger.ac.uk/cellgeninf/channels/jupyterhub>`_. There are lots of users there who can quickly answer your questions.
