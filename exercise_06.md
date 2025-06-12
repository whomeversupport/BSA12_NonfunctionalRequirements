# 🔒 Exercise 06 — Атрибуты безопасности

<!--
Student: @https://edu.21-school.ru/profile/lunchlpr
Location: SKD SAMARKAND  
GitHub: https://github.com/wh0mever
![Levi Ackerman](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fc.tenor.com%2FHGPFeIo8looAAAAC%2Flevi-ackerman.gif&f=1&nofb=1&ipt=740d7fa033496bb69c29f0fdb0f6d2c01c9e8f9d2d092c25b0c1a31079cb9c3f)
-->

## 📝 Задание

**Описать требования к безопасности в части паролей и логирования с различными подходами для каждой задачи.**

## 🏗️ Архитектура безопасности

```mermaid
graph TB
    subgraph "🅱️ Барбершоп: Потребительская безопасность"
        B_AUTH["🔐 Простая аутентификация<br/>Email + Password"]
        B_2FA["📱 SMS 2FA<br/>Для критических операций"]
        B_SESSION["🍪 Session Management<br/>30 дней"]
        B_ENCRYPT["🔒 Base Encryption<br/>HTTPS + BCrypt"]
        
        B_AUTH --> B_2FA
        B_2FA --> B_SESSION
        B_SESSION --> B_ENCRYPT
    end
    
    subgraph "🚚 Доставка: Корпоративная безопасность"
        D_AUTH["🔐 Усиленная аутентификация<br/>Login + Password + Pin"]
        D_MFA["🛡️ Multi-Factor Auth<br/>App + Биометрия"]
        D_RBAC["👥 Role-Based Access<br/>Строгая сегментация"]
        D_ENCRYPT["🔒 Enterprise Encryption<br/>End-to-end + AES-256"]
        
        D_AUTH --> D_MFA
        D_MFA --> D_RBAC
        D_RBAC --> D_ENCRYPT
    end
    
    classDef barbershop fill:#ff6b6b,stroke:#ff5252,color:#fff
    classDef delivery fill:#4ecdc4,stroke:#26a69a,color:#fff
    
    class B_AUTH,B_2FA,B_SESSION,B_ENCRYPT barbershop
    class D_AUTH,D_MFA,D_RBAC,D_ENCRYPT delivery
```

## 🔑 Требования к паролям

### 🅱️ Барбершоп - Потребительский подход

#### 📋 Состав пароля
```mermaid
graph LR
    subgraph "Базовые требования"
        LEN["📏 Длина ≥ 8 символов"]
        CHARS["🔤 Латиница + цифры"]
        SPECIAL["🔣 1+ спецсимвол"]
        CASE["🔀 Верхний + нижний регистр"]
    end
    
    subgraph "Дополнительные ограничения"
        COMMON["❌ Запрет популярных паролей"]
        PERSONAL["❌ Запрет личных данных"]
        REPEAT["❌ Запрет повторений"]
    end
    
    LEN --> CHARS
    CHARS --> SPECIAL
    SPECIAL --> CASE
    
    classDef basic fill:#ff6b6b,stroke:#ff5252,color:#fff
    classDef additional fill:#ffd93d,stroke:#ffcc02,color:#333
    
    class LEN,CHARS,SPECIAL,CASE basic
    class COMMON,PERSONAL,REPEAT additional
```

**Детальные требования:**
| Критерий | Требование | Пример валидного | Пример невалидного |
|----------|------------|------------------|-------------------|
| **Минимальная длина** | 8+ символов | `MyPass123!` | `Pass123` |
| **Латинские буквы** | Обязательно | `Password1!` | `Пароль123!` |
| **Цифры** | Минимум 1 | `Password1!` | `Password!` |
| **Спецсимволы** | Минимум 1 из: `!@#$%^&*()` | `Password1!` | `Password123` |
| **Регистр** | Верхний + нижний | `Password1!` | `password1!` |

#### 🔄 Правила сброса и смены пароля

