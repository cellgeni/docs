Data sharing
============

Globus
------

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
----------

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
