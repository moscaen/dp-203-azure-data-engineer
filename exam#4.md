# Exam #4

    # of Correct Answers/Total Questions: 32/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 86.67 % (13/15)
    Design and develop data processing 78.57 % (11/14)
    Design and implement data security 42.86 % (6/14)
    Monitor and optimize data storage and data processing 100 % (2/2)

## Design and implement data storage

### Design a partition strategy

- Cosmos DB supports 5 consitency levels, from strongest consistency to most relaxed:
  - Strong
  - Bounded staleness
  - Session
  - Consistent prefix
  - Eventual
    Eventual consistency does not provide any ordering guarantee on reads, but it provides the highest troughput
- When partitioning, only partition by attributes that have multiple values

## Design and develop data processing

### Ingest and transform data

- The filter transformation is used to filter out rows based on specific criteria. It returns a single output stream which matches the filter condition
- The conditional split transformation in mapping data flow allows you to configure conditions for each output stream. You can define multiple output streams using different conditions

### Design and develop a stream processing solution

- IoT Hub is a good choice for real-time processing of a large amount of data and it supports Azure Blob Sstorage
- Azure Events Hub is a good choice for real-time processing of a large amount of data and it supports Azure SQL Databases. It does NOT support Azure CosmosDb

## Design and implement data security

### Design security for data policies and standars

|                                       | RBAC | Authorization method |
| ------------------------------------- | ---- | -------------------- |
| Access Key                            | N    | N                    |
| Shared Key                            | N    | Y                    |
| Account shared access signature (SAS) | N    | Y                    |
| Account Active Directory (AD)         | Y    | Y                    |

|                                                                     | RBAC | Uses the storage account key | Set Expiration |
| ------------------------------------------------------------------- | ---- | ---------------------------- | -------------- |
| User Delegation Key & User Delegation Shared Access Signature (SAS) | Y    | N                            | Y              |
| User Delegation Key & Account Level Shared Access Signature (SAS)   | N    | Y                            | Y              |
| User Delegation Key & shared key access                             | N    | Y                            | N              |

- To implement Row-Level Security

```SQL
CREATE SECURITY POLICY SalesFilter
ADD FILTER PREDICATE Security.tvf_securitypredicate(SalesRep)
ON Sales.Orders
WITH (STATE = ON);
GO
```

- implement RLS (row-level-security) when you want to define a securty policy to determine when a row should be retrieved. The filter predicate, filters rows that are retrieved during SELECT, UPDATE and DELETE statements. A filter predicate is simply a function associated with a security policy
- TDE encrypts the entire database (including data files and transaction logs). It does not help when you want to choose which rows are returned for specific users.
- Always Encrypted allows you to encrypt and decrypt column-level data at the client. The data remains encrypted in the db at the server. It does not help when you want to choose which rows are returned for specific users.
- CLE uses symmetric encryption to encrypt data directly in the db. It does not help when you want to choose which rows are returned for specific users.
