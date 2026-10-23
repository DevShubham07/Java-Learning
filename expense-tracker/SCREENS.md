# Screens index

Every screen to build. Open the PNG to see it and the HTML for exact text, sizes and links. "Goes to" lists where each button or link leads (sheets close back to the screen underneath).

No button leads to these, by design: 1.1 notification, 7.1 lock screen and 7.2 widget are system surfaces that open Today; 1.5 and 1.8 are states of Today; R5 is Today when nothing was captured; A4 shows when Ask is tapped on Money without the AI model; P1 is shown when the app opens while App lock is on.

A static page can draw only one target per button, so these rules apply in the app (also in SPEC.md §1.3):
- A sheet's Close and Save return to the screen that opened it (the HTML shows Today or the usual opener).
- Onboarding O1 → O2 → O3 → O4 → 6.2 → C9 has Back to the previous step.
- O2, O3 and O4 also open from Settings (Banks, Places › + Add, Nightly reminder / App lock). Opened that way they show no step bar, Back returns there and the main button reads Save and returns there.
- 6.6 Restore opened from 6.4 goes Back to 6.4; "Add to what's here" then lands on Today. Only a new-phone restore continues to O1.
- Every list row opens its detail: payments → 2.4, people → 5.3 (a tenant → 4.3), bills → 4.4, months in 3.3 → 3.1, accounts → K4/K5 or 7.7 for Cash.

## v2 · current (source of truth)

