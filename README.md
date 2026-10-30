# 🤖 UltraBot 3 (Telegram Bots - Третья партия)

Проект для деплоя третьей партии Telegram-ботов на **Render.com** через **Web Service** с встроенной защитой от спящего режима (Anti-Sleep).

---

## ⚡ Особенности
- **Render Web Service Ready**: Встроен легковесный HTTP-сервер, слушающий порт `$PORT` (10000 по умолчанию).
- **Защита от спячки (Anti-Sleep)**: Фоновый поток автоматически каждые 10 минут отправляет GET-запрос на публичный адрес сервиса (`RENDER_EXTERNAL_URL`), сбрасывая 15-минутный таймер сна бесплатного тарифа Render.
- **Поддержка двух ботов одновременно** в рамках одного процесса (`asyncio.gather`).
- **Сброс висящих вебхуков** перед стартом для предотвращения конфликтов.

---

## 🚀 Быстрый деплой на Render

### Вариант 1: Обычный Web Service
1. Зайдите в [Render Dashboard](https://dashboard.render.com).
2. Нажмите **New +** -> **Web Service**.
3. Подключите репозиторий: `https://github.com/gpt08223-dot/ultrabot3`.
4. Укажите следующие параметры:
   - **Name**: `ultrabot3`
   - **Language / Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `python main.py`
   - **Instance Type**: `Free`
5. *(Опционально)* Во вкладке **Environment Variables** вы можете переопределить переменные:
   - `BOT_TOKEN_1` — токен первого бота.
   - `SECOND_BOT_TOKEN` — токен второго бота.
6. Нажмите **Create Web Service**.

---

### Вариант 2: Через Render Blueprint (render.yaml)
1. В Render Dashboard нажмите **New +** -> **Blueprint**.
2. Выберите репозиторий `ultrabot3`.
3. Render автоматически применит файл `render.yaml` и настроит сервис за один клик.

---

## 🛡️ Как работает защита от сна (Keep-Alive)
Render Free Tier усыпляет веб-сервис через **15 минут** отсутствия входящих HTTP-запросов.
1. Бот каждые **10 минут** опрашивает собственный внешний URL (`https://<ваш-сервис>.onrender.com/health`).
2. Переменная `RENDER_EXTERNAL_URL` передается платформой Render автоматически.
3. Входящий запрос сбрасывает счетчик бездействия Render, и бот продолжает работать без пауз.

> 💡 **Совет для 100% стабильности**:
> Скопируйте ссылку вашего сервиса на Render (например, `https://ultrabot3.onrender.com`) и добавьте бесплатный мониторинг в [UptimeRobot](https://uptimerobot.com) с интервалом проверки каждые 5-10 минут.
