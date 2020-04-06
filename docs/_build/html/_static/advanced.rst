.. _advanced:


================================
Advanced usage and configuration
================================



********************************
Further configuration
********************************


config.php
^^^^^^^^^^

Once you started Barcode Buddy for the first time, you will find the file ``config.php`` in the folder ``data``. This file is for further configuration - the following values can be changed:


+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| Argument                     | Value           | Effect                                                                                                                                              | Default                |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| PORT_WEBSOCKET_SERVER        | 1024-65535      | The port that the websocket server listens to. Change if you running multiple instances or the default port is already used by another application. | 47631                  |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| DATABASE_PATH                | A writable path | The path were the database file is written to. Make sure that the webserver does not allow the download of the file. Path includes filename.        | ./data/barcodebuddy.db |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| CURL_TIMEOUT_S               | 5-60            | How long to wait for a request                                                                                                                      | 20                     |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| CURL_ALLOW_INSECURE_SSL_CA   | true/false      | Accept self-signed SSL certificates                                                                                                                 | false                  |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| CURL_ALLOW_INSECURE_SSL_HOST | true/false      | Accept SSL certificates where the host does not match                                                                                               | false                  |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| REQUIRE_API_KEY              | true/false      | Require API key for authentication when using API                                                                                                   | true                   |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| DISABLE_AUTHENTICATION       | true/false      | Disables user management, if enabled, Barcode Buddy will not ask for local username/password                                                        | false                  |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| IS_DEBUG                     | true/false      | Enable verbose error messages. If you think something is wrong, you can enable this and send a bugreport                                            | false                  |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| OVERRIDDEN_USER_CONFIG       | mapped array    | To override settings that can be set in the UI, uncomment the specific line. Overridden values cannot be changed in the UI afterwards               | empty                  |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+

*Note:* ``config.php`` *is a copy of* ``config-dist.php`` *in the root directory, which is tracked by version control, but not included by php. Any changes in* ``config-dist.php`` *will be ignored by Barcode Buddy*


configProcessing.inc.php
^^^^^^^^^^^^^^^^^^^^^^^^^

If you need to change the paths of the ``config.php`` or the user database location, you can do this in the ``configProcessing.inc.php`` file, found in the ``incl`` folder. As editing the file might break updating Barcode Buddy, it is rather recommended to use :ref:`envvar` instead of editing this file. The following values can be changed:

+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
| Argument    | Value           | Effect                                                                                                                                            | Default           |
+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
| CONFIG_PATH | A writable path | The path were the config.php file is written to. Path includes filename.                                                                          | ./data/config.php |
+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
| AUTHDB_PATH | A writable path | The path were the user database file is written to. Make sure that the webserver does not allow the download of the file. Path includes filename. | ./data/users.db   |
+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+


.. _envvar:

********************************
Environment variables
********************************

Environment variables can be passed to Barcode Buddy - that way you can configure it without editing any files.

Using environment variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

TODO



********************************
API
********************************

TODO

*******
Plugins
*******

Barcode Buddy offers plugin support. All PHP scripts in the folder "plugins" are automatically loaded. See also the `example script <https://github.com/Forceu/barcodebuddy/blob/master/plugins/EventReceiver.php>`_
.
