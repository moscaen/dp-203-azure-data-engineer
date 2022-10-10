# Exam #1

    # of Correct Answers/Total Questions: 23/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 57.89 % (11/19)
    Design and develop data processing 46.15 % (6/13)
    Design and implement data security 45.45 % (5/11)
    Monitor and optimize data storage and data processing 50 % (1/2)

## Design and implement data storage

### Design a data storage structure

- [DTU Vs vCore](https://learn.microsoft.com/en-us/azure/azure-sql/database/purchasing-models?view=azuresql)
- OData queries:
  - Azure table
  - Azure cosmos DB with Table APis
- relational vs non relational
  - data that have attributes that can vary must use non-relational
- Disaster recovery in case of regional failure -> Failover Group
- Stack
  - Azure Data Lake -> store initial intake of data
  - Azure PolyBsae -> convert and normakize data
  - Azure Synapse Analytics -> support historical data queries. Designed to run Petabytes of data in Parallel
  - PowerBi -> visualize data
  - Azure Sql Data Warehouse -> store data long term
  - Azure DataBricks -> allow DE to visualize the data
  - Stream Analytics -> real-time processing of streaming events from mulitple sources simultaneously
                      -> Analyze telemetry data sent to an IoT Hub

### Design a partition strategy

- [partition strategy for: 200 rows, total size 100Kb, 90% of data have same postal code, table used for join](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-overview)
  - Replicated table (for small dimension table). it copies data over 60 nodes, improves join performances
  - Can't use hash distribution because 90% of the data is the same (the postal code)
  - Can't use round robin (it copies the 200 rows evenyl across all distributions)

### Implement logical data structures

- [T-SQL statement](https://learn.microsoft.com/en-us/sql/t-sql/statements/create-external-data-source-transact-sql?view=azuresqldb-current&tabs=dedicated): Create Externale DATA SOURCE BlobStorage with (type = HADOOP ...)

### Design and develop a stream processing solution

- [Windowing Functions](https://learn.microsoft.com/en-us/azure/stream-analytics/stream-analytics-window-functions)
  - Tumbling: segments data into distinct, fixed size time segments: No Overlap
  - Hopping: segments data into distinct time segments: Overlap
  - Windowin: produces output only when event occurs
  - Session: groups together streaming events that arrives at similar time. Filters out time periods with no data

- to implement stream analytics job:
    1. Configure Azure Event Hubs as stream input
    2. Set up Power Bi as stream output
    3. Define a query
    4. Start the Job

### Manage batches and pipelines

- To enable version control for artifacts in a pipeline in Azure Data Factory use:
  - Github
  - Azure DevOps Repos

## Design and implement data security

### Design security for data policies and standars

- TDE encrypts the entire database and prevents offlice access of data through raw files or backup files. It als encrypts transaction log files.

### Implement data security

- To enable TDE with a custom key from Azure Key Vault using Powershell: [link](https://learn.microsoft.com/en-us/azure/azure-sql/database/transparent-data-encryption-byok-overview?view=azuresql)
    1. Assign an azure AD identity to the Azure SQL Database Server
    2. Grant Key Vault permission to the Azure SQL Database Server
    3. Add the Key Vault key to the Azure SQL Database Server and set it as TDE Protector
    4. Turn on TDE

- To configure TDE in a geo-redundant Azure SQL Database environment to use the TDE protector from the Azure Portal:
    1. On the secondary Azure SQL Database Server assign a Key vault from the same region
    2. On the secondary Azure SQL Database Server assign the TDE protector
    3. On the primary Azure SQL Database Server assign a Key Vault from the same region
    4. On the primary Azure SQL Database Server assign the TDE protector

- To encrypt a column of a DB that is used for joins:
  - [Always Encrypted](https://learn.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver16) with a deterministic type
  - [DDM](https://learn.microsoft.com/en-us/sql/relational-databases/security/dynamic-data-masking?view=sql-server-ver16) (dynamic data masking) with random or partial masking never works (since they do not really encrypt the column)
    - Use XXXX or fewer Xs if the size of the field is less than 4 characters for string data types (nchar, ntext, nvarchar).
    - Use a zero value for numeric data types (bigint, bit, decimal, int, money, numeric, smallint, smallmoney, tinyint, float, real).
    - Use 01-01-1900 for date/time data types (date, datetime2, datetime, datetimeoffset, smalldatetime, time).
    - For SQL variant, the default value of the current type is used.
    - For XML the document '<masked/>' is used.
    - Use an empty value for special data types (timestamp table, hierarchyid, GUID, binary, image, varbinary spatial types).

  - DDM: hide specifics fields from unauthorizd users
  - RLS: operates at row level and hide the whole record (you cannot only hide specific fields)

## Monitor and optiize data storage and data processing

### Optimize and troubleshoot data storage and data processing

- To move data fast from general purpose v2 Azure storage into Azure Synapse Analytics Sql pool:
  - Write and run PolyBase T-SQL commands (fully parallel)
  - Use Copy Activity in Azure Data Factory
