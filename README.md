# SubWatch

Subscription watchdog. Finds your subscriptions from Gmail receipts and warns you before renewals.

Live app: https://jairulez.github.io/subwatch/

## How it works

- Single-file static app (`index.html`), hosted on GitHub Pages. No backend, no database, no server cost.
- Google sign-in (Google Identity Services, client-side token flow).
- `gmail.readonly` scope: searches the last ~400 days for receipt/invoice/payment emails, parses service name, amount (INR/USD) and billing cycle, and groups repeat receipts into subscriptions.
- Dashboard: monthly burn (INR + USD), next renewal per sub, days-left countdown.
- Reminders: `calendar.events` scope creates an all-day event on the user's own Google Calendar on the renewal date with an email reminder 3 days before and a popup day-of. Google delivers the notifications; there is no reminder server.
- Manual add/edit/remove for subscriptions that have no email receipt.
- All state lives in the browser's localStorage. Nothing is sent anywhere except Google APIs.

## Google Cloud setup (one-time, already done)

- GCP project: `subwatch-508807` (owned by 4jairaj4@gmail.com)
- APIs enabled: Gmail API, Google Calendar API
- OAuth consent: app name "SubWatch", External, publishing status Testing (max 100 test users, shows "unverified app" warning until verified)
- Test users: 4jairaj4@gmail.com, jairulez24@gmail.com
- Scopes: `gmail.readonly` (restricted), `calendar.events` (sensitive)
- OAuth client: "SubWatch Web", Web application, authorized JS origin `https://jairulez.github.io`
- Client ID is embedded in `index.html` (public by design). Client secret exists in the console but is unused by this app.

## Publishing for everyone (future)

To remove the testing-mode warning and 100-user cap: add a privacy policy URL + homepage to the consent screen and submit for Google verification. Restricted scope (Gmail) verification has stricter requirements (security assessment) - evaluate before going public.

## Deploy

Push to `main`; GitHub Pages serves the repo root at https://jairulez.github.io/subwatch/.
