Convert FetchXml to QueryExpression
===

```cs
string fetchXml =
    @"<fetch version='1.0' output-format='xml-platform' mapping='logical' distinct='false'>
    <entity name='account'>
    <attribute name='name' />
    <attribute name='accountid' />
    <order attribute='createdon' descending='true' />
    <order attribute='modifiedon' descending='true' />
    <filter type='and'>
      <condition attribute='createdon' operator='this-month' />
    </filter>
  </entity>
</fetch>";
	
// Run the query with the FetchXML.
var fetchExpression = new FetchExpression(fetchXml);
EntityCollection fetchResult = service.RetrieveMultiple(fetchExpression);
Console.WriteLine("\nOutput for query as FetchXML:");
//fetchResult.Entities.Dump();
	
// Convert the FetchXML into a query expression.
var conversionRequest = new FetchXmlToQueryExpressionRequest();
conversionRequest.FetchXml = fetchXml;

var conversionResponse =(FetchXmlToQueryExpressionResponse)service.Execute(conversionRequest);
var result = conversionResponse.Results.FirstOrDefault();
result.Dump();
```
---
