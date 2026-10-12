Imaging
=======

Introduction
------------

Imaging data encompasses microscopy and histology images of cells, organoids, and tissues, together with their associated metadata and molecular measurements. Our team works across a wide range of imaging technologies, including spatial transcriptomics (10x Genomics Xenium and Visium), spatial proteomics and multiplexed immunofluorescence (Akoya PhenoCycler), and RNA in situ hybridisation (RNAscope) using high-content imaging platforms such as the Opera Phenix and Operetta.

We also process and analyse whole-slide histology images, including H&E-stained tissue sections acquired using Hamamatsu scanners, and organoid images captured using Hamamatsu, Nikon and Cytation imaging systems.

Our work spans image processing, segmentation, feature extraction, spatial analysis, and the integration of imaging and molecular data to support biological discovery.

OMERO
-----

.. tip::
    Use the email ``omero-help [at] sanger.ac.uk`` or the slack channel ``#usergroup_omero`` to get support.

The `Sanger Imaging Platform <https://omero.sanger.ac.uk>`_ uses OMERO Plus, an enhanced version of the open-source OMERO platform for managing, visualising, and analysing microscopy imaging data. OMERO provides centralised storage, metadata management, and secure access to imaging datasets, enabling researchers to explore and share images across different microscopy technologies.

Cellular Genomics Informatics supports researchers with the ingestion, organisation, and management of imaging data within OMERO Plus, facilitating its integration into scientific analysis workflows.

Please watch the following 10min video from the `The Jackson Laboratory <https://www.jax.org/>`_ to learn the basics of browsing and annotating images using OMERO:

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto; margin-bottom: 2em;">
        <iframe src="https://www.youtube.com/embed/e3u-Ugd4W7w" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

OMERO uploads
-------------

Users need to get data off microscopes quickly. Choose from the following use cases on how to request your data be uploaded to Sanger's OMERO server.


Upload OME-TIFF and NDPI
^^^^^^^^^^^^^^^^^^^^^^^^

Data in OME-TIFF and NDPI format can be imported as-is provided the images have pyramids generated. Please submit a `FreshService <https://sanger.freshservice.com/>`_ ticket (category ``Imaging support``, see :doc:`requests`) with a TSV file with the following columns:

.. list-table:: OMERO_IMPORT.tsv
   :widths: 25 30
   :header-rows: 0

   * - **filename**
     - Name of the file (``my_file.ome.tiff`` or ``my_file.ndpi``)
   * - **location**
     - Path to the file folder (ie: ``/nfs/team123/``)
   * - **omero_group**
     - OMERO group name (ie. ``MJ_CDB``)
   * - **omero_project**
     - |image_omero_project| OMERO project folder name *(first level folder)*
   * - **omero_dataset**
     - |image_omero_dataset| OMERO dataset folder name *(second level folder)*
   * - **omero_username**
     - Image owner's username (ie. ``ob1``)


Upload NDPIS
^^^^^^^^^^^^

NDPIS need to be processed before importing this involves merging all NDPI image for individual channels into one image.
Please fill the following ``xlsx`` file and submit a `FreshService <https://sanger.freshservice.com/>`_ ticket (category ``Imaging support``) with it attached.

`HamamatsuLogTemplate.xlsx <https://cellgeni.cog.sanger.ac.uk/HamamatsuLogTemplate.xlsx>`_

OMERO downloads
^^^^^^^^^^^^^^^

You can retrieve the original files that were imported to OMERO using the `omero-download tool <https://github.com/cellgeni/omero-download>`_. 
Simply use the ``cellgen/omero-download`` module on the farm and the run the command line tool.

Example usage:

  a) Load the module
  
    .. code-block:: bash

      $ module load cellgen/omero-download
      ** Using custom python for this environment
      ** See avaiable options using: omero-download -h


  b) Download ImageId ``123``, ``456``, and, ``789`` and place them inside the ``/path/to/download`` folder:

    .. code-block:: bash

      omero-download --images 123 456 789 --output_dir /path/to/download


Stitching
---------

