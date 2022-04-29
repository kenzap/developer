
# Find

This type of query performs a search operation in the Cloud. It may return one or multiple records.

## Find Parameters

```json
"query": {
    "records": {
        "type":       "find",
        "key":        key,
        "id":         id,
        "fields":     fields,
        "term":       [
                        {
                            "field": field,
                            "relation": relation,
                            "type": type,
                            "value": value
                        }
                      ],
        "search":     {         
                        "field": field,
                        "s": s
                    },
        "sortby":   {
                        "field": field,
                        "order": order
                    },
        "groupby":  [
                        {
                            "field": field,
                        }
                    ],
        "count":    count,
        "sum":      sum,
        "limit":    limit,
        "offset":   offset
    }
}
```

The table below demonstrates various parameters that you can use to customize your <code>find</code> query.

Parameter | Data&nbsp;Types | Example | Required
--------- | ---------- | ------- | ------- 
type | <code>String</code> | <code>find</code> | Yes
[key](#find-key) | <code>String</code> | <code>ecommerce-product</code> | Yes
[fields](#find-fields) | <code>String</code>&nbsp;<code>Array</code> | <code>["_id", "title", "created"]</code> | Yes
[id](#find-id) | <code>String</code> | <code>00232c6e9024c0d8d064af7754e1c60580fe4407</code> | No
[term](#find-term) | <code>Array</code>&nbsp;<code>Object</code> | <code>[{"field": "created", "relation": ">=", "type": "numeric", "value": 1651198732}]</code> | No
[count](#find-count) | <code>Array</code> | <code>["_id"]</code> | No
[sum](#find-sum) | <code>Array</code> | <code>["total", "total_tax"]</code> | No
[search](#find-search) | <code>Object</code> | <code>{ field: "title", s: "sushi" }</code> | No
[sortby](#find-sortby) | <code>Array</code>&nbsp;<code>Object</code> | <code>{ field: "created", order: "DESC" }</code> | No
[groupby](#find-groupby) | <code>Array</code>&nbsp;<code>Object</code> | <code>[ { field: "category" } ]</code> | No
[limit](#find-limit) | <code>Number</code> | <code>25</code> | No
[offset](#find-offset) | <code>Number</code> | <code>10</code> | No

### Find key

```json
"key": "ecommerce-product"
```

This parameter specifies which extension data to query within the Cloud Space. Key names are **arbitrary**, meaning that each extension has its own set of available keys. Think of keys as tables in the context of relational databases.

For example, E-commerce extension keys:

* <code>ecommerce-product</code>
* <code>ecommerce-settings</code>
* <code>ecommerce-order</code>

MyTicket extension keys:

* <code>myticket-settings</code>
* <code>myticket-layouts</code>                   

### Find fields

```json
"fields": [ 
            "_id", 
            "title", 
            "created"
          ]
```

Kenzap Cloud does not follow a strict schema, meaning that any extension can define its data structure and columns. Some fields parameter examples:

* <code>"*"</code> - query all fields
* <code>["_id", "title", "created"]</code> - query system _id, title and created fields only            

### Find id

```json
"id": "e3de841f54b88997f2e18cfd60dd06ebb956db22"
```

You can query individual record by its unique ID. IDs are generated by the Cloud when data record is first inserted into the database. ID example:

<code>e3de841f54b88997f2e18cfd60dd06ebb956db22</code>

### Find term

```json
"term": [ 
          { 
            "field": "created", 
            "relation": ">=", 
            "type": "numeric", 
            "value": 1640093421 
          }, 
          { 
            "field": "status", 
            "relation": "=", 
            "type": "string", 
            "value": "completed" 
          } 
        ]
```
You can add one or multiple terms to your queries by providing additional instructions on which fields to query and specify query relation. Examples:

* <code>{ "field": "status", "relation": "=", "value": "completed" }</code> - return records where status fields equals to "completed".
* Refer to the right pane for a more complex query term example.    

Available term relations:

* <code>&gt;</code> - greater than
* <code>&lt;</code> - less than
* <code>&gt;=</code> - greater than or equals
* <code>&lt;=</code> - less than or equals
* <code>=</code> - equals

Available term types:

* <code>string</code> - consider provided value as string
* <code>numeric</code> - consider provided value as numeri

### Find search

```json
"search": {
            "field": "title",
            "s": "sushi"
          }
```
You can perform a case insensitive search within all records of the specified field.

<code>
"search": {
          "field": "title",
          "s": "sushi"
        } 
</code>

### Find sortby

```json
"sortby": {
            "field": "created",
            "order": "DESC"
          }
```
You can sort orders by a specified field. Refer to the right pane for <code>sortby</code> example.    

Available sortby orders:

* <code>DESC</code> - sort in descending order
* <code>ASC</code> - sort in ascending order

### Find groupby

```json
"groupby": [
              {
                "field": "category"
              }
            ]
```
You can aggregate records by a specified field. For example, this operation might be useful, if you want to count the total amount of sold products but want to split these records into multiple categories.

* <code>
groupby: [
            {
              field: "category"
            }
          ]
</code>

### Find limit

```json
"limit": 25
```
Limit set the maximum number of queried records to return. If value is not provided it equals to 10 records per query. By providing value that is grater than 10 limit increases total number of returned records.

<code>
limit: 10
</code>

### Find offset

```json
"offset": 10
```
Indicates how many records to skip from the beginning of the query response. By default, this value equals 0.

<code>
offset: 10
</code>

## Find Examples

### Find First 25 Products

> Find First 25 Products:

```javascript
fetch("https://api-v1.kenzap.cloud/", {
  method: "post",
  headers: {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Authorization": "Bearer your_api_key"
  },
  body: JSON.stringify({
      query: {
          products: {
              type:       "find",
              key:        "ecommerce-product",
              fields:     ["_id", "img", "status", "price", "title"],
              limit:      25
          }
      }
  })
})
.then(response => response.json())
.then(response => {

  response.products.forEach(product => {

    console.log(product);

  })
})
```
> The above command returns JSON structured like this:

```json
{
 ...
}
```

The following example demonstrates how to query the first 25 records stored under the ecommerce-product key.

Query fields:

* <code>_id</code> - cloud assigned record ID. 
* <code>img</code> - list of available product images
* <code>status</code> - product status
* <code>price</code> - product price
* <code>title</code> - product title

### Search for "Sushi" Products 

> Search for "Sushi" Products:

```javascript
fetch("https://api-v1.kenzap.cloud/", {
  method: "post",
  headers: {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Authorization": "Bearer your_api_key"
  },
  body: JSON.stringify({
      query: {
          products: {
              type:       'find',
              key:        'ecommerce-product',
              fields:     ['_id', 'id', 'img', 'status', 'price', 'title', 'updated'],
              search:     {   
                              field: 'title',
                              s: "sushi"
                          },
              sortby:     {
                              field: 'title',
                              order: 'DESC'
                          }
          }
      }
  })
})
.then(response => response.json())
.then(response => {

  response.products.forEach(product => {

    console.log(product);

  })
})
```
> The above command returns JSON structured like this:

```json
{
 ...
}
```

The following example demonstrates how to query the first 25 records stored under the ecommerce-product key.

Query fields:

* <code>_id</code> - cloud assigned record ID. 
* <code>img</code> - list of available product images
* <code>status</code> - product status
* <code>price</code> - product price
* <code>title</code> - product title

### Generate Analytics Report

> Generate Last 30 days Analytics Report:

```javascript
let today = "2023-05-01T00:00:00.001Z"

fetch("https://api-v1.kenzap.cloud/", {
  method: "post",
  headers: {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Authorization": "Bearer your_api_key"
  },
  body: JSON.stringify({
      query: {
          by_country: {
              type:       'find',
              key:        'analytics',
              fields:     ['cc'],
              count:      ['cc'],
              term:{
                  field:  'created',
                  relation:'>',
                  type:   'numeric',
                  value:  (Date.parse(today)  / 1000 | 0) - 60 * 60 * 24 * 30
              },
              groupby: [
                  {
                      field: 'cc'
                  }
              ],
              sortby: {
                  field: 'cc_count',
                  order: 'DESC'
              },
              offset: 0,
              limit:  300
          },
          by_sessions: {
              type:       'find',
              key:        'analytics',
              fields:     ['ymd'],
              count:      ['ymd'],
              term:[
                  {
                      field:  'created',
                      relation:'>',
                      type:   'numeric',
                      value:  (Date.parse(today)  / 1000 | 0) - 60 * 60 * 24 * 30
                  },
                  {
                      field:  'tag',
                      relation:'=',
                      type:   'string',
                      value:   'session:5s'
                  },
              ],
              groupby: [
                  {
                      field: 'ymd'
                  }
              ],
              sortby: {
                  field: 'ymd_count',
                  order: 'DESC'
              },
              offset: 0,
              limit:  31
          }
      }
  })
})
.then(response => response.json())
.then(response => {

  response.by_country.forEach(country => {

    console.log(country);

  })

  response.by_sessions.forEach(session => {

    console.log(session);

  })
})
```
> The above command returns JSON structured like this:

```json
{
 ...
}
```

The following example demonstrates how to generate an analytics report that can be used, for example, in Google Charts API. 
Query fields:

* <code>_id</code> - cloud assigned record ID. 
* <code>img</code> - list of available product images
* <code>status</code> - product status
* <code>price</code> - product price
* <code>title</code> - product title

<aside class="notice">
You can specify multiple query requests to better facilitate the logics of your application
</aside>