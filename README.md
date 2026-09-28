# Moih Park Kenya

### A Web-Based Modern Parking Management System

**Language:** Python (Flask + SQLite)

---

## 1. About the System and What It Does

### 1.1 Overview

Moih Park Kenya is a web-based system that automates a parking lot's
daily operations. It is built with **Python**, using **Flask** for the
web layer and **SQLite** as the database. It runs in a browser and can
be used by a real parking attendant from day one, with no special
training beyond a five-minute walkthrough.



Moih Park Kenya replaces all of this with a live slot display, a fee
calculator that works the same way every single time, a permanent
digital record of every vehicle, session and payment, and support for
both cash and M-Pesa.

### 1.2 What the System Does (Step by Step)

1. A driver arrives. The attendant sees, on screen, how many slots are
   free before letting them in.
2. The attendant types the vehicle's plate number. The system assigns
   a free slot, records the exact arrival time, and opens the barrier.
3. When the driver wants to leave, the attendant looks the plate up
   again. The system calculates exactly how long the vehicle was
   parked and how much it owes.
4. The driver pays — in cash (confirmed by the attendant) or by
   M-Pesa (a payment prompt is sent straight to their phone).
5. Once paid, the slot is freed, the barrier opens, and the visit is
   logged.
6. A dashboard shows today's revenue, vehicle count, and occupancy at
   any time.

### 1.4 Fee Structure

| Duration Parked | Fee (Kshs) |
|---|---|
| Up to 30 minutes | Free |
| Up to 2 hours | 50 |
| Up to 4 hours | 100 |
| Up to 6 hours | 300 |
| Over 6 hours | 500 |

### 1.5 Key Features

- Live, auto-refreshing visual slot display (20 slots)
- Staff login with hashed (encrypted) passwords
- Automatic vehicle entry logging
- Automatic, consistent fee calculation on exit
- Cash and M-Pesa (Safaricom Daraja API) payment
- Automatic barrier control logic
- Dashboard and full reports page
- Mobile-friendly, blue-and-white themed interface
- Fully working, well-commented Python code delivered via GitHub

### 1.6 Technology Used

| Layer | Technology |
|---|---|
| Language | Python 3 |
| Web framework | Flask |
| Database | SQLite |
| Frontend | HTML (Jinja2 templates), CSS, JavaScript |
| Payments | Cash + Safaricom Daraja API (M-Pesa STK Push) |
| Password security | Werkzeug password hashing |

### 1.7 Simple System Diagram

```
   ┌────────────┐     views slots,      ┌──────────────────────┐
   │  Attendant │────logs in, enters,──▶│   Moih Park Kenya      │
   │  (Browser) │   exits, takes payment│   (Flask web app)       │
   └────────────┘                        └──────────┬─────────────┘
                                                      │
                                                      ▼
                                          ┌──────────────────────┐
                                          │  SQLite database       │
                                          │  (parking.db)            │
                                          └──────────────────────────┘
                                                      │
                                                      ▼ (M-Pesa only)
                                          ┌──────────────────────────┐
                                          │  Safaricom Daraja API      │
                                          │  (sends STK Push to driver) │
                                          └──────────────────────────────┘
```



### 1.8 Pages in the System

| Page | Purpose |
|---|---|
| Login | Staff sign in |
| Dashboard | Today's revenue, vehicles, occupancy, recent activity |
| Slots | Live, colour-coded slot map |
| Entry | Register an arriving vehicle |
| Exit | Look up a vehicle and see the fee owed |
| Payment | Choose cash or M-Pesa and complete payment |
| Reports | Full daily report and session history |

Every page except Login shares one common layout (navigation bar and
message area), so the system looks and feels consistent no matter
which page the attendant is on.

---

## 2. Requirements

This section lists what the system must do (functional requirements),
how it must behave (non-functional requirements), and what is needed
to run it (software and hardware requirements). Together, these are
what the finished system is measured against.

### 2.1 Functional Requirements

These map directly to what the system must do:

