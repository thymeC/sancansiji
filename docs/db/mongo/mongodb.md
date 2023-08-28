# Mongodb

Reference:
- [mongodb.com](https://www.mongodb.com/docs/atlas/getting-started/)

## Mongodb resource
mongodb atlas, 云服务，用512M免费的空间

## Connect to mongodb atlas:
1. mongosh  
mongosh "mongodb+srv://cluster0.xxx.mongodb.net/" --apiVersion 1 --username chenjing  
2. compass  
mongodb+srv://chenjing:<pwd>@cluster0.xxx.mongodb.net/


## Knowledge before use mongo
Only 6 Data Types supported by JSON

- String
- Number
- Object
- Array
- Boolean
- Null

JSON - String, item with "

```javascript
'{"type": "Phone",
  "Price": 120
}'
```


JavaScript Object - map, key is not required to surround with "
```javascript
{
type: "Phone",
price: 120
}
```

JSON to JS Obj: JSON.parse()  
JS Obj to JSON: JSON.stringify()

Documents in MongoDB are stored in BSON format 
BSON binary JSON

Extended JSON is used to represent BSON data types
```javascript
{
"_id": ObjectID("xxx"),
"startTime": ISODate("2023-xx")
}

```

### Mongo Shell 
mongo shell is a javascript shell, it can be used to interact with mongodb instance

```shell
db.version()

db.help()

show dbs
admin
config
local

use admin
show collections
use local

db.stats()

```


## Issue Record
### Issue1: connection failed
- Solution1: check your ip by whatisyourip.com, then add it to the whitelist of mongodb atlas  
For my case, my ip changed for I used virtual server, disconnect the server and wait for the IP to recover to the original one, then add it to the whitelist of mongodb atlas



