Single Cell Visualizations
==========================

Authors: `Batuhan Cakir <https://www.sanger.ac.uk/person/cakir-batuhan-1015271/>`_, `Simon Murray <https://www.sanger.ac.uk/person/murray-simon/>`_ and `Vladimir Kiselev <https://www.sanger.ac.uk/person/kiselev-vladimir-yu/>`_. 

If you would like to make your single-cell RNA-seq data publicly available on a website, for example as a supplement for a publication, we can help you with that!

cellxgene
---------

We use `cellxgene <https://github.com/chanzuckerberg/cellxgene>`_ to visualize single cell RNA-seq data. **cellxgene** is an interactive data explorer which is very scalable and flexible.

To be able for us to create a **cellxgene** website for your data we need to have your data in the `h5ad (AnnData) <https://anndata.readthedocs.io>`_ format. 

AnnData
-------

AnnData format usually contains the following slots:

- **X** contains the expression matrix.
- **obsm** contains the embeddings data.
- **obs** contains the cell metadata.
- **var** contains the gene metadata.

.. image:: img/visualisations/anndata.svg
   :width: 700

When you work with **cellxgene** you only need to modify two of the slots above: **obsm** and **obs**.

X slot
^^^^^^

The **X** slot should contain a normalised expression matrix. Raw counts can still be loaded into **cellxgene**, but they are not recommended for visualization: gene search and gene colouring will use the raw count values directly, so the colour scale and value range can look skewed or difficult to interpret.

It is recommended to store the expression matrix in ``CSR`` (compressed sparse row) format to create smaller files depending on sparsity of your data instead of ``CSC`` (compressed sparse column) format or dense Numpy array. 

To convert your expression matrix into the ``CSR`` format please use:

.. code-block:: python
    
    adata.X = scipy.sparse.csr_matrix(adata.X)

To convert your expression matrix into the Numpy array please use:

.. code-block:: python

    adata.X = scipy.sparse.csr_matrix.toarray(adata.X)

obsm slot
^^^^^^^^^

To visualize your cells in 2D **cellxgene** uses **obsm** slot. If there are multiple embeddings stored in this slot they will all be available on the web interface. 

.. note:: **cellxgene** requires that all of the embeddings' names are prefixed with ``X_``. For example, ``X_umap``, ``X_pca`` or ``X_some_embedding``.

obs slot
^^^^^^^^

To highlight and colour your cells **cellxgene** uses **obs** slot. The colouring will depend on the type of you cell metadata contained in the **obs** slot.

When the metadata is *categorical*, i.e. there is one colour per category, the visualization will look like this:

.. image:: img/visualisations/categorical.png
   :width: 700

To make your cell metadata categorical please use the following code:

.. code-block:: python

    import pandas as pd
    adata.obs['metadata_name'] = pd.Categorical(adata.obs['metadata_name'])

When the metadata is *continuous*, the visualization will look like this:

.. image:: img/visualisations/continuous.png
   :width: 700

.. note:: Note there is a continuous scale on the right side of the plot.

To make your cell metadata continuous please use the following code:

.. code-block:: python

    import numpy as np
    adata.obs['metadata_name'] = np.float32(adata.obs['metadata_name'])

Optimizations
^^^^^^^^^^^^^

CELLxGENE visualizations usually don't need all the same raw data as your prepared count matrices. To make an optimize object for CELLxGENE we recommend you: remove unncesary observations, make sure your continuous data is 32bit and not 64bit, remove any var that is not the anndata index, and remove all raw and additional layers data.


.. code-block:: python

    import scanpy as sc
    adata = scanpy.read(filename)

    # cast float64 to float32
    for c in list(adata.obs.select_dtypes(include='float64')):
        adata.obs[c] = adata.obs[c].astype('float32')

    # cast int64 to int32
    for c in list(adata.obs.select_dtypes(include='int64')):
        adata.obs[c] = adata.obs[c].astype('int32')

    # remove additional var columns
    adata.var = adata.var.drop(adata.var.columns, axis='columns')

    # fix colors — if you've used #RRGGBBAA instead of #RRGGBB this will fix that
    for color_annotation in [k for k in  adata.uns.keys() if k.endswith("_colors")]:
        try:
            adata.uns[color_annotation] = [color[:7] for color in adata.uns[color_annotation]]
        except:
            pass

    # remove raw and layers
    del adata.raw
    del adata.layers    

    # write out a new 'filename.cellxgene.h5ad'
    adata.write(filename.replace(".h5ad",".cellxgene.h5ad"))