| Screen | Picture | HTML | Goes to |
|---|---|---|---|
| 1.1 · Pinned nightly notification | [png](screens/v2/V2-Notif.png) | [html](wireframes/v2/V2-Notif.html) | 1.4, 1.3, 1.2 |
| 1.2 · Today: confirm payments | [png](screens/v2/V2-Today.png) | [html](wireframes/v2/V2-Today.html) | 5.1, 1.6, 2.1, 3.1, 1.4, 5.2, 1.7, 6.1, 1.3, 1.2 |
| 1.3 · Split (sheet) | [png](screens/v2/V2-Split-Sheet.png) | [html](wireframes/v2/V2-Split-Sheet.html) | S3, 7.5, 1.2 |
| 1.4 · Not mine (sheet) | [png](screens/v2/V2-Not-Mine.png) | [html](wireframes/v2/V2-Not-Mine.html) | 7.5, 1.2 |
| 1.5 · Today: all confirmed | [png](screens/v2/V2-Today-Done.png) | [html](wireframes/v2/V2-Today-Done.html) | 5.1, 4.1, 2.1, 3.1, 5.2, 6.1, 1.2 |
| 1.6 · Pick category (sheet) | [png](screens/v2/V2-Category-Sheet.png) | [html](wireframes/v2/V2-Category-Sheet.html) | 1.2 |
| 1.7 · Money received: what is it? (sheet) | [png](screens/v2/V2-Received-Sheet.png) | [html](wireframes/v2/V2-Received-Sheet.html) | 7.4, 1.2 |
| 1.8 · Today after days unanswered | [png](screens/v2/V2-Today-Backlog.png) | [html](wireframes/v2/V2-Today-Backlog.html) | 5.1, 2.1, 3.1, 5.2, 6.1, 1.2 |
| 2.1 · Money: all | [png](screens/v2/V2-Money.png) | [html](wireframes/v2/V2-Money.html) | A1/A2, T3/T5, T1, 2.5, 5.1, 7.7, 2.1, 2.2, 2.3, 3.1, 5.2, 1.2, 2.4 |
| 2.2 · Money: received | [png](screens/v2/V2-Money-In.png) | [html](wireframes/v2/V2-Money-In.html) | 5.1, 2.1, 3.1, 5.2, 1.2, 2.4 |
| 2.3 · Money: ignored & unreadable SMS | [png](screens/v2/V2-Money-SMS.png) | [html](wireframes/v2/V2-Money-SMS.html) | 5.1, 2.1, 3.1, 5.2, 1.2 |
| 2.4 · Payment detail (sheet) | [png](screens/v2/V2-Txn-Sheet.png) | [html](wireframes/v2/V2-Txn-Sheet.html) | 1.6, 2.1, 1.3 |
| 2.5 · One account selected | [png](screens/v2/V2-Account.png) | [html](wireframes/v2/V2-Account.html) | K8, 5.1, 7.7, 2.1, 3.1, 5.2, 1.2, 2.4 |
| 3.1 · Month: in, out, saved | [png](screens/v2/V2-Month.png) | [html](wireframes/v2/V2-Month.html) | 5.1, 4.1, 7.3, 2.1, 2.2, 3.1, 3.2, 5.2, 1.2, 3.3 |
| 3.2 · Month: where it went (scrolled) | [png](screens/v2/V2-Month-Spend.png) | [html](wireframes/v2/V2-Month-Spend.html) | I3, I4, I2, 5.1, 2.1, 3.1, 5.2, 1.2 |
| 3.3 · Year view | [png](screens/v2/V2-Year.png) | [html](wireframes/v2/V2-Year.html) | 3.1 |
| 4.1 · Rent & bills this month | [png](screens/v2/V2-Bills.png) | [html](wireframes/v2/V2-Bills.html) | 4.4, 3.1, 4.3, 4.2 |
| 4.2 · Rent I pay (split with flatmates) | [png](screens/v2/V2-Rent-Pay.png) | [html](wireframes/v2/V2-Rent-Pay.html) | 4.4, 4.1 |
| 4.3 · Rent I collect (tenant) | [png](screens/v2/V2-Rent-Collect.png) | [html](wireframes/v2/V2-Rent-Collect.html) | 5.6, 4.4, 4.1 |
| 4.4 · Add a monthly bill or rent | [png](screens/v2/V2-Bill-Add.png) | [html](wireframes/v2/V2-Bill-Add.html) | 4.1 |
| 5.1 · Add (sheet): spent or received | [png](screens/v2/V2-Add.png) | [html](wireframes/v2/V2-Add.html) | 5.6, 1.6, 1.3, 1.2 |
| 5.2 · People | [png](screens/v2/V2-People.png) | [html](wireframes/v2/V2-People.html) | 5.1, 2.1, 3.1, 5.2, 5.5, 5.3, 4.3, 1.2, 5.4, 7.6 |
| 5.3 · Person (sheet) | [png](screens/v2/V2-Person-Sheet.png) | [html](wireframes/v2/V2-Person-Sheet.html) | S6, S7/S8, 5.2, 5.5 |
| 5.4 · Trip | [png](screens/v2/V2-Trip.png) | [html](wireframes/v2/V2-Trip.html) | S9, 5.2 |
| 5.5 · Manage people (search, edit, delete) | [png](screens/v2/V2-People-Manage.png) | [html](wireframes/v2/V2-People-Manage.html) | 5.2, 7.5 |
| 5.6 · Add (sheet): money received in cash | [png](screens/v2/V2-Add-Received.png) | [html](wireframes/v2/V2-Add-Received.html) | 5.1, 4.1, 1.2 |
| 6.1 · Settings (one page) | [png](screens/v2/V2-Settings.png) | [html](wireframes/v2/V2-Settings.html) | P6/P7, O2, O4, P4, L5–L7, P9, 6.3, 6.4, 4.1, 1.2 |
| 6.2 · Setup step: rent | [png](screens/v2/V2-Onb-Rent.png) | [html](wireframes/v2/V2-Onb-Rent.html) | C9, O4, 4.4 |
| 6.3 · Every alert the app sends | [png](screens/v2/V2-Alerts.png) | [html](wireframes/v2/V2-Alerts.html) | 6.1 |
| 6.4 · Backup, import & export | [png](screens/v2/V2-Backup.png) | [html](wireframes/v2/V2-Backup.html) | 6.5, 6.6, 6.1 |
| 6.5 · Lock & share file (WhatsApp) | [png](screens/v2/V2-Backup-Share.png) | [html](wireframes/v2/V2-Backup-Share.html) | 6.4 |
| 6.6 · Restore on new phone | [png](screens/v2/V2-Restore.png) | [html](wireframes/v2/V2-Restore.html) | O1, O0 |
| 7.1 · Locked phone: amounts hidden, unlock to answer | [png](screens/v2/V2-Lockscreen.png) | [html](wireframes/v2/V2-Lockscreen.html) | 1.2 |
| 7.2 · Home-screen widget | [png](screens/v2/V2-Widget.png) | [html](wireframes/v2/V2-Widget.html) | 5.1, 1.2 |
| 7.3 · Investment or EMI payment (sheet) | [png](screens/v2/V2-Invested.png) | [html](wireframes/v2/V2-Invested.html) | 1.2 |
| 7.4 · Work reimbursement received (sheet) | [png](screens/v2/V2-Reimburse.png) | [html](wireframes/v2/V2-Reimburse.html) | 1.2 |
| 7.5 · Add a person (sheet) | [png](screens/v2/V2-Person-Add.png) | [html](wireframes/v2/V2-Person-Add.html) | 4.4, 5.5 |
| 7.6 · Start a trip (sheet) | [png](screens/v2/V2-Trip-New.png) | [html](wireframes/v2/V2-Trip-New.html) | 5.2, 7.5, 5.4 |
| 7.7 · Count your cash (sheet) | [png](screens/v2/V2-Cash-Check.png) | [html](wireframes/v2/V2-Cash-Check.html) | 2.1 |

