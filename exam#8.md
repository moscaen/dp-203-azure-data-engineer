# Exam #8

    # of Correct Answers/Total Questions: 39/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 84.62 % (11/13)
    Design and develop data processing 95 % (19/20)
    Design and implement data security 77.78 % (7/9)
    Monitor and optimize data storage and data processing 66.67 % (2/3)

## Design and implement data storage

### Design a data storage structure

- Cool storage is ideal for short-term storage of data while gathering more data for processing and retention of dbs or datasets backups in the event of restoration within a defined period. It is also suitable for immediate retrieval of files when they re needed quickly.

### Implement physical data storage structures

- GRS or RA-GRS are always the right option when you need recovery after a widespread failure through a region
- ZRS protects against a zone failure, but not regional
- geo replication is not supported in Azure Data Lake Storage Gen2. What it does is to replicate a full SQL Server instance to another location

## Design and develop data processing

### Design and develop a stream processing solution

|           | Overlap | Filters period with no data | Check time duration between events | Events belong to single window | Fixed size lenght |
| --------- | ------- | --------------------------- | ---------------------------------- | ------------------------------ | ----------------- |
| Tumbling  | N       | N                           | N                                  | Y                              | Y                 |
| Hopping   | Y       | N                           | N                                  | N                              | Y                 |
| Windowing | Y       | Y                           | N                                  | N                              | Y                 |
| Session   | Y       | Y                           | Y                                  |                               | N                 |

## Design and implement data security

### Design Security for data policies and standards

- An account SAS allows access to one or more resources in a storage account. It also allows you to administer a storage account
- A service SAS allows access to a resource in on of the thress storage services:
  - table
  - blob
  - file
  It does not allow you administer storage account

## Monitor and optimize data storage and data processing

### Monitor data storage and data processing

- Blob capacity average metric shows the average blob size stored in Azure Data Lake Storage.
- BlobTier dimension separates the new files from already processed files because files are moved to the cool tier after HDInsights process them.
- File Capacity Average metric shows the average file size stored in a file share, but consolidated files are not saved in a file share.
- Used Capacity Average metric shows the average capacity for the Azure Storage Account.
- BlobType dimension shows the type of the stored blob (like block blob, page blob)
- ResponseType filters the response that is sent back to the client when a request to Cosmos DB is made. The response contains a status code and a detailed description
