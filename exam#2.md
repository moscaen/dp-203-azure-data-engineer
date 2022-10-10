# Exam #2

    # of Correct Answers/Total Questions: 33/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 71.43 % (10/14)
    Design and develop data processing 72.22 % (13/18)
    Design and implement data security 80 % (8/10)
    Monitor and optimize data storage and data processing 66.67 % (2/3)

## Design and implement data storage

### Design a data storage structure

- [disaster recovery table](./disaster_recovery.md)

### Design the serving layer

- star vs snowflake schema
- columnstore index vs clustered index
  - A nonclustered columnstore index and a clustered columnstore index function the same. The difference is that a nonclustered index is a secondary index that's created on a rowstore table, but a clustered columnstore index is the primary storage for the entire table
  - A nonclustered columnstore index enables real-time operational analytics where the OLTP workload uses the underlying clustered index while analytics run concurrently on the columnstore index

### Implement physical data storage structures

- Types of tables and tables options:
  - Fact tables -> Hash-distributed
  - Staging Tables -> Round Robin
  - Small Dimensions Table (< 1GB) -> Replicated
  - Tables with queries for date range -> Partitioned

### Implement logical data structures

- [Slowly changing dimension types](https://learn.microsoft.com/en-us/training/modules/populate-slowly-changing-dimensions-azure-synapse-analytics-pipelines/3-choose-between-dimension-types)
  - Type 1 (email address or phone number):
    - always reflects the latest values, and when changes in source data are detected, the dimension table data is overwritten
  - Type 2:
    - supports versioning of dimension members
    - includes columns that define the date range validity of the version (StartDate, EndDate)
  - Type 3:
    - supports storing two versions of a dimension member as separate columns (CurrentEmail, ModifiedDate)
  - Type 6 -> combines Type 1,2,3

## Design and develop data processing

### Design and develop a batch processing solution

- To copy a file from Azure Databrick to Azure Data Lake Storage -> dbutils.fs.cp(name_file,"abfss://sample@sample.dfs.core.windows.net/")

- To copy data from Azure Blob storage to a on-premise server using Azure Data Factory:
  - Install the self-hosted integration runtime on the local network
  - Create a self hosted integration runtime in Azure Data Factory UI

- Linked services stores the connection information from the source dataset.
- Activities are the task that are executed
- Pipelines are a group of activies linked together
- A tumbling window as the pipeline execution trigger can define:
  - starting time
  - ending time
  - time frame to run a pipeline
  