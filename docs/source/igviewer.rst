IGViewer on Remote Environments
===============================

FARM
----

To visulaise BAM files available on the FARM first you need to install some software depending on your local machine.

Mac
^^^

You will need to install the software `xquartz <https://www.xquartz.org/index.html>`__. This can either be done from the Sanger Self Serivce app on your 
machine or from the xquartz `releases page <https://www.xquartz.org/releases/index.html>`__.

Windows
^^^^^^^

You will need to install the software `MobaXterm Home edition <https://mobaxterm.mobatek.net/download.html>`__.

Connecting to FARM
^^^^^^^^^^^^^^^^^^

To enable IGViewer to be seen on your local machine, remote application forwarding is needed. This can be done by adding the ``-Y`` parameter when using SSH to connect to the FARM to enable trusted X11 forwarding. X11 forwarding is a mechanism that allows a user to start up remote applications, and then forward the application display to their local machine. The full command will become ``ssh -Y user99@farm5-login``.


Once connected simply run IGViewer using ``/software/cellgeni/IGV_2.15.2/igv.sh /path/to/file.bam`` and the BAM file should be loaded into an 
IGViewer window on your local machine.

JupyterHub
----------

To use IGViewer on JupyterHub, a python package called `igv-notebook <https://github.com/igvteam/igv-notebook>`__ is used.

You need to install it using ``pip install igv-notebook``

Copy the following code and change the paths to the local of your BAM files.

.. note::
  1. Pip installations do not save between sessions, so every time you restart your instance you need to reinstall igv-notebook
  2. The BAM files must be present on your Jupyter home folder (``/home/jovyan``) and not on a mounted FARM path
  3. When completing ``url:`` amd ``indexURL:`` fields, do not include ``/home/jovyan`` and part of the BAM path file. 

     - For example, ``/home/jovyan/bams/my.bam`` should be inputted as ``bams/my.bam``.
    
    
    
.. code-block:: bash
  
   import igv_notebook

   igv_notebook.init()

   b = igv_notebook.Browser(
     {
         "genome": "hg19",
         "locus": "chr22:24,376,166-24,376,456"
     }
   )

   b.load_track(
     {
         "name": "Local BAM",
         "url": "path/to/fibam",
         "indexURL": "path/to/bam/index",
         "format": "bam",
         "type": "alignment"
     })

   b.zoom_in()
