# 🏷️ Exercise 01 — Классификация нефункциональных требований

<!--
Student: @https://edu.21-school.ru/profile/lunchlpr
Location: SKD SAMARKAND  
GitHub: https://github.com/wh0mever
![Levi Ackerman](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fc.tenor.com%2FHGPFeIo8looAAAAC%2Flevi-ackerman.gif&f=1&nofb=1&ipt=740d7fa033496bb69c29f0fdb0f6d2c01c9e8f9d2d092c25b0c1a31079cb9c3f)
-->

## 📋 Задание

**Сопоставить нефункциональные требования с типами НФТ и разместить результаты в таблицу.**

## 🎯 Типы НФТ для классификации:
1. **Доступность** 2. **Надежность** 3. **Масштабируемость** 4. **Локализация** 5. **Удобство использования**
6. **Производительность** 7. **Совместимость** 8. **Долговечность** 9. **Безопасность**

## 🔍 Классификация НФТ

### 📊 ASCII схема классификации

```
🎯 КЛАССИФИКАЦИЯ НЕФУНКЦИОНАЛЬНЫХ ТРЕБОВАНИЙ
══════════════════════════════════════════════════════════════════════

┌─────────────────────────────────────────────────────────────────────┐
│                        КАРТА ТИПОВ НФТ                             │
└─────────────────────────────────────────────────────────────────────┘

   ┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
   │   БЕЗОПАСНОСТЬ  │      │ ПРОИЗВОДИТЕЛЬНОСТЬ│     │   ДОСТУПНОСТЬ   │
   │      🔒         │      │        ⚡       │      │       🌐        │
   ├─────────────────┤      ├─────────────────┤      ├─────────────────┤
   │ НФТ 1,3         │      │ НФТ 10          │      │ НФТ 6,9         │
   │ • Админ права   │      │ • Время отклика │      │ • 24/7 работа   │
   │ • Запрет доступа│      │ • Выбор за 3с   │      │ • SLA: 99%/90%  │
   └─────────────────┘      └─────────────────┘      └─────────────────┘

   ┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
   │  ДОЛГОВЕЧНОСТЬ  │      │ МАСШТАБИРУЕМОСТЬ│      │  ЛОКАЛИЗАЦИЯ    │
   │       📦        │      │       📈        │      │       🌍        │
   ├─────────────────┤      ├─────────────────┤      ├─────────────────┤
   │ НФТ 2           │      │ НФТ 7           │      │ НФТ 5           │
   │ • Хранение 5 лет│      │ • Поэтапное     │      │ • Региональное  │
   │                 │      │   развертывание │      │   время         │
   └─────────────────┘      └─────────────────┘      └─────────────────┘

   ┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
   │   НАДЕЖНОСТЬ    │      │  СОВМЕСТИМОСТЬ  │      │ УДОБСТВО ИСПОЛЬ.│
   │       🛡️        │      │       🔗        │      │       ✨        │
   ├─────────────────┤      ├─────────────────┤      ├─────────────────┤
   │ НФТ 4           │      │ НФТ 11          │      │ НФТ 8           │
   │ • Восстановление│      │ • Поддержка     │      │ • Современный   │
   │   за 30 минут   │      │   браузеров     │      │   дизайн        │
   └─────────────────┘      └─────────────────┘      └─────────────────┘
```

### 🧠 D2 диаграмма классификации (современная альтернатива)

```d2
# Классификация НФТ по типам
classification: Классификация НФТ {
  style.fill: "#E8F5E8"
  shape: hexagon
}

# Security cluster
security: Безопасность {
  style.fill: "#FFEBEE"
  shape: circle
  
  nft1: "НФТ 1: Админ права"
  nft3: "НФТ 3: Запрет доступа"
}

# Performance cluster  
performance: Производительность {
  style.fill: "#FFF3E0"
  shape: circle
  
  nft10: "НФТ 10: Время отклика ≤3-6с"
}

# Availability cluster
availability: Доступность {
  style.fill: "#E3F2FD"
  shape: circle
  
  nft6: "НФТ 6: 24/7 доступ"
  nft9: "НФТ 9: SLA 99%/90%"
}

# Other types
durability: Долговечность {nft2: "НФТ 2: 5 лет хранения"}
scalability: Масштабируемость {nft7: "НФТ 7: Поэтапное развертывание"}
localization: Локализация {nft5: "НФТ 5: Региональное время"}
reliability: Надежность {nft4: "НФТ 4: Восстановление 30 мин"}
compatibility: Совместимость {nft11: "НФТ 11: Браузеры"}
usability: Удобство использования {nft8: "НФТ 8: Современный UI"}

classification -> security
classification -> performance  
classification -> availability
classification -> durability
classification -> scalability
classification -> localization
classification -> reliability
classification -> compatibility
classification -> usability
```

## 📋 Результат классификации

