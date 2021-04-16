Imaging Data
============

Introduction
------------

Imaging data are the images produced by microscopy of cells, organelles, tissues etc and their associated metadata. 

OME
---

Open Miscroscopy Environment (OME) produce open source software for the analysis of microscope imaging data. OME maintains the OMERO software which can be used by scientists, developers or institutes to host imaging data remotely. Remote storage requires exporting of data which is a procedure Cellular Genetics Informatics provide. For more information on the OME software, including OMERO, check out their `Website <https://www.openmicroscopy.org>`_.

Pipeline
--------

Our imaging pipeline can work on GPU or CPU depending on availability. The pipeline is ran remotely on cloud hardware hosted by Google. It uses the software from `Cellpose <https://github.com/MouseLand/cellpose>`_ in order to produce 2D or 3D images of cells and nuclei with segmentation for downstream analysis.