| # | Requirement | Where it is met |
|---|---|---|
| 1 | Show drivers a visual display of available parking slots before entry | `2_slots.py`, `slots.html` |
| 2 | Record every vehicle's arrival automatically, with the exact time | `4_entry.py` |
| 3 | Automatically calculate total time parked and fee owed on exit | `5_exit.py` |
| 4 | Open the barrier automatically on entry | `4_entry.py` + `7_barrier.py` |
| 5 | Only open the exit barrier once the fee has been paid | `6_payment.py` + `7_barrier.py` |
| 6 | Apply the fixed fee structure (see Section 1.4) | `5_exit.py` |
| 7 | Accept payment by cash | `6_payment.py` |
| 8 | Accept payment by M-Pesa (Safaricom Daraja STK Push) | `6_payment.py` |
| 9 | Prevent a session from being paid for a second time | `5_exit.py`'s payment guard |
| 10 | Show a live dashboard of today's revenue, vehicles served, and occupancy | `8_reports.py` |
| 11 | Require staff to log in before using the system | `1_auth.py` |
| 12 | Run as a web-based system, accessible from a browser | `app.py` (Flask) |

### 2.2 Non-Functional Requirements

Functional requirements describe *what* the system does; non-functional
requirements describe *how well* it does it.

| # | Requirement | Why it matters |
|---|---|---|
| 1 | Written in Python | Required by the assignment brief |
| 2 | Simple rather than complex | Easier to read, mark, and maintain |
| 3 | Well-commented throughout | Lets a reader understand the reasoning without asking the author |
| 4 | Time must be consistent everywhere (one shared clock) | Stops fee calculations from ever disagreeing with each other |
| 5 | Organised around identified use cases, modules, algorithms, and a database | Directly required by the assignment brief |
| 6 | Usable on both desktop and mobile screens | Attendants may use a phone or tablet, not only a desktop |
| 7 | Passwords never stored as plain text | Basic security expectation for any login system |
| 8 | Delivered as a functional system via GitHub | Required delivery method for the assignment |

### 2.3 Software Requirements

| Requirement | Notes |
|---|---|
| Python 3.9+ | Core language |
| Flask | Web framework |
| Werkzeug | Password hashing (installed with Flask) |
| requests | Used to call the M-Pesa Daraja API |
| SQLite | Built into Python — no separate install needed |
| A web browser | To use the system |
| Safaricom Daraja account | Only needed for M-Pesa payments |



### 2.4 Use Cases (Summary)

| Use Case | Actor | Result |
|---|---|---|
| Log in | Attendant | Gains access to the system |
| View available slots | Attendant | Sees live slot map |
| Register vehicle entry | Attendant | Vehicle assigned a slot, barrier opens |
| Look up vehicle / calculate fee | Attendant | System shows amount owed |
| Pay by cash | Attendant | Session completed, slot freed, barrier opens |
| Pay by M-Pesa | Attendant + Driver + Safaricom | Session completed once payment confirmed |
| View dashboard / reports | Attendant | Sees today's revenue, vehicle count, occupancy |



## 3. Data Types and Names

This section names every database table, every field, and every
function in the system, along with its data type — so the exact
"identified... database" and "modules" required by the assignment are
laid out clearly and precisely. Naming things exactly, with their
types, is what lets someone else pick up this code and know immediately
what each piece of data is and what shape it takes, without having to
read the full source line by line.

### 3.1 Database Tables

The database has five tables. Four of them (`vehicles`, `sessions`,
`payments`, and indirectly `slots`) exist because of the parking
process itself; the fifth (`users`) exists purely to support login.

**Table: `users`**

| Field | Type | Meaning |
|---|---|---|
| `id` | INTEGER (PK) | Unique user id |
| `username` | TEXT | Login name |
| `password_hash` | TEXT | Encrypted password |
| `role` | TEXT | `"admin"` or `"attendant"` |
| `created_at` | TEXT | When the account was created |

**Table: `slots`**

