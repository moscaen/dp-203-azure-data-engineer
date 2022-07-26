# Components summary

- [Components summary](#components-summary)
  - [Azure Data Factory](#azure-data-factory)
  - [Azure Synapse Analytics](#azure-synapse-analytics)
    - [Azure Synapse SQL](#azure-synapse-sql)
    - [Apache Spark in Azure Synapse Analytics](#apache-spark-in-azure-synapse-analytics)
    - [Azure Synapse shared metadata](#azure-synapse-shared-metadata)
    - [Azure Synapse pipelines](#azure-synapse-pipelines)
    - [Azure Synapse Link](#azure-synapse-link)
    - [Azure Synapse Studio](#azure-synapse-studio)
    - [Building a modern data warehouse](#building-a-modern-data-warehouse)
    - [Ingestion in modern data warehouse](#ingestion-in-modern-data-warehouse)
    - [Prepare and transform data with Azure Synapse Analytics](#prepare-and-transform-data-with-azure-synapse-analytics)
    - [Tables in a data warehouse](#tables-in-a-data-warehouse)
    - [Design a data warehouse schema](#design-a-data-warehouse-schema)
    - [Table distribution design](#table-distribution-design)
    - [Optimize query performance](#optimize-query-performance)
    - [Azure Synapse Apache Spark to Synapse SQL connector](#azure-synapse-apache-spark-to-synapse-sql-connector)
    - [Windowing functions](#windowing-functions)
    - [Stored procedures](#stored-procedures)
    - [Manage and monitor data warehouse activities](#manage-and-monitor-data-warehouse-activities)
    - [Analyze and optimize data warehouse storage in Azure Synapse Analytics](#analyze-and-optimize-data-warehouse-storage-in-azure-synapse-analytics)
    - [Secure a data warehouse in Azure Synapse Analytics](#secure-a-data-warehouse-in-azure-synapse-analytics)
      - [Authentication in Azure Synapse Analytics](#authentication-in-azure-synapse-analytics)

## Azure Data Factory
>
> - Azure Data Factory is the cloud-based ETL and data integration service that allows you to create data-driven workflows for orchestrating data movement and transforming data at scale
>
> - To create data factories you need the following permission depending on the resource you want to create
>
> | Data Factory Instances | Data Factory Objects |
> | :-- | :-- |
> |Contributor <br> Owner <br> Administrator| Data Factory Contributor (Azure Portal) <br> Contributor (Powershell or SDK)|
>
> - To create Data Factory **instances**, the user account that you use to sign in to Azure must be a member of the **contributor** or **owner role**, or an **administrator** of the Azure subscription.
>
> - ![data-factory-processi](https://docs.microsoft.com/en-us/learn/wwl-data-ai/data-integration-azure-data-factory/media/data-driven-workflow.png)
>

```mermaid
graph LR;
    id1(Connect and collect)-->id2(Transform and enrich) --> id3(Publish) --> id4(Monitor) ;
```
>
> - Azure Data Factory is composed of four core components
> ![four-core-components](https://docs.microsoft.com/en-us/learn/wwl-data-ai/data-integration-azure-data-factory/media/data-factory-components.png)
> - Linked Services enables you to define data sources, or compute resource that is required to ingest and prepare data
> - Datasets represent data structures within the data store that is being referenced by the Linked Service object
> - Activities typically contain the transformation logic or the analysis commands of the Azure Data Factory’s work. Multiple activities can be logically grouped together with an object referred to as a **Pipeline**
> >
> > - there are three categories
> > >
> > > - Data movement activities:  simply move data from one data store to another
> > > - Data transformation activities
> > > - Control activities
> > >
> - Control flow is an orchestration of pipeline activities
> >
> > - **Chaining activities**: chain activities in a sequence within a pipeline. It is possible to use the dependsOn property.
> > - **Branching activities**: evaluates a set of activities and execute them when the condition is true. Else an alternate set of activities is executed
> > - **Parameters**: You can define parameters at the pipeline level and pass arguments while you're invoking the pipeline on-demand or from a trigger. Activities then consume the arguments held in a parameter as they are passed to the pipeline.
>>>
>>> - A use-case for this scenario is connecting to several different databases that are on the same SQL server, in which you might think about parameterizing the database name in the linked service definition. The benefit of doing this is that you don't have to create a single linked service for each database that is on the same SQL Server.
>>> - Setting **global parameters** in an Azure Data Factory pipeline allows you to use these constants for consumption in pipeline expressions. A use-case for setting global parameters is when you have multiple pipelines where the parameters names and values are identical.
>>>
>> - **Custom state passing**: is an activity that created output or the state of the activity that needs to be consumed by a subsequent activity in the pipeline. An example is that in a JSON definition of an activity, you can access the output of the previous activity. Using custom state passing enables you to build workflows where values are passing through activities.
>> - **Looping containers**: defines repetition in a pipeline. It enables you to iterate over a collection and runs specified activities in the defined loop.(i.e. Until activity, for each activity)
> > >
> > > - If a For Each loop is used as a control flow activity, Azure Data Factory can start multiple activities in parallel.
> > >
>> - **Trigger-based flows**: Pipelines can be triggered by on-demand (event-based, for example, blob post) or wall-clock time.
>> - **Invoke a pipeline from another pipeline**: The Execute Pipeline activity with Azure Data Factory allows a Data Factory pipeline to invoke another pipeline.
>> - **Delta flows**: Use-cases related to using delta flows are delta loads. Delta loads in ETL patterns will only load data that has changed since a previous iteration of a pipeline. Capabilities such as lookup activity, and flexible scheduling helps handling delta load jobs. In the case of using a Lookup activity, it will read or look up a record or table name value from any external source. This output can further be referenced by succeeding activities.
>> - **Web activity**:  can call a custom RESTendpoint from a Data Factory pipeline. Datasets and linked services can be passed in order to get consumed by the activity.
>> - **Get metadata activity**: The Get metadata activity retrieves the metadata of any data in Azure Data Factory.
>>
> - Integration Runtime (IR) is the compute infrastructure used by Azure Data Factory. It provides the bridge between the activity and linked services.
> - An Azure integration runtime is capable of:
>>
>> - Running Data Flows in **Azure**
>> - Running Copy Activity between **cloud data stores**
>> - Dispatching the following transform activities in **public network**: MapReduce activity, HDInsight, Spark activity
> It provides several ways of moving data such as:
>
> |IR type |Public network | Private network|
>   | --- | --- | --- |
> | Azure | Data Flow Data movement <br>Activity dispatch||
> |Self-hosted | Data movement Activity dispatch | Data movement Activity dispatch|
> |Azure-SSIS |SSIS package execution |SSIS package execution|
>
> |Self-hosted |  | |
> | -- | -- | -- |
> |Running copy activity between a cloud data store and a data store in the private network. |  |
> | Dispatching the following transform activities against compute resources in on-premises or Azure Virtual Network: <br> HDInsight Hive activity (BYOC-Bring Your Own Cluster) <br> HDInsight Pig activity (BYOC) <br> HDInsight MapReduce activity (BYOC) <br> HDInsight Spark activity (BYOC) <br> HDInsight Streaming activity (BYOC) <br> Machine Learning Batch Execution activity <br> Machine Learning Update Resource activities <br> Stored Procedure activity <br> Data Lake Analytics U-SQL activity <br> Custom activity (runs on Azure Batch) <br> Lookup activity <br> Get Metadata activity. | |
>
> ### To ingest data with Data Factory use
>
> | Copy Activity | Compute Resources | SSIS Packages |
> | --- |       --- |           --- |
> | code-free data ingestion <br> no transformation needed during extraction    |call on compute resources to process data by a data platform service that may be better suited to the job such as <br> Azure Machine Learning <br> Azure Data Lake Analytics <br> Azure SQL, Azure SQL Data Warehouse, SQL Server <br> Azure Databricks <br> Azure Function <br> |           Lift and shift existing SSIS workload by creating Azure-SSIS Integration Runtime    |
>
> ### To transform data with Data Factory use
> >
> |Mapping Data Flow | Compute Resources | SSIS Packages |
> | ---| ---| --- |
> | code free data transformation <br>  executed on scaled-out Apache Spark clusters that are automatically provisioned <br> enable debug mode, to turns on a small interactive Spark cluster| call on compute resources to process data by a data platform service that may be better suited to the job such as <br> Azure Machine Learning <br> Azure Data Lake Analytics <br> Azure SQL, Azure SQL Data Warehouse, SQL Server <br> Azure Databricks <br> Azure Function <br> |           Lift and shift existing SSIS workload by creating Azure-SSIS Integration Runtime    |
>
> Mapping Data Flows provides a number of different [transformations](https://docs.microsoft.com/en-us/azure/data-factory/data-flow-transformation-overview) types that enable you to modify data. They are broken down into the following categories:
>
>|Category Name | Description |
>| --- | --- |
>|Schema modifier transformations |Modifies a sink destination by creating new columns based on the action of the transformation (i.e. Derived Column transformation) |
>|Row modifier transformations | Impact how the rows are presented in the destination. (i.e. Sort) |
>|Multiple inputs/outputs transformations | Generate new data pipelines or merge pipelines into one. (i.e. Union transformation that combines multiple data streams) |

## Azure Synapse Analytics

> Azure Synapse Analytics is an integrated analytics platform, which combines data warehousing, big data analytics, data integration, and visualization into a single environment.
> ![azure-synapse](https://docs.microsoft.com/en-us/learn/wwl-data-ai/introduction-azure-synapse-analytics/media/synapse-overview.png)
>
> Every Synapse workspace has a primary ADLS Gen2 account associated with it. This serves as the data lake.
>
### Azure Synapse SQL

> Azure Synapse SQL enables you to implement data warehouse solutions, or to perform data virtualization
>
> - A data warehouse is a core component of Business Intelligence (BI) solutions that provides a central repository of data stored in relational tables\
Data in a data warehouse is stored in permanent tables that are populated using an extract, transform, and load (ETL) hence you need to understand the data that is stored in the sources systems)
>
> - Data virtualization allows you to interact with data without the need to understand how the data is formatted, structured, or what is its data type\
It enables you to explore the data without understanding the technical specifications of the source data\
> - Azure Synapse SQL offers both a dedicated and serverless model of the service to meet the different demands of both solutions.
>
> | Dedicated Model | Serverless Model|
> | --- |   --- |
> |Dedicated SQL pools represent a collection of analytic resources that are being provisioned when using Synapse SQL <br>When you need predictable performance and cost, creating dedicated SQL pools to reserve processing power for data permanently stored in SQL tables (data warehouse)| deal for unplanned or ad hoc workloads that the diagnostic analytics approach would generate. (data exploration)|

### Apache Spark in Azure Synapse Analytics

> Spark pools in Azure Synapse Analytics offer a fully managed Spark service
>
> The primary use case for Apache Spark for Azure Synapse Analytics is to process big data workloads that cannot be handled by Azure Synapse SQL, and where you don’t have an existing Apache Spark implementation
> If you already have a Spark implementation in place already, Azure Synapse Analytics can also integrate with other Spark implementations such as Azure Databricks, so you don’t have to use the feature in Azure Synapse Analytics if you already have a Spark setup already.

### Azure Synapse shared metadata

> Allows the different engines to share the databases and tables between Spark pools and SQL on-demand engine

### Azure Synapse pipelines

> Azure Synapse Pipelines is the cloud-based ETL and data integration service that allows you to create data-driven workflows for orchestrating data movement and transforming data at scale\
Like Azure Data Factory, Azure Synapse Pipelines is composed of four core components

### Azure Synapse Link

> Hybrid Transactional and Analytical Processing enables businesses to perform analytics over a database system that is seen to provide transactional capabilities without impacting the performance of the system
> When enabling Azure Synapse Link for Cosmos DB and the analytical store on Azure Cosmos DB containers all transactional data is automatically stored in a fully isolated column store.\
This store enables large-scale analytics against the operational data in Azure Cosmos DB, without impacting the transactional workloads or incurring resource unit (RU) costs
> By combining the distributed scale of Cosmos DB's transactional processing with the built-in analytical store and the computing power of Azure Synapse Analytics, **Azure Synapse Link enables a Hybrid Transactional/Analytical Processing (HTAP)** architecture and eliminates ETL processes. This enables business analysts, data engineers & data scientists to self-serve and run near real-time BI, analytics, and Machine Learning pipelines over operational data
>
> - Azure Synapse Link **is not enabled by default** on Cosmo Db
> - Enabling Azure Synapse Link feature **after** the container is created **does not allow** to enable the analytical store on the container
> - The partition key value should be set on a field that **is used most often in queries** and contains a relatively**high cardinality** (number of unique values) for good partitioning performance.

### Azure Synapse Studio

> Azure Synapse Studio is the primary tool to use to interact with the many components that exist in the service.\
> It organizes itself into hubs which allows you to perform a wide range of activities against your data.
>
> | Home | Data | Develop | Integrate | Monitor | Manage |
> | --- | --- | --- | --- | ---| --- |
> | contains short cuts that enable you to ingest, explore, analyze, and visualize your data. <br> Copy Data Tool for ingesting data <br> tool to connecting to a Power BI workspace for visualization| access your provisioned SQL pool databases and SQL serverless databases in your workspace, as well as external data sources, such as storage accounts and other linked services. <br> You also can preview data tables and data files| manage SQL scripts, Synapse notebooks, data flows, and Power BI report| Manage data integration pipelines within the Integrate hub. <br> it the same as Azure Data Factory pipeline creation| view pipeline and trigger runs, view the status of the various integration runtimes that are running, view Apache Spark jobs, SQL requests, and data flow debug activities| managing SQL and Spark pools <br> such as managing Linked Services and integration runtimes, and creating pipeline triggers |

### Building a modern data warehouse

> The process of building a modern data warehouse typically consists of:
> ![modern-data-warehouse](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-azure-synapse-studio/media/modern-data-warehouses-process-with-synapse.png)
>
> - Data Ingestion and Preparation.
>   >
> > - customers build a data lake to store all their data and different data types with Azure Data Lake Store Gen2.
> > >
> > > - When data is stored in Data Lake Storage Gen2, the file size, number of files, and folder structure have an impact on performance.
> > > - If you store your data as many small files, this can negatively affect performance. In general, organize your data into larger-sized files for better performance (256 MB to 100 GB in size).
> > > - Some engines and applications might have trouble efficiently processing files that are greater than 100GB in size.
> > > - Sometimes, data pipelines have limited control over the raw data, which has lots of small files. It is recommended to have a "cooking" process that generates larger files to use for downstream applications.
> > >
> > - Azure Data Factory is used for ingestion and Azure Databricks for preparation
> > - The primary component of Azure Synapse Analytics to ingest data is to use the Copy Data activity within Azure Synapse Pipelines
> > - It is  typical to store the source data within a staging area, which is also referred to as a **landing zone**. This typically is a neutral storage area that sits between the source systems and the data warehouse.
> > >
> > > - To reduce contention on source systems
> > > - Enables you to deal with the ingestion of source systems on different schedules
> > > - To join data together from different source systems
> > > - To rerun failed data warehouse loads from a staging area
> > >
>  
> - Making the data ready for consumption by analytical tools.
> >
> > - Azure Synapse Analytics implements a data warehouse using a dedicated SQL pool that leverages the Massively Parallel Processing engine that brings together enterprise data warehousing and Big Data analytics
> >
> - Providing access to the data, in a shaped format so that it can easily be consumed by data visualization tools.
> >
> > - Power BI enables customers to build visualizations on massive amounts of data and ensure that data insights are available to everyone across their organization
> > - When you create a database in a dedicated SQL pool in Azure Synapse Analytics, the automatic creation of statistics is turned on by default. This means that statistics are created when you run the following type of Transact-SQL statements:
> > >
> > > - SELECT
> > > - INSERT-SELECT
> > > - CTAS
> > > - UPDATE
> > > - DELETE
> > > - EXPLAIN when containing a join or the presence of a predicate is detected
>
> - You can either use Azure Synapse exclusively, which works very well for green field projects, but for organizations with existing investments in Azure with Azure Data Factory, Azure Databricks and Power BI, you can take a hybrid approach and combine them with Azure Synapse Analytics

### Ingestion in modern data warehouse

> - **Batch**: Queries or programs that take tens of minutes, hours, or days to complete. Activities could include initial data wrangling, complete ETL pipeline, or preparation for downstream analytics.
> >
> > - Synapse supports natively: CSV Parquet ORC JSON
> >
> - **Interactive query**: Querying batch data at "human" interactive speeds, which with the current generation of technologies means results are ready in time frames measured in seconds to minutes.
> - **Real-time / near real-time**: Processing of a typically infinite stream of input data (stream), whose time until results ready is short—measured in milliseconds or seconds in the longest of cases.
> >
> > - In your data pipeline, you need to collect messages from Azure IoT Hub, Azure Event Hubs or similar services, and process them using Azure Stream Analytics, Azure Functions, Azure Databricks, or other services that enable you to ingest and process streaming data.
> >
> - One of the most common patterns for loading a data warehouse is to transfer data from source systems to files in a data lake, ingest the file data into staging tables, and then use SQL statements to load the data from the staging tables into the dimension and fact tables.
> - When ingesting data using dedicated SQL Pools, instead of using the service administrator account, [create a specific account](https://docs.microsoft.com/en-us/learn/modules/use-data-loading-best-practices-azure-synapse-analytics/6-set-up-dedicated-accounts) assigned to different resource classes dependent on the anticipated task. This will optimize load performance and maintain concurrency as required by managing the available resource slots available within the dedicated SQL Pool
> Both options enable fast and scalable data load operations, but there are some differences between the two:
>
> | PolyBase |  COPY |
> | --- | ---|
> |Needs CONTROL permission |Relaxed permission|
>| Has row width limits |No row width limit|
>| No delimiters within text |Supports delimiters in text|
>| Fixed line delimiter |Supports custom column and row delimiters|
>| Complex to set up in code |Reduces amount of code|
> |supports custom column and row delimiters | The row delimiter in delimited-text files must be supported by Hadoop's LineRecordReader. That is, it must be either \r, \n, or \r\n. These delimiters are not user-configurable.|
> PolyBase requires the following elements:
>
> - An external data source that points to the abfss path in ADLS Gen2 where the Parquet files are located
> - An external file format for Parquet files
> - An external table that defines the schema for the files, as well as the location, data source, and file format
> Workload Isolation assigns maximum and minimum usage values for varying resources under load
>
### Prepare and transform data with Azure Synapse Analytics

> You can natively perform data transformations with Azure Synapse pipelines code free using the **Mapping Data Flow** task.

### Tables in a data warehouse

> A common pattern for relational data warehouses is to define a schema that includes two kinds of table: dimension tables and fact tables.\
>
> - **Dimension tables** **describe business entities**, such as products, people, places, and dates. Dimension tables contain columns for attributes of an entity.\
> In addition to dimension tables that represent business entities, it's common for a data warehouse to include a dimension table that represents time.\
> It's common for a dimension table to include two key columns:
>>
>> - a **surrogate key** that is **specific to the data warehouse** and uniquely identifies each row in the dimension table in the data warehouse - usually an incrementing integer number.
>> - An **alternate key**, is used to identify a specific instance of an entity in the transactional source system from which the entity record originated - such as a product code or a customer ID.\
>
> - Fact tables store details of observations or events; for example, sales orders, stock balances, exchange rates, or recorded temperatures.
>
### Design a data warehouse schema

> In most transactional databases that are used in business applications, the data is normalized to reduce duplication. In a data warehouse however, the dimension data is generally de-normalized to reduce the number of joins required to query the data.
>
> - a data warehouse is organized as a star schema, in which a fact table is directly related to the dimension tables ![star-schema](https://docs.microsoft.com/en-us/learn/wwl-data-ai/design-multidimensional-schema-to-optimize-analytical-workloads/media/star-schema.png)
> - when an entity has a large number of hierarchical attribute levels, or when some attributes can be shared by multiple dimensions, it can make sense to apply some normalization to the dimension tables and create a snowflake schema ![snowflake-schema](https://docs.microsoft.com/en-us/learn/wwl-data-ai/design-multidimensional-schema-to-optimize-analytical-workloads/media/snowflake-schema.png)
> - When designing a star schema model for small or medium sized datasets you can use your preferred database, such as Azure SQL. For larger data sets you may benefit from implementing your data warehouse in Azure Synapse Analytics instead of SQL Server
>>
>> - Dedicated SQL pools in Synapse Analytics don't support foreign key and unique constraints as found in other relational database systems like SQL Server.
>> - Synapse Analytics dedicated SQL pools support clustered indexes and the default index type is clustered columnstore. This index type offers a significant performance advantage when querying large quantities of data in a typical data warehouse schema and should be used where possible
>> - Azure Synapse Analytics dedicated SQL pools use a massively parallel processing (MPP) architecture.\
In an MPP system, the data in a table is distributed for processing across a pool of nodes. Synapse Analytics supports the following kinds of distribution:
>>
>>> - Hash: A deterministic hash value is calculated for the specified column and used to assign the row to a compute node. (If tables are too large to store on each compute node, use hash distribution.)
>>> - Round-robin: Rows are distributed evenly across all compute nodes. (for **staging tables** to **evenly distribute data** across compute nodes.)
>>> - Replicated: A copy of the table is stored on each compute node. (Use replicated distribution for smaller tables to avoid data shuffling when joining to distributed fact tables)

### Table distribution design

> There are three main table distributions available in Synapse Analytics SQL Pools
>
> | Round robin distribution |Hash distribution |Replicated Tables |
> | --- |--- |--- |
> |![rr](https://docs.microsoft.com/en-us/learn/wwl-data-ai/optimize-data-warehouse-query-performance-azure-synapse-analytics/media/round-robin.png)|![hash](https://docs.microsoft.com/en-us/learn/wwl-data-ai/optimize-data-warehouse-query-performance-azure-synapse-analytics/media/hash-distribution.png)|![replicated](https://docs.microsoft.com/en-us/learn/wwl-data-ai/optimize-data-warehouse-query-performance-azure-synapse-analytics/media/replicated.png)|
> |This is the default distribution created for a table and delivers fast performance when used for loading data.|This distribution can deliver the highest query performance for joins and aggregations on large tables.|A replicated table provides the fastest query performance for small tables which with compression should be less than 2GB as a starting point, static data can be larger.|
> |A round-robin distributed table distributes data evenly across the table but without any further optimization. A distribution is first chosen at random and then buffers of rows are assigned to distributions sequentially.|When choosing a hash column it's best to choose a column that will evenly distribute the data across all distributions that will allow for approximately the same number of rows within each distribution| Data modifications will cause the cached copy to be invalidated, and require the table be recached|
> |Joins on round-robin tables may negatively affect query workloads, as data that is gathered for processing then has to be reshuffled to other compute nodes, which take additional time and processing|Choose a column with a high number of unique values<br> Choose a column without Null values or very few null values <br> Don't choose a date column|Rebuilding frequently can cause slower performance (no requent insert, update, delete operations) <br> don't scale the SQL pool frequently as changing the nodes requires to rebuld the replicated tables
>
> When a table is created, by default the data structure has no indexes and is called a **heap**\
> Well-designed indexing strategy can reduce disk I/O operations and consume less system resources therefore improving query performance, especially when using filtering, scans, and joins in a query.
> **Dedicated SQL Pools create a clustered columnstore** index when no index options are specified on a table and allows for the use of adaptive caching.\
> Clustered columnstore indexes will generally outperform clustered rowstore indexes or heap tables and are usually the best choice for large table
> Columnstore works on segments of 1,048,576 rows that get compressed and optimized by column. To optimize segment quality, target at least 100K rows per compressed row group
> **When not to use Columnstore tables**:
>
> - Columnstore tables don't support varchar(max), nvarchar(max), and varbinary(max). Consider heap or clustered index instead.
> - Columnstore tables may be less efficient for transient data. Consider heap and perhaps even temporary tables.
> - Small tables with less than 60 million rows. Consider heap tables.

### Optimize query performance

> **Materialized views** are **prewritten queries with joins and filters whose definition is saved and the results persisted to a dedicated SQL pool**. They are not supported in by serverless SQL pools.\
> As the data in the underlying base tables change, the data in the materialized view will automatically update without user interaction\
> SQL Pools do support the use of transactions and adhere to the ACID (Atomicity, Consistency, Isolation, and Durability) transaction principles associated with relational database management systems\
> The isolation level of the transactional support is defaulted to **READ UNCOMMITTED**.
>
> - If you experience delays in the completion of queries, the **Read Committed Snapshot** Isolation level (enabling the READ_COMMITTED_SNAPSHOT on the db) should be employed to alleviate this. Read Committed Snapshot, makes a copy of the rows that are being referenced in a query if it is being updated, so that the data is consistent.
> Enable result-set caching when you expect results from queries to return the same values.
> - The capacity for the result set cache is 1 TB and the data within the result-set cache is expired and purged after 48 hours of not being accessed.
> - Azure Synapse SQL automatically caches query results in the user database for repetitive use
> - Column statistics improve query performance by keeping track of how much data exists between ranges in columns.\
> It tracks cardinality and range density to determine which data access paths return the fewest rows for speed

### Azure Synapse Apache Spark to Synapse SQL connector

> The Azure Synapse Apache Spark to Synapse SQL connector is designed to efficiently transfer data between serverless Apache Spark pools and dedicated SQL pools in Azure Synapse. At the moment, the Azure Synapse Apache Spark to Synapse SQL connector works on dedicated SQL pools only,**it doesn't work with serverless SQL pools**.\
> It uses the Azure Data Lake Storage Gen2 and PolyBase in SQL pools to efficiently transfer data between the Spark cluster and the Synapse SQL instance.
> ![spark-to-synpase-sql-architecture](https://docs.microsoft.com/en-us/learn/wwl-data-ai/integrate-sql-apache-spark-pools-azure-synapse-analytics/media/new-sql-spark-integration.png)
> The integration can be helpful in use cases where you perform an ETL process predominately using SQL but need to call on the computation power of Apache Spark to perform a portion of the extract, transform, and load (ETL) process as it is more efficient.
> The **authentication** between the two systems is made seamless in Azure Synapse Analytics. **The Token Service connects with Azure Active Directory** to obtain the security tokens to be used when accessing the storage account or the data warehouse in the dedicated SQL pool.\
> >
> > - The account user needs to be a member of the db_exporter role in the database or SQL pool from which you want to transfer data to or from.
> > - The account user needs to be a member of the Storage Blob Data Contributor role on the default storage account.
> For this reason, there's no need to create credentials or specify them in the connector API if Azure AD-Auth is configured at the storage account and the dedicated SQL pool.\
> If not, SQL Authentication can be specified. The only constraint is that this connector only works in Scala.\
>
> You can allow other users to use the Azure Synapse Apache Spark to Synapse SQL connector in your Azure Synapse workspace.
>
> - it is necessary to be a Storage Blob Data Owner in relation to the Azure Data Lake Gen 2 storage account that is connected to your workspace.
> - the user needs to have access to the Azure Synapse workspace
> - it's imperative that the user has permissions to run the notebooks.
>
> - set the right permissions by configuring Access Control Lists (ACLs) on the folder structure as follows.
>
> |Folder| / |synapse| workspaces| workspacename| sparkpools| sparkpoolname| sparkpoolinstances|
> |--|--|--|--|--|--|--|--|
> | Access Permissions|  --X |--X |--X |--X |--X |--X |-WX|
>
> #### Transfer data outside the synapse workspace using the PySpark connector
>
> You can transfer data to and from a dedicated SQL pool using a Pyspark Connector, which currently works with Scala.
>
> - write that DataFrame into the data warehouse:
>
> > - create a temporary table in a DataFrame in PySpark using the createOrReplaceTempView method\
> >
``` python
      pyspark_df.createOrReplaceTempView("pysparkdftemptable")
```
> >
> > - run a Scala cell in the PySpark notebook using magics
> >
```python
      %%spark
      val scala_df = spark.sqlContext.sql ("select * from pysparkdftemptable")
      scala_df.write.sqlanalytics("sqlpool.dbo.PySparkTable", Constants.INTERNAL)
```
>
> - to read data using the PySpark connector:
> >
> > - read the data using scala first
> > - then write it into a temporary table.
> > - Finally you use the Spark SQL in PySpark to query the temporary table into a DataFrame.

### Windowing functions

> A window function enables you to perform a mathematical equation on a set of data that is defined within a window. The mathematical equation is typically an aggregate function; however, instead of applying the aggregate function to all the rows in a table, it is applied to a set of rows that are defined by the window function, and then the aggregate is applied to it.
>
> - One of the key components of window functions is the OVER clause. This clause determines the partitioning and ordering of a rowset before the associated window function is applied. That is, the OVER clause defines a window or user-specified set of rows within a query result set.
> - Unlike aggregate functions, however, analytic functions can return multiple rows for each group. Use analytic functions to compute moving averages, running totals, percentages, or top-N results within a group.

### Stored procedures

> Azure Synapse SQL pools support placing complex data processing logic into Stored procedures. Stored procedures are great way of encapsulating one or more SQL statements or a reference to a Microsoft .NET framework Common Language Runtime (CLR) method.\
> In addition to encapsulating Transact-SQL logic, stored procedures also provide the following additional benefits:
>
> - Reduces client to server network traffic: \
> The commands in a procedure are executed as a single batch of code. This can significantly reduce network traffic between the server and client because only the call to execute the procedure is sent across the network.
>
> - Provides a security boundary: \
> The procedure controls what processes and activities are performed and protects the underlying database objects. This eliminates the requirement to grant permissions at the individual object level and simplifies the security layers.
> - Eases maintenance: \
> When client applications call procedures and keep database operations in the data tier, only the procedures must be updated for any changes in the underlying database.
> - Improved performance: \
> Stored procedures are compiled the first time they are executed, and the subsequent execution plan is held in the cache and reused on subsequent execution of the same stored procedure. As a result, it takes less time to process the procedure

### Manage and monitor data warehouse activities

> - You can scale a Synapse SQL pool either through the Azure portal, Azure Synapse Studio or programmatically using TSQL or PowerShell.  MODIFY is used to scale a dedicated SQL pool.
> - Apache Spark pools for Azure Synapse Analytics uses an Autoscale feature that automatically scales the number of nodes in a cluster instance up and down.\
> During the creation of a new Spark pool, a minimum and maximum number of nodes can be set when Autoscale is selected
> Pause compute in Azure Synapse Analytics to minimize your costs. In the Azure portal you can use the Pause command within the dedicated SQL pool
> Azure Synapse Analytics allows you to create, control and manage resource availability when workloads are competing. This allows you to manage the relative importance of each workload when waiting for available resources.
> Dedicated SQL pool workload management in Azure Synapse consists of three high-level concepts:
>
> - Workload Classification:
>       allows workload policies to be applied to requests through assigning resource classes and importance. The simplest and most common classification is load and query. You load data with insert, update, and delete statements
> - Workload Importance: Workload importance influences the order in which a request gets access to resources. There are five levels of importance: low, below_normal, normal, above_normal, and high
> - Workload Isolation: Workload isolation reserves resources for a workload group. Resources reserved in a workload group are held exclusively for that workload group to ensure execution. Workload groups also allow you to define the amount of resources that are assigned per request, much like resource classes do. Workload groups give you the ability to reserve or cap the amount of resources a set of requests can consume. Finally, workload groups are a mechanism to apply rules, such as query timeout, to requests.
> Azure Advisor recommendations are free, and the recommendations are based on telemetry data that is generated by Azure Synapse Analytics. The telemetry data that is captured by Azure Synapse Analytics include
> ![azure-advisor](https://docs.microsoft.com/en-us/learn/wwl-data-ai/manage-monitor-data-warehouse-activities-azure-synapse-analytics/media/advisor-dashboard-generation.png)
> - Data Skew and replicated table information.
> - Column statistics data.
> - TempDB utilization data.
> - Adaptive Cache.
> Dynamic Management Views provide a programmatic experience for monitoring the Azure Synapse Analytics SQL pool activity by using the Transact-SQL language. The views that are provided, not only enable you to troubleshoot and identify performance bottlenecks with the workloads working on your system, but they are also used by other services such as Azure Advisor to provide recommendations about Azure Synapse Analytics.
> - All logins to your data warehouse are logged to **sys.dm_pdw_exec_sessions**
> - All queries executed on SQL pool are logged to **sys.dm_pdw_exec_requests**

### Analyze and optimize data warehouse storage in Azure Synapse Analytics

> In simple terms, data skew is an over-represented value
> A quick way to check for data skew is to use DBCC PDW_SHOWSPACEUSED. The following SQL code returns the number of table rows that are stored in each of the 60 distributions. For balanced performance, the rows in your distributed table should be spread evenly across all the distributions.
> Dedicated SQL pools support the most commonly used data types. With that, it is essential to keep in mind that minimizing the size of data types shortens the row length, which leads to better query performance. Use the smallest data type that works for your data:
>
> - Avoid defining character columns with a large default length. For example, if the longest value is 25 characters, define your column as VARCHAR(25).
> - Avoid using NVARCHAR when you only need VARCHAR to support non-unicode data.
> - When possible, use NVARCHAR(4000) or VARCHAR(8000) instead of NVARCHAR(MAX) or VARCHAR(MAX).
> - Focus on using the minimal data type as possible. Use the minor data type that can reliably contain all possible values you have. For example, use tinyint instead of smallint or int for exact-numbers between 0-255. tinyint would be sufficient for a person's age because no one lives to be more than 255 years old.
> - Use DATETIME instead of storing data values in different formats such as string or numeric values. When it comes to querying your data based on date, an additional conversion will put a massive overhead on your queries.
> - If you only need to store a date and not the time make sure you use DATE instead of DATETIME. There is a 4 bytes difference between the two.
> - If you are using PolyBase external tables to load your SQL pool tables, the table row's defined length cannot exceed 1 MB. When a row with variable-length data exceeds 1 MB, you can load the row with BCP but not with PolyBase.
>
> SQL pool in Azure Synapse supports standard and [materialized views](https://docs.microsoft.com/en-us/learn/modules/analyze-optimize-data-warehouse-storage-azure-synapse-analytics/8-describe-impact-of-materialized-views). Both are virtual tables created with SELECT expressions and presented to queries as logical tables. Views encapsulate the complexity of common data computation and add an abstraction layer to computation changes, so there's no need to rewrite queries.
> |Comparison| View |Materialized view|
> |--- |--- | --- |
>|View definition | Stored in SQL pool. | Stored in SQL pool.|
>|View content | Generated each time when the view is used. |Pre-processed and stored in SQL pool during view creation. Updated as data is added to the underlying tables.|
>|Data refresh| Always updated| Always updated|
>|Speed to retrieve view data from complex queries| Slow| Fast|
>|Extra storage| No| Yes|
>|Syntax| CREATE VIEW| CREATE MATERIALIZED VIEW AS SELECT|
> Users can run EXPLAIN WITH_RECOMMENDATIONS <SQL_statement> to get the materialized views recommended by the query optimizer. Since these recommendations are query-specific, a materialized view that benefits a single query may not be optimal for other queries in the same workload
> Unlike fully logged operations, which use the transaction log to keep track of every row change, minimally logged operations keep track of extent allocations and meta-data changes only.\
> As much less information is tracked in the transaction log, a minimally logged operation performs better than a similarly sized fully logged operation. Furthermore, because fewer writes go the transaction log, a much smaller amount of log data is generated, and so is more I/O efficient.

### Secure a data warehouse in Azure Synapse Analytics

>There are a range of network security steps that you should consider to secure Azure Synapse Analytics. One of the first aspects that you will consider is securing access to the service itself. This can be achieved by creating the following network objects including:
>
>- Firewall rules:
>>
>> - IP firewall rules configured at the workspace level apply to all public endpoints of the workspace including dedicated SQL pools, serverless SQL pool, and the development endpoint.
>>
>- Virtual networks:
>>
>> - When you create your Azure Synapse workspace, you can choose to associate it to a Microsoft Azure Virtual Network. The Virtual Network associated with your workspace is managed by Azure Synapse. This Virtual Network is called a Managed workspace Virtual Network.
>> - With a Managed workspace Virtual Network, you can offload the burden of managing the Virtual Network to Azure Synapse.
>> - You don't have to configure inbound NSG rules on your own Virtual Networks to allow Azure Synapse management traffic to enter your Virtual Network. Misconfiguration of these NSG rules causes service disruption for customers.
>> - You don't need to create a subnet for your Spark clusters based on peak load.
>> - Managed workspace Virtual Network along with Managed private endpoints protects against data exfiltration. You can only create Managed private endpoints in a workspace that has a Managed workspace Virtual Network associated with it.
>> - it ensures that your workspace is network isolated from other workspaces.
>> - Dedicated SQL pool and serverless SQL pool are multi-tenant capabilities and therefore reside outside of the Managed workspace Virtual Network. Intra-workspace communication to dedicated SQL pool and serverless SQL pool use Azure private links. These private links are automatically created for you when you create a workspace with a Managed workspace Virtual Network associated to it.
>>
>- Private endpoints:
>>
>> - Azure Synapse Analytics enables you to connect up its various components through endpoints. You can set up managed private endpoints to access these components in a secure manner known as private links. This can only be achieved in an Azure Synapse workspace with a Managed workspace Virtual Network.
> >
>> - When you use a private link, traffic between your Virtual Network and workspace traverses entirely over the Microsoft backbone network. Private Link protects against data exfiltration risks. You establish a private link to a resource by creating a private endpoint.

#### Authentication in Azure Synapse Analytics

> [link](https://docs.microsoft.com/en-us/learn/modules/secure-data-warehouse-azure-synapse-analytics/4-configure-authentication)
> Azure Synapse Analytics dedicated SQL pools supports Conditional Access to provide protection for your data. It does require that Azure Synapse Analytics is configured to support Azure Active Directory, and that if you chose multifactor authentication, that the tool you are using support it.\
>
> If a user and an instance of Azure Synapse Analytics are part of the same Azure Active Directory, it is possible for the user to access Azure Synapse Analytics without an apparent login \
> A system-assigned managed identity is created for your Azure Synapse workspace when you create the workspace.
