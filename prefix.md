### Форматы JSON-ответов

* `status` — указывает статус-код. Присутствует **всегда**:
```json
{
    "status": "OK"
}
```

* `response` — хранит ответ на поступивший запрос. Может быть и массивом, и объектов в зависимости от запроса:
```json
{
    "status": "OK",
    "response": {
        "1": true,
        "2": false
    }
}
```

* `message` — код сообщения, который предполагается для того, чтобы показать пользователю:
```json
{
    "status": "Bad Request",
    "message": "login.bad_password"
}
```

* `problemInfo` — объект, показывающий проблемное место
  * `contract` — нарушенный контракт метода
    ```json
    {
        "status": "Bad Request",
        "message": "user.get.contract_violated",
        "problemInfo": {
            "contract": "(id == null) != (username == null)"
        }
    }
    ```
  * `badParameter` — некорректный параметр
    ```json
    {
        "status": "Bad Request",
        "message": "exception.bad_parameter",
        "problemInfo": {
            "badParameter": "id"
        }
    }
    ```

* `exceptionClass` — класс исключения. Появляется только при внутренней ошибки сервера:
* `stackTrace` — стектрейс исключения. Появляется только при внутренней ошибки сервера:
  ```json
  {
      "status": "Internal Server Error",
      "exceptionClass": "java.lang.IllegalStateException",
      "stackTrace": [
          "ru.smashup.application.controllers.entities.TrackController.get(TrackController.java:24)",
          "java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)",
          "java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)",
          "java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)",
          "java.base/java.lang.reflect.Method.invoke(Method.java:568)",
          "org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:207)",
          "org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:152)",
          "org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:118)",
          "org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:884)",
          "org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:797)"
      ]
  }
  ```

### Общие ссылки:
* **Поделиться** _**[ТЕСТ]** Неактуально_
  * Мэшапом: `/share/mashup?id=[ID]&sharedBy=[NICKNAME]`
  * Плейлистом: `/share/playlist?id=[ID]&sharedBy=[NICKNAME]`
* **Контент**
  * Получить мэшап: `/uploads/mashup/[ID].mp3?bitrate=[MAX_BITRATE]`
  * Получить обложку мэшапа: `/uploads/mashup/[ID]_[SIZE].png`
    * Возможные значения SIZE: `[100x100, 400x400, 800x800]`
  * Получить обложку плейлиста: `/uploads/playlist/[ID]_[SIZE].png`
    * Возможные значения SIZE: `[100x100, 400x400, 800x800]`
  * Получить обложку трека: `/uploads/track/[ID]_[SIZE].png`
    * Возможные значения SIZE: `[100x100, 400x400, 800x800]`
  * Получить аватар пользователя: `/uploads/user/[ID]_[SIZE].png`
    * Возможные значения SIZE: `[100x100, 400x400, 800x800]`
  * Получить мэшап на премодерации: `/uploads/moderators/mashup/[ID].mp3` _**[ТЕСТ]** Неактуально_
  * Получить обложку мэшапа на премодерации: `/uploads/moderators/mashup/[ID].png` _**[ТЕСТ]** Неактуально_

---

### Различные константы:
* **Плейлисты:** _**[ТЕСТ]** Неактуально_
  * "Новинки": `1`
  * "Чарты | 24 часа": `2`
  * "Чарты | 7 дней": `3`
* **Биты прав пользователей:**
  * Администратор: `0`
  * Модератор: `1`
  * Верифицированный пользователь: `2`
  * Мэшапер: `3`
  * Пользователь забанен: `4`
* **Биты настроек пользователей:**
  * Включено проигрывание Explicit треков: `0`
  * Включена мульти-сессия: `1`
* **Биты статусов мэшапов:**
  * Explicit: `0`
  * #mashup: `0`
  * \[alt]: `0`

---

### Метки:
* **[T]** - нужен токен пользователя в заголовке `Authorization` в формате `Bearer token` *(если он не указан, то всегда возвращается 401 код)*
* **[М]** - доступен только модераторам и администраторам *(если пользователь не является модератором или администратором, то всегда возвращается 403 код)*
* **[V]** - доступен только верифицированным пользователям, модераторам и администраторам *(если пользователь не является модератором или администратором, то всегда возвращается 403 код)*
* Если возвращаемые коды не указаны, то, значит, всегда возвращается **200** *(или 401, если метод помечен как [T]; аналогично с [M])*

### Запросы: