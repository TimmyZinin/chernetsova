# MentalStab 2.0 — SDD (Specification-Driven Development)

## Спецификация MVP лендинга

### Технология
- Single HTML file + inline CSS + vanilla JS
- Нет внешних JS-зависимостей (кроме Google Fonts)
- CSS-only анимации где возможно, JS для scroll-triggered effects
- Mobile-first responsive

### Fonts
```
Unbounded: 300-900 (display)
Onest: 300-700 (body)
JetBrains Mono: 400-600 (data/mono)
```

### Секции (top to bottom)

#### S1: Hero
- Полноэкранная секция, particle background (CSS radial-gradient + animated dots)
- Текст: "Mission Control для вайб-кодеров" (Unbounded 700)
- Подзаголовок: "Мы следим за твоим состоянием. Мы не дадим тебе сгореть."
- Animated SVG: пульсирующая линия сердечного ритма
- CTA: "Подключиться" → #pricing
- Статистика-полоса внизу: "68% выгорают | 0 конкурентов | 24/7 мониторинг"

#### S2: Problem
- Заголовок: "Ты оптимизируешь код. Кто оптимизирует тебя?"
- 4 карточки-проблемы с иконками (CSS glow):
  - 💤 "3 часа ночи. Ты ещё кодишь."
  - 🔥 "Выгорание. Ты не замечаешь, пока не поздно."
  - 📉 "HRV падает. Продуктивность — за ним."
  - 💔 "Отношения страдают. Ты 'просто занят'."
- Fade-in on scroll

#### S3: How It Works
- Заголовок: "Как работает Mission Control"
- 4 шага (горизонтальный timeline на desktop, вертикальный на mobile):
  1. 📊 "Чекин" — ежедневный опрос в Telegram-боте
  2. 📡 "Анализ" — AI + данные трекеров (HRV, сон, активность)
  3. 🚨 "Алерт" — обнаружили аномалию → уведомление
  4. 📞 "Интервенция" — бот → IVR → звонок психолога
- Каждый шаг с анимированной иконкой

#### S4: Dashboard Preview
- Mockup "Mission Control Dashboard" (CSS/SVG only):
  - Animated heartbeat line
  - HRV gauge (number + color)
  - Sleep score ring
  - Status: "NOMINAL" / "WARNING" / "CRITICAL"
  - Fake real-time updates (JS counter)
- Caption: "Вот что мы видим. Вот почему мы звоним."

#### S5: Pricing
- 3 карточки (glassmorphism, glow border):
  - **Autopilot** 4 900 ₽/мес — список фич, CTA "Начать"
  - **Co-Pilot** 14 900 ₽/мес — RECOMMENDED badge, CTA "Начать"
  - **Mission Control** 39 900 ₽/мес — premium glow, CTA "Начать"
- Под карточками: "Первый месяц -50%"
- CTA ведут на бота @mental_stability_leads_bot

#### S6: One-off Session
- Карточка: "Разовая консультация с Екатериной"
- Фото Кати (katya.jpg)
- "60 минут. IFS-подход. 7 500 ₽"
- CTA → Tribute ссылка
- Текст: "Не готов к подписке? Начни с одной сессии."

#### S7: Trackers
- Заголовок: "Мы подключаемся к твоим данным"
- Логотипы/иконки (CSS-drawn): Apple Watch, Oura Ring, WHOOP, Garmin
- Метрики-список: HRV, сон, ЧСС, стресс, активность
- "Скоро: Telegram-мониторинг, Screen Time"

#### S8: About Ekaterina
- Фото + bio
- "Екатерина Чернецова, психолог, IFS-терапевт, 5+ лет опыта"
- "4000+ консультаций. Специализация: выгорание, тревожность, вайб-кодеры."
- Calendly link

#### S9: FAQ
- Accordion (CSS-only с :target или details/summary):
  - "Что такое Mission Control?"
  - "Зачем нужен трекер?"
  - "Что если я не отвечу боту?"
  - "Это терапия?"
  - "Мои данные в безопасности?"
  - "Могу ли я отменить подписку?"

#### S10: CTA Footer
- Большой заголовок: "Не жди, пока сгоришь"
- CTA button: "Подключить Mission Control" → бот
- Мелкий текст: credits, Umami

### Responsive Breakpoints
- Mobile: < 768px (stack everything)
- Tablet: 768-1024px
- Desktop: > 1024px

### Performance
- No external JS libraries
- Lazy load below-fold content
- Preconnect Google Fonts
- Total HTML < 150KB target
