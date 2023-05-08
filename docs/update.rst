.. _update:

======================
Updating Barcode Buddy
======================

***************
Docker
***************

To update, run the following command:
::

  docker pull f0rc3/barcodebuddy:YOURTAG

Then stop the running container and follow the same steps as in SETUP. All userdata will be preserved, as it is saved to the bbuddy volume (-v command) 

**********
Bare Metal
**********

Stable version
==============

To update, download the latest release and unzip it to the directory that contains the old version. Overwrite any existing files.


Unstable version
=================

To update, execute the command ``git pull``
