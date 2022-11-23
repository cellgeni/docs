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

To enable IGViewer to be seen on your local machine, remote application forwarding is needed. This can be done by adding the ``-Y`` paramter when ssh'ing 
to the FARM. The full command will become ``ssh -Y sanger-id@farm5-login``

i.e. ``ssh -Y sm42@farm5-login``

Once connected simply run IGViewer using ``/software/cellgeni/IGV_2.15.2/igv.sh /path/to/bam/file/on/FARM`` and the BAM file should be loaded into an 
IGViewer window on your local machine.

JupyterHub
----------

To use IGViewer on JupyterHub, a python package called `igv-notebook <https://github.com/igvteam/igv-notebook>`__ is used.

Copy the following code and change the paths to the local of your BAM files.

.. note::
  **Two things to note**:
  1) The BAM files must be present on your local JupyterHub instance and not on a mounted FARM path
  2) The path to the bam file should be from the JupyterHub tree root i.e. not include ``/home/jovyan`` and not have a leading slash

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
        
        "url": "path/to/bam",
        
        "indexURL": "path/to/bam/index",
        
        "format": "bam",
        
        "type": "alignment"
    })


  b.zoom_in()




    
