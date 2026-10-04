===========
RISE To-dos
===========

Image Stacking and Noise Reduction
==================================

The image stacking software that RISE (and RAPID) use to generate stacked science and reference images is `AWAICgen`_.

AWAICgen, originally named AWAIC (A WISE Astronomical Image Coadder), is a software developed at Caltech for use on the Wide-Field Infrared Explorer (WISE).

We found that stacking with AWAICgen reduced the noise in the stacked image by roughly a factor of :math:`\sqrt{N}`, where N is the number of images used in the stack.

.. figure:: /images/stack_noise.png
   :width: 700px

   Background cutouts and RMS values of a single OU24 image,
   and 2, 4, and 8 depth stacks with the same strech applied.
   The noise reduction roughly follows :math:`\sqrt{N}`.

.. _AWAICgen: https://arxiv.org/abs/0812.4310