| Field | Type | Meaning |
|---|---|---|
| `slot_number` | INTEGER (PK) | 1 to 20 |
| `is_occupied` | INTEGER (0/1) | Whether a vehicle is in it |

**Table: `vehicles`**

| Field | Type | Meaning |
|---|---|---|
| `vehicle_id` | INTEGER (PK) | Unique vehicle id |
| `plate_number` | TEXT (unique) | Number plate |
| `owner_name` | TEXT | Optional |
| `vehicle_type` | TEXT | Optional |
| `registered_at` | TEXT | First-ever entry time |

**Table: `sessions`**

| Field | Type | Meaning |
|---|---|---|
| `session_id` | INTEGER (PK) | Unique visit id |
| `vehicle_id` | INTEGER (FK) | Which vehicle |
| `plate_number` | TEXT | Copy for fast lookup |
| `slot_number` | INTEGER | Which slot was used |
| `entry_time` | TEXT | Arrival time |
| `exit_time` | TEXT | Departure time (set once paid) |
| `fee_charged` | INTEGER | Kshs charged (set once paid) |
| `is_paid` | INTEGER (0/1) | Paid or not |
| `payment_method` | TEXT | `"CASH"` or `"MPESA"` |
| `status` | TEXT | `"PARKED"` or `"COMPLETED"` |

**Table: `payments`**

| Field | Type | Meaning |
|---|---|---|
| `payment_id` | INTEGER (PK) | Unique payment id |
| `session_id` | INTEGER (FK) | Which session was paid |
| `amount` | INTEGER | Kshs paid |
| `method` | TEXT | `"CASH"` or `"MPESA"` |
| `mpesa_receipt` | TEXT | Safaricom receipt code (M-Pesa only) |
| `paid_at` | TEXT | Time of payment |

### 3.1.1 Full SQL Schema

```sql
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'attendant',
    created_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS slots (
    slot_number INTEGER PRIMARY KEY,
    is_occupied INTEGER NOT NULL DEFAULT 0
);

CREATE TABLE IF NOT EXISTS vehicles (
    vehicle_id INTEGER PRIMARY KEY AUTOINCREMENT,
    plate_number TEXT UNIQUE NOT NULL,
    owner_name TEXT,
    vehicle_type TEXT,
    registered_at TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS sessions (
    session_id INTEGER PRIMARY KEY AUTOINCREMENT,
    vehicle_id INTEGER NOT NULL,
    plate_number TEXT NOT NULL,
    slot_number INTEGER NOT NULL,
    entry_time TEXT NOT NULL,
    exit_time TEXT,
    fee_charged INTEGER,
    is_paid INTEGER NOT NULL DEFAULT 0,
    payment_method TEXT,
    status TEXT NOT NULL DEFAULT 'PARKED',
    FOREIGN KEY (vehicle_id) REFERENCES vehicles (vehicle_id)
);

CREATE TABLE IF NOT EXISTS payments (
    payment_id INTEGER PRIMARY KEY AUTOINCREMENT,
    session_id INTEGER NOT NULL,
    amount INTEGER NOT NULL,
    method TEXT NOT NULL,
    mpesa_receipt TEXT,
    paid_at TEXT NOT NULL,
    FOREIGN KEY (session_id) REFERENCES sessions (session_id)
);
```

This exact schema is what `database/db.py`'s `init_db()` function runs
automatically the first time the system starts.

### 3.2 Module Names and Function Signatures

Every function below is named clearly for what it does — a reader
should be able to guess roughly what a function does just from its
name, before even reading its code. Each one lists its parameters
(inputs) and what it returns (output), with the Python type of each.

**Module 1 — `modules/1_auth.py` (Authentication)**

| Function | Parameters → Returns |
|---|---|
| `create_user` | `(username: str, password: str, role: str) → dict` |
| `authenticate` | `(username: str, password: str) → dict or None` |
| `login_required` | `(view_function) → function` (decorator) |
| `get_current_user` | `() → dict or None` |

