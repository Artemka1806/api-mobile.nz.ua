# Неофіційна документація API (api-mobile.nz.ua)

Ця документація описує неофіційний API для мобільного додатку NZ.UA

> **Примітка** Всі запити (окрім авторизації та перевірки робти апі) повинні включати токен доступу у форматі `Authorization: Bearer {access_token}`. 

## Перевірка роботи

**URL**: `https://api-mobile.nz.ua/v2/user/test`  
**Метод**: `GET`  
**Опис**: Метод для тестування працездатності API  

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
"2ololo"
```
</details>

## Авторизація

### Вхід в систему

**URL**: `https://api-mobile.nz.ua/v2/user/login`  
**Метод**: `POST`  
**Опис**: Авторизація користувача в системі

<details>
<summary><b>Тіло запиту</b></summary>

```jsonc
{
  "username": "логін_користувача",
  "password": "пароль_користувача",
  "exponentPushToken": "" // токен для push-сповіщень (можна залишити пустий рядок)
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "access_token": "токен_доступу",
  "refresh_token": "токен_оновлення",
  "expires_token": 1743087625,
  "email_hash": "хеш_електронної_пошти",
  "student_id": 42614414,
  "class_name": "11-Б",
  "class_manager_fio": "ПІБ_класного_керівника",
  "FIO": "ПІБ_учня",
  "avatar": {
    "image_url": "url_аватара",
    "datetime": 1694522283
  },
  "permissions": {
    "isuo_nzportal_children": ["nz", "nz_children"]
  },
  "error_message": ""
}
```
</details>

### Оновлення токену

**URL**: `https://api-mobile.nz.ua/v2/user/refresh-token`  
**Метод**: `POST`  
**Опис**: Оновлення простроченого токену авторизації  

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "refresh_token": "токен_оновлення"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "access_token": "токен_доступу",
  "error_message": ""
}
```
</details>

### Вихід з системи

**URL**: `https://api-mobile.nz.ua/v2/user/logout`  
**Метод**: `POST`  
**Опис**: Вихід користувача з системи 

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "exponentPushToken": "" // токен для push-сповіщень (можна залишити пустий рядок)
}
```
</details>

<b>Відповідь: *немає* (204 No Content)</b>

## Сповіщення

### Отримання сповіщень
**URL**: `https://api-mobile.nz.ua/v2/notification/`  
**Метод**: `GET`  
**Опис**: Отримання списку сповіщень користувача

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "data": [
    {
      "id": "132077056",
      "body": "Нова оцінка",
      "data": {
        "type": "add-mark",
        "studentName": "ПІБ_учня",
        "lessonName": "Назва_предмету",
        "markValue": "8",
        "comment": "",
        "lessonType": "Поточна"
      },
      "status": 1,
      "sentAt": "2025-03-03 14:17:06",
      "notViewedCount": "0"
    },
    // інші сповіщення...
  ],
  "error_message": ""
}
```
</details>

### Кількість непрочитаних сповіщень
**URL**: `https://api-mobile.nz.ua/v2/notification/unread-qty`  
**Метод**: `GET`  
**Опис**: Отримання кількості непрочитаних сповіщень

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "qty": "0",
  "error_message": ""
}
```
</details>

## Щоденник

### Щоденник за період

**URL**: `https://api-mobile.nz.ua/v2/schedule/diary`  
**Метод**: `POST`  
**Опис**: Отримання даних щоденника (розкладу) за вказаний період

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "start_date": "2025-03-03",
  "end_date": "2025-03-03"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "dates": [
    {
      "date": "2025-02-28",
      "calls": [
        {
          "call_id": 9308118,
          "call_number": 1,
          "call_time_start": "8:30",
          "call_time_end": "9:10",
          "subjects": [
            {
              "lesson": [
                {
                  "type": "Поточна",
                  "mark": "8",
                  "comment": "текст_до_оцінки"
                }
              ],
              "hometask": ["текст_домашнього_завдання"],
              "distance_hometask_id": null,
              "distance_hometask_is_closed": null,
              "subject_name": "Назва_предмету",
              "room": "Аудиторія",
              "teacher": {
                "id": 5876155,
                "name": "ПІБ_вчителя"
              }
            }
          ]
        },
        // інші уроки...
      ]
    }
  ],
  "error_message": ""
}
```
</details>

### Розклад

**URL**: `https://api-mobile.nz.ua/v2/schedule/timetable`  
**Метод**: `POST`  
**Опис**: Отримання розкладу за вказаний період

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "start_date": "2024-11-01",
  "end_date": "2024-11-30"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "dates": [
    {
      "date": "2024-11-04",
      "calls": [
        {
          "call_id": 12345,
          "call_number": 1,
          "time_start": "8:00",
          "time_end": "8:45",
          "subjects": [
            {
              "subject_name": "Алгебра",
              "room": "Кабінет №12",
              "teacher": {
                "id": 1234566,
                "name": "Прізвище І. Б."
              }
            }
          ]
        },
        // інші уроки в цей день
      ]
    },
    // інші дні
  ],
  "error_message": ""
}
```
</details>

## Дистанційне навчання

### Переглянути дистанційне завдання

**URL**: `https://api-mobile.nz.ua/v2/schedule/distance-hometask`  
**Метод**: `POST`  
**Опис**: Переглянути дистанційне завдання  

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "distance_hometask_id": 1234567
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "hometask": "HTML-текст завдання",
  "answer": null,
  "answer_files": [],
  "is_closed": false,
  "error_message": ""
}
```
</details>

### Відповісти на завдання

**URL**: `https://api-mobile.nz.ua/v2/schedule/student-answer`  
**Метод**: `POST`  
**Опис**: Відповісти на дистанційне завдання  

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "hometask_id": 14673116,
  "hometask_text": "текст_відповіді",
  "deleteFilesIdList": []
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "answer": "текст_відповіді",
  "answer_files": [],
  "error_message": ""
}
```
</details>

### Завантажити файл

**URL**: `https://api-mobile.nz.ua/v2/schedule/get-hometask-file?uuid=f3f2e850-b5d4-11ef-ac7e-96584d5248b2`  
**Метод**: `GET`  
**Опис**: Отримати файл за UUID  

