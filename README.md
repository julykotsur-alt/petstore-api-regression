# API Regression: Swagger Petstore

Навчальний репозиторій для практикуму «API-регресія з Claude і Postman».

Щодня задача за розкладом у Claude Code:

1. бере колекцію з Postman через конектор;
2. запускає її через [Newman](https://github.com/postmanlabs/newman);
3. рахує passed/failed і класифікує кожен збій (API defect, test-script issue, environment issue);
4. порівнює результат з попереднім запуском;
5. зберігає звіт у гілку `claude/qa-results` і надсилає сповіщення.

## Структура

| Шлях | Що там |
|---|---|
| `collections/petstore-collection.json` | Postman-колекція: 6 запитів, 15 перевірок (Create → Get → Update → Get → Delete → Verify deletion) |
| `PROMPT.md` | Промпт для задачі за розкладом |
| `results/` | JSON-звіти Newman (агент пише їх у гілку `claude/qa-results`) |
| `reports/` | Текстові QA-звіти (там само) |
| `.github/workflows/newman.yml` | Той самий прогін у GitHub Actions, для порівняння з класичним CI |

## Цільовий API

Публічний [Swagger Petstore](https://petstore3.swagger.io) (`https://petstore3.swagger.io/api/v3`). Авторизація не потрібна. Кожен запуск створює і видаляє власний тестовий запис, тому паралельні запуски не конфліктують.

## Запуск локально

```bash
npm install -g newman@6
newman run collections/petstore-collection.json
```

## Запуск у GitHub Actions

Вкладка **Actions** → **API regression (Newman)** → **Run workflow**. Також запускається автоматично при зміні колекції в `main`. JSON- і HTML-звіти можна завантажити з артефактів запуску. HTML-звіт останнього прогону з `main` публікується на GitHub Pages: https://julykotsur-alt.github.io/petstore-api-regression/

## Налаштування задачі в Claude

1. Підключити Postman до Claude: claude.ai/customize/connectors.
2. Створити хмарне середовище з **Network access: Limited**, увімкненим **Allow package managers**, доменом `petstore3.swagger.io` в **Allowed domains** і setup-скриптом `npm install -g newman@6`.
3. Створити задачу на claude.ai/code/routines: промпт з `PROMPT.md`, цей репозиторій, середовище з кроку 2, конектор Postman.