### 🎯 Основная таблица соответствий

| НФТ № | Описание требования | Тип НФТ | Ключевые слова |
|-------|-------------------|---------|----------------|
| **1** | Администратор: назначение и корректировка ролей | **9. Безопасность** | "назначение ролей", "права доступа" |
| **2** | Хранение информации об оплатах ≥ 5 лет | **8. Долговечность** | "не менее 5 лет", "сохранение данных" |
| **3** | Запрет доступа к ролям (кроме администратора) | **9. Безопасность** | "запретить доступ", "кроме роли" |
| **4** | Восстановление системы ≤ 30 минут | **2. Надежность** | "восстановление", "после сбоя" |
| **5** | Региональное время после выбора региона | **4. Локализация** | "региональное время", "выбор региона" |
| **6** | Доступ 24/7 (кроме техобслуживания) | **1. Доступность** | "круглосуточно", "семь дней в неделю" |
| **7** | Поэтапное внедрение (2+4 региона) | **3. Масштабируемость** | "внедрение в регионах", "расширение" |
| **8** | Современные тренды UI + настройка цветов | **5. Удобство использования** | "современные тренды", "настройка" |
| **9** | SLA: 99% (9:00-21:00), 90% (остальное время) | **1. Доступность** | "доступна", "процент времени" |
| **10** | Выбор мастера/услуги за 3-6 секунд при 1000 юзеров | **6. Производительность** | "в среднем за 3 секунды", "не более 6" |
| **11** | Поддержка браузеров Chrome, Safari, Firefox | **7. Совместимость** | "поддерживать", "браузеры", "версии" |

### 📊 Статистика по типам НФТ

```
РАСПРЕДЕЛЕНИЕ НФТ ПО ТИПАМ
═══════════════════════════════════════

Безопасность        ██ (2 НФТ)    18.2%
Доступность         ██ (2 НФТ)    18.2%
Долговечность       █  (1 НФТ)     9.1%
Надежность          █  (1 НФТ)     9.1%
Локализация         █  (1 НФТ)     9.1%
Масштабируемость    █  (1 НФТ)     9.1%
Удобство использов. █  (1 НФТ)     9.1%
Производительность  █  (1 НФТ)     9.1%
Совместимость       █  (1 НФТ)     9.1%

ЛИДЕРЫ: Безопасность и Доступность
```

### 🎯 Схема принятия решений

```
АЛГОРИТМ КЛАССИФИКАЦИИ НФТ
═══════════════════════════════════════════════════════════

┌─────────────────┐
│ Читаем НФТ      │
└─────────┬───────┘
          │
          ▼
    ┌───────────────────┐
    │ Есть временные    │ ──YES──► Производительность
    │ ограничения?      │         или Надежность
    └─────────┬─────────┘
              │ NO
              ▼
    ┌───────────────────┐
    │ Упоминается       │ ──YES──► Безопасность
    │ доступ/права?     │
    └─────────┬─────────┘
              │ NO  
              ▼
    ┌───────────────────┐
    │ Говорится о       │ ──YES──► Доступность
    │ времени работы?   │
    └─────────┬─────────┘
              │ NO
              ▼
    ┌───────────────────┐
    │ Период хранения   │ ──YES──► Долговечность
    │ данных?           │
    └─────────┬─────────┘
              │ NO
              ▼
          [Другие типы]
```

## 🏆 Ключевые выводы

### ✅ Основные инсайты
1. **Безопасность** и **Доступность** - самые частые типы НФТ (по 18.2%)
2. **Производительность** всегда связана с временными ограничениями
3. **Локализация** проявляется через региональную адаптацию
4. **Масштабируемость** выражается в поэтапном развертывании

### 🎯 Критерии классификации

```
╔═══════════════════════════════════════════════════════════════╗
║                    ШПАРГАЛКА ПО ТИПАМ НФТ                    ║
╠═══════════════════════════════════════════════════════════════╣
║ 🔒 Безопасность      → "права", "доступ", "запрет"          ║
║ ⚡ Производительность → "секунды", "время отклика"           ║
║ 🌐 Доступность       → "24/7", "процент времени"            ║
║ 📦 Долговечность     → "лет", "хранение данных"             ║
║ 🛡️ Надежность        → "восстановление", "сбой"             ║
║ 🌍 Локализация       → "региональный", "время зоны"         ║
║ 📈 Масштабируемость  → "внедрение", "расширение"            ║
║ ✨ Удобство          → "тренды", "интерфейс"                ║
║ 🔗 Совместимость     → "браузеры", "версии"                 ║
╚═══════════════════════════════════════════════════════════════╝
```

---

**📋 Оценка:** Шкала от 1 до 5 ⭐⭐⭐⭐⭐

**🔄 Следующий шаг:** [Exercise 02 - Выбор НФТ](exercise_02.md) 
