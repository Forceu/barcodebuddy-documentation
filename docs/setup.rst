.. _setup:

=====
Setup
=====

There are two different ways to setup Barcode Buddy: Either a bare metal approach or docker

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
| ATTACH_BARCODESCANNER | true/false | Attach barcode scanner               |
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

.. _webserverinit:

Webserver setup
"""""""""""""""""

This guide is written for a Debian based server, including Ubuntu. If you already have a webserver setup, please make sure to have a look at the `Nginx example file <https://github.com/Forceu/barcodebuddy/blob/master/example/nginxConfiguration.conf>`_, as for the folder /api/ a rewrite rule has to be added.

Installing NGINX
------------------

* Get root, ideally with ``sudo -i``
* Install nginx: ``apt-get install nginx``
* If you are running a server with ufw active, run ``ufw allow 'Nginx Full'``
* Install all php modules and other requirements ``apt-get install php-fpm php-curl php-date php-json php-sqlite sudo screen evtest``
* Check what PHP version you are using with ``php --version`` (eg. "7.2").
* Copy the `Nginx example file <https://github.com/Forceu/barcodebuddy/blob/master/example/nginxConfiguration.conf>`_ to ``/etc/nginx/sites-enabled/``
* Adjust the new file:

   * If you are not using PHP7.2, change the line  ``fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;`` to your PHP version
   * If you are not installing Barcode Buddy to /var/www/html/barcodebuddy/, change the line ``root /var/www/html/barcodebuddy/;`` to your directory
* Follow the steps below to download either the stable or unstable version
* Execute the command ``chown www-data:www-data -R /path/to/the/barcodebuddy/folder`` for the folder that you just created
* Change ``pm.max_children`` to a value of 10 in ``/etc/php/7.2/fpm/pool.d/www.conf`` (adjust path for your PHP version)
* Restart NGINX ``service nginx restart``



Configuring Apache2
--------------------

We recommend using Nginx. If you are already an Apache2 user, follow these steps to make sure that Barcode Buddy is working correctly:

* Execute ``a2enmod rewrite`` to make sure that the rewrite module is active
* Make sure that you can use .htaccess files for rewriting. For that the option ``AllowOverride`` for the directory must be set to ``All``. You can normally find this configuration in the ``apache2.conf`` file. For Ubuntu this file is located at ``/etc/apache2/apache2.conf``. Search for ``AllowOverride`` and set it to ``All`` for the root directory where Barcode Buddy is installed.

Example:
::

 [...]
 <Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
 </Directory>
 [...]



Stable version
"""""""""""""""""
`Download the project <https://github.com/Forceu/barcodebuddy/releases/>`_ and copy all files into your webserver.

Unstable version
"""""""""""""""""
Execute 
::

 git clone https://github.com/Forceu/barcodebuddy.git .

in the folder where you want to install Barcode Buddy to.


Starting the websocket service
""""""""""""""""""""""""""""""

If you have access to your webservers command line, make sure to start the websocket server. This way you can use the Screen module and if there are any changes, Barcode Buddy will automatically refresh.

Navigate to your installation folder and execute ``php wsserver.php`` to start the server. To have it run in the background, either use the screen application (recommended)
::

 screen -S bbuddyserver -d -m /usr/bin/php /path/to/the/barcodebuddy/folder/wsserver.php

or the following command:
::

 nohup php wsserver.php &

To start the websocket server after a reboot, you can use cron. Make sure to use the crontab for the webserver user (on Debian/Ubuntu this the user ``www-data``.

Open the crontab for the user:
::

 sudo crontab -e -u www-data

And insert the following new line (you might need to adjust the paths):
::

 @reboot /usr/bin/screen -S wsserver -d -m /usr/bin/php /var/www/html/barcodebuddy/wsserver.php



***********
VirtualBox
***********

We have also released a `VirtualBox <https://www.virtualbox.org/>`_ image, which automatically downloads the latest docker image and runs it.


Installation
^^^^^^^^^^^^

Open VirtualBox, and go to ``File/Host Network Manager``. If there is no network listed yet, click on "Create" and make sure that the box for ``DHCP Server`` is ticked. `Download the image <https://mega.nz/#!0dg1HbyD!gWHDReNfyJ7SE0JwPt8EylpsZEenQVHRBFEhWSLjcbI>`_ and open it with VirtualBox, then click on "Import" in the new window.

Start the image - once it is completely running, you will see a login prompt. Above that, you will see two IP addresses. Normally with the second one you can reach the server, so simply connect in your webbrowser to ``http://THE_IP/``.

If you need to log in to the image, the default username is ``root`` and the default password is ``barcode``. For security reasons, SSH is disabled, to enable it, execute  ``rc-update add sshd`` (make sure to change your password and to add a non-root user!)


********
Hass.IO
********


Connecting to Grocy
^^^^^^^^^^^^^^^^^^^^

If you are running Grocy in a HASS.io container, further configuration is needed. Open HASS and go to the Grocy plugin section (not Grocy itself). Scroll down and enter ``9192`` in the ``Network`` section and press save. Make sure that you disable SSL in the Grocy config section above, if you are not using a proper certificate. Then restart Grocy. You will now be able to access Grocy under the URL ``http://hassio.local:9192``. In Barcode Buddy setup, enter ``http://hassio.local:9192/api/`` as URL.  
