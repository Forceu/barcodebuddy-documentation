.. _advanced:

================================
Advanced usage and configuration
================================

********************************
Further configuration
********************************

.. _configphp:

config.php
==========


Once you started Barcode Buddy for the first time, you will find the file ``config.php`` in the folder ``data``. This file is for further configuration - the following values can be changed:


+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| Argument                     | Value           | Effect                                                                                                                                                                     | Default                |
+==============================+=================+============================================================================================================================================================================+========================+
| PORT_WEBSOCKET_SERVER        | 1024-65535      | The port that the websocket server listens to. Change if you running multiple instances or the default port is already used by another application.                        | 47631                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| DATABASE_PATH                | A writable path | The path were the database file is written to. Make sure that the webserver does not allow the download of the file. Path includes filename.                               | ./data/barcodebuddy.db |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| CURL_TIMEOUT_S               | 5-60            | How long to wait for a request                                                                                                                                             | 20                     |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| CURL_ALLOW_INSECURE_SSL_CA   | true/false      | Accept self-signed SSL certificates                                                                                                                                        | false                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| CURL_ALLOW_INSECURE_SSL_HOST | true/false      | Accept SSL certificates where the host does not match                                                                                                                      | false                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| REQUIRE_API_KEY              | true/false      | Require API key for authentication when using API                                                                                                                          | true                   |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| DISABLE_AUTHENTICATION       | true/false      | Disables user management, if enabled, Barcode Buddy will not ask for local username/password                                                                               | false                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| IS_DEBUG                     | true/false      | Enable verbose error messages. If you think something is wrong, you can enable this and send a bugreport                                                                   | false                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| HIDE_LINK_GROCY              | true/false      | Set to true to remove the link to Grocy in the top header                                                                                                                  | false                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| HIDE_LINK_SCREEN             | true/false      | Set to true to remove the link to the Screen module in the top header                                                                                                      | false                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| EXTERNAL_GROCY_URL           | null or URL     | If you are using an internal URL for API communication to Grocy, but require a different URL for external access, you can enter the URL here. (Replaces URL in top header) | null                   |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| AUTHENTICATION_BYPASS_NETS   | mapped array    | List of IPs and subnets that can bypass authentication. If using with a reverse proxy, ensure TRUSTED_PROXIES is set correctly | empty                   |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| TRUSTED_PROXIES              | mapped array     | List of IPs and subnets that will be trusted for X-Forwarded-For header information | empty                   |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
| OVERRIDDEN_USER_CONFIG       | mapped array    | To override settings that can be set in the UI, uncomment the specific line. Overridden values cannot be changed in the UI afterwards                                      | empty                  |
+------------------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+

*Note:* ``config.php`` *is a copy of* ``config-dist.php`` *in the root directory, which is tracked by version control, but not included by php. Any changes in* ``config-dist.php`` *will be ignored by Barcode Buddy*


configProcessing.inc.php
==============================

If you need to change the paths of the ``config.php`` or the user database location, you can do this in the ``configProcessing.inc.php`` file, found in the ``incl`` folder. As editing the file might break updating Barcode Buddy, it is rather recommended to use :ref:`envvar` instead of editing this file. The following values can be changed:

+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
| Argument    | Value           | Effect                                                                                                                                            | Default           |
+=============+=================+===================================================================================================================================================+===================+
| CONFIG_PATH | A writable path | The path were the config.php file is written to. Path includes filename.                                                                          | ./data/config.php |
+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
| AUTHDB_PATH | A writable path | The path were the user database file is written to. Make sure that the webserver does not allow the download of the file. Path includes filename. | ./data/users.db   |
+-------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+


.. _envvar:

********************************
Environment variables
********************************

Environment variables can be passed to Barcode Buddy - that way you can configure it without editing any files.

All ``const`` declarations found in :ref:`configphp` can be passed as an environment variable, but must have the prefix ``BBUDDY_``.

Example: To disable authentication, you need to set ``DISABLE_AUTHENTICATION`` to ``true``. Therefore you need to pass the variable ``BBUDDY_DISABLE_AUTHENTICATION`` with the value ``true`` (see :ref:`passingenv`)

