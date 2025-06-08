# Docker-контейнеризация и хранение данных

## 📂 Структура проекта

- `./backend/` код бэкенд-приложения и `Dockerfile`
- `./frontend/` код фронтенд-приложения и `Dockerfile`
- `docker-compose.yml` описание сервисов
- `.github/workflows/deploy.yaml` CI/CD конфигурация

---

## 🚀 Запуск приложения с Docker Compose

### 1. Сборка образов

```bash
docker compose build
```

### 2. Запуск приложения

```bash
docker compose up -d
```

Фронтенд доступен на `http://localhost:80`, а бэкенд на `http://localhost:8081`.

### 3. Остановка приложения

```bash
docker compose down
```

---

## 🐝 Запуск приложения в Docker Swarm

Docker Swarm позволяет запускать приложение в режиме кластера с масштабируемостью и высокой доступностью.

### 1. Инициализация Swarm

```bash
docker swarm init
```

Переводит Docker в режим Swarm и превращает хост в управляющий узел.

---

### 2. Развёртывание стека

```bash
docker stack deploy -c docker-compose.yml momo-store
```

Docker Swarm развернёт стек с именем `momo-store` и создаст все сервисы.

---

### 3. Проверка сервисов

```bash
docker service ls
```

Отобразит список всех сервисов в стеке.

---

### 4. Просмотр логов

```bash
docker service logs momo-store_backend
```

```bash
docker service logs momo-store_frontend
```

---

### 5. Масштабирование сервисов

```bash
docker service scale momo-store_backend=3
```

Увеличит количество реплик сервиса backend до 3. Либо же можно делать в `docker-compose.yml`, указывая там `replicas: N`.

---

### 6. Удаление стека

```bash
docker stack rm momo-store
```

Остановит и удалит стек.

---

## 📌 Основные команды Docker Swarm

| Команда                                                | Описание                    |
|--------------------------------------------------------|-----------------------------|
| `docker swarm init`                                    | Инициализирует Docker Swarm |
| `docker stack deploy -c docker-compose.yml momo-store` | Разворачивает стек          |
| `docker service ls`                                    | Список сервисов             |
| `docker service logs momo-store_backend`               | Логи сервиса backend        |
| `docker service logs momo-store_frontend`              | Логи сервиса frontend       |
| `docker service scale momo-store_backend=3`            | Масштабирует сервис backend |
| `docker stack rm momo-store`                           | Удаляет стек                |

---

## ⚠️ Важные замечания

- Docker Swarm автоматически создаёт overlay-сеть для взаимодействия сервисов.
- Если в `docker-compose.yml` указан одинаковый порт (например, `8081:8081`), при масштабировании несколько реплик будут
  пытаться забиндить один и тот же порт хоста, что вызовет ошибку. Для решения можно использовать `expose` вместо
  `ports`, чтобы контейнеры взаимодействовали только внутри сети.

---

## 🛡️ Безопасность

- **Ограничения ресурсов:** в `docker-compose.yml` задаются лимиты CPU и памяти.
- **Сеть:** контейнеры объединяются в общую сеть через `networks`.
- **Порты:** `ports` определяет доступ из контейнера наружу.
- **Ограниченные права:** `cap_drop` и `cap_add` ограничивают доступ к системным вызовам.
- **Healthcheck:** проверяет работоспособность контейнеров.
- **Trivy:** для проверки уязвимостей в образах.

---
