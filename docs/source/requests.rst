Requests FAQ
============

When submitting a request to the CellGenIT team multiple types of request can be made. These include: 

* Sharing data with external collaborators 
* Running a pipeline
* A JupyterHub issue 
* Requestion a GPU cloud notebook 
* Uploading data to OMERO or Array Express
* Building a website

This FAQ will guide you in the best practices for submitting tickets to our team.

Datsharing
----------

When sharing data with external collaborators there are two methods used. Sharing via the Globus software or sharing via an AWS bucket.
Please check with the collaborator if they have Globus or not, if they do not a download link will be provided which will work for one week. If the collaborator 
needs longer this can be sorted. 

Pipelines
---------

We run multiple pipelines each which has slightly different information. Please provide the information listed under the following commonly used pipelines 
for the pipeline you wish to run in the ticket.

Starsolo:

* Sample IDs
* Study ID
* Reference Genome
* Location of data (i.e. IRODs path or FARM path)
* 10X chemistry
* If BAM output is required (i.e. for running souporcell using starsolo output)

Souporcell:

* Sample IDs
* Study ID
* Reference Genome
* Location of data (i.e. IRODs path or FARM path)
* Number of donors for each sample (the k value)
* Common variants VCF (default we used for GRCh38 is 2p_1kgenomes_GRCh38.vcf)

Cellatac:

* Sample IDs
* Study ID
* Reference Genome
* Location of data (i.e. IRODs path or FARM path)

Cellbender:

* Sample IDs
* Study ID
* Reference Genome;
* Location of data (i.e. IRODs path or FARM path)
* Expected cells value for each sample
* Total droplets value for each sample
* The EPOCHS value (default is 150)
* The FPR value (default is 0.01)
* The learn value (default is 0.0001)

Please note we run other pipelines as seen `here <https://cellgeni.readthedocs.io/en/latest/pipelines.html>`_ but these are less common so please provide as much detail as possible in the ticket 
and we may ask for additional information.

JupyterHub
----------

If you are having issues with your JupyterHub or need guidance first check our `JupyterHub help page <https://cellgeni.readthedocs.io/en/latest/jupyterhub.html>`_.
If you're issue still isn't solve please include the following:

* Explanation of task you are trying to complete
* The code chunk executed
* Any error messages produced
* The environment you are working in (i.e. a conda environment, a specific kernal or rstudio

GPU Notebooks
-------------

To request a GPU notebook you do not need to open a ticket, instead you need to fill in this `form <https://forms.gle/NLdvCHnzjgZXcXPD7>`_. It requires you to have
a Sanger Google account.

OMERO uploads
-------------

Martin and Vasyl information needed

Array Express uploads
---------------------

Stijn information needed

Websites
--------

We can make internal or external websites ready to accompany papers that are published or to host data for other purposes. In order to do so the following
information is needed:

* The purpose of the website (i.e. to host cellxgene plots, to download a copy of some data etc)
* Whether a custom URL is needed (i.e. ``https://your-website.ac.uk`` vs ``https://your-website.cellgeni.sanger.ac.uk``)
* Cost code for custom URL (if applicable)
* A template design you like the look of to base the website off of
* Whether the website needs to be internal only (i.e. private until publication)

