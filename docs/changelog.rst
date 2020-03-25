.. _changelog:


Changelog
=========

Overview of all Changes
-----------------------

v1.4.0.0: 20 Mar 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* External websockets have been replaced with Server Sent Events. This means that you finally don't need any complicated configuration anymore and that all the websocket features should work without setting anything up. You still need to start the websocket server however, as it is used for internal communication. (SEE is basically a proxy for the websockets)

* Docker image available at https://github.com/Forceu/barcodebuddy-docker


v1.3.2.0: 16 Mar 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Moved database location to "/data/" folder. Existing database will be moved automatically, however you need to make sure that your webserver does not allow access to this directory!
* Added functionality to make running in docker easier
* Tags ignore whitespaces and special characters now
* Added a second variant that grabs barcode scanner input (see example/grabInput_variant2.py, thanks @ChadOhman )
* Refactoring of code

v1.3.1.1: 24 Oct 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Fixed bug in which the state reverted to consume immediately
* If an unknown barcode was scanned, the barcode showed up as state in the screen module
* Refactored code.

v1.3.1.0: 14 Oct 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Fixed issue #14 that disabled buttons when creating a new Grocy product
* The Screen module now shows the current state
* Added settings menu to test websocket connection
* Fixed some websocket bugs

v1.3.0.3: 28 Sep 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Grocy 2.5.1+ now required
* Screen module now features button to enable sound and wakelock for mobile devices
* Screen module can now be set to open in fullscreen
* Add barcode to add items to the default shopping list

v1.3.0.1: 4 Sep 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* fixed several issues with Quantity management


v1.3.0.0: 29 Aug 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Added feature to create new Grocy product
* Added feature to handle multipacks (eg. set quantity per barcode)

v1.2.2.0: 13 Aug 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Added Inventory mode
* Modes can be changed with GET variables
* Setup checks if all required extensions are installed
* Bug fixes

v1.2.1.0: 7 Aug 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Added option to remove purchased item from shopping list
* Many minor fixes, full support for PHP5 now
* Fixed crash from library when websockets were enabled, but server not started

v1.2.0.0:  1 Aug 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Settings are now no longer saved in the config.php file. After upgrading you will be asked to re-enter your Grocy * API details. If previously active, you need to enable websockets again as well in Menu / Settings.
* Added Chore support - add a barcode for your chore in Menu / Chores.
* Default barcodes were changed, as underscores cannot reliably be output will all barcode scanners

v1.1.2.1: 29 Jul 2019 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Fixed problems that default barcodes were processed and then added to the "unknown barcodes" list
* Added Tag viewer
* Fixed problem were products were not selectable in v1.2.0

v1.0.1.1: 28 Jul 2019 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Added PHP5 support for websocket server
* Hotfix for a communication problem with the database, which stopped Barcode Buddy from working

v1.0.0.0: 25 Jul 2019
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* First stable release of the program