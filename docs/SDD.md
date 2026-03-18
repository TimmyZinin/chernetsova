# MentalStab 2.0 — SDD (Specification-Driven Development)

> **Source of Truth:** `docs/SDD.md` в репозитории `chernetsova`
> **Версия:** 2.0 | **Дата:** 18 мар 2026 | **Автор:** Тим Зинин
> **HTML-версия:** mentalstab.com/sdd.html
> **PRD:** docs/PRD.md

---

## 1. Обзор системы

MentalStab — система из трёх компонентов:

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   Лендинг        │     │   TG-бот + API    │     │  Health API      │
│   (GitHub Pages) │     │   (Contabo:8095)  │     │  (Contabo:8096)  │
│   mentalstab.com │     │   @mental_sta...  │     │  FastAPI          │
└─────────────────┘     └──────────────────┘     └─────────────────┘
                              ↑                         ↑
                         Telegram                 Oura / Apple Watch
                         (пользователь)           (Health Auto Export)
```

---

## 2. Компонент A: Лендинг (mentalstab.com)

### Технология
- Single HTML file + inline CSS + inline JS
- Нет внешних JS-зависимостей (кроме Google Fonts)
- CSS-only анимации где возможно, JS для scroll-triggered и интерактив
- Mobile-first responsive
- GitHub Pages деплой

### Fonts
```
Unbounded:      300-900 (display, заголовки)
Onest:          300-700 (body, описания)
JetBrains Mono: 400-600 (data, метрики, статусы, код)
```

### Дизайн-система (по ресёрчу Linear, WHOOP, Vercel, Oura, Headspace)

#### Палитра
```css
--bg: #0D0D1A;                        /* тёплый near-black */
--bg-surface: #12121F;                /* elevated */
--bg-elevated: #1A1A2E;              /* cards */
--bg-card: rgba(255,255,255,0.03);   /* glassmorphism */
--border: rgba(255,255,255,0.08);
--text: #F0EEF8;                      /* тёплый white */
--text-70: rgba(240,238,248,0.7);    /* secondary */
--text-50: rgba(240,238,248,0.5);    /* tertiary */
--text-30: rgba(240,238,248,0.3);    /* muted */
--violet: #7C3AED;                    /* primary accent */
--violet-glow: rgba(124,58,237,0.5);
--cyan: #06B6D4;                      /* data/biometrics */
--green: #22EF8B;                     /* NOMINAL */
--amber: #F59E0B;                     /* WARNING */
--red: #EF4444;                       /* CRITICAL */
```

#### Фоновые эффекты
1. **Dot-grid** (Linear) — CSS `radial-gradient` точки 40x40px, `mask-image` ellipse, step-animation 3000ms
2. **Ambient glow orbs** — 2 fixed `radial-gradient` div: violet (left) + cyan (right), animation 25s drift
3. **Звёзды** — CSS `radial-gradient` множество 1-2px точек с разной opacity

#### Кнопки
- **Primary:** background var(--violet), border-radius 10px, hover: glow `0 0 20px var(--violet-glow)`, chevron SVG icon
- **Secondary:** transparent, border 1px rgba(255,255,255,0.15), hover: background rgba(255,255,255,0.05)

### AI-сгенерированные изображения (5 шт, Gemini 2.5 Flash Image через OpenRouter)

| Файл | Описание | Секция |
|------|----------|--------|
| `img1_mission_control.png` | ЦУП с голографическими экранами, cyan/amber подсветка | How It Works |
| `img2_astronaut_coder.png` | Разработчик в скафандре за рабочей станцией | Hero |
| `img3_burnout_fire.png` | Горящий силуэт человека за ноутбуком (burnout) | Problem |
| `img4_dashboard_screens.png` | Парящие голографические дашборды с health-данными | Dashboard |
| `img5_recovery.png` | Человек медитирует в космосе среди звёзд | Recovery |
| `katya.jpg` | Фото Екатерины Чернецовой | Session |

### Секции лендинга

#### S1: Hero (fullscreen)
- **Фон:** `img2_astronaut_coder.png` (full-bleed, object-fit cover)
- **Overlay:** linear-gradient снизу 60% → чёрный
- **Dot-grid** поверх изображения
- **Заголовок:** "Mission Control для вайб-кодеров" (Unbounded 900, clamp 40-96px)
- **Подзаголовок:** "Мы следим за твоим состоянием. Мы не дадим тебе сгореть."
- **Live metrics ticker** (JetBrains Mono): "HRV: 67ms ✓ | STRESS: LOW | SLEEP: 7.2h | STATUS: NOMINAL" — typing/terminal animation
- **Badge:** "В среднем HRV улучшается на 23% за первый месяц" (embedded metric, Vercel-стиль)
- **CTA:** violet button "Подключиться" → #pricing
- **Stats bar:** "68% разработчиков выгорают • 0 прямых конкурентов • 24/7 мониторинг"
- **SVG heartbeat** animated line (stroke-dashoffset)

#### S2: Ticker/Marquee
- Full-width бесконечная прокрутка (CSS translateX)
- Контент: "🔥 68% разработчиков выгорают • 📊 HRV-мониторинг • 🚀 Founding member → −20% навсегда • 🧠 AI-анализ • ⚡ Автозвонок при критических показателях"
- Тёмный фон, violet text, border-top/bottom glow

#### S3: Problem
- **Фон:** `img3_burnout_fire.png` (full-bleed, min-height 100vh)
- **Overlay:** 85% opacity dark
- **Заголовок большой:** "3:47 AM" (red, 100-140px) + "Ты ещё кодишь. Claude не устал. А ты?"
- **4 карточки** (glassmorphism поверх изображения):
  - 🌙 "3 часа ночи. Ты ещё кодишь."
  - 🔥 "Выгорание подкрадывается незаметно."
  - 📉 "HRV падает. Продуктивность — за ним."
  - 💔 "Отношения страдают. Ты 'просто занят'."
- IntersectionObserver fade-in

#### S4: How It Works
- **Фон:** `img1_mission_control.png` (full-bleed)
- **Заголовок:** "Как работает Mission Control"
- **4 шага** (timeline с glowing line):
  1. 📊 **Чекин** — "Ежедневный опрос в Telegram. 30 секунд."
  2. 📡 **Анализ** — "AI + данные трекеров: HRV, сон, стресс"
  3. 🚨 **Алерт** — "Аномалия? Мы заметим раньше тебя"
  4. 📞 **Интервенция** — "Бот → Автозвонок → Психолог лично"

#### S5: Scenario Tabs (NEW, Oura-стиль)
- **4 вкладки:** `[Во время спринта]` `[После деплоя]` `[3 часа ночи]` `[Выходной]`
- Каждая показывает "Mission Control view":
  - Sprint: "Пульс 95 bpm. Фокус: 4ч без перерыва. → 10-мин перерыв через 30 мин"
  - Deploy: "HRV ↓15%. Кортизол повышен. → Дыхательная практика 5 мин"
  - 3AM: "⚠️ TG активность обнаружена. Сон: 0ч. → Автозвонок через 15 мин"
  - Weekend: "Восстановление 87%. Сон 8.1ч. STATUS: NOMINAL ✓"
- JS tab switching с smooth transitions

#### S6: Dashboard Preview
- **Фон:** `img4_dashboard_screens.png` (full-bleed)
- **Live dashboard** (CSS/SVG):
  - Animated ECG line
  - HRV number (updating via JS setInterval)
  - Sleep score ring (SVG circle progress)
  - Status badge: NOMINAL / WARNING
  - Log feed: scrolling events
- **Caption:** "Вот что мы видим. Вот почему мы звоним."

#### S7: Social Proof
- **Dual credibility** (WHOOP): "Создано разработчиками. Проверено психологией."
- Counter: "47 вайб-кодеров подключены" (animated)
- Метрики: "HRV +23% • Deep work +4.2ч • Стресс-инциденты −67%"
- Пульсирующий green dot: "4 человека подключились сегодня"

#### S8: Recovery
- **Фон:** `img5_recovery.png` (full-bleed)
- **Заголовок:** "Мы возвращаем тебе контроль"
- Описание ценности подписки

#### S9: Pricing (id="pricing")
- **Фон:** solid dark (чистый, без изображения — контраст)
- **Toggle:** Помесячно / Годовая (−30%)
- **3 карточки:**
  - Autopilot 4 900 ₽/мес → CTA → TG bot
  - Co-Pilot 14 900 ₽/мес → **RECOMMENDED**, violet glow border → TG bot
  - Mission Control 39 900 ₽/мес → premium gradient border → TG bot
- **Founding member badge:** "Осталось 12 мест" + countdown
- **Risk reversal:** "30 дней — деньги вернём"

#### S10: Екатерина (разовая сессия)
- `katya.jpg` с glow-рамкой
- "Екатерина Чернецова • Психотерапевт • IFS • 5+ лет"
- "60 мин. 7 500 ₽. 4000+ консультаций."
- **Важно:** "MentalStab — мониторинг и поддержка. Екатерина работает в формате консультирования. Психотерапия для участников на выгодных условиях. При необходимости — направление к психиатру или неврологу."
- CTA → TG bot

#### S11: Trackers
- CSS-mockupы устройств: Apple Watch, Oura Ring, WHOOP, Garmin
- Метрики: HRV, Сон, ЧСС, Стресс, Активность
- "Скоро: Telegram-мониторинг, Git Commits, Spotify Valence"

#### S12: FAQ
- HTML `<details><summary>` (no JS):
  - "Что такое Mission Control?"
  - "Зачем мне трекер?"
  - "Что если я не отвечу боту?"
  - "Это терапия?" → "Нет. Мониторинг и консультирование. Не медицинская помощь."
  - "Мои данные в безопасности?" → "152-ФЗ, серверы в РФ, шифрование"
  - "Могу отменить?" → "В любой момент, без штрафов"

#### S13: Final CTA
- Full-height секция
- "Не жди, пока сгоришь"
- Countdown (founding member deadline)
- Giant pulsing violet button → TG bot
- Toast notifications (fake social proof, 9-12s interval)
- Ember particles (CSS) rising up

#### Footer
- "MentalStab © 2026 • Продюсер: Тим Зинин"
- Umami analytics

### Conversion элементы
- **Ticker marquee** (после hero)
- **Toast notifications** (bottom-left, каждые 9-12s): "Иван К. подключил Co-Pilot 3 мин назад"
- **Urgency:** "Осталось N мест founding member"
- **Social proof counter:** "47 вайб-кодеров подключены"
- **Risk reversal:** "30 дней — деньги вернём"

### Responsive Breakpoints
- Mobile: < 768px (stack, reduce font sizes, vertical timeline)
- Tablet: 768-1024px (2-col grids)
- Desktop: > 1024px (full layout)

### Performance
- No external JS libraries
- `loading="lazy"` на всех изображениях
- Images: `object-fit: cover` в контейнерах с `overflow: hidden`
- Preconnect Google Fonts
- CSS animations: transform + opacity only (GPU-accelerated)
- Target: < 200KB HTML + ~7MB images (lazy loaded)

---

## 3. Компонент B: TG-бот + Lead API

### Текущее состояние
- **Файл:** `~/mental-stability-bot/app.py` (513 строк, aiohttp)
- **Контейнер:** `mental-stability-bot` на Contabo:8095
- **БД:** SQLite `/data/leads.db`
- **Таблицы:** `leads`, `bot_users`, `payments`
- **Webhook:** `/webhook` (TG bot updates), `/webhook/tribute` (Tribute payments)
- **API:** `/api/lead` (сайт), `/api/crm` (дашборд), `/api/leads` (список)

### Планируемые изменения (Sprint 2)
- Добавить ежедневные чекины (настроение/сон/энергия/стресс 1-5)
- Хранить историю чекинов в `bot_users` или новой таблице `checkins`
- AI-анализ (7-дневное скользящее среднее, тренды)
- Подключить к Health API для получения алертов

---

## 4. Компонент C: Health API (ПЛАНИРУЕТСЯ)

### Спецификация

**Технология:** FastAPI + SQLite (→ PostgreSQL при масштабе)
**Порт:** 8096 на Contabo VPS 30
**Файлы:**
```
~/mental-stability-bot/
├── app.py               ← существующий бот
├── health_api.py         ← NEW: FastAPI для приёма health-данных
├── baseline.py           ← NEW: алгоритм anomaly detection
├── docker-compose.yml    ← добавить порт 8096
└── data/
    └── leads.db          ← существующая БД (расширить)
