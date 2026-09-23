# Driver Rating PWA

Taksi haydovchilarini baholash uchun Progressive Web App. Mijoz QR kod skanerlash yoki mashina raqami orqali haydovchini topib baho qo'yadi; haydovchi o'z statistikasini ko'radi; admin haydovchilarni va shikoyatlarni boshqaradi.

## Texnologiyalar

| Qatlam | Stack |
|--------|-------|
| Backend | Node.js, Express 4, TypeScript, PostgreSQL |
| Frontend | React 18, React Router 6, Vite, Tailwind CSS |
| Auth | Session-based (express-session + PostgreSQL store), Telegram Mini App / OTP |
| Test | Vitest (unit), Playwright (E2E), fast-check (property-based) |
| Infra | Docker Compose, PM2, Nginx |
| SMS | Eskiz.uz / TextUp.uz (OTP) |

## Rollar

- **Mijoz** — telefon (OTP) yoki Telegram orqali kirib, haydovchini QR kod/mashina raqami bo'yicha topadi va baholaydi
- **Haydovchi** — mashina raqami + parol bilan kirib, o'z reytingi va sharhlarini ko'radi
- **Admin** — haydovchilarni qo'shadi/boshqaradi, shikoyatlarni ko'rib chiqadi

## Arxitektura

```
Frontend (React) → REST API (/api/*) → Express routes → PostgreSQL
```

- `backend/src/routes` — auth, drivers, ratings, admin, driver/me, upload, complaints
- `backend/src/services/telegramAuth.ts` — Telegram Login (id_token, JWKS orqali tekshiriladi)
- `backend/db/migrations` — ketma-ket SQL migratsiyalar
- `frontend/src/pages` — mijoz, haydovchi va admin sahifalari
- Offline: Service Worker + Background Sync (baholash tarmoqsiz ham navbatga qo'yiladi)

## Ishga tushirish

### Backend

```bash
cd backend
cp .env.example .env   # DATABASE_URL, SESSION_SECRET va SMS kalitlarini to'ldiring
npm install
npm run dev             # http://localhost:3000
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

### Docker (PostgreSQL)

```bash
docker compose up -d
```

## Test

```bash
cd backend && npm test        # vitest
cd frontend && npm test       # vitest
cd frontend && npm run test:e2e   # playwright
```

## Muhit o'zgaruvchilari (`backend/.env`)

```
DATABASE_URL=postgresql://user:password@localhost:5432/db
SESSION_SECRET=
PORT=3000
ESKIZ_EMAIL=
ESKIZ_PASSWORD=
TEXTUP_EMAIL=
TEXTUP_PASSWORD=
TEXTUP_USER_ID=
TELEGRAM_CLIENT_ID=
```

> `.env` faylini hech qachon commit qilmang — real qiymatlar faqat lokal muhitda yoki server sirlarida saqlansin.
