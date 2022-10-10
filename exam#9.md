# Exam #9

    # of Correct Answers/Total Questions: 34/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 92.86 % (13/14)
    Design and develop data processing 58.82 % (10/17)
    Design and implement data security 72.73 % (8/11)
    Monitor and optimize data storage and data processing 100 % (3/3)

## Design and implement data storage

### Design a data storage structure

- Cosmos Db has the Table API which allows to use OData and LINQ to query data.

## Design and develop data processing

### Design and develop a batch processing solution

- Stream analytics is a big data solution that allows you to analyze real-time events simultaneously

- Filter vs conditional split:
  - Both uses a case matching algorithm to determine which particular path a data stream should take.
  - The key difference is that the conditional split only returns data from the first succesfully executed condition. If additional conditions are met, they are not executed on.
  A filter returns results from all records that satisfy the filter.
  - In programming flow, the conditional split is equivalent to a CASE statement, and the filter is equivalent to a WHERE clause

### Design and develop a stream processing solution

|           | Overlap | Filters period with no data | Check time duration between events | Events belong to single window | Fixed size lenght |
| --------- | ------- | --------------------------- | ---------------------------------- | ------------------------------ | ----------------- |
| Tumbling  | N       | N                           | N                                  | Y                              | Y                 |
| Hopping   | Y       | N                           | N                                  | N                              | Y                 |
| Windowing | Y       | Y                           | N                                  | N                              | Y                 |
| Session   | Y       | Y                           | Y                                  |                                | N                 |

- Azure Streaming Analytics does NOT support Azure Cosmos Db for reference input
- Azure Streaming Analytics DOES support Azure SQL DB for reference input
- Azure Streaming Analytics DOES support Azure Blob Storage for reference input
- When you want real-time processing for large amount of incoming data Azure Even Hubs and Azure IoT Hubs are good options

## Design and implement data security

### Design security for data policies and standards

|                                  | Azure Blob | Azure Files (SMB) | Azure Files (REST) | Azure    Queries | Azure Tables |
| -------------------------------- | ---------- | ----------------- | ------------------ | ---------------- | ------------ |
| Shared Access Signature (SAS)    | Y          | N                 | Y                  | Y                | Y            |
| Azure Active Directory (AD)      |            |                   | N                  |                  | N            |
| Shared Key (storage account key) | Y          | Y                 | Y                  | Y                | Y            |
| Anonymous public read acccess    |            |                   |                    |                  |              |

## Implement data security

[always encrypted](https://learn.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)

- GRANT ALTER ANY SCHEMA MASTER KEY TO 'USER':
    The key that is used to encrypt and decrypt the column encyrption keys is referred to as the master key.

- GRANT ALTER ANY COLUMN ENCRYPTION KEY TO 'USER':
    This grants User permission to anage column encryption keys.

- GRANT VIEW ANY COLUMN MASTER KEY DEFINITION TO 'USER'
    This grants User permission to read metadata of column master keys and to query encrypted columns

- GRANT VIEW ANY COLUMN ENCRYPTION KEY DEFINITION TO 'USER'
    This grants User permission to read metadata of column encryption keys and to query encrypted columns