```mermaid
sequenceDiagram
    participant U as 👤 Пользователь
    participant S as 🏛️ Система
    participant E as 📧 Email
    participant SMS as 📱 SMS
    
    Note over U,SMS: Сброс пароля
    U->>S: Запрос сброса (email)
    S->>E: Ссылка сброса (15 мин)
    U->>S: Переход по ссылке
    S->>SMS: SMS-код подтверждения
    U->>S: Ввод SMS-кода + новый пароль
    S->>U: Пароль изменен
    
    Note over U,SMS: Смена пароля
    U->>S: Авторизация + старый пароль
    U->>S: Новый пароль (без SMS)
    S->>U: Пароль изменен
```

**Политика смены:**
- **Принудительная смена**: Не требуется (потребительский сервис)
- **Повторное использование**: Запрет последних 3 паролей
- **Время жизни ссылки сброса**: 15 минут
- **Блокировка после неудачных попыток**: 5 попыток → блокировка на 30 минут

### 🚚 Доставка - Корпоративный подход

#### 📋 Усиленный состав пароля
```mermaid
graph LR
    subgraph "Строгие требования"
        LEN_D["📏 Длина ≥ 12 символов"]
        CHARS_D["🔤 Латиница + кириллица"]
        NUMS_D["🔢 Минимум 2 цифры"]
        SPECIAL_D["🔣 2+ спецсимвола"]
        CASE_D["🔀 3+ типа символов"]
    end
    
    subgraph "Дополнительная защита"
        DICT["❌ Проверка по словарям"]
        ENTROPY["📊 Энтропия ≥ 60 бит"]
        COMMON_D["❌ TOP-10000 паролей"]
        COMPANY["❌ Корпоративные данные"]
    end
    
    classDef strict fill:#4ecdc4,stroke:#26a69a,color:#fff
    classDef protection fill:#45b7d1,stroke:#339bc2,color:#fff
    
    class LEN_D,CHARS_D,NUMS_D,SPECIAL_D,CASE_D strict
    class DICT,ENTROPY,COMMON_D,COMPANY protection
```

**Детальные требования:**
| Критерий | Требование | Пример валидного | Пример невалидного |
|----------|------------|------------------|-------------------|
| **Минимальная длина** | 12+ символов | `MySecure123!@#Pass` | `SecPass1!` |
| **Языки** | Латиница ИЛИ кириллица | `MyPassword123!` | `МойPassword123!` |
| **Цифры** | Минимум 2 | `Password12!` | `Password1!` |
| **Спецсимволы** | Минимум 2 | `Password12!@` | `Password12!` |
| **Сложность** | 3+ типа символов | `Pass12!` | `password123` |

#### 🔄 Корпоративные правила сброса

```mermaid
sequenceDiagram
    participant U as 👤 Курьер/Оператор
    participant S as 🏛️ Система
    participant A as 👨‍💼 Администратор
    participant MFA as 🔐 MFA App
    
    Note over U,MFA: Сброс с участием админа
    U->>A: Запрос сброса (лично/тикет)
    A->>S: Подтверждение личности
    S->>U: Временный пароль (24 часа)
    U->>S: Авторизация + смена пароля
    U->>MFA: Настройка MFA
    S->>U: Полный доступ активирован
```

**Политика смены:**
- **Принудительная смена**: Каждые 90 дней
- **Повторное использование**: Запрет последних 12 паролей
- **Время жизни**: Временный пароль действует 24 часа
- **Блокировка**: 3 попытки → блокировка на 60 минут → уведомление админа

## 🛡️ Многофакторная аутентификация (MFA)

### 🅱️ Барбершоп - Селективная 2FA

#### 📱 Действия, требующие 2FA
```mermaid
graph TB
    subgraph "Критические операции"
        PAYMENT["💳 Привязка карты<br/>SMS-код"]
        DELETE["🗑️ Удаление аккаунта<br/>SMS-код"]
        CHANGE_PHONE["📞 Смена телефона<br/>SMS на оба номера"]
        SENSITIVE["🔒 Просмотр истории платежей<br/>SMS-код"]
    end
    
    subgraph "Обычные операции (без 2FA)"
        BOOKING["📅 Запись к мастеру"]
        VIEW["👀 Просмотр записей"]
        CANCEL["❌ Отмена записи"]
        REVIEW["⭐ Оставить отзыв"]
    end
    
    classDef critical fill:#ff6b6b,stroke:#ff5252,color:#fff
    classDef normal fill:#6bcf7f,stroke:#42a047,color:#fff
    
    class PAYMENT,DELETE,CHANGE_PHONE,SENSITIVE critical
    class BOOKING,VIEW,CANCEL,REVIEW normal
```

