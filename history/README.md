- Retain limited amount of history
- Query by match/ranges

### Solutions

#### Bucket by number of messages

```js
db.inbox.find()
{ owner: "Joe", sequence: 25,
	messages: [
		{ from: "Joe",
		  to: ["Bob", "Jane"],
		  sent: ISODate(),
		  message: "Hi!"
		  },
		  ...
		  ]
}

db.inbox.find({owner: "friend1", messages: {
	$elemMatch: {sent: {$gte: ISODate(...)}}
}})

db.inbox.update({owner: "friend1", messages: {
	$pull: {sent: {$gte: ISODate(...)}}
}})

```

- Shrinking documents, space can be reclaimed with `db.runCommand({compact: '<collection>'});


#### Fixed size array

```js
msg = {
	from: "Your Boss",
	to: ["Bob"],
	sent: new Date(),
	message: ""
}

db.messages.update(
	{ _id: 1},
	{$push: {message: { $each: [msg], $sort: { setnt : 1 }, $slice: -50 }}}
)
```

- Need to compute the size of the array based on retention period


#### Bucket by date + TTL collections

```js
db.messages.findOne()
...

db.messages.ensureIndex({ sequence: 1}, { expireAfterSeconds: 31536000 })

// make property a future date and set to 0
{
	msg: "hi",
	expires: ISODate("2014-02-01")
}

db.messages.ensureIndex({expires: 1}, {expireAfterSeconds: 0});
```