**Module 2 — `modules/2_slots.py` (Slots)**

| Function | Parameters → Returns |
|---|---|
| `get_all_slots` | `() → list of dict` |
| `count_available_slots` | `() → int` |
| `find_free_slot` | `() → int or None` |
| `occupy_slot` | `(slot_number: int) → None` |
| `free_slot` | `(slot_number: int) → None` |

**Module 3 — `modules/3_vehicles.py` (Vehicles)**

| Function | Parameters → Returns |
|---|---|
| `get_vehicle_by_plate` | `(plate_number: str) → dict or None` |
| `register_vehicle` | `(plate_number: str, owner_name: str, vehicle_type: str) → dict` |
| `list_vehicles` | `() → list of dict` |

**Module 4 — `modules/4_entry.py` (Entry)**

| Function | Parameters → Returns |
|---|---|
| `record_entry` | `(plate_number: str, owner_name: str, vehicle_type: str) → dict` |
| `get_active_session` | `(plate_number: str) → dict or None` |
| `get_session_by_id` | `(session_id: int) → dict or None` |

**Module 5 — `modules/5_exit.py` (Exit & Fee)**

| Function | Parameters → Returns |
|---|---|
| `calculate_duration_minutes` | `(entry_time_str: str, exit_time_str: str) → float` |
| `calculate_fee` | `(entry_time_str: str, exit_time_str: str) → dict` |
| `get_exit_quote` | `(plate_number: str) → dict` |
| `get_payable_session` | `(session_id: int) → tuple(dict, int)` |

**Module 6 — `modules/6_payment.py` (Payment)**

| Function | Parameters → Returns |
|---|---|
| `pay_with_cash` | `(session_id: int, amount: int) → dict` |
| `initiate_mpesa_payment` | `(phone_number: str, amount: int, session_id: int) → dict` |
| `confirm_mpesa_payment` | `(session_id: int, amount: int, mpesa_receipt: str) → dict` |

**Module 7 — `modules/7_barrier.py` (Barrier)**

| Function | Parameters → Returns |
|---|---|
| `open_barrier` | `(reason: str) → dict` |
| `close_barrier` | `() → dict` |
| `allow_exit_if_paid` | `(session: dict) → dict` |

**Module 8 — `modules/8_reports.py` (Reports)**

| Function | Parameters → Returns |
|---|---|
| `get_today_revenue` | `() → int` |
| `get_today_vehicle_count` | `() → int` |
| `get_occupancy_summary` | `() → dict` |
| `get_recent_sessions` | `(limit: int) → list of dict` |
| `get_daily_report` | `() → dict` |

**Shared — `database/db.py` (Database + Clock)**

| Function | Parameters → Returns |
|---|---|
| `now` | `() → datetime` |
| `now_str` | `() → str` |
| `parse_time` | `(time_string: str) → datetime` |
| `get_connection` | `() → sqlite3.Connection` |
| `init_db` | `() → None` |

### 3.3 Why These Data Types Were Chosen

- **Timestamps are stored as TEXT, not a special date type.** SQLite
  has no dedicated date/time type — storing a fixed-format string
  (`"YYYY-MM-DD HH:MM:SS"`) is simple, sortable, and easy to read
  directly in the database without any conversion.
- **Booleans are stored as INTEGER (0 or 1).** SQLite has no true
  boolean type either — 0/1 is the standard, lightweight way to
  represent yes/no values (`is_occupied`, `is_paid`).
- **IDs are all INTEGER, auto-incrementing.** This guarantees every
  row has a simple, unique, ever-increasing identifier with no extra
  code needed to generate one.
- **Money (fees, amounts) is stored as INTEGER, not a decimal type.**
  Kenyan Shilling amounts in this system are always whole numbers
  (50, 100, 300, 500), so there is no need for decimal places, and
  using plain integers avoids any floating-point rounding issues.

---

## 4. Algorithms

