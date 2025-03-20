# Неофіційна документація API (api-mobile.nz.ua)

Ця документація описує неофіційний API для мобільного додатку NZ.UA

> **Примітка** Всі запити (окрім авторизації) повинні включати токен доступу у форматі `Authorization: Bearer {access_token}`. 

## Авторизація

### Вхід в систему

**URL**: `https://api-mobile.nz.ua/v2/user/login`  
**Метод**: `POST`  
**Опис**: Авторизація користувача в системі

**Тіло запиту**:
```jsonc
{
  "username": "логін_користувача",
  "password": "пароль_користувача",
  "exponentPushToken": "" // токен для push-сповіщень (можна залишити пустий рядок)
}
```

**Відповідь**:
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

### Оновлення токену

**URL**: `https://api-mobile.nz.ua/v2/user/refresh-token`  
**Метод**: `POST`  
**Опис**: Оновлення простроченого токену авторизації  

**Тіло запиту**:
```jsonc
{
  "refresh_token": "токен_оновлення"
}
```

**Відповідь**:
```jsonc
{
  "access_token": "токен_доступу",
  "error_message": ""
}
```

## Сповіщення

### Отримання сповіщень
**URL**: `https://api-mobile.nz.ua/v2/notification/`  
**Метод**: `GET`  
**Опис**: Отримання списку сповіщень користувача

**Відповідь**:
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

### Кількість непрочитаних сповіщень
**URL**: `https://api-mobile.nz.ua/v2/notification/unread-qty`  
**Метод**: `GET`  
**Опис**: Отримання кількості непрочитаних сповіщень

**Відповідь**:
```jsonc
{
  "qty": "0",
  "error_message": ""
}
```

## Щоденник

### Щоденник за період

**URL**: `https://api-mobile.nz.ua/v2/schedule/diary`  
**Метод**: `POST`  
**Опис**: Отримання даних щоденника (розкладу) за вказаний період

**Тіло запиту**:
```jsonc
{
  "start_date": "2025-03-03",
  "end_date": "2025-03-03"
}
```

**Відповідь**:
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

### Розклад

**URL**: `https://api-mobile.nz.ua/v2/schedule/timetable`  
**Метод**: `POST`  
**Опис**: Отримання розкладу за вказаний період

**Тіло запиту**:
```jsonc
{
  "start_date": "2024-11-01",
  "end_date": "2024-11-30"
}
```

**Відповідь**:
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

## Оцінки

### Оцінки з предмету

**URL**: `https://api-mobile.nz.ua/v2/schedule/subject-grades`  
**Метод**: `POST`  
**Опис**: Отримання оцінок з предмету за вказаний період

**Тіло запиту**:
```jsonc
{
  "start_date": "2025-01-01",
  "end_date": "2025-07-30",
  "subject_id": "1111111"
}
```

**Відповідь**:
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

### Загальні оцінки

**URL**: `https://api-mobile.nz.ua/v2/schedule/student-performance`  
**Метод**: `POST`  
**Опис**: Отримання усіх оцінок та пропущених днів, уроків за вказаний період

**Тіло запиту**:
```jsonc
{
  "start_date": "2024-09-01",
  "end_date": "2025-05-31"
}
```

**Відповідь**:
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

### Пропущені уроки

**URL**: `https://api-mobile.nz.ua/v2/schedule/missed-lessons`  
**Метод**: `POST`  
**Опис**: Пропущені уроки за деякий період

**Тіло запиту**:
```jsonc
{
  "start_date": "2024-09-01",
  "end_date": "2025-05-31"
}
```

**Відповідь**:
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

## Тимчасові посилання

### Створення посилання

**URL**: `https://api-mobile.nz.ua/v2/link/generate-temporary-link`  
**Метод**: `POST`  
**Опис**: Створення тимчасового посилання  

**Тіло запиту**:
```jsonc
{
  "url": "https://example.com/"
}
```

**Відповідь**:
```jsonc
{
  "response": "http://api-mobile.nz.ua/v1/link/get-temporary-link?hash=0123456789ABCDEF",
  "error_message": ""
}
```

### Перегляд посилання

**URL**: `https://api-mobile.nz.ua/v2/link/get-temporary-link?hash=0123456789ABCDEF`  
**Метод**: `GET`  
**Опис**: Перегляд тимчасового посилання  

**Відповідь**: HTML-код або інша відповідь, яке відкривається через цей URL

## Інші

### Тест

**URL**: `https://api-mobile.nz.ua/v2/user/test`  
**Метод**: `GET`  
**Опис**: Метод для тестування працездатності API  

**Відповідь**:
```jsonc
"2ololo"
```
