Notebooks
=========

We have a set of premade notebooks available to users on JupyterHub. These notebooks are designed to guide users through various stages of downstream analysis
and to aid them is inspecting their own data. We have five notebooks available each of which we believe to cover an important aspect of downstream analysis.

Seurat
------

`Seurat Notebook <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-Seurat.Rmd>`_

Seurat is an R package designed for the analysis of single-cell RNA-seq data. For more information visit their `homepage <https://satijalab.org/seurat/>`_.
Our Seurat notebook covers the following topics:

* Insert
* Topics
* Here

Scanpy
------

`Insert-Link-To-Notebook <https://github.com/cellgeni/notebooks>`_

Scanpy is an Python package designed for the analysis of single-cell RNA-seq data. For more information visit their `homepage <https://scanpy.readthedocs.io/en/stable/>`_.
Our Scanpy notebook covers the following topics:

* Insert
* Topics
* Here

Object Exploration
------------------

`Insert-Link-To-Notebook <https://github.com/cellgeni/notebooks>`_

An important part of downstream analysis is understanding what the data represents and how it is structured. In this notebook we explore and explain the 
various components in various objects. The object types we explore are:

* Insert
* Topics
* Here

Integration in R
----------------

`Insert-Link-To-Notebook <https://github.com/cellgeni/notebooks>`_

One of the most significant areas of downstream analysis is integrating data together. This involves a variety of steps needed to prepare the data and different
types of integration. The types of integration covered in this notebook are:

* Insert
* Topics
* Here

Integration in Python
---------------------

`Insert-Link-To-Notebook <https://github.com/cellgeni/notebooks>`_

One of the most significant areas of downstream analysis is integrating data together. This involves a variety of steps needed to prepare the data and different
types of integration. The types of integration covered in this notebook are:

* Insert
* Topics
* Here

SoupX
-----

`Soupx Notebook <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-10kPBMC-SoupX.Rmd>`_

SoupX is an R package that attempts to remove mRNA that is contaminating results in drople based single-cell RNA-seq data. For more information visit their `GitHub Repository <https://github.com/constantAmateur/SoupX>`_. The topics covered in this notebook are:

* Insert
* Topics
* Here

scrublet
--------

`scrublet Notebook <https://github.com/cellgeni/notebooks/blob/master/notebooks/new-doublets-scrublet.ipynb>`_

scrublet is a Python package that attempts to identify doublets within samples in scRNA-seq data. For more information visit their `GitHub Repository <https://github.com/swolock/scrublet>`_. The topics covered in this notebook are:

* Insert
* Topics
* Here

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
                d -> e [xlabel=" R-based integration methods "];
                c -> f [label=" scanpy (Python) full basic workflow "];
                f -> g [label=" Python-based integration methods "];
        }