**Відповідь**: бінарний файл

## Оцінки

### Оцінки з предмету

**URL**: `https://api-mobile.nz.ua/v2/schedule/subject-grades`  
**Метод**: `POST`  
**Опис**: Отримання оцінок з предмету за вказаний період

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "start_date": "2025-01-01",
  "end_date": "2025-07-30",
  "subject_id": "1111111"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "number_missed_lessons": 3,
  "lessons": [
    {
      "lesson_id": "2600587",
      "subject": "Алгебра",
      "lesson_date": "2025-02-18",
      "lesson_type": "Поточна",
      "mark": "2",
      "comment": ""
    },
    // інші оцінки...
  ],
  "error_message": ""
}
```
</details>

### Загальні оцінки

**URL**: `https://api-mobile.nz.ua/v2/schedule/student-performance`  
**Метод**: `POST`  
**Опис**: Отримання усіх оцінок та пропущених днів, уроків за вказаний період

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "start_date": "2024-09-01",
  "end_date": "2025-05-31"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "missed": {
    "days": 12,
    "lessons": 42
  },
  "subjects": [
    {
      "subject_id": "11111111",
      "subject_name": "Алгебра",
      "subject_shortname": "Алг.",
      "marks": [
        {
          "value": "8",
          "type": "Контрольна робота"
        },
        {
          "value": "8",
          "type": "Самостійна робота"
        },
        {
          "value": "8",
          "type": "Самостійна робота"
        }
      ]
    },
    // інші уроки
  ],
  "error_message": ""
}
```
</details>

### Пропущені уроки

**URL**: `https://api-mobile.nz.ua/v2/schedule/missed-lessons`  
**Метод**: `POST`  
**Опис**: Пропущені уроки за деякий період

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "start_date": "2024-09-01",
  "end_date": "2025-05-31"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "missed_lessons": [
    {
      "lesson_id": "2600587",
      "lesson_number": "2",
      "subject": "Алгебра",
      "lesson_date": "2025-02-18"
    },
    // інші пропущені уроки
  ],
  "error_message": ""
}
```
</details>

## Вчителям

### Оцінки для виставлення

**URL**: `https://api-mobile.nz.ua/v2/personnel-journal/mark-list`  
**Метод**: `POST`  
**Опис**: Отримати можливі оцінки для виставлення

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "data": [
    {
      "mark_value_id": 6,
      "code": 1,
      "value": "1",
      "description": "\"Кол\"",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-1"
    },
    {
      "mark_value_id": 7,
      "code": 2,
      "value": "2",
      "description": "Не задовільно",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-2"
    },
    {
      "mark_value_id": 8,
      "code": 3,
      "value": "3",
      "description": "Не задовільно",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-3"
    },
    {
      "mark_value_id": 9,
      "code": 4,
      "value": "4",
      "description": "Задовільно-",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-4"
    },
    {
      "mark_value_id": 10,
      "code": 5,
      "value": "5",
      "description": "Задовільно",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-5"
    },
    {
      "mark_value_id": 11,
      "code": 6,
      "value": "6",
      "description": "Задовільно+",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-6"
    },
    {
      "mark_value_id": 12,
      "code": 7,
      "value": "7",
      "description": "Добре-",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-7"
    },
    {
      "mark_value_id": 13,
      "code": 8,
      "value": "8",
      "description": "Добре",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-8"
    },
    {
      "mark_value_id": 14,
      "code": 9,
      "value": "9",
      "description": "Добре+",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-9"
    },
    {
      "mark_value_id": 15,
      "code": 10,
      "value": "10",
      "description": "Відмінно-",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-10"
    },
    {
      "mark_value_id": 16,
      "code": 11,
      "value": "11",
      "description": "Відмінно",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-11"
    },
    {
      "mark_value_id": 17,
      "code": 12,
      "value": "12",
      "description": "Відмінно+",
      "max_value": 12,
      "lang_code": "UA",
      "html_class": "point-12"
    },
    {
      "mark_value_id": 1,
      "code": -5,
      "value": "Н",
      "description": "Не був",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 2,
      "code": -10,
      "value": "Н/А",
      "description": "Не атестовано",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 3,
      "code": -15,
      "value": "зар",
      "description": "Зараховано",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 4,
      "code": -20,
      "value": "зв",
      "description": "Звільнено",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 5,
      "code": -25,
      "value": "вивч",
      "description": "Вивчав",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 23,
      "code": -30,
      "value": "хв",
      "description": "Хворів",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 24,
      "code": -31,
      "value": "П",
      "description": "Початковий",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": "point-1"
    },
    {
      "mark_value_id": 25,
      "code": -32,
      "value": "С",
      "description": "Середній",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": "point-4"
    },
    {
      "mark_value_id": 26,
      "code": -33,
      "value": "Д",
      "description": "Достатній",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": "point-8"
    },
    {
      "mark_value_id": 27,
      "code": -34,
      "value": "В",
      "description": "Високий",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": "point-12"
    },
    {
      "mark_value_id": 28,
      "code": -35,
      "value": "Н/О",
      "description": "Нема оцінки",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 29,
      "code": -36,
      "value": "заув",
      "description": "Зауваження",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 30,
      "code": -37,
      "value": "п/п",
      "description": "Поважна причина",
      "max_value": 4,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 31,
      "code": -38,
      "value": "√",
      "description": "√",
      "max_value": null,
      "lang_code": "UA",
      "html_class": "point-12"
    },
    {
      "mark_value_id": 32,
      "code": -39,
      "value": "к",
      "description": "Коментар",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    },
    {
      "mark_value_id": 33,
      "code": -40,
      "value": "Н/З",
      "description": "Не зараховано",
      "max_value": null,
      "lang_code": "UA",
      "html_class": null
    }
  ],
  "error_message": ""
}
```
</details>

## Тимчасові посилання

### Створення посилання

**URL**: `https://api-mobile.nz.ua/v2/link/generate-temporary-link`  
**Метод**: `POST`  
**Опис**: Створення тимчасового посилання  

<details>
<summary><b>Тіло запиту</b></summary>
  
```jsonc
{
  "url": "https://example.com/"
}
```
</details>

<details>
<summary><b>Відповідь</b></summary>
  
```jsonc
{
  "response": "http://api-mobile.nz.ua/v1/link/get-temporary-link?hash=0123456789ABCDEF",
  "error_message": ""
}
```
</details>

### Перегляд посилання

**URL**: `https://api-mobile.nz.ua/v2/link/get-temporary-link?hash=0123456789ABCDEF`  
**Метод**: `GET`  
**Опис**: Перегляд тимчасового посилання  

**Відповідь**: HTML або інша відповідь, яка відкривається через цей URL
