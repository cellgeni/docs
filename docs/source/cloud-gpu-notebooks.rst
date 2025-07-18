GPU Notebooks on the Cloud
==========================

.. note::
    **Only members of the Cellular Genomics Programme and their collaborators are eligible for access**.

Cloud GPU Notebooks is a new service provided by CellGen IT team. The service utilizes Cloud resources (Google Compute Platform) and is paid for by the `Cellular Genomics Programme <https://www.sanger.ac.uk/programme/cellular-genetics/>`_. 

Bespoke notebooks can have T4 GPU accelerators with 16.GB GPU RAM, 16 CPU, and between 100.GB and 200.GB of RAM.

See `official list <https://cloud.google.com/compute/docs/gpus#gpus-list>`_ for specs on what types of custom machines can be requested.


Request a notebook
------------------

Please fill in `this form <https://forms.gle/NLdvCHnzjgZXcXPD7>`_ and we will action on your request and be in touch with you the same or the next day.

.. note::
    Alternatively, you can try the free version of `Google Colab <https://colab.research.google.com/>`_ has only the K80 GPU type, up to 12GB RAM and 12 hours of execution time by yourself wihtout contacting  CellGenIT. 


Accessing your instance
-----------------------

After your request has been processed, you'll receive a custom link to access your notebook.
You need to make sure that your ``user@sanger.ac.uk`` Sanger Google Account is used to login, if you are using another Google Account you'll get a ``403 Error``.
We suggest using Incognito/Private mode in your web browser to access the link so it does not get mixed with your Personal Google Account.


Installed software
------------------

The cloud notebook probably won't have the same setup as you've got on your Jupyter. Some packages are installed by default (Python and CUDA).
The best thing is to have a list of all your required packages so it's easier to install them all in a new compute instance. 
Should you require more packages, you can install them yourself. It's a good idea to add install steps or scripts to your notebooks. 

- If you choose to install packages using a script, remember to activate the conda environment before running the script or inside the script itself.

.. code-block:: bash

    conda activate myenv
    conda install YYY

- If you choose to install using a notebook, remember to use cell magic like this can help you install packages in from your notebooks:

.. code-block:: bash

    %pip install XXX 
    %conda install YYY

Alternatively, create environments from dependency files like the ones generated by

.. code-block:: bash

    conda env export > environment.yml

and then import them on the cloud env

.. code-block:: bash

    conda env create -f environment.yml

.. tip::

    GPU Cloud Notebooks have CUDA 11 installed, use the following commnad to install pytoch and guarantee it uses GPU:
    
    .. code-block:: python

        pip install torch==1.10.2+cu113 torchvision==0.11.3+cu113 torchaudio==0.10.2+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html


Getting data to the cloud
-------------------------

Notebooks on the cloud can't access the farm so you need to pivot the data to get it to the cloud. You will need a Teleport accout to be able to copy things to/from the farm.

To install teleport on your could instance simply run this:

.. code-block:: bash

    curl "https://portal.sanger.ac.uk/scripts/install.sh" | sudo bash


To copy data you'll need Teleport. You can login to Teleport from your cloud instance using two methods: 

    a) Login from your local laptop and then copy the identity file to the cloud
    b) Replace the IP in the URLs for your notebook proxy URL

After login you can use tsh to copy data from the farm to your cloud compute environment. Keep in mind that you can copy files to/from the farm from the GCP environment, but you can't access the GCP environment from the farm directly.


.. warning::

    Only copy count data or fully anonymized data. Do not copy raw reads or patient data. When in doubt check with the CellGen Research Admin team.


Generate local cert
^^^^^^^^^^^^^^^^^^^

Login **from your laptop, generate an temporary identity and copy the certificate manually to your cloud env**. 
to do that, install Teleport in your laptop using "Software Centre" (Windows) or "Self Service" (macOS). Then from a terminal run:

.. code-block:: bash

    tsh login --proxy=portal.sanger.ac.uk --auth=okta --out teleport.pem
 
After the cert file is created you can now copy ``teleport.pem`` to your cloud instance just by drag and drop it using the Jupyter UI.
 
Now, once the certificate is there, you can run Teleport commands (ssh, scp, etc.) specifying the path to the file with this command and proxy:

.. code-block:: bash

    tsh --identity=teleport.pem --proxy=portal.sanger.ac.uk scp YOURSANGERUSERNAME@farm22-head1:/path/in/farm/file.txt /home/jupyter/
 
 
Use the jupyter proxy
^^^^^^^^^^^^^^^^^^^^^
 
You can also **use the jupyter proxy and manually rewrite the URLs for Teleport login** to work on the cloud server. Directly from your cloud instance, run the Teleport login command:

.. code-block:: bash

    tsh login --proxy=portal.sanger.ac.uk --auth=okta --browser=none
 
You'll see something like this as an output:

.. code-block:: bash

    Use the following URL to authenticate:
    http://127.0.0.1:39763/3859d74e-6ff1-4739-a469-44ba16c8c386

 
Copy the URL but you need to change ``127.0.0.1:`` part for the server porxy URL of your current notebook. That is something like this:
https://blablabla-europe.notebooks.googleusercontent.com/proxy/39763/3859d74e-6ff1-4739-a469-44ba16c8c386
 
That is going to work, and probably ask you for your Okta login. But then it will then prompt you back to another error page. You'll see the URL changing to something like:
http://127.0.0.1:39763/callback?response=%7B%22ciphertext%22%3A%reallylongtexthere
 
Once again, the process needs to be repeated. Change ``127.0.0.1:`` to the server proxy URL ending up with something like this:
https://blablabla-europe.notebooks.googleusercontent.com/proxy/39763/callback?response=%7B%22ciphertext%22%3A%reallylongtexthere
 
If that all went well you'll see the “"Login Successful" message. You can now run commands to copy files like this:

.. code-block:: bash

    tsh scp YOURSANGERUSERNAME@farm22-head1:/path/in/farm/file.txt /home/jupyter/
 

Shutting down your instance
---------------------------

It's important not to keep your notebook idle. If you're not actively using it or there's no process running it will automatically shut itself off after 180min. But if you're done for the day and you're not going to be using it anymore, please shut it down using the "STOP" button in the dashboard https://gcp-notebooks.cellgeni.sanger.ac.uk/ *(requires VPN)*
