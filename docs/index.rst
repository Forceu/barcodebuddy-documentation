.. _index:

Barcode Buddy for Grocy
===========================

Barcode Buddy for Grocy is an extension for `Grocy`_, allowing to pass barcodes to Grocy. It supports barcodes for products and chores. If you own a physical barcode scanner, it can be integrated, so that all barcodes scanned are automatically pushed to BarcodeBuddy/Grocy.

Why use Barcode Buddy and how does it work
---------------------------------------------

Barcode Buddy makes using a barcode scanner a lot easier - ideally the barcode scanner is connected to a server / computer so it can send the barcodes to the programm. Alternatively the barcodes can be manually added with the web ui.
In contrast to Grocy, unknown barcodes are looked up with OpenFoodFacts.org first, in order to find out the product name / category. If found, BarcodeBuddy checks if the user has stored any tags associated with the name: For example, if the name is "Chocolate bar", it can automatically be set to the Grocy product "Chocolate".

A known barcode will be processed, so it reduces / increases the inventory or manipulates the shopping list.

If the product could not be looked up, the user can select it manually.

Chores can also be executed with barcodes.

.. _Grocy: https://github.com/grocy/grocy


Contents
========

.. toctree::
   :maxdepth: 2

   setup

#   consumption
#   usage
#   changelog
