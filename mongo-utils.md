### Basic

- Connect to mongo:
```
mongo "<Atlas Cluster URI>"
```

- List all databases:
```
show db
```

- Select a database:
```
use <db_name>
```

- List all collections:
```
show collections
```

- Find something:
```
db.<collection_name>.find( {"field": "value"} )
```
Find command returns a cursor and show only 20 documents per page. To iterate through a cursor page, we need to use `it`.
If a query is not used, find will return any 20 documents from database.


- Count the results from find:
```
db.<collection_name>.find( {"field": "value"} ).count()
```

- Show formatted documents:
```
db.<collection_name>.find( {"field": "value", "field": "value"} ).pretty()
```

- Find one random document within a collection:
```
db.<collection_name>.findOne()
```

- Insert one document into a collection:
```
db.<collection_name>.insert(<json_document>)
```

- Insert multiple documents into a collection:
```
db.<collection_name>.insert([<json_document>, <json_document>])
```

- Insert multiple documents without order:
```
db.<collection_name>.insert([<json_document>, <json_document>], { "ordered": false })
```
Documents are inserted in order by default. 
With the `ordered` option set as `false`, all valid documents will be inserted.


### Update
Updates are done by using MQL operators.

- Increase a value (sum with the previous one):
```
db.<collection_name>.updateMany( {"fieldToQuery": "valueToFilter}, {"$inc": { "fieldToIncrement": <value_to_increment>, "fieldToIncrement": <value_to_increment>, ... }} )
```

- Set a new value:
```
db.<collection_name>.updateOne( {"fieldToQuery": "valueToFilter}, {$set: { "fieldToSet": <value_to_set> }} )
```

- Add a new element to an array:
```
db.<collection_name>.updateOne( {"fieldToQuery": "valueToFilter}, {$push: { "arrayName": <json_object_to_insert> }} )
```

### Delete

- Delete one document:
```
db.<collection_name>.deleteOne( {"_id": <value>} )
```
_This should be used only when deleting by id._


- Delete many documents:
```
db.<collection_name>.deleteMany( {"fieldToQuery": "valueToFilter} )
```
_This will delete every file that matches with the specific criteria._


- Delete a collection:
```
db.<collection_name>.drop()
```

### Query Operators

```
db.<collection_name>.find( { <field_name>: { $<operator>: <value> } } )
```

Comparison operators:
- $eq: EQual to
- $ne: Not Equal to
- $gt: Greater Than
- $lt:Lesser Than
- $gte: Greater Than or Equal to
- $lte: Lesser Than or Equal to

```
db.<collection_name>.find( { "tripduration: { $lte: 70 }, "usertype": { $ne: "Subscriber" } } )
```

---

Logic operators:
- $and: Match all
- $or: At least one
- $nor: Fail to both
- $not: Negate the query

```
db.<collection_name>.find( { <operator> : [ {statement1},{statement2},... ] } )
```

_The $not operator doesn't need [] to be used:_
```
db.<collection_name>.find( { $not : {statement1} } )
```


The default operator is $and. It must be explict only when you need to include 
the same operator more than once in a query:
```
db.<collection_name>.find( { $and : [ {$or:[{dst_airport: "KZN"}, {src_airport: "KZN"}]}, {$or:[{airplane: "CR2"}, {airplane: "A18"}]} ] } )
```

Expressive Query Operator: $expr
```
db.<collection_name>.find({ $expr: <expression> })
```

Comparing the same field:
```
db.<collection_name>.find({ 
    $expr:  {
        $eq: ["$end station id", "$start station id"]
    }
}).count()
```

Example with aggregation:
```
db.<collection_name>.find({ 
    $expr:  {
        $and: [
            { $gt: ["$tripduration", 1200] },
            { $eq: ["$end station id", "$start station id"] }
        ]
    }
}).count()
```
