[Uploading README  mit.md…]()
# KORE — Canteen Pre-Ordering App

KORE is a mobile pre-ordering app for a college canteen, built with **MIT App Inventor**. It lets students order food ahead of the lunch rush, have the order approved and prepared by canteen staff, and pick it up with an order number — instead of standing in a physical line for the entire break.

The goal isn't to move the queue online. It's to cut the time students actually spend waiting.

## How it works

**Student**
1. Register / log in with a College ID and password
2. Browse today's menu and add items to the cart
3. Review the cart (live total) and place the order
4. Order goes to the canteen as a pending request
5. Get notified the moment it's approved, with an order number
6. Show the order number at the counter and collect food

**Admin / Canteen staff**
1. Log in with the admin account
2. Look up a student's pending order by their College ID
3. Review items and total
4. Approve (issues a sequential order number, e.g. `K007`) or reject
5. The student's app updates instantly — no refresh needed

## Status

This is an active work-in-progress prototype. Current state:

| Feature | Status |
|---|---|
| Registration & login (CloudDB-backed) | ✅ Working |
| Admin login (hardcoded test credentials) | ✅ Working |
| Menu browsing + quantity selection | ✅ Working |
| Cart with live total | ✅ Working |
| Placing an order request | ✅ Working |
| Admin lookup / approve / reject | ✅ Working |
| Live order status push to student (no polling) | ✅ Working |
| Sequential order numbers (`K001`, `K002`, ...) | ✅ Working |
| Order history ("My Orders") | 🔜 Not yet built |
| QR-code pickup | 🔜 Future |
| Online payment | 🔜 Future |
| Pickup time slots | 🔜 Future |

## Tech stack

- **MIT App Inventor** — the entire app is built visually with App Inventor's Designer + Blocks editor (no Flutter/Kotlin/Java)
- **CloudDB** (App Inventor's built-in Redis-backed database) — shared, real-time data store between student and admin devices; also drives live status updates via CloudDB's `DataChanged` event, so no manual refresh or polling is needed

## Repo contents

```
KORE_Canteen.aia   — the importable App Inventor project
README.md          — this file
```

## Getting started (for teammates)

1. Go to [ai2.appinventor.mit.edu](https://ai2.appinventor.mit.edu) and log in.
2. **Projects → Import project (.aia) from my computer** → select `KORE_Canteen.aia`.
3. Once imported, select the non-visible `CloudDB1` component and confirm it has a `ProjectID` and `Token` filled in (App Inventor auto-generates these). All devices testing the same imported project will automatically share the same CloudDB data.
4. Connect a phone via the **MIT AI2 Companion** app (**Connect → AI Companion**, scan the QR code) to test live.
5. To test the full loop, use **two devices**: log in as a student on one, and as admin (`admin` / `admin123`) on the other.

## Data model (CloudDB tags)

| Tag | Holds |
|---|---|
| `user_<CollegeID>` | `[CollegeID, Name, Password, Role]` |
| `orderrequest_<CollegeID>` | `[CollegeID, Name, ItemsSummary, Total, Status, OrderNumber]` — `Status` is one of `REQUESTED`, `APPROVED`, `REJECTED` |
| `lastOrderNumber` | Running counter used to generate the next `K0xx` order number |

## Known limitations (prototype-stage)

- Admin currently looks up **one student's order at a time** by College ID rather than browsing a live list of all incoming orders — a full incoming-orders list is a natural next addition.
- The admin account is a hardcoded test login, not a real managed account.
- Order numbers aren't zero-padded (`K7` rather than `K007`) — cosmetic, easy to add later.
- No order history yet — students only see their single most recent order request.

## Team

Built by a team of 6 for a one-day prototype sprint, split across UI/UX, cart & order logic, CloudDB/database, the admin system, and testing/documentation, with one integrator maintaining the master project.

## License

MIT — see [LICENSE](LICENSE).
