Rclone
======

Rclone is a software that is very useful for downloading data from remote storage locations such as Google Drive and DropBox. This page will cover 
installation, configuration and some miscellaneous information that could be of use. For a full list of storage providers rclone works with please visit their
`website <https://rclone.org>`_.

Installation
------------

Services like our JupyterHub and the FARM already have rclone installed, however if you are working on your local machine or want the latest version, here is an
install guide. To install the latest version of rclone use the followng command:

  .. code-block:: bash

    cd ~
    wget https://downloads.rclone.org/rclone-current-linux-amd64.zip

That will download a zip file of the latest rclone release. We now need to unzip it.

  .. code-block:: bash
  
    unzip rclone-current-linux-amd64.zip
    ls
    
When you ``ls`` you will see a directory that looks like ``rclone-v{RCLONEVERSION}-linux-amd64``, inside this file contains the rclone software.

Configuration
-------------

This is a guide through the configuration process of rclone. We will be using Google Drive as the example remote storage we want to access.

#. Open a new Terminal and type ``~/rclone-v{RCLONEVERSION}-linux-amd64/rclone`` config, it will show you a list of options
#. When prompted with ``e/n/d/r/c/s/q>``, write ``n``
#. Next it will ask you for the name of storage remote you want to set up, next to ``name>``, write gdrive
#. After that it willl want to know the type of storage remote you want to configure, next to ``Storage>``, write ``drive`` for Google Drive (the types of storage available are listed for you)
#. Leave the ``client_id>`` as default by pressing enter
#. Also leave ``client_secret>`` as default by pressing enter
#. Rclone will want to know the level of access it is allowed, next to ``scope>``, write ``1`` which will give rclone full access to all files, excluding Application Data Folder
#. Once again, leave ``root_folder_id>`` as default by pressing enter
#. Again leave``service_account_file>`` as default by pressing enter
#. Rclone will want to know if you want to ``Edit advanced config?``, write ``n`` for no
#. Rclone will also want to know if it should ``Use auto config?``, write ``n`` again
#. If your browser doesn't open automatically follow the link it shows you. Log in and authorize rclone for access. Copy the verification code you get and paste it after ``Enter verification code>``
#. Rclone will want to know if this is a tem drive, via ``Configure this as a team drive?``, write ``n``
#. It will show you the configuration, write ``y`` to confirm this is OK
#. You will see the same menu from the first step, write ``q`` to finish (or ``n`` if you need to set up another remove).

Miscellaneous
-------------

When setting up certain remote storages, such as box or onedrive, a verification method will be needed that requires going to a URL displayed on the command line.
The message will look something like:

  ..code-block :: bash
  
  If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w
  Log in and authorize rclone for access
  Waiting for code...
  
The URL needs to have the `http://127.0.0.1:` part replaced depending on where you are running the command from.

*. If on the FARM and on a head node (such as ``head1``), enter the following into your web browser:

``http://farm5-head1.internal.sanger.ac.uk:53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w``

*. If on the FARM and on a computer node (such as ``node-12-8-4``), enter the following into your web browser:

``http://node-12-8-4.internal.sanger.ac.uk:53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w``

*. If on JupyterHub, enter the following into your web browser:

``https://jhub.cellgeni.sanger.ac.uk/user/<USERNAME>/proxy/53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w``

.. note::
    **Please note GitHub makes all usernames lowercase for the purposes of this URL**.
