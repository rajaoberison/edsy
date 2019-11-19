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


.. code-block:: rout

    nstorm worry nevac prepared homeloc nyear gender income politics age zone      lat      long
  1      2     3     0        3       2    86      1      4        3  86    A 41.26324 -72.84297
  2      1     4     0        3       3    25      1     NA       NA  52    A 41.31061 -72.09882
  3      3     4     0        3       3    19      2      6        2  83    A 41.26523 -72.82523
  4      3     6     1        1       1    52      1      5        3  64    A 41.15419 -73.11342
  5      2     1     0        2       3    15      1      5        3  66    A 41.26944 -72.89225
  6      5     4     0        2       1    23      2     NA        3  76    A 41.27140 -72.59094


.. .. image:: https://raw.githubusercontent.com/rajaoberison/edsy/master/images/csv.png
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

In the example below, I used a spreadsheet named :code:`eds.sample.googlesheets` which contains the same tables in the previous Excel example (heatwave and hurricane). You can clone the spreadsheet via this `link <https://drive.google.com/open?id=1uIsgrcsevbm9voZU-rzqhTg2LE5SgEPlGabSXKTcQtc>`_ if you'd like to repeat the steps below using your Google account.


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

The last line will output the following where you can have the name and id of the Google Sheet you want to open in R:


.. code-block:: rout

  # A tibble: 1 x 3
    name                    id                                           drive_resource   
  * <chr>                   <chr>                                        <list>           
  1 eds.sample.googlesheets 1uIsgrcsevbm9voZU-rzqhTg2LE5SgEPlGabSXKTcQtc <named list [35]>

Because of Google authentification system, you may run into an error like below when re-running the previous code (using :code:`drive_ls()`).


.. code-block:: rout

  Error in add_id_path(nodes, root_id = root_id, leaf = leaf) : !anyDuplicated(nodes$id) is not TRUE


To avoid this, you can use the folder url instead of the folder name. The folder url can be obtained by right-clicking on the folder and click :code:`Get shareable link`. Then run the following code:


.. code-block:: r

  # If using folder name doesn't work
  folder_url = 'https://drive.google.com/open?id=1e0uJ9dwFcL34JA61F0tGSoaiMZ_xio_4'
  drive_ls(folder_url, type="spreadsheet")





