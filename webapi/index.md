**CRM Web API Guide**
===

- [Create a table row using the Web API](#create-a-table-row-using-the-web-api)
  - [Basic Create](#basic-create)
  - [Create with data returned](#create-with-data-returned)
  - [Create related table rows in one operation](#create-related-table-rows-in-one-operation)
  - [Associate table rows on create](#associate-table-rows-on-create)
  - [Create a new record from another record](#create-a-new-record-from-another-record)
    - [Step 1: Get the data using InitializeFrom](#step-1-get-the-data-using-initializefrom)
    - [Step 2: Create the new record](#step-2-create-the-new-record)
- [Retrieve a table row using the Web API](#retrieve-a-table-row-using-the-web-api)
  - [Basic retrieve example](#basic-retrieve-example)
  - [Retrieve specific properties](#retrieve-specific-properties)
  - [Retrieve using an alternate key](#retrieve-using-an-alternate-key)
  - [Retrieve a single property value](#retrieve-a-single-property-value)
  - [Retrieve the raw value of a property](#retrieve-the-raw-value-of-a-property)
  - [Retrieve navigation property values](#retrieve-navigation-property-values)
- [Update and delete table rows using the Web API](#update-and-delete-table-rows-using-the-web-api)
  - [Basic update](#basic-update)
  - [Update with data returned](#update-with-data-returned)
  - [Update a single property value](#update-a-single-property-value)
  - [Delete a single property value](#delete-a-single-property-value)
  - [Upsert a table row](#upsert-a-table-row)
  - [Create with Upsert and and `return=representation` preference](#create-with-upsert-and-and-returnrepresentation-preference)
  - [Limit upsert operations](#limit-upsert-operations)
    - [Prevent create in upsert](#prevent-create-in-upsert)
    - [Prevent update in upsert](#prevent-update-in-upsert)
- [Basic delete](#basic-delete)
- [Associate and disassociate](#associate-and-disassociate)
  - [Associate with a single-valued navigation property](#associate-with-a-single-valued-navigation-property)
  - [Disassociate with a single-valued navigation property](#disassociate-with-a-single-valued-navigation-property)
    - [Using `PUT` request](#using-put-request)
    - [Using `DELETE` request](#using-delete-request)
  - [Add a record to a collection](#add-a-record-to-a-collection)
  - [Remove a record from a collection](#remove-a-record-from-a-collection)
- [Merge table rows using the Web API](#merge-table-rows-using-the-web-api)


# Create a table row using the Web API
Use a POST request to send data to create a table row (entity record). You can create multiple related table rows in a single operation using deep insert. 

## Basic Create
This example creates a new account entity record. The response OData-EntityId header contains the Uri of the created entity.

**Request**
```js
POST [Organization URI]/api/data/v9.2/accounts HTTP/1.1
Content-Type: application/json; charset=utf-8
OData-MaxVersion: 4.0
OData-Version: 4.0
Accept: application/json

{
    "name": "Sample Account",
    "creditonhold": false,
    "address1_latitude": 47.639583,
    "description": "This is the description of the sample account",
    "revenue": 5000000,
    "accountcategorycode": 1
}
```

> When an entity is created in this way the OData-EntityId header containing the URI to the created record is returned.
**Response**
```JS
HTTP/1.1 204 No Content
OData-Version: 4.0
OData-EntityId: [Organization URI]/api/data/v9.2/accounts(7eb682f1-ca75-e511-80d4-00155d2a68d1)
```

## Create with data returned
You can compose your POST request so that data from the created record will be returned with a status of 201 (Created). To get this result, you must use the `return=representation` preference in the request headers.

To control which properties are returned, append the `$select` query option to the URL to the entity set. You may also use $expand to return related entities.

> When an entity is created in this way the OData-EntityId header containing the URI to the created record is not returned.

This example creates a new account entity and returns the requested data in the response.
```JS
POST [Organization URI]/api/data/v9.2/accounts?$select=name,creditonhold,address1_latitude,description,revenue,accountcategorycode,createdon HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
Accept: application/json
Content-Type: application/json; charset=utf-8
Prefer: return=representation

{
   "name": "Sample Account",
   "creditonhold": false,
   "address1_latitude": 47.639583,
   "description": "This is the description of the sample account",
   "revenue": 5000000
}
```
**Response**
```js
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal
Preference-Applied: return=representation
OData-Version: 4.0

{
    "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#accounts/$entity",
    "@odata.etag": "W/\"536530\"",
    "accountid": "d6f193fc-ce85-e611-80d8-00155d2a68de",
    "accountcategorycode": 1,
    "description": "This is the description of the sample account",
    "address1_latitude": 47.63958,
    "creditonhold": false,
    "name": "Sample Account",
    "createdon": "2016-09-28T22:57:53Z",
    "revenue": 5000000.0000,
    "_transactioncurrencyid_value": "048dddaa-6f7f-e611-80d3-00155db5e0b6"
}
```

## Create related table rows in one operation
You can create entities related to each other by defining them as navigation properties values. This is known as deep insert. This approach has two advantages. It is more efficient, replacing multiple simpler creation and association operations with one combined operation. Also, it is atomic where either the entire operation succeeds and all the related objects are created, or the operation fails and none are created.

As with a basic create, the response OData-EntityId header contains the Uri of the created entity. The URIs for the related entities created aren't returned. You can get the primary key values of the records if you use the `Prefer: return=representation` header so it returns the values of the created record. More information: Create with data returned

For example, the following request body posted to the accounts entity set will create a total of four new entities in the context of creating an account.

- A contact is created because it is defined as an object property of the single-valued navigation property primarycontactid.

- An opportunity is created because it is defined as an object within an array that is set to the value of a collection-valued navigation property opportunity_customer_accounts.

- A task is created because it is defined an object within an array that is set to the value of a collection-valued navigation property Opportunity_Tasks.

```js
POST [Organization URI]/api/data/v9.2/accounts HTTP/1.1
Content-Type: application/json; charset=utf-8
OData-MaxVersion: 4.0
OData-Version: 4.0
Accept: application/json

{
 "name": "Sample Account",
 "primarycontactid":
 {
     "firstname": "John",
     "lastname": "Smith"
 },
 "opportunity_customer_accounts":
 [
  {
      "name": "Opportunity associated to Sample Account",
      "Opportunity_Tasks":
      [
       { "subject": "Task associated to opportunity" }
      ]
  }
 ]
}
```
**Response**
```js
HTTP/1.1 204 No Content
OData-Version: 4.0
OData-EntityId: [Organization URI]/api/data/v9.2/accounts(3c6e4b5f-86f6-e411-80dd-00155d2a68cb)
```

## Associate table rows on create
To associate new entities to existing entities when they are created you must set the value of navigation properties using the `@odata.bind` annotation.

The following request body posted to the accounts entity set will create a new account associated with an existing contact with the `contactid` value of `00000000-0000-0000-0000-000000000001` and two existing tasks with `activityid` values of `00000000-0000-0000-0000-000000000002` and `00000000-0000-0000-0000-000000000003`.

**Request**
```js
POST [Organization URI]/api/data/v9.2/accounts?$select=name&$expand=primarycontactid($select=fullname),Account_Tasks($select=subject) HTTP/1.1
Content-Type: application/json; charset=utf-8
OData-MaxVersion: 4.0
OData-Version: 4.0
Accept: application/json
Prefer: return=representation

{
    "name": "Sample Account",
    "primarycontactid@odata.bind": "/contacts(00000000-0000-0000-0000-000000000001)",
    "Account_Tasks@odata.bind": [
        "/tasks(00000000-0000-0000-0000-000000000002)",
        "/tasks(00000000-0000-0000-0000-000000000003)"
    ]
}
```

**Response**
```js
HTTP/1.1 201 Created
OData-Version: 4.0
Preference-Applied: return=representation

{
    "@odata.context": "[Organization URI]/api/data/v9.1/$metadata#accounts(name,primarycontactid(fullname),Account_Tasks(subject))/$entity",
    "@odata.etag": "W/\"36236432\"",
    "name": "Sample Account",
    "accountid": "00000000-0000-0000-0000-000000000004",
    "primarycontactid": {
        "@odata.etag": "W/\"28877094\"",
        "fullname": "Yvonne McKay (sample)",
        "contactid": "00000000-0000-0000-0000-000000000001"
    },
    "Account_Tasks": [
        {
            "@odata.etag": "W/\"36236437\"",
            "subject": "Task 1",
            "activityid": "00000000-0000-0000-0000-000000000002"
        },
        {
            "@odata.etag": "W/\"36236440\"",
            "subject": "Task 2",
            "activityid": "00000000-0000-0000-0000-000000000003"
        }
    ]
}
```

## Create a new record from another record
Use the InitializeFrom Function to create a new record in the context of an existing record where a mapping exists for the relationship between the tables.

This is a two step process. The InitializeFrom function doesn't create the record, but it returns data you can use to create a new record with specified property values mapped from the original record. You will combine the response data returned in the InitializeFrom function with any changes you want to make and then POST the data to create the new record.

The following example shows how to create an account record using the values of an existing account record with accountid value equal to `00000000-0000-0000-0000-000000000001`.

### Step 1: Get the data using InitializeFrom
Request
```js
GET [Organization URI]/api/data/v9.2/InitializeFrom(EntityMoniker=@p1,TargetEntityName=@p2,TargetFieldType=@p3)?@p1={'@odata.id':'accounts(00000000-0000-0000-0000-000000000001)'}&@p2='account'&@p3=Microsoft.Dynamics.CRM.TargetFieldType'ValidForCreate' HTTP/1.1
If-None-Match: null
OData-Version: 4.0
OData-MaxVersion: 4.0
Content-Type: application/json
Accept: application/json
```

Response
```js
Copy
{
    "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#accounts/$entity",
    "@odata.type": "#Microsoft.Dynamics.CRM.account",
    "parentaccountid@odata.bind": "accounts(00000000-0000-0000-0000-000000000001)",
    "transactioncurrencyid@odata.bind": "transactioncurrencies(732e87e1-1d96-e711-80e4-00155db75426)",
    "address1_line1": "123 Maple St.",
    "address1_city": "Seattle",
    "address1_country": "United States of America"
}
```

### Step 2: Create the new record
The response received from InitializeFrom function consists of values of mapped columns between the source table and target table and the GUID of the parent record. The column mapping between tables that have an relationship is different for different tables and is customizable, so the response from InitializeFrom function request may vary for different organizations.

Other property values can also be set and/or modified for the new record by adding them in the JSON request body, as shown in the example below.

**Request**
```js
POST [Organization URI]/api/data/v9.2/accounts HTTP/1.1
Content-Type: application/json; charset=utf-8
OData-MaxVersion: 4.0
OData-Version: 4.0
Accept: application/json

    {
        "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#accounts/$entity",
        "@odata.type": "#Microsoft.Dynamics.CRM.account",
        "parentaccountid@odata.bind": "accounts(00000000-0000-0000-0000-000000000001)",
        "transactioncurrencyid@odata.bind": "transactioncurrencies(732e87e1-1d96-e711-80e4-00155db75426)",
        "name":"Contoso Ltd",
        "numberofemployees":"200",
        "address1_line1":"100 Maple St.",
        "address1_city":"Seattle",
        "address1_country":"United States of America",
        "fax":"73737"
    }
}
```

# Retrieve a table row using the Web API
Use a GET request to retrieve data for a record specified as the resource with a unique identifier. When retrieving a table row (entity record), you can also request specific properties and expand navigation properties to return properties from related records in different tables.

## Basic retrieve example
This example returns data for an account entity record with the primary key value equal to `00000000-0000-0000-0000-000000000001`.

```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)
```
> The above example will return all the properties for account record, which is not a performance best practice for retrieving data. As a performance best practice, you must always use the $select system query option to limit the properties returned while retrieving data.

## Retrieve specific properties
Use the $select system query option to limit the properties returned by including a comma-separated list of property names. Requesting only the properties you need is an important performance best practice. If properties aren't specified using $select, all properties will be returned.

The following example retrieves name and revenue properties for the account entity with the primary key value equal to 00000000-0000-0000-0000-000000000001

**Request**

```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)?$select=name,revenue HTTP/1.1
Accept: application/json
Content-Type: application/json; charset=utf-8
OData-MaxVersion: 4.0
OData-Version: 4.0
```
**Response**

```js
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal
OData-Version: 4.0

{
"@odata.context": "[Organization URI]/api/data/v9.2/$metadata#accounts(name,revenue)/$entity",  
"@odata.etag": "W/\"502186\"",  
"name": "A. Datum Corporation (sample)",  
"revenue": 10000,  
"accountid": "00000000-0000-0000-0000-000000000001",  
"_transactioncurrencyid_value":"b2a6b689-9a39-e611-80d2-00155db44581"  
}
```
> When you request certain types of properties, you can expect more read-only properties to be returned automatically.

## Retrieve using an alternate key
If an entity has an alternate key defined, you can also use the alternate key to retrieve the entity instead of the unique identifier for the entity. For example, if the Contact entity has an alternate key definition that includes both the firstname and emailaddress1 properties, you can retrieve the contact using a query with data provided for those keys as shown here.

**Request**
```js
GET [Organization URI]/api/data/v9.2/contacts(firstname='Joe',emailaddress1='abc@example.com')
```

> Alternate key values containing a '+' character are not currently supported for retrieval through OData.

If the alternate key definition contains lookup type field (for example, the primarycontactid property for the account entity), you can retrieve the account using the Lookup properties as shown here.

**Request**
```js
GET [Organization URI]/api/data/v9.2/accounts(_primarycontactid_value=00000000-0000-0000-0000-000000000001)
```
Anytime you need to uniquely identify an entity to retrieve, update, or delete, you can use alternate keys configured for the entity. By default, there are no alternate keys configured for entities. Alternate keys will only be available if the organization or a solution adds them.

## Retrieve a single property value
When you only need to retrieve the value of a single property for an entity, you can append the name of the property to the URI for an entity to return only the value for that property. Reducing the amount of data returned is a performance best practice.

This example returns only the value of the name property for an account entity.

**Request**
```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/name HTTP/1.1
Accept: application/json
OData-MaxVersion: 4.0
OData-Version: 4.0
```
**Response**
```js
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal
OData-Version: 4.0

{
"@odata.context":"[Organization URI]/api/data/v9.2/$metadata#accounts(00000000-0000-0000-0000-000000000001)/name",
"value":"Adventure Works (sample)"
}
```

## Retrieve the raw value of a property
To retrieve the raw value of a primitive property rather than JSON, append /$value to the url.

**Request**
```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/name/$value HTTP/1.1
Accept: application/json
OData-MaxVersion: 4.0
OData-Version: 4.0
```

**Response**
```js
HTTP/1.1 200 OK
Content-Type: text/plain
OData-Version: 4.0

Adventure Works (sample)
```

> Using the raw value is not common unless you are working with file or image data. More information: Download a file in a single request using Web API.

## Retrieve navigation property values
You can access the values of navigation properties (lookup fields) by appending the name of the navigation property to the URI referencing an individual entity.

The following example returns the fullname of the primary contact of an account using the primarycontactid single-valued navigation property.

**Request**
```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/primarycontactid?$select=fullname HTTP/1.1
Accept: application/json
OData-MaxVersion: 4.0
OData-Version: 4.0
```

**Response**
```js
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal
OData-Version: 4.0

{  
"@odata.context": "[Organization URI]/api/data/v9.2/$metadata#contacts(fullname)/$entity",  
"@odata.etag": "W/\"500128\"",  
"fullname": "Rene Valdes (sample)",  
"contactid": "ff390c24-9c72-e511-80d4-00155d2a68d1"  
}
```

For collection-valued navigation properties, you can request to return only references to the related entities or just a count of the related entities.

The following example will just return references to tasks related to a specific account by adding /$ref to the request.

**Request**
```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/AccountTasks/$ref HTTP/1.1
Accept: application/json
OData-MaxVersion: 4.0
OData-Version: 4.0
```
**Response**

```js
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal
OData-Version: 4.0
  
{
    "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#Collection($ref)",
    "value":
  [
    { "@odata.id": "[Organization URI]/api/data/v9.2/tasks(6b5941dd-d175-e511-80d4-00155d2a68d1)" },
    { "@odata.id": "[Organization URI]/api/data/v9.2/tasks(fcbb60ed-d175-e511-80d4-00155d2a68d1)" }
  ]
}
```

The following example returns the number of tasks related to a specific account using the Account_Tasks collection-valued navigation property with /$count appended.

Request
```js
GET [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/Account_Tasks/$count HTTP/1.1  
Accept: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0  
```

Response
```js
ï»¿2
```
> The value returned includes the UTF-8 byte order mark (BOM) characters (ï»¿) that represent that this is a UTF-8 document.



# Update and delete table rows using the Web API

Operations to modify data are a core part of the Web API. In addition to a simple update and delete, you can perform operations on single table columns (entity attributes) and compose upsert requests that will either update or insert data depending on whether it exists.

## Basic update
Update operations use the HTTP `PATCH` verb. Pass a JSON object containing the properties you want to update to the URI that represents the record. A response with a status of `204 No Content` will be returned if the update is successful.

The `If-Match: *` header ensures you don't create a new record by accidentally performing an upsert operation.

> The definition for attributes includes a RequiredLevel property. When this is set to SystemRequired, you cannot set these attributes to a null value. 

**Request**

```js
PATCH [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001) HTTP/1.1  
Content-Type: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0
If-Match: *  
  
{  
    "name": "Updated Sample Account ",  
    "creditonhold": true,  
    "address1_latitude": 47.639583,  
    "description": "This is the updated description of the sample account",  
    "revenue": 6000000,  
    "accountcategorycode": 2  
}  
```
**Response**
```js
HTTP/1.1 204 No Content  
OData-Version: 4.0  
```

## Update with data returned
To retrieve data from an entity you are updating you can compose your PATCH request so that data from the created record will be returned with a status of 200 (OK). To get this result, you must use the `Prefer: return=representation` request header.

To control which properties are returned, append the $select query option to the URL to the entity set. The $expand query option will be ignored if used.

This example updates an account entity and returns the requested data in the response.

**Request**
```js
PATCH [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)?$select=name,creditonhold,address1_latitude,description,revenue,accountcategorycode,createdon HTTP/1.1  
OData-MaxVersion: 4.0  
OData-Version: 4.0  
Accept: application/json  
Content-Type: application/json; charset=utf-8  
Prefer: return=representation
If-Match: * 
  
{"name":"Updated Sample Account"}  
```
**Response**
```
HTTP/1.1 200 OK  
Content-Type: application/json; odata.metadata=minimal  
Preference-Applied: return=representation  
OData-Version: 4.0  
  
{  
    "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#accounts/$entity",  
    "@odata.etag": "W/\"536537\"",  
    "accountid": "00000000-0000-0000-0000-000000000001",  
    "accountcategorycode": 1,  
    "description": "This is the description of the sample account",  
    "address1_latitude": 47.63958,  
    "creditonhold": false,  
    "name": "Updated Sample Account",  
    "createdon": "2016-09-28T23:14:00Z",  
    "revenue": 5000000.0000,  
    "_transactioncurrencyid_value": "048dddaa-6f7f-e611-80d3-00155db5e0b6"  
}  
```  

## Update a single property value
When you want to update only a single property value use a PUT request with the property name appended to the Uri of the entity.

The following example updates the name property of an existing account entity with the accountid value of `00000000-0000-0000-0000-000000000001`.

**Request**
```js
PUT [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/name HTTP/1.1  
Content-Type: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0  
  
{"value": "Updated Sample Account Name"}  
```

**Response**
```js
HTTP/1.1 204 No Content  
OData-Version: 4.0  
```

## Delete a single property value
To delete the value of a single property use a `DELETE` request with the property name appended to the Uri of the entity.

The following example deletes the value of the description property of an account entity with the accountid value of `00000000-0000-0000-0000-000000000001`.

**Request**
```js
DELETE [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001)/description HTTP/1.1  
Content-Type: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0  

```
**Response**

```js
HTTP/1.1 204 No Content  
OData-Version: 4.0  
 ``` 

## Upsert a table row
An upsert operation is similar to an update. It uses a PATCH request and uses a URI to reference a specific record. The difference is that if the record doesn't exist it will be created. If it already exists, it will be updated.

Upsert is valuable when synchronizing data between external systems. The external system may not contain a reference to the primary key of the Dataverse table, so you can configure alternate keys for the Dataverse table using values from the external system that uniquely identify the record on both systems. More information: Define alternate keys to reference rows

You can see any alternate keys that are defined for a table in the annotations for the entity type in the $metadata service document. More information: Alternate Keys.

In the following example, there is a table with the name `sample_thing` that has an alternate key that refers to two columns: `sample_key1` and `sample_key2` which are both defined to store integer values.

**Request**
```js
PATCH [Organization URI]/api/data/v9.2/sample_things(sample_key1=1,sample_key2=1) HTTP/1.1
Accept: application/json 
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Content-Type: application/json

{
    "sample_name": "1:1"
}
```
For both create or update operations you will get the same response. Notice how the OData-EntityId response header uses the key values rather than the GUID primary key identifier for the record.

**Response**
```js
HTTP/1.1 204 No Content
OData-Version: 4.0
OData-EntityId: [Organization URI]/api/data/v9.2/sample_things(sample_key1=1,sample_key2=1)
```

Because the response it the same, you cannot know whether the operation represented a Create or Update.

If you need to know, you can use the `Prefer: return=representation` request header. Whith this header you will get a 201 Created response when a record is created and a 200 OK reponse when the record is updated. This adds an additional Retrieve operation which has an impact on performance. If you use the `Prefer: return=representation` request header, make sure that your $select includes the minimal amount of data, preferably only the primary key column. More information: Update with data returned and Create with data returned.

When using alternate keys, you should not include the alternate key values in the body of the request.

When an upsert represents an Update, these alternate key values will be ignored. You cannot update alternate key values while using them to identify the record.
When an upsert represents a Create, the key values in the URL will be set for the record if they are not present in the body. So there is no need to include them in the body of the request.

Because the response it the same, you cannot know whether the operation represented a Create or Update.

If you need to know, you can use the `Prefer: return=representation` request header. Whith this header you will get a 201 Created response when a record is created and a 200 OK reponse when the record is updated. This adds an additional Retrieve operation which has an impact on performance. If you use the `Prefer: return=representation` request header, make sure that your $select includes the minimal amount of data, preferably only the primary key column.

When using alternate keys, you should not include the alternate key values in the body of the request.

When an upsert represents an Update, these alternate key values will be ignored. You cannot update alternate key values while using them to identify the record.
When an upsert represents a Create, the key values in the URL will be set for the record if they are not present in the body. So there is no need to include them in the body of the request.

## Create with Upsert and and `return=representation` preference
When you use the Prefer: return=representation header you can get a different status code in the response to indicate whether the record was created or updated.

The following request creates a new record and returns status `201 Created`.

**Request**
```js
PATCH [Organization Uri]/api/data/v9.2/example_records(example_key1=3,example_key2=3)?$select=example_recordid HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json
Prefer: return=representation
Content-Type: application/json

{ "example_name": "3:3" }
```
**Response**

```js
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal
ETag: W/"71004878"
Preference-Applied: return=representation
OData-Version: 4.0

{
  "@odata.context": "[Organization Uri]/api/data/v9.2/$metadata#example_records(example_recordid)/$entity",
  "@odata.etag": "W/\"71004878\"",
  "example_recordid": "ef0d112e-d70e-ed11-82e5-00224822577b"
}
```
Update with Upsert and return=representation preference
This request updates the record created by the request above and returns status `200 OK` to show that this was an update operation.

**Request**
```js
PATCH [Organization Uri]/api/data/v9.2/example_records(example_key1=3,example_key2=3)?$select=example_recordid HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json
Prefer: return=representation
Content-Type: application/json

{ "example_name": "3:3 Updated" }
```

**Response**

```js
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal
ETag: W/"71004880"
OData-Version: 4.0

{
  "@odata.context": "[Organization Uri]/api/data/v9.2/$metadata#example_records(example_recordid)/$entity",
  "@odata.etag": "W/\"71004880\"",
  "example_recordid": "ef0d112e-d70e-ed11-82e5-00224822577b"
}
```

## Limit upsert operations
An upsert ordinarily operates by creating an entity if it doesn't exist; otherwise, it updates an existing entity. However, `ETags` can be used to further constrain upserts to either prevent creates or to prevent updates.


### Prevent create in upsert
If you are updating data and there is some possibility that the entity was deleted intentionally, you will not want to re-create the entity. To prevent this, add an If-Match header to the request with a value of *.

**Request**

```js
PATCH [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001) HTTP/1.1  
Content-Type: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0  
If-Match: *  
  
{  
    "name": "Updated Sample Account ",  
    "creditonhold": true,  
    "address1_latitude": 47.639583,  
    "description": "This is the updated description of the sample account",  
    "revenue": 6000000,  
    "accountcategorycode": 2  
} 
```

**Response**

If the entity is found, you'll get a normal response with status 204 No Content. When the entity is not found, you'll get the following response with status 404 Not Found.

```js
HTTP/1.1 404 Not Found  
OData-Version: 4.0  
Content-Type: application/json; odata.metadata=minimal  
  
{  
 "error": {  
  "code": "",  
  "message": "account With Id = 00000000-0000-0000-0000-000000000001 Does Not Exist"
 }  
}  
```
### Prevent update in upsert
If you're inserting data, there is some possibility that a record with the same id value already exists in the system and you may not want to update it. To prevent this, add an If-None-Match header to the request with a value of "*".

**Request**
```js
PATCH [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001) HTTP/1.1  
Content-Type: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0  
If-None-Match: *  
  
{  
    "name": "Updated Sample Account ",  
    "creditonhold": true,  
    "address1_latitude": 47.639583,  
    "description": "This is the updated description of the sample account",  
    "revenue": 6000000,  
    "accountcategorycode": 2  
} 
``` 

**Response**

If the entity isn't found, you will get a normal response with status 204 No Content. When the entity is found, you'll get the following response with status 412 Precondition Failed.
```js
HTTP/1.1 412 Precondition Failed  
OData-Version: 4.0  
Content-Type: application/json; odata.metadata=minimal  
  
{  
  "error":{  
   "code":"",  
   "message":"A record with matching key values already exists."
  }  
}  
```

# Basic delete
A delete operation is very straightforward. Use the DELETE verb with the URI of the entity you want to delete. This example message deletes an account entity with the primary key accountid value equal to `00000000-0000-0000-0000-000000000001`.

**Request**
```js
DELETE [Organization URI]/api/data/v9.2/accounts(00000000-0000-0000-0000-000000000001) HTTP/1.1  
Content-Type: application/json  
OData-MaxVersion: 4.0  
OData-Version: 4.0  
```

**Response**

If the entity exists, you'll get a normal response with `status 204` to indicate the delete was successful. If the entity isn't found, you'll get a response with `status 404`.
```
HTTP/1.1 204 No Content  
OData-Version: 4.0  
```

# Associate and disassociate 
Using single-valued navigation properties
For existing records on the many side of a one-to-many or many-to-one relationship, you can associate the record by setting a Uri reference to the other record. The easiest and most common way to do this is by appending the @odata.bind annotation to the name of the single-valued navigation property and then setting the value as the Uri to the other record in a PATCH request.

## Associate with a single-valued navigation property
For example, to associate a contact record to an account using the parentcustomerid_account single-valued navigation property:

**Request**

```js
PATCH [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f) HTTP/1.1
If-Match: *
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json

{
  "parentcustomerid_account@odata.bind": "accounts(ce9eaaef-f718-ed11-b83e-00224837179f)"
}
```

**Response**

```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
OData-EntityId: [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f)
```

As described in Associate table rows on create, new records can also be associated with existing records in the same way.

## Disassociate with a single-valued navigation property
If you want to disassociate, you can simply set the value to null.

**Request**

```js
PATCH [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f) HTTP/1.1
If-Match: *
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json

{
  "parentcustomerid_account@odata.bind": null
}
```

**Response**
```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
OData-EntityId: [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f)
```

When disassociating in this manner, you don't need to include the @odata.bind annotation. You can simply use the name of the single-valued navigation property:

**Request**

```js
PATCH [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f) HTTP/1.1
If-Match: *
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json

{
  "parentcustomerid_account": null
}
```

**Response**

```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
OData-EntityId: [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f)
```


There are other ways to achieve the same results described above with single-valued navigation properties.

### Using `PUT` request
You can use the following `PUT` request to set the value of the `parentcustomerid_account` single-valued navigation property:

**Request**

```js
PUT [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f)/parentcustomerid_account/$ref HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json

{
  "@odata.id": "[Organization URI]/api/data/v9.2/accounts(ce9eaaef-f718-ed11-b83e-00224837179f)"
}
```

**Response**

```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
```

> **Note: You must use an absolute URL when setting the value for @odata.id.**

### Using `DELETE` request

To remove the reference, you can also use this DELETE request:

**Request**
```js
DELETE [Organization Uri]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f)/parentcustomerid_account/$ref HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json
```

**Response**
```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
```

## Add a record to a collection
The following example shows how to add a contact record to the account contact_customer_accounts collection which is part of a one-to-many relationship.

**Request**

```js
POST [Organization Uri]/api/data/v9.2/accounts(ce9eaaef-f718-ed11-b83e-00224837179f)/contact_customer_accounts/$ref HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json

{
  "@odata.id": "[Organization URI]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f)"
}
```

**Response**

```
HTTP/1.1 204 NoContent
OData-Version: 4.0
```

The following example shows how to add a role record to the systemuser systemuserroles_association collection which is a many-to-many relationship.

**Request**
```js
POST [Organization Uri]/api/data/v9.2/systemusers(34dcbaf5-f718-ed11-b83e-00224837179f)/systemuserroles_association/$ref HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json

{
  "@odata.id": "[Organization URI]/api/data/v9.2/roles(886b280c-6396-4d56-a0a3-2c1b0a50ceb0)"
}
```

**Response**

```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
```

## Remove a record from a collection
The following example shows how to remove a contact record to the account contact_customer_accounts collection where the contact contactid value is cf9eaaef-f718-ed11-b83e-00224837179f.

**Request**

```js
DELETE [Organization Uri]/api/data/v9.2/accounts(ce9eaaef-f718-ed11-b83e-00224837179f)/contact_customer_accounts(cf9eaaef-f718-ed11-b83e-00224837179f)/$ref HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json
```

**Response**

```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
```

The following also works:

**Request**
```js
DELETE [Organization Uri]/api/data/v9.2/accounts(ce9eaaef-f718-ed11-b83e-00224837179f)/contact_customer_accounts/$ref?$id=[Organization URI]/api/data/v9.2/contacts(cf9eaaef-f718-ed11-b83e-00224837179f) HTTP/1.1
OData-MaxVersion: 4.0
OData-Version: 4.0
If-None-Match: null
Accept: application/json
```

**Response**
```js
HTTP/1.1 204 NoContent
OData-Version: 4.0
```

# Merge table rows using the Web API

When you find duplicate records you can combine them into one using the Merge Action.
Merge is an unbound action that accepts four parameters:

Name	|Type	|Description	|Optional
--|--|--|--
Target	|crmbaseentity	|The target of the merge operation.	|No
Subordinate	|crmbaseentity	|The entity record from which to merge data.	|No
UpdateContent |crmbaseentity	|Additional entity attributes to be set during the merge operation.	|Yes
PerformParentingChecks	|Boolean	|Indicates whether to check if the parent information is different for the two entity records.	|No

Merging will move any data from the Subordinate record to the Target and then deactivate the Subordinate record. To perform this operation the caller must have privileges and access rights to both the records identified as the Target and Subordinate.

Use a `POST` request to send data to merge records. This example merges two account entity records while updating accountnumber property of the record that will remain after the merge.

**Request**

```js
POST [Organization URI]/api/data/v9.0/Merge HTTP/1.1
Content-Type: application/json; charset=utf-8
OData-MaxVersion: 4.0
OData-Version: 4.0
Accept: application/json

{
  "Target": {
    "name": "Account 1",
    "accountid": "bb8055c0-aea6-ea11-a812-000d3a55d474",
    "@odata.type": "Microsoft.Dynamics.CRM.account"
  },
  "Subordinate": {
    "name": "Account 2",
    "accountid": "c38055c0-aea6-ea11-a812-000d3a55d474",
    "@odata.type": "Microsoft.Dynamics.CRM.account"
  },
  "UpdateContent": {
    "accountnumber": "1234",
    "@odata.type": "Microsoft.Dynamics.CRM.account"
  },
  "PerformParentingChecks": false
}
```

> **Important:** Because the Target, Subordinate, and UpdateContent property types are not explicitly defined by the parameter, you must include the `@odata.type` annotation to specify the type.

**Response**

```js
HTTP/1.1 204 No Content
OData-Version: 4.0
```

