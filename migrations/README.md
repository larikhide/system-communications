[US-160] Разбейте названия заданий [Knowledge Area] Title на knowledge area и title. Это нужно сделать во всех местах, где есть старое название.
[US-170] Необходимо анонимизировать информацию о менеджерах, чтобы агрессивные кандидаты не могли угрожать несчастным менеджерам.
# Задания для домашки:

## Опишите схему события в формате json schema для любого формального и функционального события. На выходе у вас должно получиться две схемы. Если не хотите использовать json schema, можно взять avro.

### Формальное событие. Стриминг заданий для кандидата

```
JSON Schema

{
    "$schema": "https://json-schema.org/schema/task",
     "$id": "https://example.com/task.schema.json/v0",
    "title": "TaskCreated.v0",
    "description": "Initial json schema for task created formal event",

    "definitions": {
        "payload": {
            "type": "object",
            "properties": {
                "replication_id": {"type": "string"},
                "title": {"type": "string"},
                "author_replication_id": {"type": "string"},
                "task_text": {"type": "string"},
                "created_at": {"type": "string"},

                "steps": {
                    "type": "array",

                    "items": {
                        "type": "object",
                        "properties": {
                            "replication_id": {"type": "string"},
                            "author_replication_id": {"type": "string"},
                            "text": {"type": "string"},
                            "created_at": {"type": "string"},
                        },
                        "required": ["replication_id", "author_replication_id", "text", "created_at"],
                    }
                }
            },
            "required": ["replication_id", "title", "author_replication_id", "task_text", "created_at"]
        }
    },
    "type": "object",

    "properties": {
       "event_id": {"type": "string"},
        "event_version": {"enum": [0]},
        "event_name": {"enum": ["TaskCreated"]},
        "produced_at": {"type": "string"},

        "data": { "$ref": "#/definitions/event_data"}
    },
    
    "required": ["payload", "event_id", "event_version", "event_name", "produced_at"]
}
```

### Функциональное событие. Задание зарегистрировано.

```json
{
    "$schema": "https://json-schema.org/schema/task",
     "$id": "https://example.com/task.schema.json/v0",
    "title": "TaskRegistered.v0",
    "description": "Initial json schema for task registered fucntional event",

    "definitions": {
        "payload": {
            "type": "object",
            "properties": {
                "domain_id": {"type": "string"},
                "author_replication_id": {"type": "string"},
                "created_at": {"type": "string"},
            },
            "required": ["domain_id", "author_replication_id", "created_at"]
        }
    },
    "type": "object",

    "properties": {
       "event_id": {"type": "string"},
        "event_version": {"enum": [0]},
        "event_name": {"enum": ["TaskRegistered"]},
        "produced_at": {"type": "string"},

        "data": { "$ref": "#/definitions/event_data"}
    },
    
    "required": ["payload", "event_id", "event_version", "event_name", "produced_at"]
}
```

## Опишите процесс миграции четырёх связей. Если у вас нет связи с нужным условием, можно пропустить описание миграции:
### Переход формальной синхронной на асинхронную event-driven.

- добавить новое событие в schema registry
- добваить новый консьюмер, который будет сохранять логику sync обработчика
- добавить новый продьюсер, который будет отправлять событие валидное schema registry
- выключить синхронный вызов
- удостовериться, что все ок
- удалить признаки старого кода

### Переход формальной асинхронной event-driven на синхронную.

- выбрать стратегию pull/push для синхронной связи
#### Push

- добавить новый эндпоинт на стороне потребителя повторяющий логику асинхронной связи
- добавить синхронный вызов, выключить продьюсер
- удалить коньсюмер после обработки всех событий из старого топика
- почистить метрики, логи, доку и бд

### Переход функциональной синхронной на асинхронную event-driven.
- добавить новое событие в schema registry
- добавить стаб коньсюмера
- добавляем нового продьюсера
- проверяем, что все работает
- добавляем логику в консьюмер и выключаем sync обработчик
- чистим код

### Переход функциональной асинхронной event-driven на синхронную.
- добавить синхронный обработчик в потребителе
- добавить синхронный вызов и выключаем продьюсера
- ожидаем обработки оставшихся событий в топике
- вырезаем продьюсера и коньсьюмера


## UPDATED. Опишите процесс миграции одной формальной и одной функциональной связи для новых требований бизнеса. Стартовой точкой миграции стоит считать уже исправленную связь, т.е. если вы решили использовать формальную event-driven коммуникацию для передачи заданий — описывайте миграцию на новое требование сразу с асинхронной на асинхронную связь.
По итогу должно получиться два описания, одно для формальной или функцинальной связи касающейся [US-160] и одно для [US-170] (какую связь для какого требования выбрать — решайте на свое усмотрение).
tbd

## Подготовить описание способов решения проблем вокруг зачисления и списания средств, так как бизнес переживает что что-то пойдет не так и деньги потеряются. Нужно составить список подходов, которые помогут не потерять данные, объяснить почему именно эти подходы нужны и как будут обрабатываться ошибки в случае проблем.
 tbd

# memes
tbd
