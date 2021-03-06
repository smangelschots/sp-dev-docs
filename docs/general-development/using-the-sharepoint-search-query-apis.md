---
title: Using the SharePoint search Query APIs
ms.prod: SHAREPOINT
ms.assetid: ae9d73ed-1140-430b-9287-01dbbe8ae7d1
---



# Using the SharePoint search Query APIs
Learn about the query APIs available in SharePoint that enable you to add search functionality to custom solutions and applications. 
## SharePoint Query APIs
<a name="bk_QueryAPIs"> </a>

Search in SharePoint provides several query APIs, giving you lots of ways to access search results, so that you can return search results in a variety of custom solution types.
  
    
    
In addition to the server object model that was available in previous versions of SharePoint, Search in SharePoint also provides the following:
  
    
    

- Client object model (CSOM)
    
  
- JavaScript object model (JSOM)
    
  
- Representational State Transfer (REST) service
    
  
Table 1 shows the APIs that you can use to program search queries and the path to the source file on the server.
  
    
    

**Table 1. Search APIs**


|**API name**|**Class library or schema and path**|
|:-----|:-----|
|.NET CSOM  <br/> |Microsoft.SharePoint.Client.Search.dll <br/>%ProgramFiles%\\Common Files\\Microsoft Shared\\web server extensions\\15\\ISAPI  <br/> |
|Silverlight CSOM  <br/> |Microsoft.SharePoint.Client.Search.Silverlight.dll <br/>%ProgramFiles%\\Common Files\\Microsoft Shared\\web server extensions\\15\\TEMPLATE\\LAYOUTS\\ClientBin  <br/> |
|JavaScript CSOM  <br/> |SP.search.js <br/>%ProgramFiles%\\Common Files\\Microsoft Shared\\web server extensions\\15\\TEMPLATE\\LAYOUTS  <br/> |
|REST service endpoints  <br/> |http://server/_api/search/query <br/>http://server/_api/search/suggest  <br/> |
|Server object model  <br/> |Microsoft.Office.Server.Search.dll <br/>%ProgramFiles%\\Common Files\\Microsoft Shared\\web server extensions\\15\\ISAPI  <br/> |
   
As a best practice in SharePoint development, use client APIs when you can. Client APIs include the .NET, Silverlight, Phone, and JavaScript client object models, and the REST service. For more information about the APIs in SharePoint and when to use them, see  [Choose the right API set in SharePoint](choose-the-right-api-set-in-sharepoint).
  
    
    

### Query using the .NET client object model
<a name="bk_QueryNETcsom"> </a>

