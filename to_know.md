# To Know

- [Storage redundancy](https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy) (
  - GRS
  - LRS
  - ZRS
  - GRZRS
  - RAGRS
  )
- [Encrypting](https://learn.microsoft.com/en-us/azure/security/fundamentals/encryption-overview) (
  - [TDE](https://learn.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver16)
  - [Always Encrypt](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)
  - [RLS](https://learn.microsoft.com/en-us/sql/relational-databases/security/row-level-security?view=sql-server-2017)
  - [Dynamic Mask](https://learn.microsoft.com/en-us/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017))
- [Windowing](https://learn.microsoft.com/en-us/stream-analytics-query/windowing-azure-stream-analytics)(
  - [Session](https://learn.microsoft.com/en-us/stream-analytics-query/session-window-azure-stream-analytics)
  - [Hooping](https://learn.microsoft.com/en-us/stream-analytics-query/hopping-window-azure-stream-analytics)
  - [Tumbling](https://learn.microsoft.com/en-us/stream-analytics-query/tumbling-window-azure-stream-analytics)
  - [Sliding](https://learn.microsoft.com/en-us/stream-analytics-query/sliding-window-azure-stream-analytics)
  )
- Security (
  - [Azure AD](https://learn.microsoft.com/en-us/rest/api/storageservices/authorize-with-azure-active-directory)
  - [Shared Keys](https://learn.microsoft.com/en-us/rest/api/storageservices/authorize-with-shared-key)
  - [Storage Key](example.com)
  - [SAS](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json)
    - [User delegation](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#user-delegation-sas)
    - [Service](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#service-sas)
    - [Account](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#service-sas)
  )
- [Blob tier Capacity](https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview)(
  - hot
  - cold
  - premium
  - archive
)
- Monitoring (
  - <https://learn.microsoft.com/en-us/training/modules/operationalize-azure-data-factory-pipelines/5-monitor>
  - <https://learn.microsoft.com/en-us/azure/data-factory/monitor-using-azure-monitor>
  - <https://learn.microsoft.com/en-us/azure/cosmos-db/sql/time-to-live>
  - <https://learn.microsoft.com/en-us/azure/stream-analytics/stream-analytics-set-up-alerts>
  - <https://learn.microsoft.com/en-us/azure/stream-analytics/stream-analytics-monitoring>
  )
- Data storage partition (Hash, round-robin, replicated, partitioned)
- Data Schema (
  - [star](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema)
  - snowflake
  )
- [Normalization](https://learn.microsoft.com/en-us/office/troubleshoot/access/database-normalization-description)

## General notions

- only DDL operations are allowed on external tables (CREATE, DROP)
  - If you want to add a column to an external table you need to drop EXTERNAL TABLE and CREATE EXTERNAL TABLE

- Hash-distributed tables improve query performance on large fact tables.
    To balance the parallel processing, select a distribution column that:
  - Has many unique values. The column can have duplicate values. All rows with the same value are assigned to the same distribution. Since there are 60 distributions, some distributions can have > 1 unique values while others may end with zero values.
  - Does not have NULLs, or has only a few NULLs.
  - Is not a date column.

- Materialized views for dedicated SQL pools in Azure Synapse provide a low maintenance method for complex analytical queries to get fast performance without any query change.

- If you store your data as many small files, this can negatively affect performance. In general, organize your data into larger sized files for better performance (256 MB to 100 GB in size).
  Increasing file size can also reduce transaction costs. Read and write operations are billed in 4-megabyte increments so you're charged for operation whether or not the file contains 4 megabytes or not.

  - For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed. Before partitions are created, dedicated SQL pool already divides each table into 60 distributed databases.

- If a blob is moved to the Archive tier and then deleted or moved to the Hot tier after n < 180 days, you'll be charged an early deletion fee equivalent to 180 - n days of storing that blob in the Archive tier.

- in Azure Synapse Analytics dedicated SQL Pool to overwrite the data of a partitioned fact table with data from a staging table you can use the SWITCH syntax:
     SWITCH [ PARTITION source_partition_number_expression ] TO [ schema_name. ] target_table [ PARTITION target_partition_number_expression ]

- Type2 SCD:
  - MUST HAVE a surrogate key to provide a unique reference to a version of the dimension member.
    - The surrogate key identifies one unique row in the database
    - The business key identifies one unique entity of the modeled world
  - MUST HAVE a date range validity of the version (for example, StartDate and EndDate)  
  - POSSIBLY a flag column (for example, IsCurrent) to easily filter by current dimension members.

- When migrating a db that uses a third normal form schema to a star schema db to optmize read operations:
  - denormalize the db to second normal form
  - use a new identity columns as primary key
- When creating partitions on clustered columnstore tables, it is important to consider how many rows belong to each partition. For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed. Before partitions are created, dedicated SQL pool already divides each table into 60 distributions.

- You can create external tables in Synapse SQL pools via the following steps:

  1. CREATE EXTERNAL DATA SOURCE to reference an external Azure storage and specify the credential that should be used to access the storage.
  2. CREATE EXTERNAL FILE FORMAT to describe format of CSV or Parquet files.
  3. CREATE EXTERNAL TABLE on top of the files placed on the data source with the same file format.

- PySpark function explode(e: Column) is used to explode or create array or map columns to rows.

- You can use Databricks Pools to Speed up your Data Pipelines and Scale Clusters Quickly.

- Serverless SQL pool in Azure Synapse Analytics supports only external and temporary tables.

- Columnstore indexes are the standard for storing and querying large data warehousing fact tables

- Polybase loads rows that are smaller than 1 MB.
- Polybase loads can not exceed more than 1M files. You may experience the following error: Operation failed as split count exceeding upper bound of 1000000.
- [common query pattern](https://learn.microsoft.com/en-us/azure/stream-analytics/stream-analytics-stream-analytics-query-patterns)
- [sql-data-warehouse/cheat-sheet](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/cheat-sheet)

- To react to increased workloads and increase streaming units, consider setting an alert of 80% on the SU Utilization metric
  - the best practice is to start with 6 SUs for queries that don't use PARTITION BY.

- Data Factory and Synapse pipelines natively integrate with Azure Event Grid, to trigger pipelines based on events happening in storage account

- Azure Data Factory: Annotations are additional, informative tags that you can add to specific factory resources: pipelines, datasets, linked services, and triggers. By adding annotations, you can easily filter and search for specific factory resources.

- Conditional split transformation in mapping data flow:  
- <incomingStream>
    split(
        <conditionalExpression1>
        <conditionalExpression2>
        ...
        disjoint: {true | false}
    ) ~> <splitTx>@(stream1, stream2, ..., <defaultStream>)
   - disjoint false: only check the first condition
   - disjoint true: check all conditions

- CREATE PARTITION FUNCTION (Transact-SQL):
  CREATE PARTITION FUNCTION partition_function_name ( input_parameter_type )  
  AS RANGE [ LEFT | RIGHT ]
  FOR VALUES ( [ boundary_value [ ,...n ] ] )
  [ ; ]  
  - LEFT | RIGHT
      Specifies to which side of each boundary value interval, left or right, the boundary_value [ ,...n ] belongs, when interval values are sorted by the Database Engine in ascending order from left to right. If not specified, LEFT is the default.
    - Range LEFT : datecol<=20100101,  datecol>20100101    and datecol<=20110101,  datecol>20110101    and datecol<=20120101,  datecol>20120101

    - RANGE RIGHT : datecol<20100101,   datecol>=20100101   and datecol<20110101,   datecol>=20110101   and datecol<20120101,   datecol>=20120101

- Load data into Azure Synapse
  - 1) mount the data onto DBFS
  - 2) Read the file into a data frame
  - 3) Perform transformations on the file
  - 4) Specify a temporary folder to stage the data
  - 5) Write the results to a table in Azure synapse

- Azure integration runtime:
  If you have strict data compliance requirements and need to ensure that data do not leave a certain geography, you can explicitly create an Azure IR in a certain region and point the Linked Service to this IR using the ConnectVia property.

- For clusters running Databricks Runtime 6.4 and above, optimized autoscaling is used by all-purpose clusters in the Premium plan

- Databricks:
  - A Standard cluster is recommended for single users only
  - Standard and Single Node clusters terminate automatically after 120 minutes by default.
  - High Concurrency clusters do not terminate automatically by default.
    - Can't run Scala.
- In case of pipeline failures, tumbling window trigger can retry the execution of the referenced pipeline automatically, using the same input parameters, without the user intervention. This can  
  be specified using the property "retryPolicy" in the trigger definition.

- In scenarios where the trigger shouldn't proceed to the next window until the preceding window is successfully completed, build a self-dependency.  offset: "-01:00:00" size: "01:00:00"

- Use the derived column transformation to generate new columns in your data flow or to modify existing fields.
- If you need to transform data in a way that is not supported by Data Factory, you can create a custom activity
- Mapping data flows are visually designed data transformations in Azure Data Factory. Data flows allow data engineers to develop data transformation logic without writing code.

- Event Hubs partition: "Maximize the throughput of ingesting Twitter feeds from Event Hubs to Azure Storage without purchasing additional throughput or capacity units."
- A self-hosted IR is capable of running copy activity between a cloud data stores and a data store in private network.
- Azure Storage lifecycle management offers a rule-based policy that you can use to transition blob data to the appropriate access tiers or to expire data at the end of the data lifecycle.
- To react to increased workloads and increase streaming units, consider setting an alert of 80% on the SU Utilization metric. Also, you can use watermark delay and backlogged events metrics to see if there is an impact.
- Use Synapse Studio to monitor your Apache Spark applications. To monitor running Apache Spark application Open Monitor, then select Apache Spark applications. To view the details about the Apache Spark applications that are running, select the submitting Apache Spark application and view the details.

- You can implement column-level security with the GRANT T-SQL statement. With this mechanism, both SQL and Azure Active Directory (Azure AD) authentication are supported.
- Implement RLS by using the CREATE SECURITY POLICY Transact-SQL statement, and predicates created as inline table-valued functions.
- Managed Identity authentication is required when your storage account is attached to a VNet.
- Azure AD authentication uses contained database users to authenticate identities at the database level.
- Azure Databricks retains cluster configuration information for up to 70 all-purpose clusters terminated in the last 30 days and up to 30 job clusters recently terminated by the job scheduler. To keep an all-purpose cluster configuration even after it has been terminated for more than 30 days, an administrator can pin a cluster to the cluster list.
- Backlogged Input Events: Number of input events that are backlogged. A non-zero value for this metric implies that your job isn't able to keep up with the number of incoming events. If this value is slowly increasing or consistently non-zero, you should scale out your job. You should increase the Streaming Units.
- Because geo-replication is asynchronous, it is possible that data written to the primary region has not yet been written to the secondary region at the time an outage occurs. The Last Sync Time property indicates the last time that data from the primary region was written successfully to the secondary region
- Polybase: When a column or data type mismatch happens, this error could be seen in SSMS.
  Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "(null)". Query aborted- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 1 rows processed.
- Microsoft recommends use of sys.dm_pdw_nodes_db_partition_stats to analyze any skewness in the data.
- One method of identifying data skew is to use DBCC PDW_SHOWSPACEUSED()
- Use sys.pdw_nodes_column_store_row_groups to determine which row groups have a high percentage of deleted rows and should be rebuilt.
- Data Factory stores pipeline-run data for only 45 days. Use Azure Monitor if you want to keep that data for a longer time.
