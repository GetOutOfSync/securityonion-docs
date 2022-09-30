.. _strelka:

Strelka
=======

From https://github.com/target/strelka:

    Strelka is a real-time file scanning system used for threat hunting, threat detection, and incident response. Based on the design established by Lockheed Martin's Laika BOSS and similar projects (see: related projects), Strelka's purpose is to perform file extraction and metadata collection at huge scale.

Depending on what options you choose in Setup, it may ask if you want to use :ref:`zeek` or :ref:`suricata` for metadata. Whichever engine you choose for metadata will then extract files from network traffic. Strelka then analyzes those files and they end up in ``/nsm/strelka/processed/``.

Alerts
------

Strelka scans files using YARA rules. If it detects a match, then it will generate an alert that can be found in :ref:`alerts`, :ref:`dashboards`, :ref:`hunt`, or :ref:`kibana`. Here is an example of Strelka detecting Poison Ivy RAT:

.. image:: images/strelka-alert-1.png
  :target: _images/strelka-alert-1.png

Drilling into that alert, we find more information about the file and the YARA rule:

.. image:: images/strelka-alert-2.png
  :target: _images/strelka-alert-2.png

.. image:: images/strelka-alert-3.png
  :target: _images/strelka-alert-3.png

.. image:: images/strelka-alert-4.png
  :target: _images/strelka-alert-4.png

You can read more about YARA rules in the :ref:`local-rules` section.

Logs
----

Even if Strelka doesn't detect a YARA match, it will still log metadata about the file. You can find Strelka logs in :ref:`dashboards`, :ref:`hunt`, and :ref:`kibana`. Here's an example of the default Strelka dashboard in :ref:`dashboards`:

.. image:: images/strelka.png
  :target: _images/strelka.png

Rules
-----

Strelka comes with a variety of YARA rules pre-built. To add custom YARA rules, copy the YARA rule file into ``/nsm/repo/rules/strelka``. In the event you have multiple or many related files, you can copy the entire directory into the ``rules`` directory. Example: ``/nsm/repo/rules/strelka/custom_rules``. To disable rules, add the filename of the yara rules to the ``/opt/so/saltstack/default/salt/strelka/defaults.yaml`` file.

Configuration
-------------

Strelka reads its configuration from ``/opt/so/conf/strelka/``. However, please keep in mind that if you make any changes to this directory they may be overwritten since the configuration is managed with :ref:`salt`. The base ``global.sls`` name is ``strelka``, and below is a table of salt options available.

+-----------+-----------------------------------------+----------------------------------------------------+
| Options   | Default Value                           | Notes                                              |
+===========+=========================================+====================================================+
| enabled   | 1                                       | Enables Strelka. 1 to enable, 0 to disable.        |
+-----------+-----------------------------------------+----------------------------------------------------+
| rules     | 1                                       | Enables Strelka Rules. 1 to enable, 0 to disable.  |
+-----------+-----------------------------------------+----------------------------------------------------+
| ignore    | strelka_config.strelka.ignore           | File containing a list of rules to ignore.         |
+-----------+-----------------------------------------+----------------------------------------------------+
| imagerepo | 'https://<manager>/repo/rules/strelka'  | Where to pull yara rules from.                     |
+-----------+-----------------------------------------+----------------------------------------------------+

Diagnostic Logging
------------------

Strelka diagnostic logs are in ``/nsm/strelka/log/``. Depending on what you’re looking for, you may also need to look at the :ref:`docker` logs for the containers:

::

        sudo docker logs so-strelka-backend
        sudo docker logs so-strelka-coordinator
        sudo docker logs so-strelka-filestream
        sudo docker logs so-strelka-frontend
        sudo docker logs so-strelka-manager

More Information
----------------

.. seealso::

    For more information about Strelka, please see https://github.com/target/strelka.
