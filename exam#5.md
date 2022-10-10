# Exam #5

    # of Correct Answers/Total Questions: 32/45
    
    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 60 % (9/15)
    Design and develop data processing 66.67 % (8/12)
    Design and implement data security 100 % (11/11)
    Monitor and optimize data storage and data processing 57.14 % (4/7)

## Design and implement data storage

### Design a data storage structure

- Hot tier - An online tier optimized for storing data that is accessed or modified frequently. The Hot tier has the highest storage costs, but the lowest access costs.
- Cool tier - An online tier optimized for storing data that is infrequently accessed or modified. Data in the Cool tier should be stored for a minimum of 30 days. The Cool tier has lower storage costs and higher access costs compared to the Hot tier.
- Archive tier - An offline tier optimized for storing data that is rarely accessed, and that has flexible latency requirements, on the order of hours. Data in the Archive tier should be stored for a minimum of 180 days.

### Design the serving layer

- columnstore index has better performance than clustered index (Columnstore indexes are the standard for storing and querying large data warehousing fact tables.)

### Implement physical data storage structures

- always use REPLICATE if the table is small over round robin:
- Replicated tables improve performance over round-robin tables because they eliminate the need for data movement. A round-robin table always requires data movement for joins.
- A round-robin distributed table distributes data evenly across the table but without any further optimization. It is quick to load data into a round-robin but joins on round-robin tables require reshuffling data, which takes additional time.
- A hash distributed table can deliver the highest query performance for joins and aggregations on large tables. Hash distribution shares data across compute nodes by placing all data that uses the same hash key on the same compute node.

### Implement logical data structures

- The CREATE EXTERNAL DATA SOURCE statement creates the data soruce from whcih to import data. You must set the TYPE parameter to HADOOP when accessing data from Azure Blob Storage.

### Implement the serving layer

- to prevent metadata during the Copy activity copy from:
  - Amazon S3 bucket to Azure Data Lake Storage Gen2
  - Azure File Storage to Azure Blob Storage
  - Azure Data Lake Storage Gen2 to Azure File Storage

## Design and develop data processing

### Design and develop a stream processing solution

- To collect data from IoT sensors and send them to Azure Event Hub and visualize them in PowerBi you should:
  - Send application data to an Azure Event Hub instance
  - Ingress data from the Azure Event Hub instance
  - Build queries that output to Power Bi

### Manage batches and pipelines

- when setting the Recurrence in Azure Synapse Analytics solution and using Tumbling window as trigger, the UTC is implicit

## Monitor and optimize data storage and data processing

### Monitor data storage and data processing

- in Cosmos DB:
  - Total Request Units by container indicates how many Request unit are consumed. An alert can be configured to be triggered when the RU consumption is above 80%.

- in Azure Monitor:
  - Signal name: DWU (Data Warehouse Unit) Used is a unit performance that combines CPU, memory and IO
  - Signal type: Metrics. It has a numeric value and activates the alert when the resource usage is above 80%
  - Resource: server_name/resource_name (i.e. sqlserver1/synapsepool1)

- when a Stream analytics job stops and oyu want to configure an alert to monitor the issue you should use the:
  - All Administrative operations signal logic log which reports when a job enter any failed state (i.e. when a job stop unexecpectedely)
  - Runtime Errors produces an alert when the job can receive the data but generates error (i.e. 401)
