Notebooks
=========

We have a set of premade notebooks available to users on JupyterHub. These notebooks are designed to guide users through various stages of downstream analysis
and to aid them is inspecting their own data. We have five notebooks available each of which we believe to cover an important aspect of downstream analysis.

.. graphviz::

        digraph foo {
                a [shape=rectangle, style=filled, fillcolor=grey, label="10k PBMC dataset\nfrom 10X Genomics"];
                b [shape=rectangle, style=filled, fillcolor=grey, label="10k PBMC with\nambient RNA\nremoved"];
                c [shape=rectangle, style=filled, fillcolor=grey, label="10k PBMC\ndetected doublets"];
                d [shape=rectangle, style=filled, fillcolor=orange, label="10k PBMC\n-QC and filtering\n-clustering\n-UMAP/TSNE\n-marker selection\n-cell type annotation"];
                e [shape=rectangle, style=filled, fillcolor=cyan, label="10k PBMC\n-multiple integrated datasets\n-unified clustering\n-marker selection"];
                f [shape=rectangle, style=filled, fillcolor=orange, label="10k PBMC\n-QC and filtering\n-clustering\n-UMAP/TSNE\n-marker selection\n-cell type annotation"];
                g [shape=rectangle, style=filled, fillcolor=cyan, label="10k PBMC\n-multiple integrated datasets\n-unified clustering\n-marker selection"];
                a -> b [label=" soupX (R) ", href="https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-SoupX.Rmd", target="_blank"];
                b -> c [label=" scrublet (Python) ", href="https://github.com/cellgeni/notebooks/blob/master/notebooks/new-doublets-scrublet.ipynb", target="_blank"];
                c -> d [xlabel=" Seurat (R) full basic workflow ", href="https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Seurat.Rmd", target="_blank"];
                d -> e [xlabel=" R-based integration methods ", href="https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Integration.Rmd", target="_blank"];
                c -> f [label=" scanpy (Python) full basic workflow ", href="https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Scanpy.ipynb", target="_blank"];
                f -> g [label=" Python-based integration methods "];
        }

SoupX
-----

`Soupx Notebook repository <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-SoupX.Rmd>`_

`Soupx Notebook HTML <https://cellgeni.github.io/notebooks/html/new-10kPBMC-SoupX.html>`_

This notebook describes the usage of soupX R package for ambient RNA (“soup”) removal. We start from dual-indexed 10k PBMC dataset from `10X Genomics website <https://support.10xgenomics.com/single-cell-gene-expression/datasets>`_, processed by CellRanger with `GRCh38 reference 2020-A <https://support.10xgenomics.com/single-cell-gene-expression/software/release-notes/build>`_. Since soupX requires clustering, we use basic Seurat functionality to quickly normalize and cluster the expression data; detailed explanations of Seurat workflow will be given later. Corrected data matrix is written and is used in all further processing. 

Scrublet
--------

`Scrublet Notebook repository <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-doublets-scrublet.ipynb>`_

`Scrublet Notebook HTML <https://cellgeni.github.io/notebooks/html/new-doublets-scrublet.html>`_

This notebook describes the usage of scrublet Python package for doublet detection. Scrublet performed very well in a `recent benchmark <https://pubmed.ncbi.nlm.nih.gov/33338399/>`_, and is also very intuitive and computationally efficient. The results (doublet scores and binary “singlet/doublet” assignments) are saved as a text file and will be used in downstream processing with Seurat or Scanpy. 

Seurat
------

`Seurat Notebook repository <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Seurat.Rmd>`_

`Seurat Notebook HTML <https://cellgeni.github.io/notebooks/html/new-10kPBMC-Seurat.html>`_

This is a basic `Seurat <https://satijalab.org/seurat/>`_ workflow R notebook that describes all key steps of scRNA-seq processing, using 10k PBMC dual-indexed dataset from 10X Genomics. Ambient RNA removal (soupX) and doublet detection (scrublet) should be ran before starting this workflow. After this, the following steps are performed:

* Creation of Seurat object, and some basic exploration of its properties; 
* Estimation of mitochondrial and ribosomal protein percentage among all reads; 
* Calculation of cell cycle scores for each cell; 
* Log-normalization, highly variable genes selection, dimensionality reduction, clustering, and general exploration of the dataset; 
* Quality control and careful removal of low quality cells; 
* Normalization, scaling, and highly variable genes selection via SCTransform; 
* Clustering with parameter tuning allowing identification of smaller clusters; 
* Marker gene identification for each cluster; 
* Automated cell type annotation using singleR. 


Scanpy
------

`Scanpy Notebook repository <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Scanpy.ipynb>`_

`Scanpy Notebook HTML <https://cellgeni.github.io/notebooks/html/new-10kPBMC-Scanpy.html>`_

This is a basic `Scanpy <https://scanpy.readthedocs.io/en/stable/>`_ workflow Python notebook that describes all key steps of scRNA-seq processing, using 10k PBMC dual-indexed dataset from 10X Genomics. Ambient RNA removal (soupX) and doublet detection (scrublet) should be ran before starting this workflow. After this, the following steps are performed:

* Creation of Scanpy object, and some basic exploration of its properties; 
* Estimation of mitochondrial and ribosomal protein percentage among all reads; 
* Calculation of cell cycle scores for each cell; 
* Log-normalization, highly variable genes selection, dimensionality reduction, clustering, and general exploration of the dataset; 
* Quality control and careful removal of low quality cells; 
* Normalization, scaling, and highly variable genes selection in the filtered dataset; 
* Clustering with parameter tuning allowing identification of smaller clusters; 
* Marker gene identification for each cluster; 
* Automated cell type annotation using CellO. 


Integration in R
----------------

`R Integration Notebook repository <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Integration.Rmd>`_

`R Integration Notebook HTML <https://cellgeni.github.io/notebooks/html/new-10kPBMC-Integration.html>`_

This notebook shows how batch correction (for one dataset) or integration (for multiple datasets) can be performed using tools available in R. Used packages include: Harmony, batchelor, Seurat, ComBat, and Limma. 

Integration in Python
---------------------

There is no notebook available for Python integration. 

Please contact cellgeni@sanger.ac.uk if you need help with Python integration.

Monocle3
--------

`Monocle3 Notebook repository <https://github.com/cellgeni/notebooks/blob/master/notebooks/monocle3-example.Rmd>`_

`Monocle3 Notebook HTML <https://cellgeni.github.io/notebooks/html/monocle3-example.html>`_

This notebook gives a basic example of scRNAseq processing using Monocle3. 
