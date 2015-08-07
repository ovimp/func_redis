# func_redis

func_redis is an Asterisk module to allow the use of Redis from the dialplan.
It depends on the hiredis library.

This project was initially forked from Sergio Medina Toledo's func_redis (https://github.com/tic-ull/func_redis) to add hash operations, authentication and database selection.

It's expected that ongoing changes will make this fork no longer backward-compatible with the original
project.

This has been tested with Asterisk 1.8 and 11.

## Using func_redis

In order to use func_redis you need to configure the settings for the module 
in the file func_redis.conf. There is an example in samples/func_redis.conf.sample

Example:

```
[general]
; host of the redis server
; if not defined, use default of 127.0.0.1
hostname=127.0.0.1

; port of the redis server
if not defined, use default of 6379
port=6379

; database index in redis
; if not defined, use default of 0
database=0

; password for redis server
; if not defined, authentication will not be used
;password=s3cr3tp@ssw0rd

; connection time out when connecting to the server
; if not defined, use a default of 5 seconds
timeout=3
```


### Using func_redis from dialplan

#### Set a key value
```same => n,Set(REDIS(test)=${TEST})```

#### Set a hash value
```same => n,Set(REDIS(test,field)=${TEST})```

#### Get the value from a key
```same => n,Set(TEST=${REDIS(test)})```

#### Get the value from a hash
```same => n,Set(TEST=${REDIS(test,field)})```

#### Delete a key
```same => n,NoOp(Deleting test key ${REDIS_DELETE(test)})```

#### Check if a key exists
```same => n,GotoIf(${REDIS_EXISTS(test)}?exists:doesnt_exist)```

#### Publish a message to a redis channel
```same => n,Set(REDIS_PUBLISH(channel)=test)```

### Using func_redis from the CLI

You can use these commands related to func_redis in the Asterisk CLI 

1. ```redis show [pattern]```

    Shows all of the key values.

    [pattern] pattern to match keys

    Examples :
        - h?llo matches hello, hallo and hxllo
        - h*llo matches hllo and heeeello
        - h[ae]llo matches hello and hallo, but not hillo

2. ```redis hshow <hash>```

    Shows all of the hash's values for a given hash.
    
3. ```redis set <key> <value>```

    Sets the key's <key> value to <value>.

   ```redis set <key> <hash> <value>```

    Sets the hash's <hash> value <value> for a given key <key>.
    
4. ```redis del <key>```

    Deletes the key.
