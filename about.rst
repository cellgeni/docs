The group
=========

.. image:: img/vladimirgroupphoto.png
   :width: 700

Our story
---------

Cellular Genetics programme (2015)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `Cellular Genetics programme <https://www.sanger.ac.uk/science/programmes/cellular-genetics>`_ at the Sanger Institute was created in 2015 by merging with the Computational Genomics programme. In the first two years the programme didn't have any core facility groups. In 2017 it was decided to create several core facility groups including ours to help the faculty groups do their research.

Early days
~~~~~~~~~~

In the early days of the programme (the first 2 years) all of the users analysed their data by themselves on the `LSF batch compute farm <https://www.ibm.com/support/knowledgecenter/en/SSETD4/product_welcome_platform_lsf.html>`_ available at the Sanger. This included both computational and non-computational users. Some people had never coded before. This introduced a lot of heterogeneity in the programme data.

Cloud (2017)
~~~~~~~~~~~~

In 2017 it was decided by Wellcome Trust to adapt to the modern cloud technologies and to move their compute to the cloud. The Sanger Institute decided to start with having a private OpenStack cloud on premises and gradually move analysis from the LSF farm to the cloud. It created additional challenges for the end users and therefore it was decided to create the Cellular Genetics core facility informatics group which would standardize the analysis and help with transitioning to the cloud.

Cellular Genetics Informatics group (2018)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`Our group <https://www.sanger.ac.uk/science/groups/cellular-genetics-informatics>`_ was created in the beginning of 2018 and `Vladimir Kiselev <https://www.sanger.ac.uk/people/directory/vladimir-yu-kiselev>`_ started as the head of the group. `Stijn van Dongen <https://www.sanger.ac.uk/people/directory/van-dongen-stijn>`_ joined the group in May 2018 and `Anton Khodak <https://www.sanger.ac.uk/people/directory/khodak-anton>`_ joined the group in June 2018.

Our remit
---------

Group
~~~~~
Our team provides efficient access to cutting-edge analysis methods for `Cellular Genetics programme <https://www.sanger.ac.uk/science/programmes/cellular-genetics>`_ at the Sanger Institute, which leads a number of world-class research initiatives including the Human Induced Pluripotent Stem Cell Initiative (`HIPSCI <http://www.hipsci.org/>`_) and `Human Cell Atlas <https://www.humancellatlas.org/>`_. Our team’s focus is the development and operation of workflows, tools and infrastructure for data analysis that support the programme’s research goals.

Compute framework
~~~~~~~~~~~~~~~~~

To run our pipelines we use both the `LSF compute cluster <https://www.ibm.com/support/knowledgecenter/en/SSETD4/product_welcome_platform_lsf.html>`_ and `OpenStack private cloud <https://www.openstack.org/>`_ solutions. We extensively use `Kubernetes <https://kubernetes.io/>`_ to orchestrate pipelines on the cloud. To run the pipelines we use both `Nextflow <https://www.nextflow.io/>`_ and `CWL <https://www.commonwl.org/>`_.

`Our GitHub organisation <https://github.com/cellgeni>`_

.. image:: img/cellgeni-compute.png
   :width: 700

Our vision
----------

When working on our projects and pipelines we follow several important principles.

No duplication of effort
~~~~~~~~~~~~~~~~~~~~~~~~

If something has been or can be done by others we will not develop our own tool for this.

Agile
~~~~~

In our work we try use elements of the `Agile methodology <https://en.wikipedia.org/wiki/Agile_software_development>`_. We use `Jira <https://www.atlassian.com/software/jira>`_ as our main project management software.

User-oriented
~~~~~~~~~~~~~

We value our users not only as our customers (we use `Jira Service Desk <https://www.atlassian.com/software/jira/service-desk>`_ for all of the users' requests) but also as the most important source of ideas and feedback. Anyone can either email us on vk6@sanger.ac.uk or talk to us in `our Mattermost group <https://mattermost.sanger.ac.uk/cellgeninf>`_ (Sanger users can login using their Sanger credentials).

Feedback
~~~~~~~~

We also have monthly meetings with faculty group's representatives and weekly coffee standups where anyone can come, comment on our work, feedback or ask us any question.


