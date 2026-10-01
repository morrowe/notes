# Урок 5. Подключение Redis к Python

Видео: https://itproger.com/course/redis/5

Для работы с Redis из Python используется библиотека `redis`.

## Установка

```bash
pip install redis
```

## Подключение

Пример:

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)
```

`host` — адрес Redis.

`port` — порт Redis.

`decode_responses=True` позволяет получать строки вместо `bytes`.

## Проверка соединения

```python
print(client.ping())
```

Если Redis доступен:

```text
True
```

## Запись

```python
client.set("name", "User")
```

## Получение

```python
name = client.get("name")
print(name)
```

## Удаление

```python
client.delete("name")
```

## Срок жизни

```python
client.set("code", "1234", ex=60)
```

Получить TTL:

```python
print(client.ttl("code"))
```

## Список

```python
client.rpush("tasks", "task 1")
client.rpush("tasks", "task 2")

tasks = client.lrange("tasks", 0, -1)
print(tasks)
```

## Hash

```python
client.hset(
    "user:1",
    mapping={
        "name": "User",
        "age": "20"
    }
)

print(client.hgetall("user:1"))
```

## Set

```python
client.sadd("users", "User")
client.sadd("users", "Ivan")

print(client.smembers("users"))
```

## Обработка ошибок

При реальном приложении соединение может быть недоступно, поэтому операции желательно выполнять с обработкой исключений:

```python
try:
    client.ping()
    print("Redis работает")
except redis.RedisError as error:
    print("Ошибка Redis:", error)
```

## Полный маленький пример

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

try:
    client.ping()

    client.set("user:1:name", "User")
    client.set("user:1:city", "City")

    print(client.get("user:1:name"))
    print(client.get("user:1:city"))

except redis.RedisError as error:
    print("Ошибка:", error)
```

## Практика

1. Запустить Redis.
2. Установить `redis`.
3. Подключиться к `localhost:6379`.
4. Проверить `ping()`.
5. Создать ключ пользователя.
6. Создать список задач.
7. Создать Hash пользователя.
8. Проверить TTL.

## Кратко

```text
pip install redis
redis.Redis(...)   подключение
ping()             проверка
set()              запись
get()              чтение
delete()           удаление
rpush()/lrange()   список
hset()/hgetall()   hash
sadd()/smembers()  set
```
