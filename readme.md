# Azure Stream Analytics
https://learn.microsoft.com/en-us/stream-analytics-query/system-timestamp-stream-analytics  

Can take stream inputs as event hubs and IoT Hub to fetch data from and reference niputs as database and blobs.  
For outputs it can also take all of the above but also functions.

Every stream has to have query defined with one input and one output. Multiple selects can be defined in one query but with different outputs. 

Uses SQL like queries to transform data e.g.:  
```
SELECT
    *
INTO
    [cosmos]
FROM
    [test-topic]
```
where [cosmos] is db output while [test-topic] is event hub topic input.  
SQL query like language:  
https://learn.microsoft.com/en-us/stream-analytics-query/system-timestamp-stream-analytics
https://learn.microsoft.com/en-us/azure/stream-analytics/stream-analytics-stream-analytics-query-patterns  
Query language supports built in functions:  
https://learn.microsoft.com/en-us/stream-analytics-query/built-in-functions-azure-stream-analytics#types-of-functions  
and import limited stateless javascript functions with compulsory parameters:  
https://learn.microsoft.com/en-us/azure/stream-analytics/functions-overview

Built in functions can use input metadata (such as partitionId and timestamp from Event Hub events):  
https://learn.microsoft.com/en-us/stream-analytics-query/getmetadatapropertyvalue

## Errors

Data errors on inputs or outputs:  
https://learn.microsoft.com/en-us/azure/stream-analytics/data-errors


# Azure Stream Download and Upload
The jobs that exist on Azure can be exported to local machine:  
https://learn.microsoft.com/en-us/azure/stream-analytics/visual-studio-code-explore-jobs#export-job-to-local-machine
# Azure Stream Error Handling
OutputErrorPolicy has to be "Drop", not "Retry", in order not to endlessly retry corrupted messages and increase cost.

# Azure Stream Cosmos DB as output
To be able store an output object to Cosmos DB the object/document has to contain two fields: id and partitionKey as above.  
PartitionKey can be null but it has to exist in the data because otherwise deserialisation do DB will fail with OutputDataConversionError.RequiredColumnMissing error.
```
{
  "id": "1e888a8e-70c5-49b2-9942-224e70b6a015", 
  "partitionKey": "1",
  "title": "Test message",
  "description": "Final test on azure2"
}
```