## Reused from v1 (only these; restyle to v2 rules, see SPEC.md §1.5)

| Screen | Picture | HTML | Goes to |
|---|---|---|---|
| O0 · Welcome | [png](screens/reused-v1/Onb-Welcome.png) | [html](wireframes/reused-v1/Onb-Welcome.html) | O1, 6.6 |
| O1 · Permissions | [png](screens/reused-v1/Onb-Permissions.png) | [html](wireframes/reused-v1/Onb-Permissions.html) | O2, O0 |
| O2 · Banks & apps | [png](screens/reused-v1/Onb-Banks.png) | [html](wireframes/reused-v1/Onb-Banks.html) | O1, O3 |
| O3 · First places | [png](screens/reused-v1/Onb-Places.png) | [html](wireframes/reused-v1/Onb-Places.html) | O2, O4 |
| O4 · Review time & lock | [png](screens/reused-v1/Onb-Review-Lock.png) | [html](wireframes/reused-v1/Onb-Review-Lock.html) | O3, 6.2 |
| C9 · Import past SMS | [png](screens/reused-v1/Onb-Import.png) | [html](wireframes/reused-v1/Onb-Import.html) | 6.2, 1.2 |
| P1 · App lock | [png](screens/reused-v1/Lock.png) | [html](wireframes/reused-v1/Lock.html) | 1.2 |
| P4 · Permission health | [png](screens/reused-v1/Permission-Health.png) | [html](wireframes/reused-v1/Permission-Health.html) | 6.1 |
| P6/P7 · AI model & map data | [png](screens/reused-v1/Downloads.png) | [html](wireframes/reused-v1/Downloads.html) | 6.1 |
| P9 · Merchant memory & rules | [png](screens/reused-v1/Rules.png) | [html](wireframes/reused-v1/Rules.html) | 6.1 |
| L5–L7 · Places | [png](screens/reused-v1/Places.png) | [html](wireframes/reused-v1/Places.html) | O3, 6.1 |
| A1/A2 · Ask | [png](screens/reused-v1/AI-Chat.png) | [html](wireframes/reused-v1/AI-Chat.html) | 2.5, 2.1 |
| A4/P6 · Assistant not installed | [png](screens/reused-v1/AI-Off.png) | [html](wireframes/reused-v1/AI-Off.html) | P6/P7, 6.1 |
| S3 · Someone else paid | [png](screens/reused-v1/Split-Paid-By.png) | [html](wireframes/reused-v1/Split-Paid-By.html) | 1.3, 1.2 |
| S6 · Person | [png](screens/reused-v1/Person-Detail.png) | [html](wireframes/reused-v1/Person-Detail.html) | S7/S8, 5.2, 5.5 |
| S7/S8 · Settle up | [png](screens/reused-v1/Settle.png) | [html](wireframes/reused-v1/Settle.html) | 5.2, 5.3 |
| S9 · Trip settle-up | [png](screens/reused-v1/Trip-Settle.png) | [html](wireframes/reused-v1/Trip-Settle.html) | 5.2, 5.4 |
| I3 · Budgets | [png](screens/reused-v1/Budgets.png) | [html](wireframes/reused-v1/Budgets.html) | 3.2 |
| I4 · Subscriptions | [png](screens/reused-v1/Recurring.png) | [html](wireframes/reused-v1/Recurring.html) | 3.2 |
| I2 · Spending map | [png](screens/reused-v1/Spend-Map.png) | [html](wireframes/reused-v1/Spend-Map.html) | 2.1, 3.2 |
| K1/K10 · New account found | [png](screens/reused-v1/Account-Confirm.png) | [html](wireframes/reused-v1/Account-Confirm.html) | K8 |
| K8 · Accounts overview | [png](screens/reused-v1/Accounts.png) | [html](wireframes/reused-v1/Accounts.html) | K1/K10, K4/K5, K6, 2.5, 7.7 |
| K4/K5 · Bank account | [png](screens/reused-v1/Account-Detail.png) | [html](wireframes/reused-v1/Account-Detail.html) | K8, 2.1 |
| K6 · Credit card | [png](screens/reused-v1/Card-Detail.png) | [html](wireframes/reused-v1/Card-Detail.html) | K8, 2.1 |
| R5 · Nothing to review | [png](screens/reused-v1/Review-Empty.png) | [html](wireframes/reused-v1/Review-Empty.html) | 5.1, 1.2 |
| T1 · Filters | [png](screens/reused-v1/Txn-Filters.png) | [html](wireframes/reused-v1/Txn-Filters.html) | 2.1 |
| T3/T5 · Select many | [png](screens/reused-v1/Txn-Bulk.png) | [html](wireframes/reused-v1/Txn-Bulk.html) | 2.1 |
