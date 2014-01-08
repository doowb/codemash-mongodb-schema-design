### Ability to look up by a number of different identities
 - Username
 - Email
 - Facebook
 - Github

### Single Document by User

```js
db.users.findOne()
{ _id: "joe",
	email: "joe@example.com",
	fb: "joe.smith",
	li: "joe.e.smith",
	other: {}
}

db.shardCollection("<dbname>.users", { _id: 1 });

```

 - Lookup by shard key is routed to 1 shard
 - Lookup by other identity is scatter gathered across all shards
 - Secondary keys cannot have a unique index

 ### Document per Identity
 ```js
 db.identities.ensureIndex({identifier: 1}, {unique: true})

 // create document for each users identity
 db.identities.save(
 	{identifier: { hndl: "joe"}, user_id: '1200-42'}
 )

 db.identities.save(
 	{identifier: { fb: "joe.smith"}, user_id: '1200-42'}
 )
 ```

 - Lookup to Identities is a routed query
 - Lookup to Users is a routed query
 - Unique indexes available
 - Must do two queries per lookup