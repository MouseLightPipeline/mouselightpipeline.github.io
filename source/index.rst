.. Mouse Light Pipeline documentation master file, created by
   sphinx-quickstart on Mon Sep 16 09:54:37 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Home
****

.. image:: _static/mouselight-pipeline.svg

Mouse Light Pipeline is a collection of services that facilitate the multi-step data processing of the several-thousand tiles per acquisition sample. The processing may occur in real-time as the data becomes available, offline after acquisition is complete, or a combination of both. Although originally designed to utilize metadata specific to the Mouse Light acquisition process, it has since been generalized to allow processing of additional input sources.

Although similar to other computation pipelines, the Mouse Light Pipeline is designed around the concept of a 2-D or 3-D lattice of work units and manages the bookkeeping of spatially related elements.  This simplifies tasks that require patterns of elements such as using adjacent tiles in one or more planes.  The second significant feature is the integrated real-time monitoring of the pipeline with the ability to pause and resume stages of the pipeline and the porject itself independently.  This includes multiple ways to view the status of each stage (waiting, in process, complete) each plane (which work units are in what state for which stage), and for each work unit (history or past and, if applicable, current processing attempts).

.. toctree::
   :maxdepth: 0

   usage/installation
   usage/quickstart
   usage/getting_started
