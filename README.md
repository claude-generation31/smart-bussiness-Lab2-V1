# Telegram-бот Coffee Lab

Telegram-бот підприємства «Coffee Lab» (м. Львів) на платформі n8n.  
Лабораторна робота №2, дисципліна «Інформаційні технології розумного підприємства», варіант 1 (кав’ярня).

**Бот:** @CoffeeLabHorbatiukBot

## Функціонал

| Команда / кнопка | Відповідь |
|---|---|
| `/start` | Привітання та клавіатура з кнопками |
| Меню напоїв | Перелік актуальних кавових напоїв |
| Контакти | Адреса та номер телефону для бронювання |
| Акції | Поточні акції та бонусна програма |
| Графік роботи | Режим роботи кав’ярень мережі |
| Будь-який інший текст | Стандартна відповідь з контактами |

## Стек
- Telegram Bot API
- n8n (Telegram Trigger → Switch → Telegram Send Message)
- Node.js LTS

## Схема роботи
```
Telegram Trigger ──► Switch (Rules)
                       ├─ /start ─────────► Привітання + клавіатура
                       ├─ Меню напоїв ────► Відповідь
                       ├─ Контакти ───────► Відповідь
                       ├─ Акції ──────────► Відповідь
                       ├─ Графік роботи ──► Відповідь
                       └─ Fallback ───────► Стандартна відповідь
```

## Структура репозиторію
```
lab2-coffeelab-bot/
├── README.md
├── .env.example
├── .gitignore
├── workflow/coffee_lab_bot.json
└── docs/
    ├── ТЗ.md
    └── screenshots/
```

## Встановлення та запуск

### 1. Вимоги
- Node.js LTS 20.x+ (`node -v`)
- Обліковий запис Telegram

### 2. Отримання токена
1. Відкрити @BotFather → `/newbot`.
2. Ввести ім’я та нікнейм бота (закінчується на `bot`).
3. Скопіювати токен у `.env` (див. `.env.example`). **Не комітити `.env`!**

### 3. Змінні середовища
```bash
cp .env.example .env   # Windows: copy .env.example .env
```
Заповнити значення. n8n **не читає `.env` автоматично**, тому змінні задаються в терміналі перед запуском:
```bat
:: Windows (cmd)
set WEBHOOK_URL=https://xxxx.lhr.life/
set GENERIC_TIMEZONE=Europe/Kyiv
```
або одним рядком через dotenv-cli:
```bash
npx dotenv-cli -e .env -- npx n8n start
```

### 4. Запуск n8n
```bash
npx n8n start --tunnel
```
Якщо висить `Waiting for tunnel...`:
```bash
ssh -R 80:localhost:5678 nokey@localhost.run   # отримати https://xxxx.lhr.life
set WEBHOOK_URL=https://xxxx.lhr.life/
npx n8n start
```
Інтерфейс: http://localhost:5678

### 5. Імпорт workflow
1. Credentials → Create Credential → **Telegram API** → вставити токен → Save.
2. Workflows → Import from File → `workflow/coffee_lab_bot.json`.
3. У вузлах Telegram Trigger і Send Message обрати створені Credentials.
4. Save → перемкнути **Active**.

## Перевірка
1. Знайти бота в Telegram за нікнеймом, натиснути **Start**.
2. Перевірити кожну кнопку.
3. Надіслати довільний текст — має прийти стандартна відповідь.

Повний перелік тест-кейсів — `docs/ТЗ.md`, розділ 9.

## Скріншоти
| Привітання | Кнопки | Workflow |
|---|---|---|
| ![](docs/screenshots/02_start.png) | ![](docs/screenshots/03_menu1.png) | ![](docs/screenshots/08_workflow.png) |

## Безпека
- Токен зберігається лише в n8n Credentials і локальному `.env`.
- `.env` і папка `.n8n/` додані до `.gitignore`.
- Перед публікацією перевірити експорт workflow на відсутність токена.

## Автор
Горбатюк Данило, студент групи ОІ-31сп  
НУ «Львівська політехніка», ІКНІ, кафедра АСУ, 2026
