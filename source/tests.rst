==========
RISE Tests
==========

Image Stacking and Noise Reduction
==================================

The image stacking software that RISE (and RAPID) use to generate stacked science and reference images is `AWAICgen`_.

AWAICgen, originally named AWAIC (A WISE Astronomical Image Coadder), is a software developed at caltech for use on the Wide-Field Infrared Explorer (WISE).

We found that stacking with AWAICgen reduced the noise in the stacked image by roughly a factor of :math:`\sqrt{N}`, where N is the number of images used in the stack.

.. figure:: /images/stack_noise.png
   :width: 400px

   Background cutouts and RMS values of a single OU24 image,
   and 2, 4, and 8 depth stacks with the same strech applied.
   The noise reduction roughly follows :math:`\sqrt{N}`.

.. _AWAICgen: https://arxiv.org/abs/0812.4310

PSF & Photometry
================

Effective PSF
-------------

We tested building an Effective Point Spread Function (EPSF) from field stars within the stacked image using `photutils.psf.EPSFBuilder`_.

.. _`photutils.psf.EPSFBuilder`: https://photutils.readthedocs.io/en/stable/api/photutils.psf.EPSFBuilder.html

.. figure:: images/bkgsubbed_stacked_image_frac_error_stacked_epsf_os1x.png
    :width: 500px

.. figure:: images/bkgsubbed_stacked_image_frac_error_stacked_epsf_os2x.png
    :width: 500px

.. figure:: images/bkgsubbed_stacked_image_frac_error_stacked_epsf_os3x.png
    :width: 500px


    Overampling Factor = 1, 2, & 3. Fractional error in measured PSF photometry in field stars versus magntiude of field star.
    Grey points are all used field stars, red points are the mean in 0.5 magnitude bins.

Averaged PSF
------------

We tested rotating and averaging PSF models. Without Roman data, we tested Galsim psf models that were generated to mimic the PSFs used to generate the OpenUniverse2024 dataset.
With the launch of Roman and the beginning of commissioning, we hope to test PSF models from the Roman SOC on real Roman data.

.. figure:: images/frac_error_averaged_psf.png
    :width: 500px

    Rotated and averaged Galsim PSFs. Galsim PSFs generated to mimic the PSFs used to generate OU24 simulations.
    Grey points are all used field stars, red points are the mean in 0.5 magnitude bins.

Stacking Scheme Comparisons
===========================

We tested the order in which we stack and subtract. More info on the two stacking methods can be found `here (Link to pipeline_strcture.rst)`.
With the recent launch of the Roman Space Telescope, we eagerly await commissioning and early science data to perform analysis on real data.
In the mean time, our tests use the OpenUniverse2024 (OU24) simulated Roman data.

Open Universe 2024 Simulations
------------------------------

To test the efficacy of the two methods, we injected fake sources with randomly generated magnitudes between 25.5 and 29 into galaxies within the OU24 simualations.
We tested to see which stacking and subtracting strategy had the least amount of noise, extracted the most sources (highest completeness), and had a fainter S/N vs truth magnitude relation.

Background Noise
^^^^^^^^^^^^^^^^

We find differences in background noise between the stacking schemes and the differencing algorithms to be neglible.
The difference in background noise between the most and least noisy image is <1.5%.

.. figure:: /images/noise_comparison.png
    :width: 700px

    Noise comparison between varying stacking scheme and differencing algorithm.


Injections
^^^^^^^^^^

To choose candidate galaxies to inject into, we used the truth catalog to identify galaxies that had flux counts above some threshold to exclude particularly faint galaxies.
We somewhat aribitratily choose this threshold to be 10,000 counts.
We then made a full-coverage cut, using only galaxies that had coverage in all epochs that were used in both the science and reference image stacks.

For these injections atop of galaxies, we use an 8-stack science image, and a 16-stack reference.

|sci_image_injections| |diff_image_injections|

*Science image and SFFT Stack+Sub difference image with injections marked. Science image is 8-stack depth, and reference image is 16-stack depth.*

.. |sci_image_injections| image:: /images/cropped_sci_image_galaxy_injections.png
   :width: 500

.. |diff_image_injections| image:: /images/cropped_diff_image_galaxy_injections.png
   :width: 500

For reference, after all cuts a single field in filter F184 had 513 full-coverage galaxies. To improve statstics on our measurments, we iterated over each field 5 times,
and used 3 fields per filter. This takes the test of our first filter (F184) from 513 injections to  ~7,700.

For injection positions, we used randomly choosen offset radii between 1.5px = .165" and 10px = 1.1" from the galaxy centroid position as defined in the truth catalog.

We then ran Source Extractor (SExtractor) on the differenced images with these injections, using a 1.5 :math:`\sigma` threshold across 3 adjacent pixels. We then matched the SExtractor
catalogs to the injection truth positions. To determine a matching radius between sextractor coordinates and truth coordinates, we took a look at how the number of sources extracted
chnaged as a function of matching radius.

We find that it varies between between fields and each iteration in a field, though we consistenly find that there is a change in slope around 0.5px = 0.055",
which is interpreted as the point when residual contamintation dominates over matching to truth injections.
We adopt 0.5px = 0.055" as the match radius for our tests.

We also find that SFFT is less-sensitive to the stacking scheme, while for ZOGY, we see a preference for the Stack+Sub method.
Below are 3 examples of our match radius findings.

|match_vs_rad_ex1|

|match_vs_rad_ex2|

|match_vs_rad_ex3|

*Number of matched injections vs match radius. Dashed horizontal black line marks 0.5px.*

.. |match_vs_rad_ex1| image:: /images/match_vs_rad_iter1.png
   :width: 800

.. |match_vs_rad_ex2| image:: /images/match_vs_rad_iter4.png
   :width: 800

.. |match_vs_rad_ex3| image:: /images/match_vs_rad_iter5.png
   :width: 800


S/N vs Truth Magnitude relation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Using our inejections on top of galaxies, we find the S/N vs truth magnitudes to be comparable. There is neglible difference between the S/N of a given source between the two stacking methods
or the two differencing algorithms.


.. figure:: /images/091126/snr_fig.png
    :width: 1000px

    SNR vs Truth Magnitude


Though the S/N vs magnitude relation is comparable between the two schemes and differencing algorithms, the differences in completess are more apparent.
We find that SFFT outperforms ZOGY when it comes to number of sources extracted, while between both differencing algorithms, Stack+Sub appears to extract more sources. 

|zogy_bar| |sfft_bar|

*Sextractor detections* :math:`> 5\sigma`\ *. Number of detections in each stacking scheme compared to truth in 0.25 magntiude bins*

.. |zogy_bar| image:: /images/091126/zogy_bar.png
   :width: 500

.. |sfft_bar| image:: /images/091126/sfft_bar.png
   :width: 500

|zogy_completeness| |sfft_completeness|

*Sextractor detections* :math:`> 5\sigma`\ *. Completeness in each stacking scheme in 0.5 magnitude bins*

.. |zogy_completeness| image:: /images/091126/zogy_completeness.png
   :width: 500

.. |sfft_completeness| image:: /images/091126/sfft_completeness.png
   :width: 500


Roman Data
----------

With the successful launch of Roman on Aug. 30th, 2026, we eagerly await a chance to test our pipeline on WFI data!

More details to come.

Limiting Magntiude
==================





