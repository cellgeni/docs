Services we provide
===================

Ticketing system
----------------

We operate using JIRA ticketing system. To be able to put in tickets you need to contact us by either emailing to vk6@sanger.ac.uk or using `our Mattermost channel <https://mattermost.sanger.ac.uk/cellgeninf/channels/jira-requests>`_ (please use your Sanger credentials to login).

Once registered you can put in a ticket using either `our portal <https://cellgeni.atlassian.net/servicedesk/customer/portal/1>`_ or by emailing to support@cellgeni.atlassian.net.

Interactive analysis environment
--------------------------------

We support a Jupyter Hub server running on Sanger Cloud. It is available for everyone in our Programme. Jupyter allows you to run your analysis in multiple environments (``R``, ``python``, ``Julia``, etc.) and also to create and share notebooks containing your analysis, code, equations and visualisations. We think this is an ideal environment for any kind of downstream analysis. For more details please refer to `Jupyterhub documentation <http://jupyter.org/hub>`_
 
Before logging in to JupyterHub please read our instructions here:
https://github.com/cellgeni/notebooks#user-instructions
 
Once you read the instructions go ahead and login with you Sanger credentials here (it may take some time when you do it first time):
https://jupyter.cellgeni.sanger.ac.uk
 
We provide some notebook templates with the pre-installed software. These are located in the ``notebooks`` folder. Corresponding example data is located in the ``data`` folder. Before running your analysis, please make a copy of a notebook template, save it to your home folder and work with the copy. At the moment we have the following notebook templates:

1. `scanpy notebook for analysis of 10X data <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-scanpy.ipynb>`_
2. `Seurat notebook for analysis of 10X data <https://github.com/cellgeni/notebooks/blob/master/files/notebooks/10X-Seurat.Rmd>`_


Run a pipeline
--------------

We offer a range of sequencing processing pipelines. We develop them in two workflow languages: `Nextflow <https://www.nextflow.io/>`_ (very user-friendly) and `CWL <https://www.commonwl.org/>`_ (universal standard).

For the end-users we offer Nextflow pipelines available to run on both Sanger *LSF* farm or *OpenStack* flexible compute environment. The pipelines can be run either by us or by the end-users themselves.

Here is the list of pipelines we develop:

1. `RNA-seq <https://github.com/cellgeni/rnaseq-noqc>`_ (both bulk and single cell)
2. `Chip-seq <https://github.com/cellgeni/chipseq>`_
3. `ATAC-seq <https://github.com/cellgeni/atacseq>`_

.. note:: If you would like to run ``cellranger`` by *10X* please put in a ticket for the NPG group by emailing to new-seq-pipe@sanger.ac.uk and specify which version of the cellranger and the genome you would like to use.

.. important:: Our pipelines start from aligned cram files and mostly concentrate on the downstream analysis (starting from the count matrix). Therefore, when submitting your experiment to sequencing please make sure you specify the correct aligner for your data.

Share data with external collaborators
--------------------------------------

Globus
``````

We use `Globus <https://www.globus.org/>`_ network to share the data with external collaborators. It allows us to share data from e.g. a specific folder on the Sanger LFS cluster directly with the external world.

The sharing process consists of the following steps:

1. We share the data with the user's personal/work email address
2. The user `creates/logs in their Globus account <https://docs.globus.org/how-to/get-started/>`_ using the sharing email
3. The user needs to create a personal Globus endpoint either on their `Linux laptop / compute cluster <https://docs.globus.org/how-to/globus-connect-personal-linux/>`_ or on their `Mac laptop <https://docs.globus.org/how-to/globus-connect-personal-mac/>`_ or on their `Windows laptop <https://docs.globus.org/how-to/globus-connect-personal-windows/>`_.
4. The user activates their personal Globus endpoint by starting globus from the command line if on a cluster/Linux, or by starting the globus application if on Mac/Windows.
5. Once the users personal endpoint is setup they can transfer the data by simply `logging in to their Globus account using the sharing email address and drag and dropping the data <https://docs.globus.org/how-to/get-started/>`_.

For more information please visit the `Globus official documentation <https://docs.globus.org/how-to/>`_.

.. note:: If the user would like to check MD5 hash, the MD5 sum files will be located in the same sharing folder with the data files.

cram files
``````````

Sanger default file format for storing NGS data is ``CRAM`` and this is what we provide to the user when share data with them. Typically ``CRAM`` achieves 40-50% space saving over the alternative ``BAM`` format and much more than that over the compressed ``fastq`` files. For more information please visit `this page <https://www.sanger.ac.uk/science/tools/cram>`_. 

Once the user obtained the data from Globus, the data can be converted from ``CRAM`` to ``fastq`` format using the following steps:

* Install ``samtools`` with version **>=1.8** (in this case ``samtools`` should automatically download the right genome reference if your local installation does not have it) 

* Run the following commands (set NCPU to a number of CPUs, if you are on a multi-cpu machine). This will create paired fastq files ``samplename_1.fastq.gz`` and ``samplename_2.fastq.gz``:

.. code-block:: bash

    samtools collate -O -u -@ NCPU samplename.cram tmppfx | \
        samtools fastq -N -F 0x900 -@ NCPU -1 samplename_1.fastq.gz -2 samplename_2.fastq.gz -

If this does not work, you could try running these first:

.. code-block:: bash

    unset REF_PATH
    unset REF_CACHE

Data submission
---------------

To submit your data to a public repository, e.g. `EGA <https://www.ebi.ac.uk/ega/home>`_ or `ENA <https://www.ebi.ac.uk/ena>`_, please contact datahose@sanger.ac.uk. More information can be found `here <https://stackoverflow.sanger.ac.uk/question/935801792929730560/how-to-submit-data-to-a-public-repository>`_.