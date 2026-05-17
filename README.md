# Лабораторная работа №33-34. Полноценный CRUD с базой данных

---

## Основная информация 

**ФИО:** Гвоздева В.А, Деушев Т.Т  
**Группа:** ИСП-231  
**Дата:** 17.05.2026 

---

## Описание проекта

**NotesApp** - это приложение для управления заметками с категориями.

---

## Структура проекта

- Lab33-34_FullCRUD_Gvozdeva_Deushev/
    - img/ 
        - gitPushLab33_34_Gvozdeva_Deushev.png
        - step6_migrationLab33_34_Gvozdeva_Deushev_1.png
        - step6_migrationLab33_34_Gvozdeva_Deushev_2.png
        - step7_categoriesLab33_34_Gvozdeva_Deushev_1.png
        - step7_categoriesLab33_34_Gvozdeva_Deushev_2.png
        - step8_notesLab33_34_Gvozdeva_Deushev_1.png
        - step8_notesLab33_34_Gvozdeva_Deushev_2.png
    - NotesApp/
        - Controllers/
            - CategoriesController.cs
            - NotesController.cs
        - Data/
            - AppDbContext.cs
        - Helpers/
            - ApiResponse.cs
        - Models/
            - DTOs/
                - CategoryDtos.cs
                - NoteDtos.cs
                - NoteFilterDto.cs
            - Category.cs
            - Note.cs
        - Repositories/
            - CategoryRepository.cs
            - ICategoryRepository.cs
            - INoteRepository.cs
            - NoteRepository.cs
        -  Program.cs
        - appsettings.json

---

## Таблица всех маршрутов

|Метод|URL|Описание|Коды ответа|
|:----|:--|:-------|:----------|
|**GET**|/api/categories|Все категории с кол-вом заметок|200|
|**GET**|/api/categories/{id}|Одна категория|200, 404|
|**GET**|/api/categories/{id}/notes|Категория с заметками|200, 404|
|**POST**|/api/categories|Создать категорию|201, 400|
|**PUT**|/api/categories/{id}|Обновить категорию|200, 400, 404|
|**DELETE**|/api/categories/{id}|Удалить категорию|204, 400, 404|
|**GET**|/api/notes|Все заметки с фильтрами|200|
|**GET**|/api/notes/{id}|Одна заметка|200, 404|
|**POST**|/api/notes|Создать заметку|201, 400|
|**PUT**|/api/notes/{id}|Обновить заметку|200, 400, 404|
|**PATCH**|/api/notes/{id}/pin|Закрепить / открепить|200, 404|
|**PATCH**|/api/notes/{id}/archive|Архивировать / восстановить|200, 404|
|**DELETE**|/api/notes/{id}|Удалить заметку |204, 404|

---

## Паттерн Repository

**Repository** - это паттерн проектирования, который изолирует логику работы с
данными от логики контроллера.

**Без Repository:**  
*Контроллер -> напрямую работает с DbContext -> База данных*

**С Repository:**  
*Контроллер -> Repository -> DbContext -> База данных*

Зачем это нужно:
|Проблема без Repository|Решение с Repository|
|:----------------------|:-------------------|
|**Логика запросов к БД размазана по контроллерам**|Все запросы в одном месте|
|**Трудно тестировать контроллер отдельно от БД&**|Можно подменить репозиторий на тестовый|
|**При смене БД нужно менять все контроллеры**|Меняется только репозиторий|
|**Дублирование одинаковых запросов**|Переиспользуемые методы|

**Аналог из реального мира:** представьте склад (база данных) и кладовщика (Repository). Продавцы (контроллеры) не ходят на склад сами — они отправляют запрос кладовщику. Кладовщик знает, где всё лежит и как правильно работать со складом

---

## Главные выводы:

1. Паттерн **Repository** - не лишняя абстракция, а способ держать код управляемым при росте проекта.
2. **Data Annotations** решают двойную задачу: валидируют входные данные и задают ограничения в БД.
3. Единый формат ответа **(ApiResponse)** упрощает жизнь фронтенду - он всегда знает, что ждать от сервера.
4. **DeleteBehavior.Restrict** защищает данные от случайного каскадного удаления.
5. **Include()** и проекция в **DTO** через **Select()** - правильный способ получить связанные данные без N+1 проблемы.