This section spells out, step by step, exactly how the system's core
logic works — the same process a marker could follow by hand to check
the system is doing the right thing at each stage. Each algorithm is
kept deliberately short and linear, in line with the requirement that
the system be "simple rather than complex."

### 4.1 Slot Assignment Algorithm (First-Fit)

Decides which parking slot a newly arrived vehicle is given.

```
1. Look through the slots table for rows where is_occupied = 0.
2. Sort by slot_number, smallest first.
3. Return the first (lowest-numbered) free slot.
4. If none are free, return None (lot is full).
```
Complexity: O(n), n = 20 — effectively instant.

**Why first-fit, and not something more complex?** A first-fit
algorithm — always take the lowest-numbered free slot — was chosen
over alternatives like nearest-to-entrance or round-robin rotation for
three reasons: it's trivially easy to check by eye during a demo
(slot 1 always fills before slot 2), it needs no extra memory or state
beyond what's already stored in the `slots` table, and for a lot of
only 20 spaces, a smarter algorithm would give no meaningful real-world
benefit over this simple one.

### 4.2 Vehicle Entry Algorithm

Runs the moment a vehicle arrives — this is the busiest, most
time-sensitive operation the system performs.

```
1. Reject if plate_number is empty.
2. Run the Slot Assignment Algorithm.
   → If no slot is free, stop: "Parking lot is full."
3. Look up the vehicle by plate number.
   - If it exists, reuse it.
   - If not, create a new vehicle record.
4. Get the current time from the shared clock.
5. Create a new session: vehicle_id, plate_number, slot_number,
   entry_time, status = "PARKED".
6. Mark the assigned slot as occupied.
7. Open the barrier.
8. Return the session id, slot number, and entry time.
```
Complexity: O(1) — a small, fixed number of database operations,
regardless of how many vehicles have ever used the system.

### 4.3 Fee Calculation Algorithm

The core algorithm the assignment brief asks for directly — it turns
an entry time into a duration, and a duration into a fee.

```
1. Read the session's entry_time.
2. Get the current time from the shared clock (or the exit time,
   if already known).
3. Subtract: duration = current_time − entry_time.
4. Convert to minutes, then to hours.
5. Apply the fee bands, in order:
       minutes <= 30           → fee = 0
       hours   <= 2             → fee = 50
       hours   <= 4              → fee = 100
       hours   <= 6                → fee = 300
       otherwise                     → fee = 500
6. Return minutes parked, hours parked, and the fee.
```

**Worked Examples**

| Entry | Exit | Duration | Fee |
|---|---|---|---|
| 08:00 | 08:25 | 25 min | Free |
| 08:00 | 08:30 | 30 min | Free |
| 08:00 | 08:31 | 31 min | Kshs 50 |
| 08:00 | 10:00 | 2 h | Kshs 50 |
| 08:00 | 10:01 | 2 h 1 min | Kshs 100 |
| 08:00 | 12:01 | 4 h 1 min | Kshs 300 |
| 08:00 | 14:01 | 6 h 1 min | Kshs 500 |

Complexity: O(1) — a fixed number of comparisons, no matter how long
the vehicle has actually been parked.

### 4.4 Payment Authorisation Guard

Runs before any payment is accepted, to make sure the driver is being
charged the correct amount and hasn't already paid.

```
1. Look up the session by id.
2. If it doesn't exist → reject: "Session not found."
3. If its status is not "PARKED" → reject: "Already completed."
4. Otherwise, recalculate the fee fresh (Section 4.3) — never trust
   an amount sent from the browser.
5. Return the session and the confirmed fee.
```
This is what stops a driver being charged the wrong amount, and stops
one session being paid for twice.

### 4.5 Payment Finalisation Algorithm (Cash and M-Pesa)

Both payment methods end the same way — this shared sequence runs once
money has actually been confirmed received.

```
1. Get the current time (paid_at) from the shared clock.
2. Insert a payments row: session_id, amount, method, mpesa_receipt,
   paid_at.
3. Update the sessions row: is_paid = 1, payment_method, exit_time,
   fee_charged, status = "COMPLETED".
4. Free the vehicle's slot.
5. Open the barrier.
```

