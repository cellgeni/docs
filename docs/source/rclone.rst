Rclone
======

Rclone is a software that is very useful for downloading data from remote storage locations such as Google Drive and DropBox. This page will cover 
installation, configuration and some miscellaneous information that could be of use. For a full list of storage providers rclone works with please visit their
`website <https://rclone.org>`_.

Installation
------------

Services like our JupyterHub and the FARM already have rclone installed. To check if you have rclone installed you can do the command ``which rclone`` and to check the version you can do ``rclone --version``. If you are don't have rclone installed or you want the latest version, here is an install guide. To install the latest version of rclone use the followng command:

  .. code-block:: bash

    cd ~
    wget https://downloads.rclone.org/rclone-current-linux-amd64.zip

That will download a zip file of the latest rclone release. We now need to unzip it.

  .. code-block:: bash
  
    unzip -j rclone-current-linux-amd64.zip "rclone-*-linux-amd64/rclone"
    ls
    
When you ``ls`` you will see the package ``rclone``.

Configuration
-------------

This is a guide through the configuration process of rclone. We will be using Google Drive as the example remote storage we want to access.

#. Open a new Terminal and type ``~/rclone config``, it will show you a list of options
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


Usage Examples
--------------

For all the examples we will be using Google Drive.

**Copy** 

The `copy command<https://rclone.org/commands/rclone_copy/>`_ copies files from a source source to a destination. This process doesn't transfer unchanged files, testing by size and modification time or MD5SUM and it doesn't delete files from the destination. The basic layout is as followed:

  .. code-block:: bash
  
    rclone copy <source> <destination>

* To copy a local directory called "data" to a Google Drive directory called "backup"

``rclone copy /home/local/data gdrive:backup``

* Copy a local directory called "data" to a Google Drive directory that someone shared with you named "collaboration", it is under the "Shared with me" section of your google drive page.

``rclone copy /home/local/data  gdrive:collaboration --drive-shared-with-me``

* Copy a Google Drive directory called "latest" to a local directory called "data"

``rclone copy gdrive:latest  /home/local/data``

* Copy a Google Drive directory that someone shared with you named "collaboration" to a local directory called "data". The drive directory is under the "Shared with me" section of your google drive page.

``rclone copy gdrive:collaboration /home/local/data --drive-shared-with-me``

.. note::
  **Track progress.** Add the ``--progress`` option at the end of any command to view real time statistics of the transfer.

**Mount**

Mounting allows you to access your remote file system from your local filesystem. The official mount documentation can be found on their `website <https://rclone.org/commands/rclone_mount/>`_. 

#. Firstly, you want to create a directory to be mounted ``mkdir -p ~/mount/gdrive/``
#. Next, you want to mount the remote storage file system to this path ``rclone mount gdrive:/ ~/mount/gdrive/ --daemon --vfs-cache-mode full``
#. Check is works by doing ``ls ~/mount/gdrive/`` and you should see your remote storage files linked.

.. note::
    **Mount can be slow.** Mounting does a lot of copying back a forth, if you are going to edit large files this may end up being slow. To solve this it's better to copy the files first and work on them locally.
    
* To unmount your remote storage, do ``fusermount -u ~/mount/gdrive/``

**ls**

The ``ls`` command allows you to list a remote file system and see the structure within it, the website link is `this <https://rclone.org/commands/rclone_ls/>`_. TheThe standard command looks like this:

  .. code-block:: bash
  
    rclone ls remote:path
 
* ``ls`` lists the size and path of objects only
* ``lsl`` lists the modification time, size and path of objects only
* ``lsd`` lists the directories only
* ``lsf`` lists objects and directories in easy to parse format

Miscellaneous
-------------

When setting up certain remote storages, such as box or onedrive, a verification method will be needed that requires going to a URL displayed on the command line.
The message will look something like:

  .. code-block :: bash
  
    If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w
    Log in and authorize rclone for access
    Waiting for code...
  
The URL needs to have the `http://127.0.0.1:` part replaced depending on where you are running the command from.

* If on the FARM and on a head node (such as ``head1``), enter the following into your web browser:

``http://farm5-head1.internal.sanger.ac.uk:53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w``

* If on the FARM and on a computer node (such as ``node-12-8-4``), enter the following into your web browser:

``http://node-12-8-4.internal.sanger.ac.uk:53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w``

* If on JupyterHub, enter the following into your web browser:

``https://jhub.cellgeni.sanger.ac.uk/user/<USERNAME>/proxy/53682/auth?state=V_bmyC_dSCuuBc6uYbFE7w``

.. note::
    **Please note GitHub makes all usernames lowercase for the purposes of this URL**.
    
Once that has occurred there will be a sign in page. Once you sign in you will be redirected again and shown an error message. That is ok, take the URL from the webpage, which will look something like this:

  .. code-block :: bash
    
    http://127.0.0.1:53682/?code=M.R3_BAY.6cbffffd-7232-af3d-4b73-fa56f97e32be
    &state=V_bmyC_dSCuuBc6uYbFE7w
    
and again replace the ``http://127.0.0.1`` with the correct option from the above list i.e. if you were using JupyterHub the final URL would be: 

  .. code-block :: bash
  
    https://jhub.cellgeni.sanger.ac.uk/user/<USERNAME>/proxy/53682/
    ?code=M.R3_BAY.6cbffffd-7232-af3d-4b73-fa56f97e32be&state=V_bmyC_dSCuuBc6uYbFE7w

You can then return to the terminal.
