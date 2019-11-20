.. _importing_and_scraping_data:

=============================
Importing and Scraping data
=============================

.. contents::
   :local:
   :depth: 2


CSV Files
==========

Reading csv with column headers and separated by :code:`,`. These parameters are also the default values for the :code:`read.csv` function.


.. code-block:: r

   data <- read.csv(file = '/path/to/csv', header = TRUE, sep = ',')
   
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
  heatwave <-  read_excel(path='eds.excel.sample.xlsx', sheet = 'heatwave')
  hurricane <-  read_excel(path='eds.excel.sample.xlsx', sheet = 'hurricane')

  # Once the tables are stored into individual r variable, you can perform exploration and analysis with them.


Google Spreadsheets
====================

If the data is stored in a Google spreadsheet, we can read it using the :code:`googledrive` and :code:`googlesheet4` packages from the :code:`tidyverse` collection. We use the :code:`googledrive` package to log into our Google Drive account and :code:`googlesheets4` to read the speadsheets in our drive.

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


Then you can load the spreadsheet by using its :code:`id`


.. code-block:: r

 eds.sample.spreadsheet <- drive_get(id = '1uIsgrcsevbm9voZU-rzqhTg2LE5SgEPlGabSXKTcQtc')


It also possible to read the spreadsheet right way by using its link / :code:`path` (without using :code:`drive_ls()`)


.. code-block:: r

  eds.sample.spreadsheet <- drive_get(path = 'https://drive.google.com/open?id=1uIsgrcsevbm9voZU-rzqhTg2LE5SgEPlGabSXKTcQtc')


Once the spreadsheet is loaded, we run a similar code used for the Excel files to read tables within the spreadsheet. But for Google Sheets, function is called :code:`read_sheet`


.. code-block:: r

  # Loading the library
  library(googlesheets4)
  # Authorizing the googlesheets4 package
  sheets_auth(token=drive_token())
  # Readind the tables
  heatwave <- read_sheet(eds.sample.spreadsheet, sheet = 'heatwave')
  hurricane <- read_sheet(eds.sample.spreadsheet, sheet = 'hurricane')


Web Scraping
=============

Web scraping is the process of fteching a webpage and extracting information / data from it. It is very useful if you want to create a dynamic database that updates based on the content of a specific website.

To scrap a webpage, we first need to know how to get to the webpage, a url that you can use to directly access the content. For example, to obtain the Google search results for "data science", you can simply copy and paste this url to your browser: https://www.google.com/search?q=data+science, without having to type "data science" on Google search web page. Some website like Twitter or Facebook will require to you to use an API and authenticate in order to access some of their data. 

For this example, we're going to use The Weather Channel website which do not require autentification. We'll to extract the 10-day forecast for a specific location and store the data in a dataframe.

After inspecting the website and it's url, I have noticed that you can view the weather data by zip code using this url pattern:

:code:`https://weather.com/weather/` + :code:`forecast type` + :code:`/l/` + :code:`zip_code` + :code:`:4:US`

For example, if we want to view the 10-day forecast for New Haven, we can go to: https://weather.com/weather/tenday/l/06511:4:US. And for today's forecast: https://weather.com/weather/today/l/06511:4:US

Once we have the webpage url, we can read it into R and extract the data using :code:`rvest` from the :code:`tidyverse` collection.

The New Haven 10-forecast webpage looks like this:

.. image:: https://raw.githubusercontent.com/rajaoberison/edsy/master/images/weatherpage.png
   :height: 300px
   :align: center
   :alt: weatherpage

Basically, what we want is the table that have the weather information. In order to extract the values that we want, we have to know where in the source code they are located. For example, in the "DAY" column, we want to extract the `exact date` instead of the `days of the week`. And we can do that by:

* inspecting the tag or class of exact date from the website. Move the cursor to the exact date, right-click, then choose :code:`Inspect`
* then, a window will open, which will point directly to location of the `exact date` in the source code. Take notes of the css (tag or class name), and use it to get the `exact date` value using the :code:`html_nodes()` function.

