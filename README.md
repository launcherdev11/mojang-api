# Mojang API
API для работы с профилями Minecraft, скинами, UUID и другими сервисами Mojang.

**Важные замечания:**
- Все публичные API имеют ограничение по количеству запросов: 600 запросов на 10 минут
- Результаты должны кэшироваться
- Demo-аккаунты могут быть включены или не включены в ответы


## Version: 1.0.0

**Contact information:**  
Mojang  
https://www.mojang.com  

**License:** [Creative Commons Attribution Share Alike](http://creativecommons.org/licenses/by-sa/3.0/)

### /check

#### GET
##### Summary:

Проверка статуса API

##### Description:

Возвращает статус различных сервисов Mojang. Возможные значения - green (нет проблем), yellow (некоторые проблемы), red (сервис недоступен)

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |

### /users/profiles/minecraft/{username}

#### GET
##### Summary:

Получить UUID по имени пользователя

##### Description:

Возвращает UUID пользователя по имени в указанный момент времени.

- Параметр `?at=0` можно использовать для получения UUID оригинального владельца имени (работает только если имя менялось хотя бы раз, или аккаунт legacy)
- Timestamp - это UNIX timestamp (без миллисекунд)
- Если параметр `at` не указан, используется текущее время


##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| username | path | Имя пользователя Minecraft | Yes | string |
| at | query | UNIX timestamp (без миллисекунд) | No | long |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |
| 204 | Игрок с таким именем не найден |
| 400 | Неверный timestamp |

### /user/profiles/{uuid}/names

#### GET
##### Summary:

История имён пользователя

##### Description:

Возвращает все имена пользователя, которые использовались в прошлом и текущее. UUID должен быть указан без дефисов.

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uuid | path | UUID игрока без дефисов | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |

### /profiles/minecraft

#### POST
##### Summary:

Получить UUID для нескольких имён

##### Description:

Возвращает UUID и дополнительную информацию для списка игроков.

- Имена в ответе приводятся к правильному регистру
- `legacy` появляется только когда true (профиль не мигрирован на mojang.com)
- `demo` появляется только когда true (неоплаченный аккаунт)
- Content-Type должен быть application/json
- Максимум 100 имён за запрос


##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |
| 400 | Неверный запрос (null или пустое имя, или более 100 имён) |

### /session/minecraft/profile/{uuid}

#### GET
##### Summary:

Получить профиль с скином и плащом

##### Description:

Возвращает имя игрока и дополнительную информацию (скины, плащи).

**Строгие ограничения по количеству запросов:**
- Один и тот же профиль можно запрашивать раз в минуту
- Количество уникальных запросов не ограничено


##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uuid | path | UUID игрока без дефисов | Yes | string |
| unsigned | query | Если false, включает подпись в ответ | No | boolean |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |

### /user/profile/{uuid}/skin

#### POST
##### Summary:

Изменить скин (по URL)

##### Description:

Устанавливает скин для выбранного профиля. Сервера Mojang загрузят скин по указанному URL.
Работает также для legacy аккаунтов.


##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uuid | path | UUID игрока без дефисов | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Скин успешно изменён |
| 400 | Ошибка при изменении скина |
| 401 | Неавторизован |

##### Security

| Security Schema | Scopes |
| --- | --- |
| bearerAuth | |

#### PUT
##### Summary:

Загрузить скин (файл)

##### Description:

Загружает скин на сервера Mojang и устанавливает его для пользователя.
Работает также для legacy аккаунтов.


##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uuid | path | UUID игрока без дефисов | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Скин успешно загружен |
| 400 | Ошибка при загрузке скина |
| 401 | Неавторизован |

##### Security

| Security Schema | Scopes |
| --- | --- |
| bearerAuth | |

#### DELETE
##### Summary:

Сбросить скин

##### Description:

Сбрасывает скин пользователя на стандартный

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| uuid | path | UUID игрока без дефисов | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Скин успешно сброшен |
| 400 | Ошибка при сбросе скина |
| 401 | Неавторизован |

##### Security

| Security Schema | Scopes |
| --- | --- |
| bearerAuth | |

### /blockedservers

#### GET
##### Summary:

Список заблокированных серверов

##### Description:

Возвращает список SHA1 хэшей, используемых для проверки адресов серверов при подключении клиента.

Клиенты проверяют имя в нижнем регистре, используя кодировку ISO-8859-1.
Также проверяются поддомены с заменой каждого уровня на `*`.


##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |

### /orders/statistics

#### POST
##### Summary:

Статистика продаж

##### Description:

Получить статистику продаж Minecraft

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Успешный ответ |
