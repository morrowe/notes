# Шпаргалка Redis

## Подключение

```bash
docker run --name redis -p 6379:6379 -d redis
docker exec -it redis redis-cli
```

## Проверка

```redis
PING
INFO
```

## Strings

```redis
SET key value
GET key
DEL key
EXISTS key
INCR counter
DECR counter
INCRBY counter 5
MSET a 1 b 2
MGET a b
APPEND key value
STRLEN key
```

## Keys

```redis
KEYS *
SCAN 0
TTL key
EXPIRE key 60
PERSIST key
```

## Lists

```redis
LPUSH list value
RPUSH list value
LRANGE list 0 -1
LPOP list
RPOP list
```

## Sets

```redis
SADD users User
SMEMBERS users
SISMEMBER users User
SREM users User
```

## Sorted Sets

```redis
ZADD rating 100 User
ZRANGE rating 0 -1 WITHSCORES
```

## Hashes

```redis
HSET user:1 name User age 20
HGET user:1 name
HGETALL user:1
HDEL user:1 age
```

## HyperLogLog

```redis
PFADD visitors user1 user2
PFCOUNT visitors
```

## Pub/Sub

Терминал 1:

```redis
SUBSCRIBE news
```

Терминал 2:

```redis
PUBLISH news "Hello"
```

## Transactions

```redis
MULTI
SET a 10
INCR a
EXEC
```

## Python

```bash
pip install redis
```

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

client.ping()
client.set("name", "User")
print(client.get("name"))
```