.. image:: https://raw.githubusercontent.com/rajaoberison/edsy/master/images/webcss.png
   :height: 100px
   :align: center
   :alt: webcss

Here is how we extract the dates:


.. code-block:: r

  # Loading library
  library(rvest)

  # Get the webpage url
  url = 'https://weather.com/weather/tenday/l/06511:4:US'
  # Load the webpage using the url
  webpage <- read_html(url)

  # Getting the exact date
  # Filtering the relevant css / location
  date_locations <- html_nodes(webpage, "span.day-detail.clearfix")
  # Extracting the exact value
  raw_date <- html_text(date_locations)
  # Because the value are formatted like "Nov 21" we have to convert to a date format
  exact_date <- as.Date(raw_date, format="%b %d") # b = month, d = day


.. code-block:: rout

  # raw date
   [1] "NOV 19" "NOV 20" "NOV 21" "NOV 22" "NOV 23" "NOV 24" "NOV 25" "NOV 26" "NOV 27" "NOV 28"
  [11] "NOV 29" "NOV 30" "DEC 1"  "DEC 2"  "DEC 3" 

  # exact_date
   [1] "2019-11-19" "2019-11-20" "2019-11-21" "2019-11-22" "2019-11-23" "2019-11-24" "2019-11-25"
   [8] "2019-11-26" "2019-11-27" "2019-11-28" "2019-11-29" "2019-11-30" "2019-12-01" "2019-12-02"
  [15] "2019-12-03"


And here is the full code that extract the complete table:

.. code-block:: r

# Loading library
library(rvest)

# Get the webpage url
url = 'https://weather.com/weather/tenday/l/06511:4:US'
# Load the webpage using the url
webpage <- read_html(url)

# Getting the exact date
# Filtering the relevant css / location
date_locations <- html_nodes(webpage, "span.day-detail.clearfix")
# Extracting the exact value
raw_date <- html_text(date_locations)
# Because the value are formatted like "Nov 21" we have to convert to a date format
exact_date <- as.Date(raw_date, format="%b %d") # b = month, d = day

# Getting the weather description
desc_loc <- html_nodes(webpage, "td.description")
desc <- html_text(desc_loc)

# Getting the temperature
temp_loc <- html_nodes(webpage, "td.temp")
temp <- html_text(temp_loc)
# High and Low temperature values
high_temp <- rep(NA, length(temp))
low_temp <- rep(NA, length(temp))
for (i in 1:length(temp)){
  all <- unlist(strsplit(temp[i], "°"))
  if (length(all) > 1){
    high_temp[i] <- all[1]
    low_temp[i] <- all[2]
  } else {
    low_temp[i] <- 38
  }
}

# Getting the precipitation
precip_loc <- html_nodes(webpage, "td.precip")
precip <- as.numeric(sub("%", "", html_text(precip_loc))) / 100

# Getting the wind
wind_loc <- html_nodes(webpage, "td.wind")
wind <- html_text(wind_loc)
# Wind direction and speed
wind_dir <- rep(NA, length(wind))
wind_speed <- rep(NA, length(wind))
for (i in 1:length(wind)){
  all <- unlist(strsplit(wind[i], " "))
  wind_dir[i] <- all[1]
  wind_speed[i] <- all[2]
}

# Getting the humidity
humidity_loc <- html_nodes(webpage, "td.humidity")
humidity <- as.numeric(sub("%", "", html_text(humidity_loc))) / 100

# Save the data in tibble
library(tibble)
new_haven_forecast <- tibble('day' = exact_date, 'description' = desc,
                             'high_temp' = high_temp, 'low_temp' = low_temp,
                             'precip' = precip, 'wind_dir' = wind_dir,
                             'wind_speed' = wind_speed, 'himidity' = humidity)
                             
                             
                             
