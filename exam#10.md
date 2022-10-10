# Exam #10

    # of Correct Answers/Total Questions: 33/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 94.74 % (18/19)
    Design and develop data processing 81.82 % (9/11)
    Design and implement data security 37.5 % (3/8)
    Monitor and optimize data storage and data processing 42.86 % (3/7)

## Design and implement data storage

### Implement physical data storage structures

- CREATE TABLE (...) WITH (DISTRIBUTION=REPLICATE) creates a replicated table
- possible values for DISTRIBUTION are:
      - | DISTRIBUTION = HASH ( distribution_column_name ) -> only use this if you are SURE that the data will be DISTRIBUTED EVENLY
      - | DISTRIBUTION = HASH ( [distribution_column_name [, ...n]] )
      - | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics -> distributes the data evenly across compute nodes
      - | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse -> the table is copied across all the compute nodes (ONLY SUITABLE FOR SMALL TABLES)
- the PARTITION Option creates a partitioned table

## Design and develop data processing

### Design and develop a batch processing solution

- To copy data from Azure Blob storage to a on-premise server using Azure Data Factory:
  - Install the self-hosted integration runtime on the local network
  - Create a self hosted integration runtime in Azure Data Factory UI

## Design and implement data security

### Design security for data policies and standards

- Account shared access signature vs Service shared access signature
  - A service SAS allows you to give clients access to a storage account resource. In this scenario, the resource is the Azure table. An SAS is a URL that embeds the resource, permissions, Ip range, protocol, access times and token.
  - You should NOT provide an account SAS. An account SAS allows you to give clients access to multiple resources in one or more storage services. In this scenario, you only want to allow the clients to access a table, not the blob container.

### Implement data security

- To enable TDE with a custom key from Azure Key Vault using Powershell: [link](https://learn.microsoft.com/en-us/azure/azure-sql/database/transparent-data-encryption-byok-overview?view=azuresql)
    1. Assign an azure AD identity to the Azure SQL Database Server
    2. Grant Key Vault permission to the Azure SQL Database Server
    3. Add the Key Vault key to the Azure SQL Database Server and set it as TDE Protector
    4. Turn on TDE
