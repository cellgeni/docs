Data sharing
============

Globus
------

We use `Globus <https://www.globus.org/>`_ network to share the data with external collaborators. It allows us to share data from e.g. a specific folder on the Sanger farm directly with the external world.

The sharing process consists of the following steps:

1. We share the data with the user's personal/work email address
2. The user `creates/logs in their Globus account <https://docs.globus.org/how-to/get-started/>`_ using the sharing email
3. The user needs to create a personal Globus endpoint either on their `Linux laptop / compute cluster <https://docs.globus.org/how-to/globus-connect-personal-linux/>`_ or on their `Mac laptop <https://docs.globus.org/how-to/globus-connect-personal-mac/>`_ or on their `Windows laptop <https://docs.globus.org/how-to/globus-connect-personal-windows/>`_.
4. The user activates their personal Globus endpoint by starting globus from the command line if on a cluster/Linux, or by starting the globus application if on Mac/Windows.
5. Once the users personal endpoint is setup they can transfer the data by simply `logging in to their Globus account using the sharing email address and drag and dropping the data <https://docs.globus.org/how-to/get-started/>`_.

For more information please visit the `Globus official documentation <https://docs.globus.org/how-to/>`_.

.. note:: If the user would like to check MD5 hash, the MD5 sum files will be located in the same sharing folder with the data files.

cram files
----------

Sanger default file format for storing NGS data is ``CRAM`` and this is what we provide to the user when share data with them. Typically ``CRAM`` achieves 40-50% space saving over the alternative ``BAM`` format and much more than that over the compressed ``fastq`` files. For more information please visit `this page <https://www.sanger.ac.uk/tool/cram/>`_. 

For an automated approach that retrieves sequencing data directly from Sanger's iRODS archive and converts ``CRAM`` files to ``FASTQ``, see :ref:Retrieving sequencing data from iRODS.

Once the user obtained the data from Globus, the data can be converted from ``CRAM`` to ``FASTQ`` format using the following steps:

* Install ``samtools`` with version **>=1.8** (in this case ``samtools`` should automatically download the right genome reference if your local installation does not have it) 

* Run the following commands (set NCPU to a number of CPUs, if you are on a multi-cpu machine). This will create paired fastq files ``samplename_1.fastq.gz`` and ``samplename_2.fastq.gz``:

.. code-block:: bash

    samtools collate -O -u -@ NCPU samplename.cram tmppfx | \
        samtools fastq -N -F 0x900 -@ NCPU -1 samplename_1.fastq.gz -2 samplename_2.fastq.gz -

If this does not work, you could try running these first:

.. code-block:: bash

    unset REF_PATH
    unset REF_CACHE


Retrieving sequencing data from iRODS
-------------------------------------

We provide `nf-irods-to-fastq <https://github.com/cellgeni/nf-irods-to-fastq>`_, a scalable and reproducible Nextflow pipeline that simplifies retrieving sequencing data from Sanger's iRODS archive and converting ``CRAM/BAM`` files into ``FASTQ`` format.

The pipeline automates sample discovery, metadata retrieval, file downloads, and CRAM-to-FASTQ conversion. It supports processing multiple samples in parallel on Sanger's LSF compute cluster, concatenating FASTQ files and generating MD5 checksums.

Researchers can retrieve and convert their sequencing data by providing a simple CSV file containing sample identifiers, without needing to manually locate CRAM files or manage individual conversions.

For example, create a ``samples.csv`` file:

.. code-block:: text

   sample
   SAMPLE_001
   SAMPLE_002

Then run:

.. code-block:: bash

   nextflow run cellgeni/nf-irods-to-fastq --samples samples.csv --cram2fastq

The pipeline also supports metadata-only discovery and processing existing CRAM files directly. For external data submissions, FASTQ files can be uploaded to FTP servers using a separate pipeline step.

For more information, see the `nf-irods-to-fastq documentation <https://github.com/cellgeni/nf-irods-to-fastq>`_ or contact the Cellular Genomics Informatics team.
