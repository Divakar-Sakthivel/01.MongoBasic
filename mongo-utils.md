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




