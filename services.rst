Services we provide
===================

Ticketing system
----------------

We operate using JIRA ticketing system. To be able to put in tickets you need to contact us by either emailing to vk6@sanger.ac.uk or using `our Mattermost channel <https://mattermost.sanger.ac.uk/cellgeninf/channels/jira-requests>`_ (please use your Sanger credentials to login).

Once registered you can put in a ticket using either `our portal <https://cellgeni.atlassian.net/servicedesk/customer/portal/1>`_ or by emailing to support@cellgeni.atlassian.net.

Run a pipeline
--------------

We offer a range of sequencing processing pipelines. We develop them in two workflow languages: `Nextflow <https://www.nextflow.io/>`_ (very user-friendly) and `CWL <https://www.commonwl.org/>`_ (universal standard).

For the end-users we offer Nextflow pipelines available to run on both Sanger *LSF* farm or *OpenStack* flexible compute environment. The pipelines can be run either by us or by the end-users themselves.

Here is the list of pipelines we develop:

1. `RNA-seq <https://github.com/cellgeni/rnaseq-noqc>`_ (both bulk and single cell)
2. `Chip-seq <https://github.com/cellgeni/chipseq>`_
3. `ATAC-seq <https://github.com/cellgeni/atacseq>`_

.. note:: If you would like to run ``cellranger`` by *10X* please put in a ticket for the NPG group by emailing to new-seq-pipe@sanger.ac.uk and specify which version of the cellranger and the genome you would like to use.

.. important:: Our pipelines start from aligned cram files and mostly concentrate on the downstream analysis (starting from the count matrix). Therefore, when submitting your experiment to sequencing please make sure you specify the correct aligner for your data.

Share data with external collaborators
--------------------------------------

We use `Globus <https://www.globus.org/>`_ network to share the data with external collaborators. It allows us to share data from e.g. a specific folder on the Sanger LFS cluster directly with the external world.

The sharing process consists of the following steps:

1. We share the data with the user's personal/work email address
2. The user `creates/logs in their Globus account <https://docs.globus.org/how-to/get-started/>`_ using the sharing email
3. The user needs to create a personal Globus endpoint either on their `Linux laptop / compute cluster <https://docs.globus.org/how-to/globus-connect-personal-linux/>`_ or on their `Mac laptop <https://docs.globus.org/how-to/globus-connect-personal-mac/>`_ or on their `Windows laptop <https://docs.globus.org/how-to/globus-connect-personal-windows/>`_.
4. The user activates their personal Globus endpoint by starting globus from the command line if on a cluster/Linux, or by starting the globus application if on Mac/Windows.
5. Once the users personal endpoint is setup they can transfer the data by simply `logging in to their Globus account using the sharing email address and drag and dropping the data <https://docs.globus.org/how-to/get-started/>`_.

For more information please visit the `Globus official documentation <https://docs.globus.org/how-to/>`_.