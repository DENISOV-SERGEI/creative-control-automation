# Техническое задание на автоматизацию «Контроль сроков годности рекламных креативов»

## 1. Цель и ожидаемый результат
Ежедневная проверка баннеров, отключение просроченных, предупреждение за 3 дня, логирование всех действий.

## 2. Схема процесса
(см. диаграмму в README)

## 3. Подробное описание шагов
- **Триггер**: Schedule Trigger (11:00 МСК)
- **Источник данных**: Google Sheets (лист `creatives_monitor`, колонки: id, Title, End_date, Active, log)
- **Логика**:
  - Условие 1: `End_date < сегодня AND Active = TRUE` → отключение (Update Row: Active=FALSE, log=время) → Telegram → Audit Log (deactivated)
  - Условие 2: `0 < (End_date - сегодня) <= 3 дня AND Active = TRUE` → Telegram (предупреждение) → Audit Log (warning)
- **Обработка ошибок**: отдельный workflow с Error Trigger → Telegram админу + запись в таблицу Ошибки

## 4. Сервисы и интеграции
- n8n
- Google Sheets (чтение, обновление, добавление строк)
- Telegram Bot

## 5. Безопасность
- OAuth2 для Google Sheets
- Токен Telegram хранится в credentials n8n
- Доступ к таблицам только у владельца

## 6. Артефакты
- JSON workflows
- Инструкция
- ТЗ
- Скриншоты
