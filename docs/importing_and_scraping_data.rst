.. _importing_and_scraping_data:

=============================
Importing and Scraping data
=============================

.. contents::
   :local:
   :depth: 2


CSV files
==========

Reading csv with column headers and separated by ",". These parameters are also the default values for the :code:`read.csv` function.

.. code-block:: r

   data <- read.csv(file = '/path/to/csv', header = TRUE, sep = ',')

   # example
   data <- read.csv(file = 'eds.data.hurricane.csv', header = TRUE)
   head(data)
   
.. image:: raw.githubusercontent.com/edsy/images/csvsample.png
   :width: 400
   :alt: csvsample
   
