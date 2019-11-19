.. _importing_and_scraping_data:

=============================
Importing and Scraping data
=============================

.. contents::
   :local:
   :depth: 2


CSV files
==========

Reading csv with column headers and separated by :code:`,`. These parameters are also the default values for the :code:`read.csv` function.

.. code-block:: r

   data <- read.csv(file = '/path/to/csv', header = TRUE, sep = ','
   
   # Example
   data <- read.csv(file = 'eds.data.hurricane.csv', header = TRUE)
   head(data)

.. image:: https://raw.githubusercontent.com/rajaoberison/edsy/master/images/csv.png
   :height: 100px
   :alt: csvsample

Excel Files
===========
The main advantage of Excel files is that they can store multiple tables. But reading these tables at once is different from a CSV. For this example, we're going to use the :code:`readxl` package from the `tidyverse` collection. Please visit this `website <https://www.tidyverse.org/>`_ to learn more about `tidyverse`.
To read an excel file, you can use the `read_excel` function and specify at least the `path/to/the/file` and `sheet` you want to open. If you don't specify the `sheet`, `read_excel` will automatically open the first table in the spreadsheet.

.. code-block:: r

   # In the 'eds.excel.sample.xlsx' file, there are 2 tables: heatwave and hurricane
   # Here's how we read both tables into r

   # Loading the library
   library(readxl)

   # Reading the tables
   heatwave <-  read_excel(path='eds.excel.sample.xlsx', 'heatwave')
   hurricane <-  read_excel(path='eds.excel.sample.xlsx', 'hurricane')

   # Once the tables are stored into individual r variable, you can perform exploration and analysis with them.
