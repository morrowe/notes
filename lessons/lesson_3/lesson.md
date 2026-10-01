# Урок 3. Продвинутые структуры данных

Видео: https://www.youtube.com/watch?v=7Xd1gh7YJN8

Redis умеет хранить не только простые строки. В этом уроке рассматриваются множества, упорядоченные множества, списки, хэши и HyperLogLog.

## Set

`Set` — неупорядоченное множество уникальных строк.

Добавить:

```redis
SADD users "User"
SADD users "Ivan"
SADD users "User"
```

Второй `User` не создаст дубликат.

Получить элементы:

```redis
SMEMBERS users
```

Проверить наличие:

```redis
SISMEMBER users "User"
```

Удалить:

```redis
SREM users "Ivan"
```

## Sorted Set

`Sorted Set` хранит элементы со значением `score`.

```redis
ZADD rating 100 "User"
ZADD rating 80 "Ivan"
ZADD rating 95 "Anna"
```

Получить элементы с баллами:

```redis
ZRANGE rating 0 -1 WITHSCORES
```

Такая структура удобна для:

- рейтингов;
- таблиц лидеров;
- сортировки по числовому показателю.

## List

`List` — упорядоченный список.

Добавить в начало:

```redis
LPUSH tasks "task 1"
```

Добавить в конец:

```redis
RPUSH tasks "task 2"
```

Получить элементы:

```redis
LRANGE tasks 0 -1
```

Удалить первый:

```redis
LPOP tasks
```

Удалить последний:

```redis
RPOP tasks
```

List удобно использовать для очередей.

## Hash

`Hash` похож на словарь внутри одного ключа.

```redis
HSET user:1 name "User" age 20 city "City"
```

Получить поле:

```redis
HGET user:1 name
```

Получить все:

```redis
HGETALL user:1
```

Удалить поле:

```redis
HDEL user:1 city
```

Hash удобен для хранения объекта пользователя:

```text
user:1
 ├─ name
 ├─ age
 └─ city
```

## HyperLogLog

HyperLogLog используется для приблизительного подсчета уникальных элементов при небольшом расходе памяти.

Добавление:

```redis
PFADD visitors user1 user2 user3
```

Количество уникальных:

```redis
PFCOUNT visitors
```

Это удобно, например, для подсчета уникальных посетителей.

## Практика

Создать очередь:

```redis
RPUSH tasks "task 1"
RPUSH tasks "task 2"
RPUSH tasks "task 3"
LRANGE tasks 0 -1
```

Создать пользователя:

```redis
HSET user:1 name "User" age 20
HGETALL user:1
```

Создать рейтинг:

```redis
ZADD rating 100 "User"
ZADD rating 70 "Ivan"
ZADD rating 90 "Anna"
ZRANGE rating 0 -1 WITHSCORES
```

## Кратко

```text
Set        уникальные элементы
Sorted Set элементы + score
List       упорядоченный список
Hash       поля и значения
HyperLogLog приблизительный подсчет уникальных элементов
```
