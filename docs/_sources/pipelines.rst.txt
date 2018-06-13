What we can do for you
======================

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

We use `Globus <https://www.globus.org/>`_ network to share the data with external collaborators. It allows us to share data from e.g. a specific folder on the Sanger LFS cluster directly with the external world.