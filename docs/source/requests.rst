Jira Requests
=============

.. tip::
    You can submit a CellGenIT support request at any time by creating a ticket using the `CellGenIT Customer Portal <https://cellgeni-jira.sanger.ac.uk/servicedesk/customer/portal/1>`_.

To create a support ticket log in to the `CellGenIT Customer Portal`_, select one of the categories (``Data sharing``, ``Imaging``, ``JupyterHub``, ``Run a pipeline``, ``Websites`` or ``Other questions``) and then fill in the information form.
Make sure any information needed resides in the ticket. You can attach files after creating the ticket. Having all the possible information there will ensure that we fully document any issues or requests for a proper resolution. To view your existing tickets click 'Requests' in the top navigation bar. 


When submitting a request to the CellGenIT team multiple types of request can be made. These include: 

* Sharing data with external collaborators 
* Running a pipeline
* A JupyterHub issue 
* Requestion a GPU cloud notebook 
* Uploading imaging data to OMERO 
* Uploading data to Array Express
* Downloading public data
* Building a website

This FAQ will guide you in the best practices for submitting tickets to our team.


Datsharing
----------

When sharing data with external collaborators there are two methods used. Sharing via the **Globus** software or sharing via an **S3** bucket.
Please check with your collaborator if they have a Globus account or not, if they do not then a download link will be provided which will work for 7 days. 
Uppon request, a new link can be generated for the collaborator that lasts longer. To make easier for everyone, if the collaborators now in advance they'll need more time this information needs to be part of the ticket.

Pipelines
---------

We run multiple pipelines each which has slightly different information. Please provide the information listed under the following commonly used pipelines 
for the pipeline you wish to run in the ticket.

**STARsolo:**

* Sample IDs
* Study ID
* Reference Genome
* Location of data (i.e. IRODs path or FARM path)
* 10X chemistry
* BAM output is required? (i.e. for running souporcell using starsolo output)

**Souporcell:**

* Sample IDs
* Study ID
* Reference Genome
* Location of data (i.e. IRODs path or FARM path)
* Number of donors for each sample (the k value)
* Common variants VCF (default we used for GRCh38 is ``2p_1kgenomes_GRCh38.vcf``)

**Cellatac:**

* Sample IDs
* Study ID
* Reference Genome
* Location of data (i.e. IRODs path or FARM path)

**Cellbender:**

* Sample IDs
* Study ID
* Reference Genome;
* Location of data (i.e. IRODs path or FARM path)
* Expected cells value for each sample
* Total droplets value for each sample
* The EPOCHS value (default is ``150``)
* The FPR value (default is ``0.01```)
* The learn value (default is ``0.0001``)

**Cell2location:**

* reference data, one of the following:

 * h5ad file with raw read counts (please specify it if raw counts are in adata.raw). Celltypes, library_id and all other covariates that are to be taken into account should be provided as adata.obs columns. Please specify corresponding column names.
 * csv file with pre-estimated reference signatures

* visium data, one of the following:

 * pre-compiled h5ad with all visium samples and raw read counts
 * paths to SpaceRanger outputs (farm or irods)

Please note we run other pipelines as seen `the Pipelines section <https://cellgeni.readthedocs.io/en/latest/pipelines.html>`_, but these are less common so please provide as much detail as possible in the ticket and we may ask for additional information.

JupyterHub
----------

If you are having issues with your JupyterHub or need guidance first check our `JupyterHub help page <https://cellgeni.readthedocs.io/en/latest/jupyterhub.html>`_.
If you're issue still isn't solve please include the following:

* GitHub username
* Explanation of task you are trying to complete
* The code chunk executed
* Any error messages produced
* The environment you are working in (i.e. a conda environment, a specific kernal or rstudio

GPU Notebooks
-------------

To request a GPU notebook you do not need to open a ticket, instead you need to fill in this `form <https://forms.gle/NLdvCHnzjgZXcXPD7>`_. It requires you to have a Sanger Google account. More information on the cloud notebooks can be found in the `Cloud GPU Notebook <https://cellgeni.readthedocs.io/en/latest/cloud-gpu-notebooks.html>`_ section. 


OMERO image uploads
-------------------

NDPI and TIFF images can be uploaded the `Sanger Imaging Platform <https://omero.sanger.ac.uk>`_ by the CellGenIT team without processing but PerkinElmer's Phoenix *(Opera and Operetta)* or Hamamatsu's NanoZoomer Image Sets *(NDPIS - one NDPI per channel)* need to be processed before importing. Read more about how to prepare log files for importing or stitching files on the `Imaging section <https://cellgeni.readthedocs.io/en/latest/imaging.html>`_. 

ArrayExpress uploads
--------------------

For ArrayExpress submissions we take care of the uploading of the sequencing
data. The submission itself should be handled by a scientist, usually one of
the first authors. In the submission process you will be given an ArrayExpress
FTP address to upload the data to. You can open a JIRA ticket with us (this can
be done in advance of receiving the FTP location), specify the sample names for
which you need data uploaded (ideally split into types if the samples have
different library types), and supply the FTP address when you have it.  We will
retrieve the sequencing data from IRODS, convert it from CRAM to fastq format,
upload the data, and supply a file that has md5sums for each file that was
uploaded. ArrayExpress will request this file from you during the submission
process so that it can check the integrity of the uploaded sequencing data.



Websites
--------

We can make internal or external websites ready to accompany papers that are published or to host data for other purposes. In order to do so the following
information is needed:

* The purpose of the website (i.e. to host cellxgene plots, to download a copy of some data etc)
* Whether a custom URL is needed:

  * Domains like ``https://your-website.cellgeni.sanger.ac.uk`` are provided for free
  * Domains like ``https://your-website.org`` need a  cost code so the the WebTeam can buy them

* A template design you like the look of to base the website off of
* Where it needs to be accesible from:

  * internal only (accesible on site or using VPN)
  * external (accesible from everywhere)

* Whether it needs to be password protected or not
