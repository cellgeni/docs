Nextflow pipelines
==================

We offer a range of sequencing processing pipelines. We develop them using `Nextflow <https://www.nextflow.io/>`_ workflow language.

We run Nextflow pipelines on both Sanger *LSF* farm and on public Clouds using `Netflow Tower <https://tower.nf/>`_.

Here is the list of the major pipelines we maintain and support:

1. `cellranger (for 10x data) <https://github.com/cellgeni/various_cellrangers>`_
2. `cellranger (for CITEseq and HTO data) <https://github.com/cellgeni/cellranger_cite_hash>`_
3. `souporcell (for 10X data) <https://github.com/cellgeni/nf-souporcell>`_
4. `ATAC-seq (for 10X and Smartseq2 data) <https://github.com/cellgeni/cellatac>`_
5. `velocyto (for 10X data) <https://github.com/cellgeni/nf-velocyto>`_
6. `STARsolo (for 10X data) <https://github.com/cellgeni/STARsolo>`_
7. `Bulk RNA-seq <https://github.com/cellgeni/bulk_rnaseq>`_
8. `CellBender <https://github.com/cellgeni/nf-cellbender>`_ 
9. `cell2location <https://github.com/cellgeni/c2l>`_
10. `reprocessing public data <https://github.com/cellgeni/reprocess_public_10x>`_
11. Data Sharing - via S3 or Globus
12. Image import to Omero Plus
13. Uploading data to ArrayExpress FTPs

Notes
-----

* The pipelines listed above are our most commonly requested pipelines, if you want help with any other tool/software/pipeline then please get in touch and we will try to help you as best we can!

* If you would like to run ``cellranger`` on samples sequenced *at Sanger* please put in a ticket for the NPG group by emailing ``new-seq-pipe [at] sanger.ac.uk`` with the version of the cellranger and the genome you would like to use.