### 4.6 M-Pesa STK Push Algorithm

Handles sending a payment request directly to a driver's phone through
Safaricom's Daraja API.

```
1. Check the phone number matches the format 254XXXXXXXXX.
2. Run the Payment Authorisation Guard to confirm the fee.
3. If the fee is 0, skip M-Pesa and finalise it as free immediately.
4. Otherwise:
   a. Request an access token from Safaricom's Daraja API.
   b. Build the STK Push request (shortcode, amount, phone number,
      callback URL, reference).
   c. Send the request. Safaricom prompts the driver's phone.
5. When the driver enters their PIN, Safaricom calls back the system,
   which runs the Payment Finalisation Algorithm.
```

### 4.7 Barrier Control Algorithm

```
1. On successful entry → open the barrier.
2. On successful payment → open the barrier.
3. (Optional) After a delay, close the barrier again.
```

### 4.8 Daily Report Algorithm

```
1. Get today's date from the shared clock.
2. Sum all payments made today → revenue.
3. Count all sessions started today → vehicle count.
4. Count occupied vs free slots → occupancy.
5. List the most recent sessions, newest first.
6. Combine into one report.
```

### 4.9 Login Algorithm

```
1. Look up the username.
2. If not found → reject.
3. Compare the entered password against the stored encrypted password.
4. If they match → log the user in.
5. If not → reject with an error message.
```

### 4.10 Time Consistency Rule

Only one function in the whole system — `now()` in `database/db.py` —
is allowed to read the real system clock. Every module calls that same
function whenever it needs "the current time," so entry time, exit
time, fee calculation, and payment time can never disagree with each
other. This directly satisfies the requirement that "the time should
be consistent and has no errors."

---

## 5. The Modules — Simple Explanation and How They Connect

This section explains, in plain language, what each of the 8 modules
does and why it exists as its own separate file, rather than being
mixed into one large program.

### 5.1 The 8 Modules, Explained

**Module 1 — `1_auth.py` (Authentication).** This is the first thing
that runs when anyone tries to use the system. It checks a typed-in
username and password against what's stored in the database, and only
lets a person through if they match. Passwords are never stored as
plain text — they go through an encryption function first, so even
someone looking directly inside the database cannot read anyone's
actual password.

**Module 2 — `2_slots.py` (Slots).** This module's whole job is
answering one question, over and over: "which slots are free right
now?" It doesn't know anything about vehicles, fees, or payments — it
only tracks the 20 slots and whether each one is occupied. Keeping this
separate means the visual slot display can be built, and even changed,
without touching any other part of the system.

**Module 3 — `3_vehicles.py` (Vehicles).** Remembers every vehicle
that has ever used the parking lot. When the same car returns a second
time, this module recognises the plate number and reuses the existing
record instead of creating a duplicate — so a regular visitor's details
only ever need to be typed in once.

**Module 4 — `4_entry.py` (Entry).** Runs the moment a vehicle arrives.
It asks Module 2 for a free slot, asks Module 3 to register the
vehicle, and then creates a new "session" — the record of this
particular visit — stamped with the current time.

**Module 5 — `5_exit.py` (Exit & Fee).** Runs when a driver wants to
leave. It looks up the vehicle's session, works out exactly how long
it has been parked, and applies the fee table to calculate what is
owed. This module never trusts a fee typed into a form — it always
recalculates the amount itself, directly from the stored entry time.

**Module 6 — `6_payment.py` (Payment).** Takes the money — either
confirming cash was received, or triggering an M-Pesa prompt on the
driver's phone. Once payment succeeds, it marks the visit complete,
which is what allows Modules 2 and 7 to free the slot and open the
barrier.

**Module 7 — `7_barrier.py` (Barrier).** A small, focused module that
only knows how to "open" and "close" the barrier. In this version it
prints a message to represent the signal; in a real installation, this
is the only file that would need to change to control real hardware.

