# AZ-203
[Developer learning path](https://docs.microsoft.com/en-us/learn/browse/?products=azure&roles=developer&resource_type=learning%20path)  
[Self-paced diagram](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWtQqM)  

> 考试范围
> https://www.microsoft.com/zh-cn/learning/exam-AZ-203.aspx   

## Azure fundmental
1 Region, Geographies and Availability Zones


# Key content and Practice
## Azure Search
The process of creating an Azure Search index using C# and the .NET SDK. Index creation is accomplished by performing these tasks:
* Create a SearchServiceClient object to connect to a search service.  
* Create an Index object to pass as a parameter to Indexes.Create.  
* Call the Indexes.Create method on SearchServiceClient to send the Index to a service. 

Import data into an Azure Search index using C# and the .NET SDK. Pushing documents into your index is accomplished by performing these tasks:
* Create a SearchIndexClient object to connect to a search index. 
* Create an IndexBatch object containing the documents to be added, modified, or deleted. 
* Call the Documents.Index method on SearchIndexClient to upload documents to an index. 

> https://docs.microsoft.com/en-us/azure/search/search-import-data-dotnet 

## Azure Table Storage
Generates a property filter condition string for the string value.
```C#
public static string GenerateFilterCondition (string propertyName, string operation, string givenValue);

Parameters
propertyName
String
A string containing the name of the property to compare.

operation
String
A string containing the comparison operator to use.

givenValue
String
A string containing the value to compare with the property.

Returns
String
A string containing the formatted filter condition.
```
> https://docs.azure.cn/zh-cn/dotnet/api/microsoft.windowsazure.storage.table.tablequery.generatefiltercondition?view=azure-dotnet

## CosMosDB Consistency level 

* Strong: **Strong consistency offers a linearizability guarantee**. The reads are guaranteed to return the most recent committed version of an item. A client never sees an uncommitted or partial write. Users are always guaranteed to read the latest committed write. 

* Bounded staleness: The reads are guaranteed to honor the consistent-prefix guarantee. The reads might lag behind writes by at most "K" versions (i.e., "updates") of an item or by "T" time interval. In other words, when you choose bounded staleness, the "staleness" can be configured in two ways:  
The number of versions (K) of the item  
The time interval (T) by which the reads might lag behind the writes  

* Session: The reads are guaranteed to honor the consistent-prefix (assuming a single “writer” session), monotonic reads, monotonic writes, read-your-writes, and write-follows-reads guarantees. **Session consistency is scoped to a client session.** 

* Consistent prefix: Updates that are returned contain some prefix of all the updates, with no gaps. Consistent prefix consistency level guarantees that **reads never see out-of-order writes.** 

* Eventual: There's no ordering guarantee for reads. In the absence of any further writes, **the replicas eventually converge.** 
> https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels


## Azure Table Storage SDK (.NET)
**Create a Table**
The CloudTableClient class enables you to retrieve tables and entities stored in Table storage. Because we don’t have any tables in the Cosmos DB Table API account, let’s add the CreateTableAsync method to the Common.cs class to create a table:
```C#
public static async Task<CloudTable> CreateTableAsync(string tableName)
  {
    string storageConnectionString = AppSettings.LoadAppSettings().StorageConnectionString;

    // Retrieve storage account information from connection string.
    CloudStorageAccount storageAccount = CreateStorageAccountFromConnectionString(storageConnectionString);

    // Create a table client for interacting with the table service
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient(new TableClientConfiguration());

    Console.WriteLine("Create a Table for the demo");

    // Create a table client for interacting with the table service 
    CloudTable table = tableClient.GetTableReference(tableName);
    if (await table.CreateIfNotExistsAsync())
    {
      Console.WriteLine("Created Table named: {0}", tableName);
    }
    else
    {
      Console.WriteLine("Table {0} already exists", tableName);
    }

    Console.WriteLine();
    return table;
}
```
**Define the entity**
Entities map to C# objects by using a custom class derived from TableEntity. To add an entity to a table, create a class that defines the properties of your entity.
```C#
public static async Task<CloudTable> CreateTableAsync(string tableName)
  {
    string storageConnectionString = AppSettings.LoadAppSettings().StorageConnectionString;

    // Retrieve storage account information from connection string.
    CloudStorageAccount storageAccount = CreateStorageAccountFromConnectionString(storageConnectionString);

    // Create a table client for interacting with the table service
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient(new TableClientConfiguration());

    Console.WriteLine("Create a Table for the demo");

    // Create a table client for interacting with the table service 
    CloudTable table = tableClient.GetTableReference(tableName);
    if (await table.CreateIfNotExistsAsync())
    {
      Console.WriteLine("Created Table named: {0}", tableName);
    }
    else
    {
      Console.WriteLine("Table {0} already exists", tableName);
    }

    Console.WriteLine();
    return table;
}
```
**Insert or merge an entity**
The following code example creates an entity object and adds it to the table. The InsertOrMerge method within the TableOperation class is used to insert or merge an entity. The **CloudTable.ExecuteAsync** method is called to execute the operation.
```C#
public static async Task<CustomerEntity> InsertOrMergeEntityAsync(CloudTable table, CustomerEntity entity)
    {
      if (entity == null)
    {
       throw new ArgumentNullException("entity");
    }
    try
    {
       // Create the InsertOrReplace table operation
       TableOperation insertOrMergeOperation = TableOperation.InsertOrMerge(entity);

       // Execute the operation.
       TableResult result = await table.ExecuteAsync(insertOrMergeOperation);
       CustomerEntity insertedCustomer = result.Result as CustomerEntity;
        
        // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
        if (result.RequestCharge.HasValue)
          {
            Console.WriteLine("Request Charge of InsertOrMerge Operation: " + result.RequestCharge);
          }

        return insertedCustomer;
        }
        catch (StorageException e)
        {
          Console.WriteLine(e.Message);
          Console.ReadLine();
          throw;
        }
    }
```