Visium data
^^^^^^^^^^^

If your want to use cellxgene with **Visium data**, you need to follow these steps:

You can use ``scanpy.read_visium`` function to read from Space Ranger output folder and it will import everything needed to AnnData. Be careful that the images should contain one sample only. If not, you need to crop the other samples out.

The spatial embedding layer should be contained in ``obsm`` and be named ``X_spatial``. Other layers can exist, but only this one will have the spatial feature enabled.

.. code-block:: python

    adata.obsm['X_spatial'] = adata.obsm['spatial']
    del adata.obsm['spatial']

If you already have h5ad file before importing Visium and add spatial features from the Visium data that, you should transfer the same slots from the h5ad file created after you imported Visium data.

.. code-block:: python

    # adata is the original one, adata2 is the AnnData you imported to Python via `scanpy.read_visium`

    adata.uns['spatial'] = adata2.uns['spatial']
    adata.obsm['X_spatial'] = adata2.obsm['X_spatial']
    
Cell metadata (e.g. clustering) should be imported manually. You should use ``pandas.read_csv`` to add them, and change their type with ``.astype("category")`` if not continuous:

.. code-block:: python

    adata.obs['clustering_x'] = pandas.read_csv('clustering_x.csv', index_col = 'Barcode')
    adata.obs['clustering_x'] = adata.obs['clustering_x'].astype("category")
    
If you want to integrate your Cell2Location output to your h5ad file, you should add the cell abundance tables from csv or obsm slot to obs slot to be able to color your cells by cell abundances.
If you have csv files, you have to import csv to h5ad.

.. code-block:: python

    # if your cell abundance data are in csv files
    cell_abundance = pd.read_csv('/your/path/to/csv')
    adata.obs = pd.concat([adata.obs, cell_abundance], axis=1)
    adata.obsm['q05_cell_abundance_w_sf'] = pd.read_csv('/your/path/to/csv')
    
    # if your cell abundance data are in obsm slot
    adata.obs = pd.concat([adata.obs, adata.obsm['q05_cell_abundance_w_sf']], axis=1)

If you have cell abundance table in another h5ad, you can transfer it to main h5ad file:

.. code-block:: python

    adata.obs = pd.concat([adata.obs, adata2.obsm['q05_cell_abundance_w_sf']], axis=1)

You can check and download the notebook to make your Visium data compatible with cellxgene from Google Colab:  
https://colab.research.google.com/github/cellgeni/docs/blob/master/docs/source/visium_to_cxg.ipynb

Data Conversion
^^^^^^^^^^^^^^^

We recommend `schard <https://github.com/cellgeni/schard>`_ for loading
AnnData (``h5ad``) files into R as **Seurat** or **SingleCellExperiment**
objects. It is implemented in R without ``reticulate`` and can load
expression matrices, cell and gene metadata, and supported dimensionality
reductions. It also supports spatial Visium data in AnnData format.

Install ``schard`` from GitHub:

.. code-block:: r

   devtools::install_github("cellgeni/schard")

AnnData to Seurat
~~~~~~~~~~~~~~~~~

.. code-block:: r

   seurat_object <- schard::h5ad2seurat("filename.h5ad")

AnnData to SingleCellExperiment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: r

   sce_object <- schard::h5ad2sce("filename.h5ad")

AnnData (Visium) to spatial Seurat
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: r

   spatial_object <- schard::h5ad2seurat_spatial("visium.h5ad")

   # Alternatively, return a list of Seurat objects, one per slide.
   spatial_slides <- schard::h5ad2seurat_spatial("visium.h5ad", simplify = FALSE)

Loading AnnData metadata only
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you only need cell metadata, you can avoid loading the full expression matrix:

.. code-block:: r

   cell_metadata <- schard::h5ad2data.frame("filename.h5ad", "obs")

.. note::

   ``schard`` reads AnnData files into R but does not write ``h5ad`` files.
   For Seurat ``.rds`` files, use ``py8rds`` to convert directly into AnnData
   without an R installation. Some other format conversions still require
   ``sceasy``.