**Note:** ``OVERRIDDEN_USER_CONFIG`` is declared as an array in ``config.php``. This is the only environment variable you need to pass as an array with the delimiter ``;``.

Example: To set the Grocy API details you need to declare ``GROCY_API_URL`` and ``GROCY_API_KEY``. As you can see in ``config.php``, they are part of the ``OVERRIDDEN_USER_CONFIG`` declaration (basically all configurations that can be changed through the web ui are part of that). You therefore need to pass the environment variable ``OVERRIDDEN_USER_CONFIG`` with the value ``GROCY_API_URL=https://myurl/api/;GROCY_API_KEY=1234``


.. _passingenv:

Passing environment variables to Barcode Buddy
===============================================


Docker
------

Pass the variable with the ``-e`` argument. Example for disabling authentication and setting curl timeout to 30:
::

 docker run -d -v bbconfig:/config -e BBUDDY_DISABLE_AUTHENTICATION=true -e BBUDDY_CURL_TIMEOUT_S=30 -p 80:80 f0rc3/barcodebuddy-docker:latest

Example for passing API details:
::

 docker run -d -v bbconfig:/config -e BBUDDY_OVERRIDDEN_USER_CONFIG="GROCY_API_URL=https://myurl/api/;GROCY_API_KEY=1234" -p 80:80 f0rc3/barcodebuddy-docker:latest


Bare Metal
----------

You need to add the variable to your Nginx configuration that you created in :ref:`webserverinit`. For each environment variable, add the following line in the ``location ~ \.php$`` block:
::

 fastcgi_param BBUDDY_XXXXX 'value';

Example: Disabling authentication and setting curl timeout to 30:
::

 	[...]
 	location ~ \.php$ {
                fastcgi_param BBUDDY_DISABLE_AUTHENTICATION 'true';
                fastcgi_param BBUDDY_CURL_TIMEOUT_S '30';
                fastcgi_read_timeout 80; 
                include fastcgi_params;
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }
 	[...]

Example: Passing API details:
::

 	[...]
 	location ~ \.php$ {
                fastcgi_param BBUDDY_OVERRIDDEN_USER_CONFIG 'GROCY_API_URL=https://myurl/api/;GROCY_API_KEY=1234';
                fastcgi_read_timeout 80; 
                include fastcgi_params;
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }
 	[...]


.. _api:

********************************
API
********************************

Barcode Buddy offers an API that can be reached at ``http(s)://your.bbinstall.url/api``. By visiting the URL, you will get current the documentation website with an overview of all API functions.


Interacting with the API
-------------------------


Unless disabled, all API calls will need an API key as authentication. A key can be generated in the web UI in the menu "API". The API key needs to be passed in the body or can be added as a GET variable. Example getting info with curl:
::

 curl -X GET -H "BBUDDY_API_KEY: myApiKey" "https://your.bbuddy.url/api/system/info"

Manually getting the info by adding the GET variable:
::

 https://your.bbuddy.url/api/system/info?apikey=myApiKey


All functions that require parameters *(except* ``/action/scan`` *)*, expect them as a form/post parameter.
Example: Setting the current mode to STATE_PURCHASE(2):
::

 curl -X POST -H "BBUDDY_API_KEY: [[apiKey]]" -F 'state=2' "https://your.bbuddy.url/api/state/setmode" 


Non-standard API: /action/scan
-------------------------------

As mentioned above, the ``/action/scan`` also looks for GET parameters, in addition to the regular form/post parameters. This is to make it easier for scripts / apps to pass barcodes to Barcode Buddy.

Instead of the POST parameter ``barcode`` you can also pass the GET parameter ``add`` or ``text`` instead. Example Passing the barcode 123456 by just requesting the URL:
::

 https://your.bbuddy.url/api/action/scan?apikey=myApiKey&add=123456

*******
Plugins
*******

Barcode Buddy offers plugin support. All PHP scripts in the folder ``plugins`` are automatically loaded. See also the `example script <https://github.com/Forceu/barcodebuddy/blob/master/plugins/EventReceiver.php>`_
.