```

### Endpoints

| Method | Path | Описание |
|--------|------|----------|
| POST | `/health/apple` | Приём данных от Health Auto Export |
| POST | `/health/oura/webhook` | Oura webhook callback |
| GET | `/health/oura/auth` | Начало OAuth flow для Oura |
| GET | `/health/oura/callback` | OAuth callback для Oura |
| GET | `/health/user/{chat_id}/summary` | Текущий статус пользователя |
| GET | `/health/user/{chat_id}/baseline` | Baseline metrics |

### Модель данных (расширение leads.db)

```sql
CREATE TABLE health_data (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    chat_id INTEGER NOT NULL,
    source TEXT NOT NULL,           -- 'apple_watch', 'oura'
    metric TEXT NOT NULL,           -- 'hrv', 'rhr', 'sleep_score', 'spo2'
    value REAL NOT NULL,
    recorded_at TEXT NOT NULL,      -- ISO timestamp от устройства
    received_at TEXT NOT NULL,      -- когда получили
    FOREIGN KEY (chat_id) REFERENCES bot_users(chat_id)
);

CREATE TABLE baselines (
    chat_id INTEGER PRIMARY KEY,
    hrv_7d_avg REAL,
    rhr_7d_avg REAL,
    sleep_score_7d_avg REAL,
    updated_at TEXT NOT NULL,
    FOREIGN KEY (chat_id) REFERENCES bot_users(chat_id)
);