#### 🔐 Порядок аутентификации для клиентов
1. **Основная авторизация**: Email + пароль
2. **Триггер 2FA**: При выполнении критической операции
3. **SMS-код**: Отправка на зарегистрированный номер
4. **Время действия кода**: 5 минут
5. **Повторная отправка**: Через 60 секунд
6. **Блокировка**: После 3 неверных попыток на 15 минут

### 🚚 Доставка - Обязательная MFA

#### 📱 Многоуровневая аутентификация
```mermaid
graph TB
    subgraph "Уровень 1: Базовый вход"
        LOGIN["🔐 Логин + пароль + PIN"]
    end
    
    subgraph "Уровень 2: MFA подтверждение"
        APP_MFA["📱 Authenticator App<br/>(Google Authenticator)"]
        BIOMETRIC["👆 Биометрия<br/>(отпечаток/Face ID)"]
        BACKUP["🔑 Backup коды<br/>(8 одноразовых)"]
    end
    
    subgraph "Уровень 3: Критические операции"
        ADMIN_APPROVAL["👨‍💼 Подтверждение админа<br/>(изменение реквизитов)"]
        SMS_MANAGER["📱 SMS менеджеру<br/>(блокировка аккаунта)"]
    end
    
    LOGIN --> APP_MFA
    LOGIN --> BIOMETRIC
    APP_MFA --> ADMIN_APPROVAL
    BIOMETRIC --> SMS_MANAGER
    
    classDef level1 fill:#4ecdc4,stroke:#26a69a,color:#fff
    classDef level2 fill:#45b7d1,stroke:#339bc2,color:#fff
    classDef level3 fill:#ff6b6b,stroke:#ff5252,color:#fff
    
    class LOGIN level1
    class APP_MFA,BIOMETRIC,BACKUP level2
    class ADMIN_APPROVAL,SMS_MANAGER level3
```

#### 🎯 Действия по ролям
| Роль | Требуемые факторы | Критические операции |
|------|-------------------|---------------------|
| **👤 Курьер** | Пароль + Биометрия | Принятие заказа, изменение статуса |
| **📞 Оператор** | Пароль + Authenticator | Создание заказа, отмена заказа |
| **👨‍💼 Диспетчер** | Пароль + Authenticator + SMS | Переназначение заказов, блокировка курьеров |
| **👨‍💻 Администратор** | Пароль + Authenticator + Approval | Управление ролями, финансовые операции |

#### 🔐 Корпоративный порядок аутентификации
```mermaid
sequenceDiagram
    participant U as 👤 Сотрудник
    participant S as 🏛️ Система
    participant MFA as 📱 MFA App
    participant M as 👨‍💼 Менеджер
    
    U->>S: Логин + пароль + PIN
    S->>MFA: Запрос TOTP кода
    MFA->>U: Генерация 6-значного кода
    U->>S: Ввод TOTP кода
    
    alt Критическая операция
        S->>M: Запрос подтверждения
        M->>S: Подтверждение/отказ
        S->>U: Результат операции
    else Обычная операция
        S->>U: Доступ предоставлен
    end
```

## 📋 Требования к логированию

### 🅱️ Барбершоп - Базовое логирование

#### 📊 Операции, требующие логирования
```mermaid
graph LR
    subgraph "Пользовательские действия"
        AUTH_LOG["🔐 Авторизация/выход"]
        BOOKING_LOG["📅 Запись/отмена"]
        PAYMENT_LOG["💳 Платежные операции"]
        PROFILE_LOG["👤 Изменения профиля"]
    end
    
    subgraph "Системные события"
        ERROR_LOG["❌ Ошибки системы"]
        PERF_LOG["📈 Производительность"]
        SECURITY_LOG["🔒 События безопасности"]
        BACKUP_LOG["💾 Операции резервирования"]
    end
    
    classDef user fill:#ff6b6b,stroke:#ff5252,color:#fff
    classDef system fill:#45b7d1,stroke:#339bc2,color:#fff
    
    class AUTH_LOG,BOOKING_LOG,PAYMENT_LOG,PROFILE_LOG user
    class ERROR_LOG,PERF_LOG,SECURITY_LOG,BACKUP_LOG system
```

