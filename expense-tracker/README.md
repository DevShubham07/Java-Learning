# Expense Tracker: build package for Claude Code

This folder holds everything needed to build the app. Nothing outside it is required.

| File | What it is |
|---|---|
| `SPEC.md` | Full spec: stack, data model, money rules, SMS engine, backup, build order. **Start here.** |
| `SCREENS.md` | Every screen with its picture, HTML and where each button goes |
| `screens/v2/*.png` | The screens to build (source of truth) |
| `screens/reused-v1/*.png` | Older screens reused as listed in SPEC.md §1.5 |
| `wireframes/**/*.html` | The same screens as static HTML (exact text, sizes, links); open in a browser |

## Prompt to paste into Claude Code

```
Build the Android app described in the expense-tracker/ folder.
Read expense-tracker/SPEC.md completely first: it defines stack, data model, money rules, SMS engine, backup, categories, alerts and build order.
Then read expense-tracker/SCREENS.md and look at every PNG in expense-tracker/screens/. Build the screens as drawn in screens/v2 (the HTML in wireframes/v2 gives exact text, sizes and links). Use screens/reused-v1 only for the screens SPEC.md §1.5 lists; where they differ from v2, v2 wins.
Kotlin + Jetpack Compose + Material 3, Room + SQLCipher, no network APIs, no analytics, no Play Services location.
Work phase by phase from SPEC.md §5. After each phase: build an APK, run unit tests for the money rules (§4), the SMS engine corpus (§6) and backup round-trip (§7), and stop for my review.
Placeholders like ₹[x] and [N] are computed values; names are sample data.
```
