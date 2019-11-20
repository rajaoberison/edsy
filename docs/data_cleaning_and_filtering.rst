.. _data_cleaning_and_filtering:

============================
Data Cleaning and Filtering
============================


Data Cleaning
==============

Data cleaning is the process of correcting or removing inaccurate records of a "raw data" so that, after the treatment, the transformed data will have the quality necessary for statistical analysis or be consistent with an existing database. More explicitly, the variable names, types, and values will be consistent and uniform.

In order to clean a dataset, first, we need to be able to detect the anomalies within the data. Types of nomalies include the values that are stored in the wrong format (ex: a number stored as a string), the values that fall outside of the expected range (ex: outliers), values with inconsistent patterns (ex: dates stored as mm/dd/year vs dd/mm/year), trailing spaces in strings (ex: "data" vs "data "), etc.

One method of detecting these anomalies is the summary statistics of the variables, which can be obtained by using :code:`summary()`. Here is an example using the hurricane data showing the range of responses for numerical variables and the types of responses for categorical variables.


.. code-block:: r

  # Summary for a numerical variables
  summary(hurricane$age)


.. code-block:: rout

     Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    19.00   49.00   60.00   58.69   69.00  106.00      40 


.. code-block:: r

  # Summary for a categorical variable
  summary(factor(hurricane$prepared))


.. code-block:: rout

     1    2    3    4    5 NA's 
    92  322  428  130   17    7 