Opera Phenix exported raw tiles need to be stitched together before importing to OMERO. 
Please fill the following ``xlsx`` file and submit a `FreshService <https://sanger.freshservice.com/>`_ ticket (category ``Imaging support``) with it attached.

`Operetta-PhenixLogTemplate.xlsx <https://cellgeni.cog.sanger.ac.uk/Operetta-PhenixLogTemplate.xlsx>`_


Segmentation
------------

We can also run segmentation on your images provided you give us the following information:

.. list-table::
   :widths: 25 30
   :header-rows: 0

   * - **input_image**
     - Full path to the image file. ie. ``/nfs/team123/my_file.tiff``
   * - **dapi_ch**
     - DAPI channel index (default ``0``)
   * - **cyto_ch**
     - Cyto channel index (default ``''``)
   * - **object_diameter**
     - Object diameter (default ``35``)
   * - **flow_threshold**
     - Flow threshold (default ``0.5``)
   * - **expand**
     - Cell border expansion ammount (default ``35``)
   * - **magnification**
     - Magnification level (default ``20``)
   * - **model_type**
     - Cellpose model type (default ``cyto``)

Our segmentation pipeline works on GPU using `Cellpose <https://github.com/MouseLand/cellpose>`_ in order to produce 2D or 3D images of cells and nuclei with segmentation for downstream analysis.

If you want to run the pipeline yourslef you can do so following the instructions on GitLab `<https://gitlab.internal.sanger.ac.uk/cellgeni/imaging/segmentation-cellpose>`_


Registration
------------

We can run registration of the images for:
 - H&E serial images of the same tissue (`code to repoitory <https://github.com/cellgeni/image_registration_tools/tree/main/serial_registration_HE>`__)
 - DAPI images of the same tissue taken at different imaging cycles (`microaligner page <https://github.com/VasylVaskivskyi/microaligner>`__)
 - Multimodal registration between DAPI and H&E image (in development/testing phase)


Analysis of bespoke ISS and MERFISH-like epxeriments
----------------------------------------------------

With our `pipeline <https://github.com/BioinfoTongLI/Image-ST>`_ we can perform image registration, peak calling and decoding using `PostCode <https://github.com/BioinfoTongLI/postcode/>`_. Also the pipeline can perform segmentation and transcript assignment. The output dataset can be easily visualised with napari plugin `spatialdata_napari <https://github.com/scverse/napari-spatialdata>`_. For details please contact us


Visium Spots feature extraction
-------------------------------

For Visium experiment output we can run segmentation on H&E image and add segmentation information for each visium spot (number of cells, coverage area etc) using `Cells2Visium <https://github.com/cellgeni/cells2visium>`_.

.. |image_omero_project| image:: https://omero-guides.readthedocs.io/en/latest/_images/management3b.png
   :height: 0.245in
.. |image_omero_dataset| image:: https://omero-guides.readthedocs.io/en/latest/_images/management3c.png
   :height: 0.215in


Xenium segmentation and reprocessing
------------------------------------

For 10x Genomics Xenium experiments, we provide `nf-xenium`, a Nextflow pipeline for scalable and reproducible cell segmentation, transcript reassignment, and downstream processing.

The pipeline supports multiple segmentation approaches, including `Cellpose <https://github.com/MouseLand/cellpose>`_, `ProSeg <https://github.com/dcjones/proseg>`_, `Baysor <https://github.com/kharchenkolab/Baysor>`_, Segger, StarDist, and InstanSeg. These methods allow researchers to refine cell boundaries and improve transcript-to-cell assignments beyond the default Xenium segmentation.

Multiple segmentation methods can be applied to the same experiment, enabling comparison and evaluation of alternative approaches. The pipeline also supports integration with Xenium Ranger, optional quality control, and export to `SpatialData <https://spatialdata.scverse.org/>`_ for downstream analysis and visualisation.

This allows users to evaluate segmentation strategies according to tissue type, imaging characteristics, and scientific objectives.

For assistance with Xenium resegmentation or specific analysis requirements, please contact the Cellular Genomics Informatics team.