Seurat (RDS) to AnnData using py8rds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`py8rds <https://github.com/cellgeni/py8rds>`_ reads R ``.rds`` and
supported ``.qs2`` files directly in Python, without requiring R or
``reticulate``. For Seurat objects, it can create an AnnData object suitable
for downstream Python analysis and export to ``h5ad``.

Install ``py8rds``:

.. code-block:: bash

   pip install git+https://github.com/cellgeni/py8rds.git

Convert a saved Seurat object:

.. code-block:: python

   import py8rds

   adata = py8rds.seurat2adata("seurat.rds")
   adata.write_h5ad("filename.h5ad")

.. note::

   By default, ``seurat2adata`` reads the first assay and its ``counts``
   layer. Select the appropriate assay and layer for your analysis, and
   verify the resulting expression matrix, cell metadata, and embeddings.
   Not all Seurat content has an equivalent AnnData representation.

Reading RDS metadata and expression matrices
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``py8rds`` can also read R data frames and named dense or sparse matrices
without converting an entire Seurat object:

.. code-block:: python

   import py8rds

   metadata = py8rds.as_data_frame("metadata.rds")
   matrix_adata = py8rds.as_anndata("matrix.rds")

For unfamiliar RDS structures, inspect them before selecting components:

.. code-block:: python

   robj = py8rds.parse_rds("data.rds")
   robj.show(level=2)

``py8rds`` also provides ``seurat2adata_spatial`` for spatial Seurat
objects. See the `py8rds documentation <https://github.com/cellgeni/py8rds>`_
for supported formats and examples.

Other format conversions using sceasy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`sceasy <https://github.com/cellgeni/sceasy>`_ remains available for
conversions not covered by ``schard`` or ``py8rds``, including conversions
between **SingleCellExperiment**, **Seurat**, and **Loom**. See its
`sceasy installation instructions <https://github.com/cellgeni/sceasy>`_.
AnnData files can be used with
`cellxgene <https://github.com/chanzuckerberg/cellxgene>`_ for interactive
exploration of single-cell data.

Seurat to SingleCellExperiment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: r

   sceasy:::convertFormat(seurat_object, from="seurat", to="sce",
                          outFile="filename.rds")

SingleCellExperiment to AnnData
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: r

   sceasy:::convertFormat(sce_object, from="sce", to="anndata",
                          outFile="filename.h5ad")

SingleCellExperiment to Loom
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: r

   sceasy:::convertFormat(sce_object, from="sce", to="loom",
                          outFile="filename.loom")

Loom to AnnData
~~~~~~~~~~~~~~~

.. code-block:: r

   sceasy:::convertFormat("filename.loom", from="loom", to="anndata",
                          outFile="filename.h5ad")

Loom to SingleCellExperiment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: r

   sceasy:::convertFormat("filename.loom", from="loom", to="sce",
                          outFile="filename.rds")

Examples
--------

We have already created a couple of websites for some of our programme members. You can have a look at them at the following links:

| `https://www.kidneycellatlas.org <https://www.kidneycellatlas.org/>`_
| `https://www.tissuestabilitycellatlas.org <https://www.tissuestabilitycellatlas.org/>`_
| `https://www.cvidcellatlas.org <https://www.cvidcellatlas.org/>`_
| `https://www.gutcellatlas.org <https://www.gutcellatlas.org/>`_
| `https://www.heartcellatlas.org <https://www.heartcellatlas.org/>`_
| `https://www.covid19cellatlas.org <https://www.covid19cellatlas.org/>`_
| `https://developmentcellatlas.cellgeni.sanger.ac.uk <https://developmentcellatlas.cellgeni.sanger.ac.uk/>`_
| `https://maternal-fetal-interface.cellgeni.sanger.ac.uk <https://maternal-fetal-interface.cellgeni.sanger.ac.uk/>`_
| `https://hemocytes.cellgeni.sanger.ac.uk <https://hemocytes.cellgeni.sanger.ac.uk/>`_
| `https://melanoma.cellgeni.sanger.ac.uk <https://melanoma.cellgeni.sanger.ac.uk/>`_

Demo
----

.. raw:: html

    <iframe width="560" height="315" src="https://www.youtube.com/embed/5Fg5admFe9M" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

