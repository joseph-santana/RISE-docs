RISE Pipeline Products
======================

These are the products produced by the RISE pipeline:

Stack then Sub:
---------------

.. list-table::
   :header-rows: 1
   :widths: 40 25 50

   * - Directory
     - Filename
     - Description
   * - ``awaicgen_outputs/{sci_depth}stack_sciimage/``
     - ``awaicgen_output_mosaic_cov_map.fits``
     - AWAICgen coverage map for the stacked science image
   * - ``awaicgen_outputs/{sci_depth}stack_sciimage/``
     - ``awaicgen_output_mosaic_uncert.fits``
     - AWAICgen uncertainty image for the stacked science image
   * - ``awaicgen_outputs/{sci_depth}stack_sciimage/``
     - ``awaicgen_output_mosaic.fits``
     - AWAICgen stacked science image
   * - ````
     - ````
     - 
   * - ``injected_images/``
     - ``injected_images_epoch_{epoch}_jid*.fits``
     - Science image with optional RISE injections. Directory contains a file for each image going into the science stack.
   * - ````
     - ````
     - 
   * - ``injection_catalogs/``
     - ``inj_catalog_epoch_{epoch}_jid*.txt``
     - Science image injection catalog. Directory contains a file for each image going into the science stack.