Search in SharePoint includes a client object model that enables access to search results for online, on-premises, and mobile development. The Search in SharePoint CSOM is built on the SharePoint CSOM. Therefore, your client code first needs to access the SharePoint CSOM and then access the Search in SharePoint CSOM. For more information about the SharePoint CSOM, see  [SharePoint 2010 Client Object Model](http://msdn.microsoft.com/library/8c086b11-2b8b-41ec-82ae-cd4fef0aeac6%28Office.15%29.aspx). For more information about the  [ClientContext](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.ClientContext.aspx) class, which is the entry point to the CSOM, see [Client Context as Central Object](http://msdn.microsoft.com/library/6299f0df-ab4c-40e6-b709-ec80271c99b3%28Office.15%29.aspx).
  
    
    
For the .NET managed CSOM, get a **ClientContext** instance (located in the [Microsoft.SharePoint.Client](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.aspx) namespace in the Microsoft.SharePoint.Client.dll). Then use the object model in the **Microsoft.SharePoint.Search.Client.Query** namespace in the Microsoft.SharePoint.Search.Client.dll.
  
    
    
Here's a basic example.
  
    
    



```cs

using (ClientContext clientContext = new ClientContext("http://<serverName>/sites/<siteCollectionPath>"))
{
    KeywordQuery keywordQuery = new KeywordQuery(clientContext);
    keywordQuery.QueryText = "SharePoint";
    SearchExecutor searchExecutor = new SearchExecutor(clientContext);
    ClientResult<ResultTableCollection> results = searchExecutor.ExecuteQuery(keywordQuery);
    clientContext.ExecuteQuery();
}
```

To download an example, see the following code sample posted by SharePoint MVP  [Corey Roth](http://mvp.microsoft.com/en-us/mvp/Corey%20Roth-4029260):  [SharePoint: Query Search with the Managed Client Object Model](http://code.msdn.microsoft.com/Query-Search-with-the-649f1bc1).
  
    
    

### Query using the JavaScript client object model
<a name="bk_QueryJSOM"> </a>

For the JavaScript CSOM, get a  [ClientContext](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.ClientContext.aspx) instance, and then use the object model in the SP.Search.js file.
  
    
    
Here's a basic example.
  
    
    



```

var clientContext = new SP.ClientContext("<serverRelativeUrl>");
var contextSite = clientContext.get_site();
var keywordQuery = new Microsoft.SharePoint.Client.Search.Query.KeywordQuery(clientContext); 
keywordQuery.set_queryText("SharePoint"); 
var searchExecutor = new Microsoft.SharePoint.Client.Search.Query.SearchExecutor(clientContext);  
var results = searchExecutor.executeQuery(keywordQuery); 
context.executeQueryAsync(onQuerySuccess, onQueryError);
```

To download an example, see the following code sample posted by SharePoint MVP  [Corey Roth](http://mvp.microsoft.com/en-us/mvp/Corey%20Roth-4029260):  [SharePoint: Querying Search with the JavaScript Client Object Model](http://code.msdn.microsoft.com/SharePoint-Querying-a629b53b).
  
    
    

### Query using the REST service
<a name="bk_QueryREST"> </a>

SharePoint includes a REST service that enables you to remotely execute queries against the SharePoint Search service from client applications by using any technology that supports REST web requests. The Search REST service exposes two endpoints, **query** and **suggest**, and will support both **GET** and **POST** operations. Results are returned in either XML or JavaScript Object Notation (JSON) format.
  
    
    
The following is the access point for the service:  `http://server/_api/search/`. You can also specify the site in the URL, as follows:  `http://server/site/_api/search/`. The search service returns results from the entire site collection, so the same results are returned for both ways to access the service.
  
    
    
See  [SharePoint Search REST API overview](sharepoint-search-rest-api-overview) and [Retrieving query suggestions using the Search REST service](retrieving-query-suggestions-using-the-search-rest-service) for more information.
  
    
    

### Query using the .NET server object model
<a name="bk_QuerySOM"> </a>

Applications that use the server object model must run directly on a server that is running SharePoint. The search Query server object model resides in the  [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) namespace, which is located in Microsoft.Office.Server.Search.dll.
  
    
    
As in SharePoint Server 2010, you use the  **KeywordQuery** class to define the query, and then called the [Execute()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.Query.Execute.aspx) method to submit the query. In SharePoint, the **Execute** method is obsolete, and while it will still work, you should use the [SearchExecutor](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.SearchExecutor.aspx) class instead. To submit the query, call the [ExecuteQuery()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.SearchExecutor.ExecuteQuery.aspx) method, passing the instance of the **KeywordQuery** class in the call.
  
    
    
Here's a basic example.
  
    
    



```cs

using (SPSite siteCollection = new SPSite("<serverRelativeUrl>"))
{
    KeywordQuery keywordQuery = new KeywordQuery(siteCollection);
    keywordQuery.QueryText = "SharePoint";
    SearchExecutor searchExecutor = new SearchExecutor(); 
    ResultTableCollection resultTableCollection = searchExecutor.ExecuteQuery(keywordQuery); 
    resultTableCollection = resultTableCollection.Filter("TableType", KnownTableTypes.RelevantResults); 
    ResultTable resultTable = resultTableCollection.FirstOrDefault(); 
    DataTable dataTable = resultTable.Table; 
}
```

To download an example, see the following code sample posted by SharePoint MVP  [Corey Roth](http://mvp.microsoft.com/en-us/mvp/Corey%20Roth-4029260):  [SharePoint: Query Search with the KeywordQuery Class](http://code.msdn.microsoft.com/Query-Search-with-the-372139b5).
  
    
    

## Additional resources
<a name="bk_addresources"> </a>


-  [Building search queries in SharePoint](building-search-queries-in-sharepoint)
    
  
-  [SharePoint Search REST API overview](sharepoint-search-rest-api-overview)
    
  
-  [SharePoint: Using the search REST service from an app for SharePoint](http://code.msdn.microsoft.com/sharepoint/SharePoint-Perform-a-1bf3e87d)
    
  

  
    
    
