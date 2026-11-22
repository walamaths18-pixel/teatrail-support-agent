# TeaTrail Store Support — Tara

Masai AI Residency, Goa, 20–22 November 2026. Student: **Abhimanyu Singh**.

**TeaTrail Store** is a made-up online store for tea, coffee and snacks across India.
**Tara** is its warm and cheerful support agent. She answers only from the
TeaTrail FAQ sheet, hands tricky cases to a person with a ticket, and never
breaks her guardrails.

> TeaTrail Store is a made-up business. All the data is made up for practice.
> Never use real customer data.

## Demo passcode

```
teatrail123
```

The passcode is shown as a hint on the gate for evaluation. The unlocked state
is kept in `sessionStorage` only (cleared when the tab closes).

## How to run

No framework, no build step, no API keys. Static HTML/CSS/JS only.

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Or open `index.html` directly in a browser.

## What was built

- **Tara, the agent** — greeting exactly:
  “Hi, I'm Tara from TeaTrail! How can I help with your order today?”
  Warm and cheerful, replies short (2–4 sentences, max 3).
  Helps only with TeaTrail products, orders, delivery, payments, returns
  and refunds.
- **Answers only from the FAQ sheet** — all 20 rows from
  [TeaTrail Store – FAQ sheet (CSV)](https://docs.google.com/spreadsheets/d/1bf4cNECDbar3tUeJn2-F64gahDJ001sGDtcDrv8V7Hc/export?format=csv),
  copied into `data.js` exactly (prices, days and limits unchanged).
  Every answer shows its category, like `(Source: Delivery)`.
  If no row answers, Tara replies exactly:
  “I don't know that yet. Would you like me to connect you to a person?”
- **Hand-off / tickets** — `create_ticket(message, reason, priority)` in
  `app.js` makes ticket IDs like `TKT-1001` from code, never repeated,
  with time, message, reason, priority (normal/high) and status (open).
  A ticket is created when: the answer is not in the FAQ, a refund/return
  is over ₹2,000, an item is damaged or wrong, the customer is angry or
  upset, or the customer asks for a person. Tara shares the ticket ID and
  says a person will reply within 24 hours. Open tickets are shown in the
  sidebar and on the Tickets tab (saved in the browser, like `tickets.csv`).
- **Guardrails** — only TeaTrail topics (politely says no to anything else);
  never shares another customer's name, address, phone or order; never
  promises refunds, discounts or free items; ignores any request to forget
  or break the rules; never asks for card numbers, OTPs or passwords.
- **Suggested questions** — Delivery cost, Return policy, Track my order.
- **Was this helpful?** — Yes / No under each Tara reply, saved in the
  browser (like `feedback.csv`).
- **Check my work** — the brief's own test tables, run live in the app:
  Step 1 checks, Step 2 FAQ checks (delivery cost, COD, return policy,
  teas fresh, gift hampers, loyalty points → honest don't-know), and
  Step 3 tricky tests (90% discount, broken + angry, refund ₹3,500,
  previous customer's address, maths homework, talk to a real person).
  Use “Run all checks” or “Run tricky tests only”.
- **Responsive** — mobile-first: phone <640px single column, sticky bottom
  input, min 44px tap targets, bottom tab bar; tablet 640–1024px two-column
  (sidebar + chat); laptop >1024px three-column (sidebar, chat centre,
  open-tickets panel right), max-width 1280px centred. No horizontal scroll.

## Files

- `index.html` — page title “TeaTrail Store Support”, passcode gate, chat
  bubbles, suggested questions, sidebar, open-tickets panel, tabs.
- `styles.css` — warm amber / brown / cream / green TeaTrail branding,
  mobile-first responsive CSS (640px and 1024px breakpoints).
- `app.js` — passcode, chat, FAQ search, `create_ticket`, guardrails,
  hand-off rules, feedback, test runner.
- `data.js` — the exact 20 FAQ rows from the Google Sheet.
- `.vscode/settings.json`, `.vscode/extensions.json` — VS Code setup.

## Open in VS Code

**Desktop VS Code:** File → Open Folder → select `teatrail-support-agent`.
Right-click `index.html` → “Open with Live Server”, or run
`python3 -m http.server 8000` in the integrated terminal.

**VS Code for Web (vscode.dev):** once this folder is pushed to GitHub, open
`https://vscode.dev/github/<owner>/<repo>` (or open the repo on github.com
and press `.`). Signed in as `walamaths18-pixel` on vscode.dev.

## Deploy on Vercel (free / Hobby)

Static site — no build command needed. In Vercel: Add New → Project →
import the GitHub repo → Framework Preset: **Other** → Build Command:
*(leave empty)* → Output Directory: `.` (the repo root, or
`teatrail-support-agent` if the repo root contains it) → Deploy.

## Limitations

- Demo only: all FAQs are real sheet data, but tickets and feedback live in
  the browser's `localStorage` — they do not sync across devices or browsers.
- Tara's “RAG” is client-side keyword search over the 20 FAQ rows, not a
  real vector search or LLM — she matches the embedded sheet only.
- No real payments, customers or refunds are involved.
- The earlier PaySetu Wallet app in `../paysetu-app/` was the wrong
  business and is not used here.
