.. _changelog:


Changelog
=========

Overview of all Changes
-----------------------



v1.6.0.0: 22 Dec 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Grocy >= 3.0.0 required
* Update API for Grocy 3.0.0 (#99)
* Added UPCDatabase.org as a provider (#97) @Teagan42
* Added jumbo.com as provider #96
* Add baseURL config (#93)
* Check if word is used multiple times when creating possible tags, ignore case
* Add to "action required" if unknown barcode associated with item is tare #100 (#101)
* Refactor Provider Lookups for Scalability (#97) @Teagan42
* Added overflow for item name #88, removed X-Frame-Options to work in iframe
* Added min-width for wrapping
* Change Google URL to HTTPS


v1.5.0.4: 13 May 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


* Grocy version 2.7.1+ now required
* Sped up Screen module to display output (up to 3 times faster now)
* Added barcode BBUDDY-CA to consume all items in stock
* Added authentication by IP/IP range - Thanks @cyberjacob
* No API key required if already authorised in web UI
* Added option to consume the same quantity as stored with BBUDDY-Q barcode
* Added example and documentation for Nginx reverse proxy
* API: price and best before date can be passed
* Added option to set external Grocy URL 
* Added option to respect grocys quantity conversion
* API menu is now always shown
* Fixed "Add" button not showing on small screens
* Fixed JS problems in Screen module
* Fixed SSE reconnection issue - Thanks @glaaze
* Fixed that barcodes were associated with a product multiple times
* Fixed that tags were not found if the name was using hyphens or other characters
* UI enhancements



v1.5.0.2: 15 Apr 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Fixed API call state/setmode
* Environment variable for CONFIG_PATH was not parsed
* Added variables to hide Grocy and Screen module links
* Other minor changes



v1.5.0.1: 14 Apr 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Added authentication, can be disabled through config.php
* Added support for Grocys tare feature
* Config.php now found in the data directory
* Added API (if you are adding barcodes with GET, you will need to use the API instead)
* Support for official `Barcode Buddy App <https://play.google.com/store/apps/details?id=de.bulling.barcodebuddyscanner>`_
* Fixed race conditions
* Show more detailed errors if they occur
* Barcode Buddy settings can be set through environment variables
* Barcode grabber bash variables can be set through environment variables Thanks. @Biont
* Add logs when adding/consuming new products through UI
* New theme for Screen module. Thanks @hanerd
* Python barcode grabber script for devices that disconnect frequently. Thanks @vondit
* Main ui refreshes through AJAX now without reloading
* Better support for iOS13
* Created robots.txt. Thanks @bbreton09
* Added favicon
* Added admin menu for dowloading database backup
* Refactoring / Minor fixes


v1.4.1.1: 26 Mar 2020
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Fixed problems with SSE, in some cases it was stuck in a feedback loop, causing PHP-FPM to freeze
* Fixed bug that fullscreen setting was not acknowledged
* Added "text" GET variable (alias of "add") so it can used with the Android App QR & Barcode Scanner
* Added option to ignore invalid SSL certificates (config.php)
* Added bash script for grabbing barcode scanner input
* Moved documentation to ReadTheDocs
* Detect if API key is rejected by Grocy
* Items that were added by clicking on "Add" in the UI were not removed from the shoppinglist
* Ignore case for sorting products
* Display errors in log view as well
* Set HTTP Agent to BarcodeBuddy
* Check if php-sockets is installed
* Added more docker options
* Minor UI changes

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
