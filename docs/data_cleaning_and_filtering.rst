.. _data_cleaning_and_filtering:

============================
Data Cleaning and Filtering
============================


Data Cleaning
==============

Data cleaning is the process of correcting or removing inaccurate records of a "raw data" so that, after the treatment, the transformed data will have the quality necessary for statistical analysis or be consistent with an existing database. More explicitly, the variable names, types, and values will be consistent and uniform.

In order to clean a dataset, first, we need to be able to detect the anomalies within the data. Types of anomalies include the values that are stored in the wrong format (ex: a number stored as a string), the values that fall outside of the expected range (ex: outliers), values with inconsistent patterns (ex: dates stored as mm/dd/year vs dd/mm/year), trailing spaces in strings (ex: "data" vs "data "), etc.

One method of detecting these anomalies is the summary statistics of the variables, which can be obtained by using :code:`summary()`. Here is an example using the hurricane data:


.. code-block:: rout

    > # Summary for a numerical variables
    > summary(hurricane$age)
     Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    19.00   49.00   60.00   58.69   69.00  106.00      40


.. code-block:: rout

    > # Summary for a categorical variable
    > summary(factor(hurricane$prepared))
     1    2    3    4    5 NA's 
    92  322  428  130   17    7 


We also summarize all of the variables at once but first we have to set them to match their exact data types. If we run :code:`summary(hurricane)` right away then we'll get this unintended result:


.. code-block:: rout

         nstorm           worry           nevac           prepared        homeloc          nyear          gender          income         politics          age         zone         lat             long
     Min.   : 0.000   Min.   :1.000   Min.   :0.0000   Min.   :1.000   Min.   :1.000   Min.   : 0.0   Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   : 19.00   A:622   Min.   :41.00   Min.   :-73.66
     1st Qu.: 2.000   1st Qu.:3.000   1st Qu.:0.0000   1st Qu.:2.000   1st Qu.:1.000   1st Qu.:12.0   1st Qu.:1.000   1st Qu.:3.000   1st Qu.:2.000   1st Qu.: 49.00   B:374   1st Qu.:41.16   1st Qu.:-73.22
     Median : 2.000   Median :4.000   Median :0.0000   Median :3.000   Median :2.000   Median :25.0   Median :2.000   Median :4.000   Median :3.000   Median : 60.00           Median :41.26   Median :-72.95
     Mean   : 2.584   Mean   :4.235   Mean   :0.4257   Mean   :2.654   Mean   :2.182   Mean   :28.8   Mean   :1.545   Mean   :3.751   Mean   :2.889   Mean   : 58.69           Mean   :41.22   Mean   :-72.91
     3rd Qu.: 3.000   3rd Qu.:5.000   3rd Qu.:1.0000   3rd Qu.:3.000   3rd Qu.:3.000   3rd Qu.:43.0   3rd Qu.:2.000   3rd Qu.:5.000   3rd Qu.:3.000   3rd Qu.: 69.00           3rd Qu.:41.29   3rd Qu.:-72.67
     Max.   :20.000   Max.   :7.000   Max.   :4.0000   Max.   :5.000   Max.   :3.000   Max.   :92.0   Max.   :2.000   Max.   :6.000   Max.   :5.000   Max.   :106.00           Max.   :41.45   Max.   :-71.83
     NA's   :28       NA's   :5       NA's   :7        NA's   :7       NA's   :11      NA's   :15     NA's   :32      NA's   :100     NA's   :81      NA's   :40


Variables like :code:`worry`, :code:`prepared`, :code:`homeloc`, :code:`gender`, :code:`income`, :code:`politics` are supposed to be categorical and not numeric. We can easily convert them using :code:`mutate_at()` function from the :code:`dplyr` package (also :code:`tidyverse`)

