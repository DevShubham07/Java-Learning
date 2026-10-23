# Personal Expense Tracker — Build Spec (complete)

> **For Claude Code.** This folder is the whole input; nothing else is needed.
> - `SPEC.md` (this file) is the source of truth for behaviour, data and rules.
> - `wireframes/v2/` + `screens/v2/` are the source of truth for screens, layout and navigation (HTML you can open, plus a PNG of each). `SCREENS.md` lists every screen, its file and where each button goes.
> - `wireframes/reused-v1/` + `screens/reused-v1/` hold only the old screens §1.5 says to reuse. Where they differ from v2, **v2 wins**.
> - The same material lives on the canvas https://claude.ai/artifact/MSPvqa51JrbTWt2SmzeubR (private) for people.
> - In the wireframes, values like ₹[x] or [N] stand for computed data; names (Rahul, Sana, Flat 4C…) are sample data. Each HTML file is a static mock: ignore `support.js`, `{{accent}}` (= #0B57D0) and the `x-dc` / `DCLogic` wrapper.

---

## Prompt for Claude Code (copy as is)

```
Build the Android app described in the expense-tracker/ folder.
Read expense-tracker/SPEC.md completely first: it defines stack, data model, money rules, SMS engine, backup, categories, alerts and build order.
Then read expense-tracker/SCREENS.md and look at every PNG in expense-tracker/screens/. Build the screens as drawn in screens/v2 (the HTML in wireframes/v2 gives exact text, sizes and links). Use screens/reused-v1 only for the screens SPEC.md §1.5 lists; where they differ from v2, v2 wins.
Kotlin + Jetpack Compose + Material 3, Room + SQLCipher, no network APIs, no analytics, no Play Services location.
Work phase by phase from §5. After each phase: build an APK, run unit tests for the money rules (§4), the SMS engine corpus (§6) and backup round-trip (§7), and stop for my review.
Placeholders like ₹[x] and [N] are computed values; names are sample data.
```

---

## 1. Read me first

### 1.1 What the app is
A single-user Android app (sideloaded APK) that is a complete personal money manager. It captures every debit and credit from bank SMS and payment-app notifications, tracks **rent the user pays and rent the user collects**, monthly bills, splits with friends and flatmates, and shows **money in, money out and savings** each month. Nothing ever leaves the phone.

**Core promise:** by night everything is captured and labelled; the user only answers **"Paid by you?"** for each payment.

### 1.2 Principles
- **Zero manual entry by default.** SMS and notifications create transactions; manual add is for cash only.
- **Every payment gets an answer.** Mine / Split / Not mine. The nightly notification stays pinned until all are answered.
- **Your share is the truth.** All spend totals count only the user's share.
- **Income is honest.** Paybacks, refunds, own-account transfers, deposits and reimbursements are never income.
- **Quiet by default.** No due-date reminders of any kind (card, bill, rent). Status is shown in the app only.
- **Private by construction.** No servers, analytics, cloud AI or Google Play Services location.
- **Easy.** 4 tabs, sheets instead of new pages, nothing more than 2 taps from a tab.

### 1.3 Navigation
Bottom bar: **Today · Money · (+) Add · Month · People**. Settings = gear icon on Today. Detail, split, category, person, "not mine", "money received" etc. open as **bottom sheets** over the current tab.
Not reached by a button (by design): 1.1 notification, 7.1 lock screen and 7.2 widget are system surfaces that open Today; 1.5 and 1.8 are states of Today (nothing left / 2+ days unanswered). Back from any reused v1 screen returns to the v2 screen that opened it.

### 1.4 Screen map (v2 artboard titles)
| Area | Screens |
|---|---|
| Notification | 1.1 Pinned nightly notification |
| Today | 1.2 Today · 1.3 Split sheet · 1.4 Not mine sheet · 1.5 All answered · 1.6 Category sheet · 1.7 Money received sheet · 1.8 Backlog after days unanswered |
| Money | 2.1 All · 2.2 Received · 2.3 SMS not used · 2.4 Payment detail sheet · 2.5 One account selected |
| Month | 3.1 In / out / saved · 3.2 Where it went · 3.3 Year |
| Rent & bills | 4.1 This month · 4.2 Rent I pay · 4.3 Rent I collect · 4.4 Add a monthly bill |
| Add & People | 5.1 Add sheet: spent · 5.6 Add sheet: received · 5.2 People · 5.3 Person sheet · 5.4 Trip · 5.5 Manage people |
| Settings | 6.1 Settings · 6.2 Setup: rent step · 6.3 Alerts · 6.4 Backup, import & export · 6.5 Lock & share file · 6.6 Restore (also from Welcome and "Open with" from WhatsApp) |
| More situations | 7.1 Locked phone · 7.2 Widget · 7.3 Investment / EMI · 7.4 Reimbursement · 7.5 Add person · 7.6 Start trip · 7.7 Count your cash |

### 1.5 Reused from v1 (build as drawn, restyled to v2 rules)
- **Onboarding:** O0 Welcome, O1 Permissions, O2 Banks & apps, O3 First places, O4 Review time & lock, C9 Import past SMS (O0 has "I have a backup file" → 6.6). Insert v2 **6.2 Rent step** as step 5, before Import.
- P1 App lock · P4 Permission health · P6/P7 AI model & map data · P9 Merchant memory & rules · L5–L7 Places · A1/A2 Ask (opened from a search bar on Money, only when the AI model is installed) · A4 Assistant not installed.
- Opened from v2 buttons: **S3** Someone else paid (= 1.3 with "Someone else paid") · **S7/S8** Settle up (Person sheet › Record payment) · **I3** Budgets (3.2 › Edit budgets; add a "Whole month" limit) · **I4** Subscriptions and **I2** Spending map (3.2 buttons) · **K1/K10** New account found / edit account · **K6** Credit card detail (bill amount and status only, no reminders) · **R5** Nothing to answer today (Today empty state).
- Features carried over from v1 (do not lose them):
  - **T1 Filters** — Money › Filters: date range, method, person, place, trip, answer status.
  - **T3/T5 Select many** — Money › Select: recategorise, split, merge duplicates, ignore in bulk.
  - **Full / Your share switch** (T6) — on Money (2.1), remembered.
  - **K4/K5 Bank account detail** — on 2.5 when a bank account is selected: in / out / net per period, linked UPI IDs, rename / merge / hide / not mine.
  - **K8 Manage accounts** — 2.5 › Manage accounts: add manually, wallets, hidden accounts.
  - **I2 breakdown by merchant and by account** — tabs on 3.2 (Category · Merchant · Account).
  - **I1 day / week trend** — Days · Weeks · Months toggle on 3.3.
  - **R3 "Name this payee"** — on Today cards with an unknown UPI ID (E1); name once, used for every later payment.
  - **A3 AI one-line summary** — in the nightly notification, only when the model is installed.
  - **S9 Trip settle-up details** — net per person and "Pay by UPI", opened from 5.4.
  - **S6 reminder preview** — WhatsApp reminder text preview on the Person sheet.
- Restyle rules: 48 dp targets, sheets, plain words.
- C10 Unparsed SMS is replaced by the "Couldn't read" card on 2.3.

### 1.6 Do not build from v1
R1, R2, R4 and R6 review flow (replaced by Today and 1.8; R3 only for its payee-naming field), S1/S2/S4 split screens (replaced by 1.3), H1/H2 Home, T1 list and T2/T4 detail (replaced by Money and 2.4), I1 Insights screen (replaced by Month; its trend lives on 3.3), More/Settings, v1 Notification set (replaced by 6.3), P2/P3 Backup (replaced by 6.4–6.6). **Remove the card-due reminder entirely.**

### 1.7 Targets
APK under 15 MB without the AI model · cold start to Today under 1 s · answering 10 payments under 60 s · cash add under 10 s · optional AI model ≈ 2.4 GB downloaded later · offline map + places for Delhi NCR under 150 MB · location log every 15–30 min, paused when stationary.

---

## 2. Stack, permissions, background jobs

### 2.1 Tech stack
| Layer | Choice |
|---|---|
| Language / UI | Kotlin, Jetpack Compose, Material 3, single activity, Navigation Compose, ModalBottomSheet for all sheets |
| Database | Room (SQLite) encrypted with SQLCipher; key in Android Keystore. All money as Long paise |
| Background | WorkManager + BroadcastReceiver; no permanent foreground service |
| SMS | RECEIVE_SMS receiver + one-time inbox import (READ_SMS), dual SIM |
| Notifications in | NotificationListenerService for GPay, PhonePe, Paytm, CRED, bank apps (catches UPI Lite) |
| Location & map | LocationManager GPS provider; OpenStreetMap POI extract (Delhi NCR) in SQLite with spatial index; MapLibre or osmdroid offline tiles |
| On-device AI (optional) | LiteRT-LM running Gemma 4 E2B with tool calls over fixed DB queries; loaded only while in use |
| Voice | Android on-device SpeechRecognizer (offline) |
| Security | BiometricPrompt with PIN fallback; FLAG_SECURE on screens with amounts |
| Backup | Encrypted .etb file (§7): nightly auto, share to WhatsApp/Drive, import/restore, merge; readable Excel/CSV/PDF export; tax PDFs |
| Widget | Jetpack Glance home-screen widget (7.2) |

**Never include:** Firebase, Crashlytics, any analytics SDK, Google Maps SDK, Play Services location/geocoder, any cloud LLM or network API. Internet permission is for one-way downloads only (AI model, map data).

### 2.2 Permissions (asked on O1, each with a one-line reason)
| Permission | Reason |
|---|---|
| RECEIVE_SMS, READ_SMS | Turn bank SMS into transactions |
| Notification access | Catch payment-app notifications |
| ACCESS_FINE + BACKGROUND_LOCATION | Tag where each payment happened |
| POST_NOTIFICATIONS | Nightly pinned notification and the few allowed alerts |
| Ignore battery optimisation | Keep capture running |
| RECORD_AUDIO (on first use) | Voice add, offline |

Skipping is allowed; a missing permission shows a persistent "Fix" banner on Settings and Today (P4 lists live status).

### 2.3 Background jobs
| Job | Does |
|---|---|
| SMS receiver | Stores every bank/app SMS as SmsMessage, runs the SMS engine (§6), creates a Transaction for payment types, requests a location fix |
| Notification listener | Parses payment notifications; dedupes against SMS (amount + 5-min window + ref no.) |
| Matcher | On each new transaction: merchant memory → category; bill match (counterparty UPI ID / name, amount within ±10% for variable bills) → marks BillPeriod done; credit from a person's UPI ID → offers payback; employer credit matching open work items → offers reimbursement |
| Location logger | Every 15–30 min, paused when stationary; 7-day rolling log |
| Place resolver | Saved places first, then offline POIs; stores location age |
| Nightly notifier | At review time posts an **ongoing** notification (setOngoing, not dismissible) with the count of unanswered items and Mine / Split / Not mine actions for the oldest one. Updates after each answer, shows "[merchant] marked [answer] · Undo" until the next tap; removed only when count = 0; re-posted after reboot or if cleared by the system. New payments after review time add to the count. Actions from a locked phone require unlock (setAuthenticationRequired); amounts hidden when Hide amounts is on |
| Month roll-over | On the 1st, creates pending BillPeriod rows for every active Bill and pre-computes last month's totals |
| Budget check | After each answer: 80% / 100% category alert (if enabled) |
| Auto backup | Nightly while charging: .etb to the user-picked folder, keep 7; also before every app upgrade (§7) |
| Capture watchdog | Detects revoked SMS/notification access; posts "Capture stopped"; on fix re-scans inbox since last captured SMS |

---

## 3. Data model (Room, all amounts in paise)

Every table also has `uid` (UUID string, unique), `created_at` and `updated_at`. These are used for backup merge (§7); not repeated below.

| Table | Fields |
|---|---|
| **Transaction** | id, amount_paise, direction (debit\|credit), **type** (spent \| income \| payback \| refund \| self_transfer \| failed \| not_counted \| work_reimbursable \| reimbursement \| invested \| deposit \| borrowed \| held_for_other), **answer** (unanswered \| mine \| split \| not_mine), merchant_id, category_id, account_id, method (upi\|debit_card\|credit_card\|net_banking\|atm\|wallet\|cash), counter_account_id, bill_period_id?, trip_id?, refund_of_id?, occurred_at, ref_no, source (sms\|notification\|manual\|ai), sms_id?, lat, lng, location_age_min, place_id, note, tags, photo_path, imported (bool), edited (bool) |
| **SmsMessage** | id, sender, body, received_at, sim_slot, **sms_type** (payment \| otp \| promo \| future_or_request \| failed \| balance_only \| statement_due \| unreadable \| other), confidence (high\|medium\|low), parsed_ok, transaction_id?, rule_id? |
| **Category** | id, name, icon, kind (spend\|income), is_fixed (rent/bills/subscriptions/EMI), monthly_budget_paise?, sort, archived |
| **Merchant** | id, raw_identifier (UPI ID or SMS name), display_name, default_category_id, default_place_id, usual_people_ids, always_use_category (bool), default_type? (e.g. invested) |
| **Account** | id, name, bank, last4, type (savings\|current\|credit_card\|debit_card\|wallet\|cash), linked_upi_ids, is_mine, confirmed, last_balance_paise, balance_as_of, credit_limit_paise, statement_paise, due_date, hidden |
| **Person** | id, name, phone?, upi_ids, **group** (friend \| flatmate \| tenant \| landlord \| family \| other), archived, merged_into_id? |
| **Split** | id, transaction_id, person_id (null = me), share_paise, paid_paise, method (equal\|amount\|percent\|shares), shares? |
| **Settlement** | id, person_id, amount_paise, direction (they_paid_me\|i_paid_them), method (cash\|upi), occurred_at, transaction_id?, write_off (bool) |
| **Bill** | id, name, **direction** (pay\|collect), kind (rent\|electricity\|wifi\|maintenance\|house_help\|gas\|phone\|other), amount_paise, amount_from_month?, variable (bool), due_day (1–31), counterparty_person_id (landlord or tenant), pay_via (landlord_direct\|through_flatmate), via_person_id?, match_upi_ids, match_names, account_id, category_id, split_person_ids (flatmates), deposit_paise?, lease_start?, lease_end?, active |
| **BillPeriod** | id, bill_id, month (YYYY-MM), expected_paise, done_paise, status (pending \| partial \| done), done_at?, transaction_ids |
| **Budget** | month_total_paise (overall "Whole month" limit); per-category limits live on Category |
| **Trip** | id, name, start_date, end_date, member_ids, active |
| **Place** | id, name, lat, lng, radius_m, source (saved\|osm) |
| **LocationLog** | id, lat, lng, accuracy_m, recorded_at (kept 7 days) |
| **CashCheck** | id, counted_paise, expected_paise, resolution (untracked_spend\|split_into_expenses\|received\|correction), at |
| **Rule** | id, sender, pattern, result (type / direction / ignore), source (template\|learned), version |
| **MonthTotal** (cache) | month, account_id?, category_id?, in_paise, out_paise, invested_paise — pre-computed so screens never scan full history |

### Key formulas
- **My share** of a transaction = its Split row with person_id = null; with no splits, the full amount. Equal splits: leftover paise go to me.
- **Person balance** = Σ their share on transactions I paid + their share of bills I pay − Σ my share on transactions they paid − settlements ± borrowed/lent. Rent a tenant owes is tracked on BillPeriod, not as a balance.
- **Delete person** blocked while balance ≠ 0 or a collect bill is pending; archive always allowed.
- **Editing an old split** that was already settled creates a new balance for the difference; settlements are never edited silently.

### AI tools (optional model, fixed queries only)
`get_spend(period, category, place, person, trip, share)` · `get_income(period, source)` · `list_transactions(filters)` · `get_balances(person)` · `bill_status(month)` · `top_merchants(period, n)` · `add_transaction(draft)` · `parse_sms(text)`

---

## 4. Money rules, categories, alerts

### 4.1 Month formulas (Month tab, Money header, Today summary)
- **Money in** = Σ type = income (salary, rent collected, freelance, interest, other income).
- **Money out** = Σ my share of type = spent (includes my share of rent and bills paid, EMIs).
- **Saved** = Money in − Money out. Compare with the previous month. Show **"of which invested"** = Σ type = invested.
- **Never in Money in or Money out:** payback (settles a balance), refund (subtracts from Spent in its original category), self_transfer (own accounts, card bill payment, ATM → Cash), failed, not_counted, invested, deposit (paid or received), reimbursement, borrowed, held_for_other. work_reimbursable sits apart until reimbursed.
- **Money you have** = banks + wallets + cash − card dues, shown on the first chip of the account strip (2.1) with each balance's date.
- Cash: ATM withdrawal moves money to the Cash account; cash counts as spent only when a cash expense is added.

### 4.2 Answering every payment
- Every captured debit starts **unanswered** and appears on Today and in the pinned notification.
- Credits are auto-typed when certain (own account → self_transfer; tenant's UPI ID → rent collected; employer → income or reimbursement; refund linked to a payment). A credit from a person, or any credit not certain, joins Today as **"Money received · What is it?" (1.7)**: Paying me back (suggested when they owe; partial allowed; any extra asked separately) · Rent I collect · Income · Lending me (I now owe them) · For something I'll pay for them · Refund · From my account · Ignore.
- Debit answers: **Mine** (my share = full) · **Split** (1.3) · **Not mine** (1.4: paid for someone → they owe full; work expense → reimbursable; between my accounts; wrong/duplicate → not_counted).
- Investment-like debits (SIP, FD, RD, PPF, stocks, own savings) are suggested as **invested** (7.3); EMIs as spent in Loans & EMI.
- Suggested answer highlighted: merchant memory first, then "usually split with these people at this place", then AI if installed.
- **No Skip.** Unanswered items stay; the notification stays until the count is 0. Undo last answer on Today and in the notification.
- **Backlog (1.8):** more than 15 unanswered → Today groups them by day. "Accept N" applies only confident suggestions, shows what will change first, and can be undone. Money received, new accounts and unknown merchants are never accepted in bulk. Notification reads "N payments from D days".
- Imported history (C9) is saved as answered = mine with suggested categories; it never fills Today.
- Failed payments and linked refunds are never asked.

### 4.3 Rent & bills
- A Bill is **pay** (rent to landlord, electricity, Wi-Fi, house help…) or **collect** (rent from each tenant). Each month gets a BillPeriod: pending → partial → done.
- Auto-match: a debit to / credit from the bill's UPI IDs or names in the month (amount within ±10% when variable) marks it done; a credit from a tenant is typed income (rent collected). Manual "Mark paid / received" asks: full or part, cash or UPI, account. Cash rent can be added via 5.6.
- **Rent I pay with flatmates:** my share counts in Money out; each flatmate's share is added to their balance every month automatically.
- **Paid through a flatmate:** pay_via = through_flatmate; my payment to that flatmate marks my part of the month done.
- **Rent change:** editing the amount asks "from which month"; past months never change.
- **End tenancy (4.3) / Moving out (4.2):** final month, deposit returned/received (type deposit, never income or spending). Deposit received from a tenant is held, not income.
- **Tax exports:** Rent I pay › "Rent receipts for HRA" (PDF per financial year Apr–Mar, landlord name, monthly amounts); Rent I collect › "Rental income statement" (CSV/PDF).
- Status appears on Today (1.5), Month (3.1) and 4.1 only. **No due-date notifications.**

### 4.4 Categories
- **Spend (default):** Food & drink · Groceries · Transport · Rent & home (fixed) · Bills & utilities (fixed) · Subscriptions (fixed) · Loans & EMI (fixed) · Shopping · Health · Entertainment · Travel · Education · Personal care · Gifts & donations · Work (reimbursable) · Cash · untracked · Other.
- **Income:** Salary · Rent collected · Freelance · Interest · Other income.
- Every payment card and row shows a tappable **category chip** that opens 1.6 (with this month's total per category). "Always use for this merchant" saves merchant memory. Users can add, rename, archive and set a monthly budget per category (P9 / I3). Month 3.2 shows fixed vs everyday and spend by category.
- **Overall monthly budget** ("Whole month" on I3) drives the Month budget bar and "₹ a day left".

### 4.5 Alerts (6.3)
- **Always:** nightly pinned "N payments need an answer"; "Capture stopped · Fix".
- **Optional (on):** money arrived from a person (payback or rent received? Yes/No); location off when paying; budget 80% / 100%.
- **Optional (off):** every payment as it happens.
- **Never:** card, bill or rent due reminders; offers.

### 4.6 More situations
- **Lock screen (7.1):** notification amounts hidden while Hide amounts is on; answering from a locked phone needs unlock.
- **Widget (7.2):** today's spend (your share), payments to answer, "+ Cash". Respects Hide amounts.
- **Work expenses (7.4):** a credit from the employer matching open work items is offered as "Reimbursement" (not income) and clears them; mismatch → tick which items, difference asked.
- **Loans:** EMI debits → spent, Loans & EMI (fixed). Money borrowed (1.7 "Lending me") creates a balance I owe, not income. Lending cash: Add › Spent › Split › "only them" = they owe the full amount.
- **Cash check (7.7):** enter cash actually held; difference recorded as untracked cash spending, split into expenses, money received, or a plain correction.
- **Fixing a payment:** amount and date of any captured payment are editable (original SMS kept, edit logged). Only manual entries can be deleted; captured ones use "Don't count".
- **Add person (7.5):** name required; group, UPI IDs and phone optional (typed, contacts never read). Tenant → continues to rent setup.
- **New trip (7.6):** name, dates, members; turns trip mode on; new payments suggested as trip splits until the end date.
- **First use / empty states:** Today shows R5; Money, Month and People show one line on what will appear.
- **Dark theme:** follows the phone; same contrast rules.

### 4.7 Edge cases (must not lose or double-count money)
SMS + notification for one payment → merge (amount, 5 min, ref no.) · same amount twice with different ref → keep both · failed/reversed → original marked failed · refund days later → linked to original, reduces spend · card bill payment → self_transfer · UPI on credit card → one transaction on the card · revoked access → re-scan inbox since last capture · split doesn't add up → Save disabled · unknown new account in SMS → added unconfirmed ("Is it yours?") · stale balance → shown with its date · no location → saved without place, marked approximate if > 2 h old · tenant pays from someone else's UPI → arrives in 1.7, user picks "Rent I collect".

---

## 5. Build order, accessibility

### 5.1 Build order (each phase ends in an installable APK)
1. **Foundation:** project, encrypted Room DB (§3), app lock, bottom nav + empty tabs, cash Add sheet, CSV export. *Done when* cash entries appear on Money and Month totals are right.
2. **Capture & SMS engine (§6):** SMS receiver, inbox import, bank templates, notification listener, dedupe, SMS types, account strip, refunds/failed/self-transfer. *Done when* a real week is captured with no misses or doubles and the test corpus passes 100% on direction and amount.
3. **Answering:** Today cards, pinned nightly notification with actions and undo, Split / Not mine / Category / Money received sheets, backlog view, merchant memory. *Done when* 10 payments are answered in under 60 s and the notification can't be dismissed early.
4. **People:** groups, search, manage (edit, merge, archive, delete rules), add person, balances, settlements, auto-match, WhatsApp reminder, trips.
5. **Month:** In / Out / Saved / invested, honest income rules, categories and budgets (incl. whole-month), fixed vs everyday, year view, CSV statement, cash check.
6. **Rent & bills:** Bill + BillPeriod, pay and collect, flatmate splits, pay-through-flatmate, tenant ledger, rent change, end tenancy, auto-match, setup step 6.2, HRA/rental tax exports. *Done when* a month of rent paid and collected marks itself without manual taps.
7. **Location:** GPS logger, saved places, offline POIs, place on every card.
8. **Optional AI:** model download, Ask bar, typed/spoken add, unreadable SMS parsing. *Done when* answers equal DB totals and nothing breaks without the model.
9. **Hardening:** encrypted .etb backup, share, import and merge (§7), pre-upgrade backup, migration tests, permission health, widget, lock-screen rules, battery and memory profiling, edge-case tests from §4.7.

### 5.2 Accessibility & usability rules (WCAG 2.2 AA, Android)
- Every tap target ≥ 48 dp; text ≥ 12 sp; supports 200% font scale (no fixed row heights).
- Contrast ≥ 4.5:1 in light and dark; status never by colour alone (text + icon).
- TalkBack: icon buttons have contentDescription; rows/cards use mergeDescendants and read as one sentence ("Blue Tokai Coffee, ₹840, your share ₹280, split, Food and drink"); hidden amounts read "amount hidden"; charts have a one-line text summary.
- Undo and status messages are live regions and stay until dismissed. Nothing is swipe-only.
- One primary button style (filled), one secondary (outlined); back arrow on pages, close X + drag handle on sheets.
- Plain words: "Your share", "Between my accounts", "Paid by you?", "Money in / Money out / Saved". Avoid "debit", "credit", "self-transfer", "net position" in UI copy.

---

## 6. SMS engine — how debit and credit are decided

Rules first, AI only as a fallback, **never a silent guess**: anything uncertain goes to the user ("Couldn't read" on 2.3, or "Money received · What is it?" on 1.7).

### 6.1 Pipeline
1. **Intake.** SMS receiver, notification listener, one-time inbox import. Every message is stored raw first (SmsMessage).
2. **Sender gate.** Indian bank SMS use DLT headers like `VM-HDFCBK`, `AD-ICICIT-S`: strip the 2-letter prefix and -S/-T/-P suffix, match against the bank/app sender table (O2 + learned). Personal numbers and unknown senders are ignored; an unknown sender with a strong money pattern goes to a "new sender?" check.
3. **Normalise.** Lower-case copy, collapse spaces, unify currency (Rs, Rs., INR, ₹ → ₹), remove thousands commas; keep the original for display.
4. **Message type** (before direction, first match wins): OTP → promo/offer → future or request ("will be debited", "is due", "mandate", "has requested", "collect request") → failed/reversed → statement/due → balance-only → payment. Only **payment** creates a transaction.
5. **Debit or credit** (§6.2).
6. **Fields:** amount, your account/card (last 4), counterparty (UPI ID, name or merchant), reference no. (UPI Ref, RRN, UTR, txn id), date/time (in-SMS date else received time), method (UPI if VPA/UPI ref; card; ATM; NEFT/IMPS/RTGS → net banking), available balance.
7. **Confidence:** bank template match = high · generic rules with amount + your account + one clear verb = medium · anything missing or conflicting = low → AI parse if installed, else "Couldn't read".
8. **Dedupe & link:** SMS ↔ notification (amount, ±5 min, ref no.); failed/reversal cancels the original; refund links to the original.
9. **Type it** (§4): own account / card bill → between my accounts; ATM → Cash; investment house → invested; tenant / employer / person / refund rules for credits; everything else → Spent and onto Today.
10. **Learn:** every user correction becomes a Rule (sender + wording → type; counterparty → merchant memory).

### 6.2 Deciding debit vs credit
- **a. Bank templates first.** Each supported bank has exact patterns stored as data (versioned JSON rules, not code). A template match decides direction with high confidence.
- **b. Otherwise, keywords tied to your account.** Find your account/card mask first (`XX1234`, `A/c *1234`, `card ending 1234`), then the verb in the same clause:
  - **Debit:** debited · spent · paid to · sent to · withdrawn · purchase · transferred to · used at · Dr · deducted
  - **Credit:** credited · received · deposited · added · refund of · Cr · reversed to your a/c
- **c. Both words present:** the verb attached to *your* account wins. "₹500 debited from A/c XX1234 and credited to VPA abc@ybl" → **debit**.
- **d. Cards:** "spent on your card / used at" → debit on the card. "Payment of ₹X received towards your card" → card bill payment = between my accounts, matched to the bank debit.
- **e. Ignore lookalikes:** OTPs with amounts, "will be debited" reminders, auto-pay set-up, collect/request messages, offers mentioning money, "available limit".
- **f. Amount:** the first amount next to the deciding verb, never one after "Avl Bal", "limit" or "balance" (that one updates the account balance with its date).
- **g. No clear answer** (no verb, conflicting verbs on your account, no amount) → not guessed: AI parse (must quote an amount that appears in the SMS, else rejected) or "Couldn't read".

### 6.3 Worked examples (sample wording)
| SMS | Result |
|---|---|
| `Rs.840.00 debited from A/c XX1102 to VPA bluetokai@okaxis on 23-10-26. UPI Ref 6297… Avl Bal Rs 48,210` | Debit ₹840 · HDFC ••1102 · UPI · merchant from VPA · balance ₹48,210 noted |
| `Your A/c XX1102 is credited with Rs 500 by VPA rohit@ybl. UPI Ref 7731…` | Credit ₹500 from Rohit's UPI ID → 1.7 "Paying me back?" |
| `Rs 2,000 withdrawn at ATM from A/c XX1102` | Debit · ATM → moved to Cash (not spending) |
| `Txn of Rs 540 at ZOMATO failed. Amount if debited will be reversed.` | Failed → no transaction, cancels any matching debit |
| `Rs 1,499 will be debited on 5-Nov for your SIP via auto-pay` | Future → ignored (the real debit comes later) |
| `123456 is your OTP for txn of Rs 3,000. Do not share.` | OTP → ignored, listed under "SMS not used" |
| `Payment of Rs 12,640 received towards your Credit Card XX4321` | Card bill paid → between my accounts (pairs with bank debit) |
| `Aman has requested Rs 400 from you on UPI` | Request → ignored (no money moved) |

### 6.4 Quality bar
- Build a test corpus from the user's own bank SMS (names and numbers masked) for every bank and app used; add every message the engine got wrong.
- Each release must get **direction and amount 100% right** on the corpus; anything unsure lands in "Couldn't read" or 1.7, never guessed.
- Templates and keyword lists live in a versioned rules file: a new bank format = a data change plus a test, not new code.

---

## 7. Backup, import, export, moving phone (screens 6.4, 6.5, 6.6)

Three situations: **(a)** updating the app: data must survive with no action; **(b)** reinstall or new phone; **(c)** sending the data to another phone, e.g. on WhatsApp. (b) and (c) use the same file.

### 7.1 The backup file (`.etb`)
| Part | Rule |
|---|---|
| Name | `ExpenseTracker-23Oct2026.etb` (the date is when it was made) |
| Container | A ZIP file holding `manifest.json` (not encrypted) + `payload.bin` (encrypted) |
| manifest.json | `format:"etb"`, `formatVersion:1`, appVersionName, appVersionCode, dbSchemaVersion, createdAt (ISO-8601 with zone), deviceModel, counts per table, `kdf:{alg:"PBKDF2WithHmacSHA256", iterations:600000, salt}`, `cipher:{alg:"AES-256-GCM", nonce}`, payloadSha256. No amounts, names or other personal data. |
| payload.bin | AES-256-GCM over an inner ZIP: `data/<table>.jsonl` (one JSON row per line, every table in §3, including raw SmsMessage text and learned rules), `receipts/` (photos), `settings.json` |
| Key | Built from the **backup password** with PBKDF2 (javax.crypto, no extra library). The password is never stored. For nightly auto-backups, the derived key is stored wrapped by an Android Keystore key. |
| Why not copy the DB file | The SQLCipher key lives in the Android Keystore, which is wiped on uninstall or a new phone. JSON rows tagged with a schema version also let a newer app import an older backup and merge rows. |
| IDs | Every row has a UUID string `uid` (alongside the Room Long id) so that merges and dedupes are exact across phones. Foreign keys are exported as uid. |
| Left out | AI model, offline map tiles (downloaded again), app-lock PIN, permissions, the location log older than 7 days |
| Size | About 2–5 MB for 2 years; receipts compressed to JPEG 1600 px. WhatsApp accepts documents up to 2 GB. |

### 7.2 Sharing (WhatsApp and others) — 6.5
"Make file & share…" → password sheet → file written to app cache → `ACTION_SEND` via FileProvider, MIME `application/octet-stream`, `EXTRA_STREAM`, read-URI grant. WhatsApp sends it as a Document (never as media, so it is not recompressed). Also offered: Save to Downloads (MediaStore), Drive, Gmail, Nearby Share.

### 7.3 Import — 6.6
Ways in: (1) Welcome O0 › "I have a backup file"; (2) Settings › Backup › Import; (3) tapping the file in WhatsApp or Files → "Open with Expense Tracker". The activity declares intent-filters for `ACTION_VIEW` and `ACTION_SEND` with `application/octet-stream`, `application/zip` and `*/*` plus the path pattern `.*\.etb`. (1) and (2) use the system picker (`ACTION_OPEN_DOCUMENT`, any type).

Steps: read the manifest (show the date, counts and app version **before** asking for the password) → password → check the SHA-256 and GCM tag → open. A wrong password and a damaged file give different messages. After 5 wrong attempts, wait 60 s.

| Mode | Behaviour |
|---|---|
| Replace everything | Default on a fresh install. Current data is first saved as an automatic backup, kept 7 days ("Undo restore"). |
| Add to what's here | Same uid → keep the newer `updatedAt`. Different uid but same SMS (sender + body hash) or same transaction (amount + account + time ±5 min + ref) → keep one. People merged by uid, then by UPI ID. |
| Version rules | Backup schema older than the app → run the importer upgrade steps (one per schema version, tested). Newer than the app → refuse: "Update the app first". |
| After restore | Ask again only for SMS and notification access (O1), then go straight to Today, skipping O2–O4 and the rent step because those settings came from the backup. Re-scan the inbox from the newest SMS in the backup, so messages that arrived during the move are caught. |

### 7.4 Updating the app (no user action)
- Same package name and the **same signing keystore** for every release (keep the keystore safe; without it you can't update, only reinstall and restore). versionCode always goes up. Install the new APK over the old one and never uninstall first.
- Room migrations for every schema change, each with a test. **Never** use `fallbackToDestructiveMigration`.
- On the first launch after versionCode changes: write an automatic `.etb` **before** running migrations. If a migration fails, show "Restore yesterday's backup", never an empty app.
- Manifest: `android:allowBackup="false"` and `dataExtractionRules` that exclude everything. Google cloud or phone-to-phone copies would arrive without the Keystore key and could not be read.

### 7.5 Automatic backup
WorkManager job each night while charging and idle. It writes `.etb` to a folder the user picks once (`ACTION_OPEN_DOCUMENT_TREE` with a persisted URI permission; suggested: Documents/ExpenseTracker). That folder survives uninstall. Keep the last 7. If no folder is picked, the app reminds once in Settings, never with a notification.

### 7.6 Readable export (not importable)
Excel `.xlsx` with sheets Payments, Received, Splits, People balances, Bills & rent, Months (in / out / saved); CSV (one file per sheet, zipped); PDF month or year statement; HRA rent receipts and rental income (§4.3). Range: month, year, financial year, all. Not encrypted, and labelled so on screen.
