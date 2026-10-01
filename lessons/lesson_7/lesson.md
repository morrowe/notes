# Урок 7. Разработка проекта на Python + Redis

Видео: https://itproger.com/course/redis/7

В этом уроке рассматривается проект **Task Manager** на Python + Redis с графическим интерфейсом Flet.

Идея проекта:

```text
Flet GUI
   ↓
Redis
   ↓
очередь задач
   ↓
worker
   ↓
результат
   ↓
Redis Pub/Sub
   ↓
Flet GUI
```

## Flet

Flet используется для создания графического интерфейса на Python.

Установка:

```bash
pip install flet
```

Redis-клиент:

```bash
pip install redis
```

## Очередь задач

Redis List можно использовать как очередь.

Добавить задачу:

```python
client.rpush("tasks", "task 1")
```

Воркер получает задачу:

```python
task = client.lpop("tasks")
```

Таким образом:

```text
Flet → RPUSH → Redis List → LPOP → Worker
```

## Статус задачи

После обработки воркер может отправить сообщение:

```python
client.publish(
    "task_status",
    "Задача выполнена"
)
```

GUI подписывается на канал:

```python
pubsub = client.pubsub()
pubsub.subscribe("task_status")
```

Теперь приложение может получать изменения статуса.

## Почему здесь Redis

Redis подходит сразу для нескольких задач:

- List — очередь;
- Pub/Sub — уведомления;
- String/Hash — хранение простой информации;
- TTL — временные данные.

## Логика Task Manager

Пользователь:

```text
1. Открывает приложение
2. Создает задачу
3. Задача попадает в Redis
4. Worker берет задачу
5. Worker выполняет работу
6. Worker отправляет статус
7. Интерфейс показывает новый статус
```

## Упрощенная модель

```python
import redis

client = redis.Redis(
    host="localhost",
    port=6379,
    decode_responses=True
)

def add_task(task):
    client.rpush("tasks", task)

def get_task():
    return client.lpop("tasks")

def send_status(message):
    client.publish("task_status", message)
```

## Практика

Сделать консольную версию без Flet:

1. Добавлять задачи через `RPUSH`.
2. Получать задачи через `LPOP`.
3. Публиковать статус через `PUBLISH`.
4. Создать отдельного подписчика через `SUBSCRIBE`.

После этого можно подключать графический интерфейс.

## Важная идея

Один и тот же Redis может выполнять несколько ролей:

```text
Redis
├── List → очередь
├── Pub/Sub → сообщения
├── Hash → объект
├── String → простое значение
└── TTL → временные данные
```

Это один из главных практических смыслов курса.
