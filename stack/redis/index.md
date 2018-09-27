REDIS
=====
* [Try Redis -- Hands On](https://try.redis.io/)
* [Redis Summary](https://www.tutorialspoint.com/redis/)

NOTES
=====

* In memory KV store with rich data types (string, hash, list, set, etc)
```
$ redis-cli -h host -p port -a password
redis 127.0.0.1:6379> COMMAND KEY_NAME
redis 127.0.0.1:6379> SET tutorialspoint redis
```

COMMANDS
========

* Keys
DEL key
This command deletes the key, if it exists.

DUMP key
This command returns a serialized version of the value stored at the specified key.

EXISTS key
This command checks whether the key exists or not.

KEYS pattern
Finds all keys matching the specified pattern.

TYPE key
Returns the data type of the value stored in the key.

* Strings
SET key value
This command sets the value at the specified key.

GET key
Gets the value of a key.

STRLEN key
Gets the length of the value stored in a key

INCR key
Increments the integer value of a key by one

APPEND key value
Appends a value to a key

* Hashes
HASHES
HDEL key field2 [field2]
Deletes one or more hash fields.

HEXISTS key field
Determines whether a hash field exists or not.

HGET key field
Gets the value of a hash field stored at the specified key.

HGETALL key
Gets all the fields and values stored in a hash at the specified key

* Lists

BLPOP key1 [key2 ] timeout
Removes and gets the first element in a list, or blocks until one is available

BRPOP key1 [key2 ] timeout
Removes and gets the last element in a list, or blocks until one is available

BRPOPLPUSH source destination timeout
Pops a value from a list, pushes it to another list and returns it; or blocks until one is available

LINDEX key index
Gets an element from a list by its index

* Sets
SADD key member1 [member2]
Adds one or more members to a set

SCARD key
Gets the number of members in a set

SDIFF key1 [key2]
Subtracts multiple sets

* SORTED SET
ZADD key score1 member1 [score2 member2]
Adds one or more members to a sorted set, or updates its score, if it already exists
