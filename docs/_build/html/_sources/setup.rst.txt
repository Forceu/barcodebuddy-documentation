.. _setup:

=====
Setup
=====

There are two different ways to setup BarcodeBuddy: Either a bare metal approach or docker

******
Docker
******


Requirements
^^^^^^^^^^^^


* A host running docker


Installation
^^^^^^^^^^^^
To download, run the following command, and replace YOURTAG with one from the list below:
::

  docker pull f0rc3/barcodebuddy-docker:YOURTAG

+--------------------+--------------+----------+
|         Tag        | Architecture |  Version |
+--------------------+--------------+----------+
|       latest       |    x86-64    |  stable  |
+--------------------+--------------+----------+
|   arm32v7-latest   |     armhf    |  stable  |
+--------------------+--------------+----------+
|   arm64v8-latest   |     arm64    |  stable  |
+--------------------+--------------+----------+
|     latest-dev     |    x86-64    | unstable |
+--------------------+--------------+----------+
| arm32v7-latest-dev |     armhf    | unstable |
+--------------------+--------------+----------+
| arm64v8-latest-dev |     arm64    | unstable |
+--------------------+--------------+----------+

Most of the time, you will need the *latest* tag. If you are running docker on a Raspberry Pi, you will need the *arm32v7-latest* tag.

*Stable* indicates, that you are using the latest release which should work without any bugs. *Unstable* is the latest developer version, which might include more features, but could also contain bugs.

If you don't want to download the prebuilt image, you can find the Dockerfile on the `Github project page <https://github.com/Forceu/barcodebuddy-docker>`_
. 

Starting the Container
^^^^^^^^^^^^^^^^^^^^^^

To start the container, run the following command: ::

 docker run -d -v bbconfig:/config -p 80:80 -p 443:443 f0rc3/barcodebuddy-docker:YOURTAG

You can now open ``http://DOCKER_HOST_IP/`` to set up BarcodeBuddy. If you are already serving a webserver on your Docker host, you need to change the ports, eg.:
::

 docker run -d -v bbconfig:/config -p 8080:80 -p 9443:443 f0rc3/barcodebuddy-docker:latest

The following arguments can also be passed:

+-----------------------+------------+-------------------------------------+
|        Argument       |    Value   |                Effect               |
+-----------------------+------------+-------------------------------------+
| ATTACH_BARCODESCANNER | true/false | Attach barcodescanner               |
+-----------------------+------------+-------------------------------------+
| IGNORE_SSL_CA         | true/false | Accept self-signed SSL certificates |
+-----------------------+------------+-------------------------------------+
| IGNORE_SSL_HOST       | true/false | Accept SSL certificates where the   |
|                       |            | host does not match                 |
+-----------------------+------------+-------------------------------------+

*For more information on how to attach a barcode scanner, see* :ref:`attachingscanner`

To pass an argument, use the -e function, eg:
::

 docker run -d -v bbconfig:/config -e ATTACH_BARCODESCANNER=true -p 80:80 -p 443:443 f0rc3/barcodebuddy-docker:latest

**********
Bare Metal
**********

Requirements
^^^^^^^^^^^^

* webserver (eg. NGINX, Apache)
* curl
* Access to the command line
* PHP (PHP5 min, however PHP7+ highly recommended)
* The following PHP modules:

  * curl
  * date
  * json
  * sqlite3
  * sockets
* When using a barcode scanner:

  * Root access!
  * sudo
  * screen
  * evtest


Installation
^^^^^^^^^^^^

*Stable* indicates, that you are using the latest release which should work without any bugs. *Unstable* is the latest developer version, which might include more features, but could also contain bugs.

It is strongly recommended to change ``pm.max_children`` to a value of 10 or higher in ``/etc/php7/php-fpm.d/www.conf`` (path might be different, depending on PHP version and distribution; for Ubuntu 18.04 it is ``/etc/php/7.2/fpm/pool.d/www.conf``).

Stable version
"""""""""""""""""
`Download the project <https://github.com/Forceu/barcodebuddy/releases/>`_ and copy all files into your webserver.

Unstable version
"""""""""""""""""
Execute 
::

 git clone https://github.com/Forceu/barcodebuddy.git

and move the folder into your webserver directory.

Webserver setup
"""""""""""""""""

Make sure that all permissions are set, so the script can create a database file. For Linux servers, you could execute the command ``sudo chown www-data:www-data -R /path/to/the/barcodebuddy/folder`` for the folder that you just created. Make sure to disallow the download of the database by using the .htaccess file for Apache or the part from the `Nginx example file <https://github.com/Forceu/barcodebuddy/blob/master/example/nginxConfiguration.conf>`_.

Starting the websocket service
""""""""""""""""""""""""""""""

If you have access to your webservers command line, make sure to start the websocket server. This way you can use the Screen module and if there are any changes, BarcodeBuddy will automatically refresh.

Navigate to your installation folder and execute ``php wsserver.php`` to start the server. To have it run in the background, either use the screen application (recommended)
::

 screen -S bbuddyserver -d -m /usr/bin/php /path/to/the/barcodebuddy/folder/wsserver.php

or the following command:
::

 nohup php wsserver.php &



Further Setup
^^^^^^^^^^^^^^

You will find the file ``config.php`` in the folder "incl". This file is for further configuration - the following values can be changed:


+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
|           Argument           |      Value      |                                                                        Effect                                                                       |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
|     PORT_WEBSOCKET_SERVER    |    1024-65535   | The port that the websocket server listens to. Change if you running multiple instances or the default port is already used by another application. |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
|         DATABASE_PATH        | A writable path | The path were the database file is written to. Make sure that the webserver does not allow the download of the file.                                |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
|        CURL_TIMEOUT_S        |       5-60      | How long to wait for a request                                                                                                                      |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
|  CURL_ALLOW_INSECURE_SSL_CA  |    true/false   | Accept self-signed SSL certificates                                                                                                                 |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| CURL_ALLOW_INSECURE_SSL_HOST |    true/false   | Accept SSL certificates where the host does not match                                                                                               |
+------------------------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+


