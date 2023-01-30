# Azure Synapse

[Dedicated SQL Pool Cheat Sheet](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/cheat-sheet)

[Partition Video](https://www.youtube.com/watch?v=4SQouxsR7DQ)

## External Data Source

```sql
CREATE EXTERNAL DATA SOURCE nyc_taxi_data
WITH (
    LOCATION = 'abfss://<azure_container>@<azure_storage_account>/'
)
    
# Use it in BULK like DATA_SOURCE
    

```

## OPENROWSET

[Documentation](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-openrowset)

* Supported Formats
    * CSV
    * PARQUET
    * DELTA

* For better debugging change PARSER_VERSION to 1.0 version

```sql
SELECT *
FROM 
    OPENROWSET(
        BULK 'abfss://<azure_container>@<azure_storage_account>/file.csv', # OR BULK 'file.csv' with DATA_SOURCE TAG
      #  DATA_SOURCE = 'nyc_taxi_data',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = ',',
        ROWTERMINATOR = '\n'
    )
    WITH (
        LocationID SMALLINT 1,
        Borough VARCHAR(15) 2 COLLATE Latin1_General_100_CI_AI_SC_UTF8
    ) AS [result]
```

### JSON Files

* Single Line JSON (JSON_VALUE)

```sql
SELECT CAST(JSON_VALUE(jsonDoc, $.payment_type) AS SMALLINT) payment_type,
       CAST(JSON_VALUE(jsonDoc, $.payment_type_desc) AS VARCHAR(15)) payment_type_desc
FROM 
    OPENROWSET(
        BULK 'file.csv'
        DATA_SOURCE = 'nyc_taxi_data'    
        FORMAT = 'CSV'
        PARSER_VERSION = '2.0'
        FIELDTERMINATOR = '0x0b'
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0a'
    )
    WITH (
       jsonDoc NVARCHAR(MAX)
    ) AS [result]
```

* Query Standard JSON

```sql
SELECT rate_code_id, rate_code
FROM 
    OPENROWSET(
        BULK 'file.csv'
        DATA_SOURCE = 'nyc_taxi_data'    
        FORMAT = 'CSV'
        FIELDTERMINATOR = '0x0b'
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
       jsonDoc NVARCHAR(MAX)
    ) AS rate_code
    CROSS APPLY OPENJSON(jsonDoc)
    WITH(
    rate_code_id TINYINT,
    rate_code VARCHAR(20)
  );
```

### File Metadata Functions

```sql
SELECT result.filename() as file_name,
       result.filepath(1) as year,
       result.filepath(2) as month,
       COUNT(1) as record_count
FROM 
    OPENROWSET(
        BULK 'trip_data_green_csv/year=*/month=*/*.csv'
        DATA_SOURCE = 'nyc_taxi_data',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0',
        HEADER_ROW = TRUE,
    ) AS [result]
WHERE result.filename() IN ('green_tripdata_2021-01.csv', 'green_tripdata_2020-01.csv')
GROUP BY result.filename()
ORDER BY result.filename();
```

## Stored Procedures

### Describe First Result

```sql
EXEC sp_describe_first_result_set N'
SELECT *
FROM 
    OPENROWSET(
        BULK ''file.csv''
        DATA_SOURCE = ''nyc_taxi_data'',
        FORMAT = ''CSV'',
        PARSER_VERSION = ''2.0'',
        HEADER_ROW = TRUE,
        FIELDTERMINATOR = '','',
        ROWTERMINATOR = ''\n''
    )
    WITH (
        LocationID SMALLINT 1,
        Borough VARCHAR(15) 2 COLLATE Latin1_General_100_CI_AI_SC_UTF8
    ) AS [result]'
```

## Statistics

[Link](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-tables-statistics#examples-create-statistics)

* To verify

```sql
SELECT name, is_auto_create_stats_on
FROM sys.databases
```

* Turn On

```sql
ALTER DATABASE <tourstuffhere>
SET AUTO_CREATE_STATISTICS ON
```

## Dynamic Management Views

### dm_pdw_nodes_tran_database_transactions

If your queries are failing or taking a long time to proceed, you can check and monitor if you have any transactions rolling back.

### dm_pdw_nodes_db_partition_stats

To analyze any skewness in the data.

### dm_pdw_request_steps 

To analyze data movements behind queries, monitor the time