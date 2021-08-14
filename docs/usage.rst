.. _usage:

=====
Usage
=====

.. _firststart:

First Start
===============

Open your Grocy website. Click on Settings in the top right corner and then click on *Manage API keys*. Click on *add* and copy the long API key to your clipboard. You can now open Barcode Buddy by visiting your webservers URL. The setup will first let you create a user and then ask you to enter the API details. Make sure to have the trailing "/api/" for your Grocy URL at the end.


Using The Web UI
================

Overview
--------
When you open the web ui, you will see three cards:

* New Barcodes: Barcodes that are unknown to Grocy, but the name could be looked up
* Unknown Barcodes: Barcodes that are unknown to Grocy and could not be looked up
* Processed Barcodes: A history of all barcodes that were processed by Barcode Buddy

If you have entered a barcode that is linked to a Grocy product with the tare feature enabled, a fourth section will pop-up named "Actions required", where you can enter the weight for the product.

Special Barcodes
----------------
There are seven special barcodes - if you scan the barcode, Barcode Buddy goes into a different mode: Eg. in Purchase mode all barcodes that are scanned will be added to Grocys inventory.


+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Mode                | Default Barcode | Explanation                                                                             |
+=====================+=================+=========================================================================================+
| Consume             | BBUDDY-C        | All items scanned in this mode will be removed from the inventory                       |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Consume (spoiled)   | BBUDDY-CS       | All items scanned in this mode will be removed from the inventory and marked as spoiled |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Consume all         | BBUDDY-CA       | All products scanned in this mode will have all stock removed from their inventory      |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Purchase            | BBUDDY-P        | All items scanned in this mode will be added to the inventory                           |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Open                | BBUDDY-O        | All items scanned in this mode will be marked as opened                                 |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Inventory           | BBUDDY-I        | Displays the current amount of items in stock of that product                           |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Add to shoppinglist | BBUDDY-AS       | All items scanned in this mode will be added to the default shopping list               |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+
| Quantity            | BBUDDY-Q-?      | Sets a quantity for a barcode. Replace ? with number of items. Eg. if you have a carton |
|                     |                 | with 10 eggs, scan the barcode BBUDDY-Q-10 and then the barcode of the carton. The next |
|                     |                 | time you scan the carton barcode, it will register as 10 units.                         |
+---------------------+-----------------+-----------------------------------------------------------------------------------------+

You can find printable copies of the barcodes in the `example folder <https://github.com/Forceu/barcodebuddy/tree/master/example/defaultBarcodes>`_
.



Using the UI
------------

Once a barcode was added and is not recognised by Grocy, it will be added to the list in the web ui. Simply select the Grocy product from the drop-down list. If the name could be looked up, you will also see checkboxes for each word that was used in the name. If you want the selected Grocy product preselected for a different barcode that includes such a word, tick it and then press either "Consume" or "Add". The barcode will then saved to Grocy and the next time you scan it, the product will automatically be processed.

.. image:: https://github.com/Forceu/barcodebuddy/raw/master/example/screenshots/FullSite_small.png


Adding Barcodes
===============

Adding Barcodes Manually
------------------------

The easiest option, ideally for testing out Barcode Buddy: Simply open the web ui and click on "Add barcode". Enter the barcode (use one line per barcode) and click on "Add".

If you are using a barcode scanner, but don't want to attach it to Barcode Buddy (yet), you can also plug it into the device that runs the webbrowser and use it to enter the barcodes in the text field. Each line is parsed as a barcode.

Adding Barcodes Automatically
-----------------------------

The preferred way. Most barcode scanners register as a USB keyboard. That way, it is possible to grab the input and send it to Barcode Buddy.

.. _attachingscanner:

Using a physical barcode scanner
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Barcode scanner connected to an Android device
""""""""""""""""""""""""""""""""""""""""""""""""


If you use a barcode scanner that is connected to your Android device, you can use the `Android App <https://play.google.com/store/apps/details?id=de.bulling.barcodebuddyscanner>`_ to send all input to Barcode Buddy. Open the app, go to Settings and activate ``Enable Bluetooth Barcode Scanner``. Then return to the main menu. Every time you scan a barcode while being in the app's main menu, all input is sent to Barcode Buddy. For more info, refer to :ref:`androidapp`.


