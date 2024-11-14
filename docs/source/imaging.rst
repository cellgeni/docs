Imaging
=======

Introduction
------------

Imaging data are the images produced by microscopy of cells, organelles, tissues etc and their associated metadata. 

OMERO
-----

The `Sanger Imaging Platform <https://omero.sanger.ac.uk>`_ uses the OMERO software for the analysis of microscope imaging data. Storing data in OMERO is a procedure Cellular Genetics Informatics provide.

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

Data in OME-TIFF and NDPI format can be imported as-is provided the images have pyramids generated. Please submit a ticket with a TSV file with the following columns:

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
Please fill the following ``xlsx`` file and submit a ticket with it attached. 

`HamamatsuLogTemplate.xlsx <https://cellgeni.cog.sanger.ac.uk/HamamatsuLogTemplate.xlsx>`_

Stitching
---------

Phoenix exported tiles need to be stitched together before importing to OMERO.
Please fill the following ``xlsx`` file and submit a ticket with it attached.

`PhonexLogTemplate.xlsx <https://cellgeni.cog.sanger.ac.uk/PhonexLogTemplate.xlsx>`_


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
 - H&E serial images of the same tissue `link to repo <https://github.com/cellgeni/image_registration_tools/tree/main/serial_registration_HE>`_
 - DAPI images of the same tissue taken at different imaging cycles `link to repo <https://github.com/VasylVaskivskyi/microaligner>`_
 - Multimodal registration between DAPI and H&E image (in development/testing phase)



.. |image_omero_project| image:: https://omero-guides.readthedocs.io/en/latest/_images/management3b.png
   :height: 0.245in
.. |image_omero_dataset| image:: https://omero-guides.readthedocs.io/en/latest/_images/management3c.png
   :height: 0.215in
