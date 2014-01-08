- fan out on read
	- create shard on to
	- look up reader and pull message


- fan out on write
	- create shard on recipient
	- create new msg document for each recipient

- fan out on write with buckets
	- create buckets and paged documents with X msgs
	  for each recipient
