# Getting Started with BigQuery

# OBJECTIVES
    1. Load data from Cloud Storage into BigQuery.
    2. Perform a query on the data in BigQuery.

# STEP 1 : Sign into cloud console and load data from cloud storage to Big Query
    1. Using the navigation menu click BigQuerry and click done
    2. Create a dataset in your project using the create dataset button specifying the following:
        -Dataset ID~~~logdata
        -Data location~~~continent closest to you
        -click create and create the Dataset
    
    3. Create a table in the dataset to store the data in the dataset logdata.
        - Create table from, choose select Google Cloud Storage
        - field, type gs://cloud-training/gcpfci/access_log.csv
        - Verify File format is set to CSV

    4. Specify the following in the Destination set
        -Dataset name==logdata
        -Table name==accesslog
        -Table type==Native table
        -Accept the remaining as defaults and create.

 # STEP 2 : Perform a query on the data using the BigQuery web UI
    1. Use the BigQuery UI to access the accesslog table and query the following.
        -select int64_field_6 as hour, count(*) as hitcount from logdata.accesslog group by hour order by hour

 # STEP 3 : Perform a query on the data using the bq command

    1. Activate the cloud shell and enter the following command
        -bq query "select string_field_10 as request, count(*) as requestcount from logdata.accesslog group by request order by requestcount desc"

        