# Exam #3

    # of Correct Answers/Total Questions: 31/45
    
    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 60 % (6/10)
    Design and develop data processing 82.35 % (14/17)
    Design and implement data security 75 % (9/12)
    Monitor and optimize data storage and data processing 33.33 % (2/6)

## Design and implement data storage

### Design a data storage structure

- Hash distribution is used for tables larger than 2GB and tables that uses a lot of INSERT, UPDATE, DELET
- Round Robin is used for smaller tables when you want to optimize data loads

### Design a partition strategy

- Sharding partitions data horizontally to distribuite data across multiple dbs in a scahled-out design. This requires that the schema is the same on all of the dbds involved. Shardings helps to minimize the size of individual dbs which improves transactional process
- Vertical partitioning
- Vertical Partitioning stores tables &/or columns in a separate database or tables.
- Horizontal Partitioning (sharding) stores rows of a table in multiple database clusters

### Implement physical data storage structures

- CREATE TABLE (...) WITH (DISTRIBUTION=REPLICATE) creates a replicated table
- possible values for DISTRIBUTION are:
      - | DISTRIBUTION = HASH ( distribution_column_name ) -> only use this if you are SURE that the data will be DISTRIBUTED EVENLY
      - | DISTRIBUTION = HASH ( [distribution_column_name [, ...n]] )
      - | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics -> distributes the data evenly across compute nodes
      - | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse -> the table is copied across all the compute nodes (ONLY SUITABLE FOR SMALL TABLES)

## Design and develop data processing

### Design and develop a batch processing solution

- SSAS data source cannot be used in a ADF Copy Activit
- The ADF Copy Activity has an option to enable Polybase
- The ADF Copy Activity has an option to enable change tracking which allows incremental load fromthe Azure SQL Db

### Design and develop a stream processing solution

- Stream Analytics -> real-time processing of streaming events from multiple sources (i.e. IoT Hub) simultaneously

### Manage batches and pipelines

- Azure monitor can receive diagnostics logs containing metrics about pipeline runs from Azure Data Factory. The metrics are stored lognger than 45 days
- Data Factoy Monitor & Manage also stores the metrics, but only up to 45 days

## Design and implement data security

### Design security for data policies and standars

|                                                               | RBAC | Uses the storage account key | Set Expiration |
| ------------------------------------------------------------- | ---- | ---------------------------- | -------------- |
| User Delegation Key & user delegation Shared Access Signature | Y    | N                            | Y              |
| User Delegation Key & account level shared Access Signature   | N    | Y                            | Y              |
| User Delegation Key & shared key access                       | N    | Y                            | N              |

### Implement data security

- When Always encypted is activated to query encrypted columns a user must have:
  - VIEW ANY COLUMN MASTER KEY DEFINITION permission
  - VIEW ANY COLUMN ENCRIPTION KEY DEFINITION permission

- Always Encrypted allows you to encrypt specific columns in a table
- Always Encrypted allows you to encrypt data while it is in transit between the client application and the db server
- TDE allows you to encrypt an entire dbs at rest

## Monitor and optimize data storage and data processing

### Monitor data storage and data processing

- sys.dm_pdw_exec_requests: returns all queries that are currently running or that were recently running

### Optimize and troubleshoot data storage and data processing

- in Azure Streaming Analytics Job to administer a job so that it uses optimal performance
  - DO allocate more SUs (Streaming Unit) than you need
  - DO keep the SU metric below 80%
