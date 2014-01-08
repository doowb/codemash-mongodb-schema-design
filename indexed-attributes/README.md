 - Application needs to store a variable number of attributes
 	- User defined Form
 	- Meta Data tags

 - Queries needed
 	- Equality
 	- Range based

### Attributes as Embedded Document

```js
db.files.insert( { attr: { type: "text", size: 64, createdDate: ISODate() }})
db.files.insert( { attr: { type: "text", size: 128, createdDate: ISODate() }})
db.files.insert( { attr: { type: "binary", size: 356, createdDate: ISODate() }})

db.files.ensureIndex( {"attr.type": 1})
db.files.find( {"attr.type": "text" })

db.files.ensure({"attr.size": 1})
db.files.find({"attr.size": {$gt: 64}})
```

 - Each attribute needs an index (limit to 64 indexes on collection)
 - Each time you extend, you add an index
 - lots and lots of indexes

### Attributes as Objects in Array

```js
db.files.insert({_id: "",
	attr: [
		{ type: "text" },
		{ size: 64 },
		{ created: ISODate() }
	]})

db.files.insert({_id: "",
	attr: [
		{ type: "text" },
		{ size: 128 },
		{ created: ISODate() }
	]})

db.files.insert({_id: "",
	attr: [
		{ type: "binary" },
		{ size: 256 },
		{ created: ISODate() }
	]})

db.files.ensureIndex({attr:1})

```

 - Only one index needed on attr
 - Can support range queries
 - Index can be used only once per query