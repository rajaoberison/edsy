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
The main advantage of Excel files is that they can store multiple tables. But reading these tables at once is different from a CSV. For this example, we're going to use the :code:`readxl` package from the :code:`tidyverse` collection. Please visit this `website <https://www.tidyverse.org/>`_ to learn more about :code:`tidyverse`.

To read an excel file, you can use the :code:`read_excel` function and specify at least the :code:`path/to/the/file` and :code:`sheet` you want to open. If you don't specify the :code:`sheet`, :code:`read_excel` will automatically open the first table in the spreadsheet.

.. code-block:: r

   # In the 'eds.excel.sample.xlsx' file, there are 2 tables: heatwave and hurricane
   # Here's how we read both tables into r

   # Loading the library
   library(readxl)

   # Reading the tables
   heatwave <-  read_excel(path='eds.excel.sample.xlsx', 'heatwave')
   hurricane <-  read_excel(path='eds.excel.sample.xlsx', 'hurricane')

   # Once the tables are stored into individual r variable, you can perform exploration and analysis with them.


Google Spreadsheets
====================
If the data is stored in a Google spreadsheet, we can use the :code:`googledrive` and :code:`googlesheet4` packages from the :code:`tidyverse` collection. We use the :code:`googledrive` package to log into our Google Drive account and :code:`googlesheets4` to read the speadsheets in our drive.

In the example below, I used a spreadsheet named `eds.sample.googlesheets` which contains the same tables in the previous Excel example (heatwave and hurricane). You can clone the spreadsheet via this `link <https://drive.google.com/open?id=1uIsgrcsevbm9voZU-rzqhTg2LE5SgEPlGabSXKTcQtc>`_ if you'd like to repeat the steps below using your Google account.

.. code-block:: r
   # Logging into Google Drive
   # Loading the library
   library(googledrive)

   # To authenticate and authorize googledrive package
   # When prompted: log in, authorized googledrive, and use the authorization code provided
   drive_auth()
   # Then, to view the list of files in a folder
   drive_ls("EDS") # where "EDS" is the folder name
   # To also get the files within the subfolders
   drive_ls("EDS", recursive = TRUE)
   # To view the list of spreadsheets within a folder
   drive_ls("EDS", type="spreadsheet")
..

Because of Google authentification system, you may run into an error like below when re-running the previous code.

.. code-block:: rout
   Error in add_id_path(nodes, root_id = root_id, leaf = leaf) : !anyDuplicated(nodes$id) is not TRUE
..

To avoid this, you can use the folder url instead of the folder name. The folder url can be obtained by right-clicking on the folder and click :code:`Get shareable link`. Then run the code below