#### 📝 Логируемые данные для клиентов
| Категория | Данные | Формат | Хранение |
|-----------|--------|--------|----------|
| **🔐 Авторизация** | User ID, IP, время, результат | JSON | 90 дней |
| **📅 Записи** | User ID, мастер, время, статус | JSON | 3 года |
| **💳 Платежи** | User ID, сумма, метод, статус | Encrypted JSON | 7 лет |
| **👤 Профиль** | User ID, измененные поля, старые значения | JSON | 1 год |
| **❌ Ошибки** | User ID, endpoint, error code, stack trace | JSON | 30 дней |

#### 🔍 Пример лог-записи (Барбершоп)
```json
{
  "timestamp": "2024-01-15T14:30:00.000Z",
  "level": "INFO",
  "event": "user_booking_created",
  "user_id": "usr_12345",
  "ip_address": "192.168.1.100",
  "user_agent": "Mozilla/5.0...",
  "data": {
    "booking_id": "bk_67890",
    "master_id": "mst_456",
    "service": "haircut",
    "scheduled_time": "2024-01-20T10:00:00.000Z",
    "salon_id": "salon_789"
  },
  "session_id": "sess_abc123"
}
```

### 🚚 Доставка - Корпоративное логирование

#### 📊 Расширенные операции логирования
```mermaid
graph TB
    subgraph "Операционные логи"
        ORDER_CREATE["📦 Создание заказа"]
        ORDER_STATUS["🔄 Изменение статуса"]
        COURIER_ASSIGN["🚴‍♂️ Назначение курьера"]
        GPS_TRACK["📍 GPS трекинг"]
        PAYMENT_PROCESS["💰 Обработка платежей"]
    end
    
    subgraph "Безопасность и аудит"
        LOGIN_ATTEMPTS["🔐 Попытки входа"]
        PERMISSION_CHANGE["👥 Изменения ролей"]
        SENSITIVE_ACCESS["🔒 Доступ к чувствительным данным"]
        ADMIN_ACTIONS["👨‍💻 Административные действия"]
        API_CALLS["🔗 API вызовы"]
    end
    
    subgraph "Финансовые операции"
        COURIER_PAYMENT["💵 Выплаты курьерам"]
        COMMISSION_CALC["📊 Расчет комиссий"]
        REFUND_PROCESS["↩️ Возвраты"]
        ACCOUNTING_SYNC["📋 Синхронизация с 1С"]
    end
    
    classDef operational fill:#4ecdc4,stroke:#26a69a,color:#fff
    classDef security fill:#ff6b6b,stroke:#ff5252,color:#fff
    classDef financial fill:#ffd93d,stroke:#ffcc02,color:#333
    
    class ORDER_CREATE,ORDER_STATUS,COURIER_ASSIGN,GPS_TRACK,PAYMENT_PROCESS operational
    class LOGIN_ATTEMPTS,PERMISSION_CHANGE,SENSITIVE_ACCESS,ADMIN_ACTIONS,API_CALLS security
    class COURIER_PAYMENT,COMMISSION_CALC,REFUND_PROCESS,ACCOUNTING_SYNC financial
```

#### 📝 Детальные логируемые данные
| Категория | Данные | Уровень детализации | Хранение |
|-----------|--------|-------------------|----------|
| **📦 Заказы** | Полный lifecycle заказа | Каждое изменение | 7 лет |
| **🚴‍♂️ Курьеры** | GPS, статусы, платежи | Real-time трекинг | 3 года |
| **💰 Финансы** | Все транзакции + комиссии | Полная детализация | 10 лет |
| **🔐 Безопасность** | Все события доступа | Максимальная детализация | 5 лет |
| **📊 Аналитика** | Агрегированные метрики | Ежечасные/ежедневные | 2 года |

