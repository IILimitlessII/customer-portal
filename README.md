# Customer Portal (PayView)

Full-stack customer portal for secure online payments, account management, address book, payment history, and AI-assisted customer support.

## Project layout

- `frontend/` — React + TypeScript + Vite SPA (Radix UI, Tailwind CSS, React Router)
- `customer-express/` — Express API packaged as Netlify serverless functions (`netlify/functions/api.js`)

## Tech stack

- React 18, TypeScript, Vite
- Express, Netlify Functions, Supabase
- Resend (email), Google reCAPTCHA, Gemini AI
- Tailwind CSS, shadcn/ui-style components

## Prerequisites

- Node.js 18+
- [Netlify CLI](https://docs.netlify.com/cli/get-started/) (for local API development)
- Supabase project, Resend API key, reCAPTCHA keys, and Gemini API key

## Quick start

### Frontend

From this folder:

```bash
cd frontend
npm install
npm run dev
```

Create `frontend/.env` with at least:

- `VITE_EXPRESS_URL` — API base URL (e.g. `http://localhost:8888/.netlify/functions/api` for local Netlify dev)
- `VITE_SITE_KEY` — reCAPTCHA site key
- `VITE_GEMINI` — Gemini API key (client-side chat widget)

### API (local)

```bash
cd customer-express
npm install
npm start
```

Create `customer-express/.env` with at least:

- `GEMINI_API`
- `RESEND_API_KEY`
- `SUPABASE_KEY`
- `RECAPTCHA_SECRET`

`npm start` runs `netlify dev`, which serves functions and applies the `/api/*` redirect from `netlify.toml`.

## Deployment

- Frontend and API are intended for [Netlify](https://www.netlify.com/) deployment.
- Production API routes are prefixed with `/api/` and proxied to `/.netlify/functions/api/:splat`.

## API reference

All endpoints below are relative to the API base URL. POST requests use a JSON body unless noted. Authenticated routes require a `sessionToken` (or `token`) from login/OTP flow.

### Authentication

| Method | Path | Description |
| --- | --- | --- |
| POST | `/register` | Register user; sends verification email. Body: `fullname`, `email`, `password`, `capVal`. |
| GET | `/confirm-email` | Confirm email via query `id`, `confirmation_gen`; redirects to login on success. |
| POST | `/login` | Login with `accountNumber`, `password`, `capVal`; sends OTP email; returns `sessionToken`. |
| POST | `/verify-otp` | Verify OTP with `otp`, `sessionToken`. |
| POST | `/forgotpassword` | Reset password email. Query: `accountNumber`, `token`. |
| POST | `/changepassword` | Change password. Body: `token`, `password`, `newpassword`. |
| POST | `/logout` | Clear session. Body: `sessionToken`. |

### User data (authenticated)

| Method | Path | Description |
| --- | --- | --- |
| POST | `/address` | Add address: `line_1`, `line_2`, `province`, `city`, `postal_code`, `token`. |
| POST | `/myaddresses` | List addresses for user. Body: `token`. |
| POST | `/payment` | Process payment. Body includes `email`, `fullName`, `paymentMethod`, `amount`, `token`, and card fields when required. |
| POST | `/mypayments` | List payments for user. Body: `token`. |
| POST | `/userdetails` | Get user profile. Body: `token`. |
| POST | `/edituser` | Update `fullname` and `email`. Body: `token`, `fullname`, `email`. |

### Customer support

| Method | Path | Description |
| --- | --- | --- |
| POST | `/customerform` | Submit support message; Gemini generates a reply emailed via Resend. Body: `fullname`, `email`, `message`. |

### Notes

- reCAPTCHA (`capVal`) is required on `/register` and `/login`.
- Payment methods: `1` Credit Card, `2` PayPal, `3` Stripe, `4` Apple Pay.
- Password complexity is enforced on registration and password changes.