**Module 8 — `8_reports.py` (Reports).** Adds up numbers for the
dashboard and reports page — today's revenue, how many vehicles have
been served, and how full the lot currently is — by reading directly
from the database.

### 5.2 The 8 Modules, at a Glance

| # | Module | What it does, in plain terms |
|---|---|---|
| 1 | `1_auth.py` | Lets staff log in safely. Checks passwords without ever storing them as plain text. |
| 2 | `2_slots.py` | Keeps track of which of the 20 parking slots are free or full. |
| 3 | `3_vehicles.py` | Remembers every vehicle that has ever visited, so returning cars don't need re-entering. |
| 4 | `4_entry.py` | Handles a car arriving — picks a slot, logs the time, opens the barrier. |
| 5 | `5_exit.py` | Works out how long a car was parked and what it owes. |
| 6 | `6_payment.py` | Takes the payment — cash or M-Pesa — and finishes the visit. |
| 7 | `7_barrier.py` | Opens and closes the barrier. |
| 8 | `8_reports.py` | Adds up the day's numbers for the dashboard and reports page. |

Plus one shared file, `database/db.py`, which holds the database
connection and the one shared clock every module uses.

### 5.3 How They Connect

```
   Driver arrives
        │
        ▼
 ┌─────────────┐   asks for a free slot   ┌──────────────┐
 │  4_entry.py │ ────────────────────────▶│  2_slots.py  │
 └─────┬───────┘                            └──────────────┘
       │  registers the vehicle
       ▼
 ┌────────────────┐
 │ 3_vehicles.py   │
 └────────────────┘
       │  writes the new session using the time from...
       ▼
 ┌──────────────┐
 │ database/db.py│  ← every module gets its clock and its
 └──────────────┘     database connection from here
       ▲
       │
 ┌─────┴───────┐   calculates the fee owed   ┌───────────────┐
 │  5_exit.py  │◀────────────────────────────│   sessions     │
 └─────┬───────┘                              └───────────────┘
       │  driver chooses how to pay
       ▼
 ┌─────────────────┐   frees the slot    ┌──────────────┐
 │ 6_payment.py     │────────────────────▶│  2_slots.py  │
 └────────┬─────────┘                     └──────────────┘
          │  opens the barrier
          ▼
 ┌─────────────────┐
 │  7_barrier.py    │
 └─────────────────┘

 At any time: 8_reports.py reads from the database to build the
 dashboard and reports page.
```

### 5.4 Why Split Into 8 Separate Files

- Each file does **one job only** — easy to find, read, and fix.
- Each module can be tested on its own, without running the whole
  website.
- It matches exactly what the assignment asked for: a system built
  around clearly identified modules.
- `app.py` (the main Flask file) does not contain any business logic
  itself — it only calls these 8 modules and displays the result. This
  keeps the "thinking" part of the system separate from the "web
  page" part.

### 5.5 A Note on the Module Filenames

The modules are named `1_auth.py`, `2_slots.py`, and so on. Python
normally can't `import` a file whose name starts with a number, so
every file that needs another module loads it like this instead:

```python
import importlib
auth = importlib.import_module("modules.1_auth")
```

This is the only special step the numbering requires — everything else
works exactly like a normal Python import.

### 5.6 A Worked Example — All 8 Modules Together

To make the connection between modules completely concrete, here is
exactly what happens, module by module, for one single car:

1. **08:00 —** A car with plate `KDA 123A` arrives. The attendant types
   the plate into the Entry page.
   - `4_entry.py` asks `2_slots.py` for a free slot → gets slot 7.
   - `4_entry.py` asks `3_vehicles.py` to register the plate → creates
     a new vehicle record.
   - `4_entry.py` reads the time from `database/db.py` → `08:00:00`.
   - `4_entry.py` creates a new session row, and tells `2_slots.py` to
     mark slot 7 occupied.
   - `4_entry.py` tells `7_barrier.py` to open the barrier.
