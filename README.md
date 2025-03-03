# Неофіційна документація API (api-mobile.nz.ua)

Ця документація описує неофіційний API для мобільного додатку NZ.UA


> **Примітка** Всі запити (окрім авторизації) повинні включати токен доступу у форматі `Authorization: Bearer {access_token}`. 

## Авторизація

### Вхід в систему

**URL**: `https://api-mobile.nz.ua/v2/user/login`  
**Метод**: `POST`  
**Опис**: Авторизація користувача в системі

**Тіло запиту**:
```json
{
  "username": "логін_користувача",
  "password": "пароль_користувача",
  "exponentPushToken": "" // токен для push-сповіщень (залишити пустий рядок)
}
```

**Відповідь**:
```json
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

## Сповіщення

### Отримання сповіщень
**URL**: `https://api-mobile.nz.ua/v2/notification/`
**Метод**: `GET`
**Опис**: Отримання списку сповіщень користувача

**Відповідь**:
```json
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
```json
{
  "qty": "0",
  "error_message": ""
}
```

## Розклад

### Щоденник за період

**URL**: `https://api-mobile.nz.ua/v2/schedule/diary`
**Метод**: `POST`
**Опис**: Отримання даних щоденника (розкладу) за вказаний період

**Тіло запиту**:
```json
{
  "start": "2025-03-03",
  "end": "2025-03-03"
}
```

**Відповідь**:
```json
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
                  "comment": "текс_коментаря"
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

## Оцінки

### Оцінки з предмету

**URL**: `https://api-mobile.nz.ua/v2/schedule/subject-grades`
**Метод**: `POST`
**Опис**: Отримання оцінок з предмету за вказаний період

**Тіло запиту**:
```json
{
  "start_date": "2025-01-01",
  "end_date": "2025-07-30",
  "subject_id": "36613478"
}
```

**Відповідь**:
```json
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