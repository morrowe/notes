# Урок 2. Основы работы с Redis

Видео: https://www.youtube.com/watch?v=5CquDNYN37U

В этом уроке основа работы с Redis — строки и пары **ключ → значение**.

## SET — записать значение

```redis
SET name "User"
```

Теперь ключ `name` содержит значение `User`.

## GET — получить значение

```redis
GET name
```

Результат:

```text
"User"
```

## DEL — удалить ключ

```redis
DEL name
```

После удаления:

```redis
GET name
```

вернет `nil`.

## EXISTS — проверить существование

```redis
EXISTS name
```

Если ключ есть:

```text
1
```

Если нет:

```text
0
```

## Ключи

Лучше использовать понятные имена:

```text
user:1:name
user:1:email
product:10:price
session:abc123
```

Двоеточие здесь используется как разделитель частей имени.

## Числа

Redis может хранить число как строковое значение и выполнять над ним операции.

```redis
SET counter 10
INCR counter
GET counter
```

После `INCR` значение станет `11`.

Уменьшение:

```redis
DECR counter
```

Увеличение на несколько:

```redis
INCRBY counter 5
```

## Работа со строкой

Добавить текст в конец:

```redis
APPEND message "Hello"
```

Получить длину:

```redis
STRLEN message
```

## Несколько значений

Можно записать сразу несколько ключей:

```redis
MSET name "User" city "City"
```

Получить несколько:

```redis
MGET name city
```

## Просмотр ключей

Для учебных целей можно использовать:

```redis
KEYS *
```

Но на больших production-базах `KEYS *` может быть тяжелой операцией. Для безопасного перебора ключей обычно используют `SCAN`.

## Практика

Создать пользователя:

```redis
SET user:1:name "User"
SET user:1:age 20
SET user:1:city "City"
```

Получить данные:

```redis
GET user:1:name
GET user:1:age
GET user:1:city
```

Изменить возраст:

```redis
INCR user:1:age
```

Удалить город:

```redis
DEL user:1:city
```

## Кратко

```text
SET    записать
GET    получить
DEL    удалить
EXISTS проверить наличие
INCR   увеличить число
DECR   уменьшить число
MSET   записать несколько
MGET   получить несколько
```
