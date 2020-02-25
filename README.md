# GDS Атласа

# Atlasbus GDS

  * формат взаимодействия — REST API;
  * формат данных — JSON;
  * мнемонические HTTP коды ответа;
  * в случае ошибки в ответе присутствует поле `error`:
  ```
  {
      "error": {
          "code": <код ошибки>,
          "message": <расшифровка ошибки>
      }
  }
  ```



### Методы API
#### <a name="async-api">Асинхронное API</a>
Методы отмеченные * являются асинхронными. Первый запрос инициализирует задачу, статус ответа в случае успешной инициализации —`202 Accepted`.
Для получения результата работы асинхронного метода делаются GET-запросы с url запроса, который инициировал запрос,
до тех пор пока метод не вернет статус `200 OK`, либо не вернет статус ошибки. Рекомендуется вызывать GET для получения результата не чаще, чем раз в секунду.

#### 1. Список всех пунктов отправления/назначения
  * Запрос:
  ```http
  GET /rides/endpoints
  ```
  * Ответ:
  ```
  [
    {
      "id": <string, id пункта>,
      "name": <string, название пункта>,
      "region_name": <string, название региона>,
      "country": <string, iso код страны>,
      "longitude": <float>,
      "latitude": <float>
    },
    ...
  ]
  ```

#### 2. Список всех возможных сегментов направлений
  * Запрос:
  ```http
  GET /rides/segments
  ```
  * Ответ:
  ```
  [
    [
      <raw, id пункта отправления>,
      <raw, id пункта назначения>
    ],
    ...
  ]
  ```

#### 3. Поиск рейсов
  * Запрос:
  ```http
  GET /rides/search?from-id=<point-id>&to-id=<point-id>&date=<date>
  ```
  | parameter | location |          type |
  | --------- | --------:| -------------:|
  | `from-id` |    query |           string |
  | `to-id`   |    query |           string |
  | `date`    |    query | [date](#date) |
  | `passengers-count`|    query | int |



#### 4. Параметры бронирования рейса[*](#async-api)
  * Запрос:
  ```http
  GET /rides/<ride-id>/book-params?from-id=<from-id>&to-id=<to-id>&date=<date>
  ```
  | parameter | location |          type |
  | --------- | --------:| -------------:|
  | `ride-id` |    query |           string |
  | `from-id` |    query |           string |
  | `to-id`   |    query |           string |
  | `date`    |    query | [date](#date) |
  

#### 5. Бронирование[*](#async-api)
  * Запрос:
  ```http
  POST /rides/<ride-id>/book
  ```


#### 7. Подтверждение бронирования
  * Запрос:
  ```http
  POST /orders/<order-id>/confirm
  ```
  | parameter  | location | type |
  | ---------- | --------:| ----:|
  | `order-id` |    query |  raw |

#### 8. Получение информации о билете
  * Запрос:
  ```http
  GET /tickets/<ticket-id>
  ```
  | parameter   | location | type |
  | ------------| --------:| ----:|
  | `ticket-id` |    query |  raw |


#### 9. Расчет возврата
  * Запрос:
  ```http
  GET /tickets/<ticket-id>/calc-refund
  ```
  | parameter   | location | type |
  | ----------- | --------:| ----:|
  | `ticket-id` |    query |  raw |


#### 10. Возврат
  * Запрос:
  ```http
  POST /tickets/<ticket-id>/refund
  ```
  | parameter   | location | type |
  | ----------- | --------:| ----:|
  | `ticket-id` |    query |  raw |
  * Ответ:
  ```
  {
      "id": <raw, optional>,
      "price": <decimal>, // сумма к возврату
  }
  ```

### Формат даты и времени
<a name="date">date: YYYY-MM-DD</a>

<a name="datetime">datetime: YYYY-MM-DDThh:mm:ss.ssssss±hh:mm</a>