CREATE TABLE alerts (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    chat_id INTEGER NOT NULL,
    type TEXT NOT NULL,             -- 'hrv_drop', 'night_activity', 'missed_checkin'
    severity TEXT NOT NULL,         -- 'info', 'warning', 'critical'
    message TEXT NOT NULL,
    escalation_level INTEGER DEFAULT 1,  -- 1=bot, 2=repeat, 3=ivr, 4=call
    created_at TEXT NOT NULL,
    acknowledged_at TEXT,
    FOREIGN KEY (chat_id) REFERENCES bot_users(chat_id)
);
```

### Baseline-алгоритм

```python
def check_anomaly(chat_id: int, metric: str, value: float) -> Optional[Alert]:
    baseline = get_baseline(chat_id)
    if metric == "hrv" and baseline.hrv_7d_avg:
        drop_pct = (baseline.hrv_7d_avg - value) / baseline.hrv_7d_avg * 100
        if drop_pct > 20:
            return Alert(type="hrv_drop", severity="warning",
                         message=f"HRV упал на {drop_pct:.0f}% ({value:.0f}ms vs avg {baseline.hrv_7d_avg:.0f}ms)")
        if drop_pct > 35:
            return Alert(type="hrv_drop", severity="critical", ...)
    return None
```

---

## 5. Верификация

### Лендинг
1. `curl -sI https://mentalstab.com` → HTTP 200
2. Визуальная проверка (Playwright / Safari screenshot)
3. Mobile responsive (768px, 375px)
4. Все CTA → `https://t.me/mental_stability_leads_bot`
5. Анимации 60fps, нет layout shift
6. Изображения загружаются (lazy)

### TG-бот
1. `/start` → приветствие + опрос
2. Заполнение анкеты → лид в БД → уведомление админам
3. `/leads` `/stats` — работают для админов
4. Health endpoint: `curl -sI http://185.202.239.165:8095/health` → 200

### Health API (после реализации)
1. POST `/health/apple` с тестовым JSON → 200, данные в БД
2. Oura OAuth flow → токен сохранён → webhook получает данные
3. Baseline рассчитывается после 7 дней данных
4. Алерт при HRV < baseline−20% → TG notification пользователю

---

## Changelog

| Дата | Версия | Изменения |
|------|--------|-----------|
| 18 мар 2026 | 2.0 | Полная переработка. Добавлены: дизайн-система (палитра по ресёрчу конкурентов), AI-иллюстрации (5 шт), scenario tabs, social proof, conversion элементы, Health API спецификация, baseline-алгоритм, модель данных, эскалация интервенций. |