Barcode scanner connected to a Linux device
"""""""""""""""""""""""""""""""""""""""""""""


Plug in your barcode scanner to the Linux computer / server you will be using. Run the command ``evtest`` as root. You will see a list of devices, select the one that is your barcode scanner and remember the number (e.g.. event6). Scan a barcode. You will now see output in the evtest program. If not, you have selected the wrong source.


Docker
******

Create a docker container with
::

 docker run -v bbconfig:/config -e ATTACH_BARCODESCANNER=true -p 80:80 -p 443:443 --device /dev/input/eventX f0rc3/barcodebuddy-docker:YOURTAG

where X in ``--device /dev/input/eventX`` is the number of your event you selected previously. You might need to change the values for the ports. Scan a barcode - it should be sent directly to Barcode Buddy.

Bare Metal
************

Navigate to the example folder in the Barcode Buddy directory. In the file ``grabInput.sh`` edit the following values:

* If your barcode scanner is attached to the same computer / server:
   * ``SCRIPT_LOCATION``: Replace with the location where your index.php file is located
* If the scanner is attached to a different computer / server:
   * ``SERVER_ADDRESS``: Replace with the URL where your index.php file can be accessed from
   * ``USE_CURL``: Set to "true"
* If the web server does not run as user www-data (uncommon):
   * ``WWW_USER``: Set to the name of the user

Then run as root
::

 bash grabInput.sh /dev/input/eventX

where X is the number of your event you selected previously. Scan a barcode - it should be sent directly to Barcode Buddy.

To run the script in the background, run
::

 screen -S barcodegrabber -d -m /bin/bash /path/to/the/barcodebuddy/folder/example/grabInput.sh /dev/input/eventX


.. _androidapp:

Using Barcode Buddy app for Android
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Download the app here: `Google Play Store <https://play.google.com/store/apps/details?id=de.bulling.barcodebuddyscanner>`_

Once installed, open Barcode Buddy and navigate to the menu ``API``. There click on the three dots in the top right corner and select ``Add mobile app``. Open your Barcode Buddy app and then scan the displayed QR code. Once the automatic setup is complete, tap on the barcode symbol to start scanning.


Using a 3rd party application / script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you want to write your own script, there are two ways to send the barcodes to Barcode Buddy: either by calling ``php index.php yourBarcode`` or by calling the URL: ``https://your.bbuddy.url/api/action/scan?apikey=myApiKey&add=123456``. Only one barcode can be given with each call. Replace myApiKey with an API key generated in the main menu. For more information about the API visit :ref:`api`.


Menus
======

Settings menu
----------------

General Settings
^^^^^^^^^^^^^^^^^^^^^^

In this tab you can set the barcodes for changing Barcode Buddy modes. For example, if you scan the barcode "BBUDDY-P", Barcode Buddy will change to "Purchase" mode and add all following items to your Grocy inventory. By default it is in "Consume" mode. The edit field below allows you to set the time in minutes, which is required to pass in order to revert back to the default "Consume" mode. E.g. if "Purchase" mode is active and the field is set to 10 minutes, Barcode Buddy will revert back to "Consume" mode 10 minutes later. If set to 0, reverting is disabled.

If you scan the "Inventory" barcode, Barcode Buddy will simply output the current stock, but not change any values. If an unknown barcode is scanned, it is added to the regular list.

The "Add to shopping list" barcode adds all future barcodes to the default shopping list.

With the "Revert after single item scan in "Open" or "Spoiled" mode" checkbox ticked, Barcode Buddy only stays in this mode for one scan and then reverts back to the default "Consume" mode. It does not affect the "Purchase" mode however!

With "Remove purchased items from shopping list" enabled, items that are scanned in purchase mode are removed from all Grocy shopping lists.

With "Consume amount of quantity saved for barcode" enabled, Barcode Buddy will check if you saved a quantity for this barcode with the default barcode BBUDDY-Q-[X]. Normally, it will only use this amount for adding products to the inventory, with this option however it will also remove the same amount from the inventory when you are scanning the barcode in Consume mode.

The option "Use generic names for lookup" makes it easier to tag products. If found, it will use the generic name for a product instead of a brandname. For example instead of using "GreatCompany extra virgin oil", Barcode Buddy will name the product "Olive Oil".

When "more verbose logs" is disabled, only barcode scans are logged in the log part of the main page.

You will also find several checkboxes to enable / disable lookup providers. Some might require an API key, which can be entered at the lower section. The lookup order can also be changed by clicking on a provider and moving it to a different position without releasing the mouse key.

The more providers you have activated, the slower the lookup will be (especially for failed lookups). However using more providers also means having higher chances of a successful lookup.

Grocy API
^^^^^^^^^^^
Here you can change your Grocy API details. Refer to :ref:`firststart`.


Redis Status
^^^^^^^^^^^^^^^^^^^^^^
If you have installed a redis server (:ref:`setup`), you can enable it here. Once it is enabled, it will cache the Grocy products and therefore speed up Barcode Buddy a lot. It is recommended to use a redis server, but not required.



Websocket Status
^^^^^^^^^^^^^^^^^^^^^^
This section gives the status of the websocket server and if Barcode Buddy is able to connect to it

Chores
--------------------------

This menu lists all available Grocy chores. Simply enter a barcode for a chore and press "Add". The next time you scan this barcode, the chore will be executed. To change the barcode, simply edit it and press "Edit". To remove, delete the barcode and press "Edit".


Tags
----------------

All saved tags are listed here

Adding tags
^^^^^^^^^^^

Scan a barcode that was not recognized by Grocy yet, but could be looked up. Before pressing "Add" or "Consume" in the main menu, select a word from the list to the right. The next time a barcode is looked up that contains the word, the product is preselected.

Managing tags
^^^^^^^^^^^^^^

The list shows an overview of the tags. Click on "Delete" to remove the tag.


Quantities
--------------------

This features is for products that come in packs containing more than one item.

In the settings you see the quantity barcode (default "BBUDDY-Q-"). If you scan a barcode that starts with this text and has a number at the end, Barcode Buddy sets the quantity of the units from the previously scanned barcode to the number. For example: You scan Barcode "123", which is a pack of 6 eggs. Then you scan the barcode "BBUDDY-Q-6". The next time you scan the barcode "123" in purchase mode, Barcode Buddy will automatically add 6 eggs. All barcodes are synced with Grocy. Deleting a quantity in this menu will also unlink the quantity in Grocy.

API
--------------------

In this menu you can create and revoke Barcode Buddy API keys. Refer to :ref:`api`


Admin
--------------------

In this menu you can download a backup of your database file. To restore a backup, simply overwrite your current database file (default: ``/data/barcodebuddy.db``.

It is also possible to logout, so that you need to enter your username and password again.


Barcode Buddy Federation
=========================


Overview
----------------

Barcode Buddy Federation is an external service, which enables the user to search the Barcode Buddy Federation database for unknown barcodes. By default it is turned off.

It works by sending all barcodes that are associated with a Grocy product to an external server, so that other users can look them up. No personal data will be stored in this process and no data will be used for commercial purposes.

Enabling Federation Lookup
----------------------------

The Federation lookup can be enabled and disabled in the menu "Federation". There you will find a button to enable / disable this feature.


How to use Federation Lookup
------------------------------


Barcode Lookup
^^^^^^^^^^^^^^^^^^^

In order to use the lookup feature, Federation must be enabled. If a lookup was unsuccessful with other lookup providers, Barcode Buddy will connect to the Federation database and request the name of the product. If found, the name will be used for the barcode and displayed in the main page under "New Barcodes".
You can also change the lookup order in the settings menu.


Multiple names
^^^^^^^^^^^^^^^^

Sometimes you might notice that a blue button appears next to the name. This is the case if multiple names were returned by the Federation server. Click on the button to change the name to another one displayed on the selection. Note: By selecting a new name, you actually vote for the name. So if more people select a better fitting name, this name will be the top result the next time.


Reporting Barcode Names
^^^^^^^^^^^^^^^^^^^^^^^

In case you encounter an offensive or malicious name, you can report it by clicking on the flag next to the name in the main menu. This flag is only visible if the name was actually provided by a Federation lookup.
