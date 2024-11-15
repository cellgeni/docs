Farmer
======

We have a Slack bot called `Farmer <https://github.com/cellgeni/farmer>`_ that can tell you when your farm jobs are done, so you don't have to keep checking ``bjobs``.
You can also ask it for a list of jobs that are currently running, whether they belong to you or to someone else.

Get notified when your jobs finish
----------------------------------

The easiest way to use Farmer is if you use ``bsub`` directly: pass the argument ``-Ep /software/cellgen/cellgeni/etc/notify-slack.sh`` when submitting your job.
Alternatively, you can include a ``#BSUB`` comment in your job submission script::

  #BSUB -Ep /software/cellgen/cellgeni/etc/notify-slack.sh

.. note::

  If you submit a job array, Farmer will only notify you once every job in the array has finished.
  This kind of batching is not supported for jobs outside arrays.

If you don't use ``bsub`` to submit your jobs, then you need to somehow ensure that the ``notify-slack.sh`` script is executed at the end of your job: for example, at the end of your script, include the line ``/software/cellgen/cellgeni/etc/notify-slack.sh``.
You can specify a label which Farmer will include in the notification, to help you remember which job it's talking about (since it won't be able to tell you the job ID or command you ran)::

  /software/cellgen/cellgeni/etc/notify-slack.sh --label="my long-running job"

Be aware that Farmer won't be able to give you any details about your job (e.g. whether it succeeded or failed, or its CPU/memory efficiency) if you use it this way.

Jobs running as a different user
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you want to get notified about a job running under a different user (for example, a service user that doesn't have a Slack account), you have to tell Farmer who to notify.
Do this by including a username as an argument to the ``notify-slack.sh`` script -- for example, to notify ``zz0`` about a job, you would write something like::

  #BSUB -Ep "/software/cellgen/cellgeni/etc/notify-slack.sh zz0"

You could alternatively write ``notify-slack.sh --user=zz0`` to produce the same effect.

Farmer will also look at the ``$FARMER_SLACK_USER`` environment variable, so if for some reason your Unix username doesn't match your Slack username, you could add something like this to your ``~/.bashrc``, so you don't have to pass your username every time::

  export FARMER_SLACK_USER=zz0

Inspecting running jobs
-----------------------

If you message Farmer the word "jobs", it will print a list of all the unfinished farm jobs owned by you.
You can click the "View Details" button next to any job to see all the data LSF has about that job.

To see jobs owned by another user, use a command like "jobs for cellgeni-su".

Limitations
-----------

* Farmer only has visibility into jobs running on **farm22**.
  Other farms could be supported in the future -- please let us know if you'd find this helpful.
* Requests are heavily rate-limited to avoid causing excessive load on LSF, so you may find that Farmer is sometimes slow to respond.
  It should let you know roughly how many requests are currently being dealt with if this is the case.
  If Farmer is completely unresponsive, we are available in Slack or by email: see :doc:`contact`.