2. **10:30 —** The driver wants to leave. The attendant enters the
   plate on the Exit page.
   - `5_exit.py` finds the session (via `4_entry.py`'s lookup
     function), reads its `entry_time` (`08:00:00`), and reads the
     current time from `database/db.py` (`10:30:00`).
   - Duration = 2 hours 30 minutes → fee = Kshs 100 (the "up to 4
     hours" band).
3. **Payment —** The attendant clicks "Confirm Cash Payment."
   - `6_payment.py` re-confirms the fee with `5_exit.py` (never
     trusting a typed-in amount), records the payment, and marks the
     session complete.
   - `6_payment.py` tells `2_slots.py` to free slot 7.
   - `6_payment.py` tells `7_barrier.py` to open the barrier again.
4. **Any time after —** `8_reports.py` reads the completed session
   from the database and includes it in today's revenue (+Kshs 100)
   and vehicle count (+1) on the dashboard.

This single example touches all eight modules, and shows exactly how
they hand work off to each other without any module needing to know
the internal details of how another one works — each only calls the
other's clearly named functions.

---

## 6. Later Improvements

The current system meets every requirement in Section 2, but there is
always room to take it further. Below are realistic next steps, with a
short note on why each one would help.

- **Connect the barrier module to real hardware** (e.g. Raspberry Pi +
  relay) instead of the current software placeholder — this is the one
  piece of the system that is currently only "simulated" in the code.
- **Move M-Pesa from Safaricom's sandbox to production credentials**
  so real payments can be collected, not just test ones.
- **Verify that `/mpesa/callback` requests genuinely come from
  Safaricom** — right now the system trusts any request sent to that
  address; a production system should check it is really Safaricom
  calling.
- **Add automatic session timeout / "remember me" login** — so a
  forgotten, unattended login doesn't stay open indefinitely.
- **Add exportable CSV or PDF reports** — so management can download
  and archive a day's or month's figures, not just view them on screen.
- **Support more than one parking lot or branch** — the current design
  assumes one fixed 20-slot lot; a business with several sites would
  need this extended.
- **Add SMS receipt confirmation** alongside the M-Pesa prompt, for
  drivers who want a text confirmation as well as the phone prompt.
- **Add a dedicated vehicle history page** — the function for this
  (`list_vehicles()`) already exists in `3_vehicles.py`, it just isn't
  linked to a page yet.
- **Add login rate-limiting** to block repeated password guessing
  attempts — a common basic security improvement for any login page.

---

## 7. Finish

### 7.1 Conclusion

Moih Park Kenya meets every requirement set out in the assignment: a
working, web-based parking system, built in Python, organised around
eight clearly identified modules and a shared SQLite database, that
shows drivers live slot availability, calculates fees automatically
and consistently, and accepts both cash and M-Pesa payment. Every
module was tested individually and as part of the full system before
being considered finished, and the fee logic was checked against
worked examples by hand to confirm it matched the tariff exactly. The
system is delivered here as a complete, ready-to-run, and
well-documented project.

### 7.2 How to Run It

```bash
git clone <your-repo-url>
cd moih_park_kenya
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Open `http://localhost:5000` in a browser. The database and 20 slots
are created automatically the first time it runs.

### 7.3 Default Login

| Username | Password |
|---|---|
| `Sawyer` | `admin1234` |

### 7.4 M-Pesa Setup (Optional)

Set these before running the app if you want to test M-Pesa payments:

```bash
export DARAJA_CONSUMER_KEY="your_key"
export DARAJA_CONSUMER_SECRET="your_secret"
export DARAJA_SHORTCODE="your_shortcode"
export DARAJA_PASSKEY="your_passkey"
export DARAJA_CALLBACK_URL="https://your-public-url/mpesa/callback"
```

Without these, everything else in the system still works normally —
only the M-Pesa button is affected.

### 7.5 GitHub Delivery

```bash
git init
git add .
git commit -m "Moih Park Kenya — parking management system"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

`.gitignore` already excludes the virtual environment and the local
database file, so the repository stays clean.


