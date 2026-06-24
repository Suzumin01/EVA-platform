<div align="center">
  <h1>ЕВА — Единый Врачебный Ассистент</h1>
  <p>Мобильная медицинская платформа: запись к врачу, AI-анализ симптомов, история приёмов, push-уведомления</p>

  ![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?logo=kotlin&logoColor=white)
  ![Android](https://img.shields.io/badge/Android-3DDC84?logo=android&logoColor=white)
  ![Jetpack Compose](https://img.shields.io/badge/Jetpack_Compose-4285F4?logo=jetpackcompose&logoColor=white)
  ![Ktor](https://img.shields.io/badge/Ktor-087CFA?logo=ktor&logoColor=white)
  ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?logo=postgresql&logoColor=white)
  ![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black)
  ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
</div>

---

## Репозитории

| Репозиторий | Описание | Стек |
|-------------|----------|------|
| [eva-server](https://github.com/Suzumin01/eva-server) | REST API сервер | Kotlin, Ktor, Exposed ORM, PostgreSQL, Flyway |
| [eva-app](https://github.com/Suzumin01/eva-app) | Android-приложение | Kotlin, Jetpack Compose, Hilt, Retrofit, Room |
| [eva-admin](https://github.com/Suzumin01/eva-admin) | Веб-панель (Admin + Doctor) | React 18, TypeScript, Vite, shadcn/ui |

---

## Возможности

- **Запись к врачу** — выбор специализации, клиники, врача и удобного слота
- **AI-анализ симптомов** — Yandex AI Studio (Alice LLM): диагноз, рекомендации и уровень срочности на русском языке; квота 10 запросов в день
- **Push-уведомления** — FCM, напоминания за 24 часа и за 1 час до приёма
- **Отзывы** — только после завершённого приёма, со звёздным рейтингом; влияют на общий рейтинг врача
- **Медицинские документы** — загрузка PDF и изображений, доступ врачу и администратору
- **Фото профиля** — загрузка из галереи, хранение на сервере
- **Веб-панель Admin** — управление врачами, клиниками, записями, пользователями, модерация отзывов
- **Веб-панель Doctor** — просмотр записей, заключения, расписание

---

## Стек

| Компонент | Технологии |
|-----------|------------|
| **Android** | Kotlin, Jetpack Compose, Hilt, Retrofit, Room, DataStore, OkHttp |
| **Backend** | Kotlin, Ktor, Exposed ORM, HikariCP, Flyway, bcrypt, JWT |
| **База данных** | PostgreSQL 15 |
| **Admin-панель** | React 18, TypeScript, Vite, shadcn/ui, TanStack Query, Zustand |
| **AI** | Yandex AI Studio (Alice LLM) через Responses API |
| **Push** | Firebase Cloud Messaging (FCM) |
| **Инфраструктура** | Docker, Docker Compose |

---

## Архитектура

```
┌─────────────────┐     ┌──────────────────────┐
│  Android App    │     │    React Admin SPA   │
│  Compose + Hilt │     │  shadcn/ui + Zustand │
└────────┬────────┘     └──────────┬───────────┘
         │  HTTP / JWT             │  HTTP / JWT
         ▼                         ▼
┌─────────────────────────────────────────────┐
│           Ktor REST API (:8081)             │
│  Routing → Routes → Repository → Exposed   │
│  AiService · AuthService · FcmService      │
└─────────────────────┬───────────────────────┘
                      │  HikariCP + Flyway
                      ▼
            ┌──────────────────┐
            │  PostgreSQL 15   │
            └──────────────────┘
```

**Backend** — слоистая архитектура: `plugins/` → `api/routes/` → `data/repository/` → `data/tables/`.  
**Android** — MVVM: `EvaApi (Retrofit)` + `Room` → `Repository` → `ViewModel` → `Composable`.  
**Admin** — роль-зависимый роутинг: `ProtectedRoute` проверяет `role` из JWT при каждом переходе.

---

## Тестирование

| Тип | Покрытие |
|-----|----------|
| Backend unit-тесты (JUnit) | 28/28 |
| Android UI-тесты (Espresso + Hilt) | 13/13 |
