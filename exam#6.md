# Exam #6

    # of Correct Answers/Total Questions: 33/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 71.43 % (10/14)
    Design and develop data processing 94.12 % (16/17)
    Design and implement data security 60 % (6/10)
    Monitor and optimize data storage and data processing 25 % (1/4)

## Design and implement data storage

### Design a data storage structure

- Geo Replication is used for only Azure SQL Database
- Failover groups is applicable to both Azure SQL DB and Azure SQL DB managed instance

### Design the serving layer

- Ultra SSD Managed Disk of 128GB can support up to 38400 IOPS

### Implement physical data storage structures

- with GRS data remains available even if an entire datacenter becomes unavailable of if there is a widespread regional faiulure.

### Implement the serving layer

- to prevent metadata during the Copy activity copy from:
  - Amazon S3 bucket to Azure Data Lake Storage Gen2
  - Azure File Storage to Azure Blob Storage
  - Azure Data Lake Storage Gen2 to Azure File Storage

## Design and implement data security

### Design security for data policies and standards

|                                       | Administer storage Account in blob storage account |
| ------------------------------------- | -------------------------------------------------- |
| Service Shared access signature (SAS) | N                                                  |
| Account Shared access signature (SAS) | Y                                                  |
| Primary Access Key                    | Y                                                  |
| Secondary Access Key                  | Y                                                  |

|                                  | Azure Blob | Azure Files (SMB) | Azure Files (REST) | Azure    Queries | Azure Tables |
| -------------------------------- | ---------- | ----------------- | ------------------ | ---------------- | ------------ |
| Shared Access Signature (SAS)    |            | N                 |                    |                  |              |
| Azure Active Directory (AD)      |            |                   | N                  |                  | N            |
| Shared Key (storage account key) | Y          | Y                 | Y                  | Y                | Y            |
| Anonymous public read acccess    |            |                   |                    |                  |              |

## Monitor and optimize data storage and data processing

### Monitor data storage and data processing

- to see the list of the last 100 users login in to Azure Synaps Analtics, gran the VIEW DATABASE STATE permission to a user

- Blob capacity average metric shows the average blob size stored in Azure Data Lake Storage.
- BlobTier dimension separates the new files from already processed files because files are moved to the cool tier after HDInsights process them.
- File Capacity Average metric shows the average file size stored in a file share, but consolidated files are not saved in a file share.
- Used Capacity Average metric shows the average capacity for the Azure Storage Account.
- BlobType dimension shows the type of the stored blob (like block blob, page blob)
- ResponseType filters the response that is sent back to the client when a request to Cosmos DB is made. The response contains a status code and a detailed description