#### 🔍 Пример лог-записи (Доставка)
```json
{
  "timestamp": "2024-01-15T14:30:00.000Z",
  "level": "INFO",
  "event": "order_status_changed",
  "user_id": "courier_789",
  "role": "courier",
  "ip_address": "10.0.1.50",
  "location": {
    "lat": 55.7558,
    "lng": 37.6176,
    "accuracy": 5
  },
  "data": {
    "order_id": "ord_12345",
    "old_status": "assigned",
    "new_status": "picked_up",
    "restaurant_id": "rest_456",
    "estimated_delivery": "2024-01-15T15:00:00.000Z"
  },
  "session_id": "courier_sess_xyz789",
  "device_id": "device_courier_001",
  "app_version": "2.1.0"
}
```

## 🔐 Дополнительные меры безопасности

### 🅱️ Барбершоп - Потребительская защита
```mermaid
graph LR
    subgraph "Базовая защита"
        HTTPS["🔒 HTTPS везде"]
        BCRYPT["🔑 BCrypt пароли"]
        SESSION["🍪 Secure sessions"]
        RATE_LIMIT["⏱️ Rate limiting"]
    end
    
    subgraph "Защита данных"
        PII_MASK["🎭 Маскировка PII"]
        GDPR["📋 GDPR compliance"]
        DATA_MIN["📉 Минимизация данных"]
        AUTO_LOGOUT["⏰ Автологаут"]
    end
    
    classDef basic fill:#ff6b6b,stroke:#ff5252,color:#fff
    classDef privacy fill:#6bcf7f,stroke:#42a047,color:#fff
    
    class HTTPS,BCRYPT,SESSION,RATE_LIMIT basic
    class PII_MASK,GDPR,DATA_MIN,AUTO_LOGOUT privacy
```

### 🚚 Доставка - Корпоративная защита
```mermaid
graph LR
    subgraph "Усиленная защита"
        TLS_MUTUAL["🔒 Mutual TLS"]
        AES_256["🔐 AES-256 шифрование"]
        RBAC_STRICT["👥 Строгий RBAC"]
        WAF["🛡️ Web Application Firewall"]
    end
    
    subgraph "Мониторинг безопасности"
        SIEM["📊 SIEM система"]
        THREAT_DETECT["🎯 Threat detection"]
        ANOMALY["📈 Anomaly detection"]
        SOC_24_7["👁️ 24/7 SOC"]
    end
    
    classDef enhanced fill:#4ecdc4,stroke:#26a69a,color:#fff
    classDef monitoring fill:#45b7d1,stroke:#339bc2,color:#fff
    
    class TLS_MUTUAL,AES_256,RBAC_STRICT,WAF enhanced
    class SIEM,THREAT_DETECT,ANOMALY,SOC_24_7 monitoring
```

## 📊 Сравнительная таблица безопасности

| Аспект | 🅱️ Барбершоп (B2C) | 🚚 Доставка (B2B) |
|--------|---------------------|-------------------|
| **🔑 Пароли** | 8+ символов, базовая сложность | 12+ символов, высокая сложность |
| **🛡️ MFA** | SMS для критических операций | Обязательно для всех |
| **📋 Логирование** | Основные события, 90 дней | Полное логирование, 7+ лет |
| **🔒 Шифрование** | HTTPS + BCrypt | End-to-end + AES-256 |
| **👥 Доступ** | Простая авторизация | Role-based + принцип минимальных привилегий |
| **📊 Мониторинг** | Базовый | 24/7 SOC + SIEM |

## 🏆 Стратегические выводы

### ✅ Ключевые принципы:

1. **B2C (Барбершоп)**: **Баланс безопасности и удобства** - не отпугнуть клиентов
2. **B2B (Доставка)**: **Максимальная защита** - финансовые операции требуют строгости
3. **Логирование**: **Соответствие требованиям** - B2C минимум, B2B максимум
4. **MFA**: **Селективное vs обязательное** - разные подходы к внедрению

### 🎯 Практические рекомендации:

- **Барбершоп**: Постепенное внедрение 2FA, акцент на образование пользователей
- **Доставка**: Немедленное внедрение всех мер, обучение персонала обязательно
- **Обе системы**: Регулярные security аудиты и проникновения тесты

---

**📋 Оценка:** Шкала от 1 до 5 ⭐⭐⭐⭐⭐

**🎉 Проект завершен!** [Вернуться к обзору проекта](README.md) 