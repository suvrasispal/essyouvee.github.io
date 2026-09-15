# The Pint Bar — Inventory Management

A branded stock-control application that uses an Excel workbook as its
database. No install, no server, no npm.

---

## What's in this package

```
The Pint Bar Inventory/
├── index.html            ← THE APPLICATION. Open this.
├── data/
│   └── inventory.xlsx    ← THE DATABASE. 82 products, ready to use.
├── assets/               Brand assets
│   ├── lockup-horizontal.svg           cream / light grounds
│   ├── lockup-horizontal-reversed.svg  dark green grounds (app header)
│   ├── mark.svg                        tankard mark, full colour
│   ├── mark-cream.svg                  tankard mark, reversed
│   └── favicon.svg
├── src/                  Editable source, for future changes
│   ├── index.dc.html
│   ├── support.js
│   └── assets/
└── README.md             This file
```

---

## Setting it up — once, takes ten seconds

1. Open `index.html` in **Google Chrome** or **Microsoft Edge** on a desktop
   or laptop.
2. Go to **Settings**.
3. Press **Connect workbook** and choose `data/inventory.xlsx` from this
   folder.
4. When the browser asks whether to let the site save changes, choose
   **Save changes** / **Allow**.

The dashboard now shows the stock held in that file, and every change you make
is written straight back into it.

## Using it from then on

Open `index.html` and it reconnects to the same workbook on its own. You never
have to find the file again.

If the browser has forgotten the permission — usually after a full restart — an
amber bar appears with a single **Reconnect workbook** button. One click and
you are back. The header chip shows which state you are in:

- **Green, "Live · inventory.xlsx"** — connected, saving to the file.
- **Amber, "Reconnect workbook"** — one click needed.
- **Amber, "Workbook unreachable"** — the file is locked or moved (see below).

---

## How it works

The workbook is the single source of truth. The browser does not keep its own
copy of the inventory.

- **On open** the app reads `inventory.xlsx` and displays what it holds.
- **On every change** — add stock, record a sale, adjust, add, edit or delete a
  product — it writes the whole workbook back, then reads it again, so the
  screen always matches the file.
- **Every five seconds** it re-reads the file, so a change made in Excel, or by
  another computer on a synced folder, appears on its own.
- **Before each save** it checks the file has not changed since it was last
  read. If it has, your view refreshes to the newer data instead of overwriting
  it, and you are asked to redo that one change.

The connection is remembered by the browser itself, not by a cookie or local
storage, so clearing browsing data does not lose your inventory — the data is
in the Excel file, not in the browser.

---

## Sharing between computers

Put the package in a folder that syncs — **OneDrive, Dropbox, Google Drive**, or
a **shared network drive** — and connect each computer to its own path to that
same file.

Bartender A saves → the folder syncs → Bartender B's screen picks it up within
a few seconds.

Two caveats worth knowing:

- Sync services are not instant. If two people change stock in the same few
  seconds on different machines, the sync service may keep both versions as
  conflicted copies. In practice, at one bar with one till computer, this does
  not arise.
- Each computer connects to the file once, the first time.

---

## Which browsers work

| Browser | Reads and writes the file |
|---|---|
| Chrome, Edge, Opera (desktop) | Yes |
| Brave, Arc, other Chromium desktop browsers | Yes |
| Safari | No |
| Firefox | No |
| Any phone or tablet browser | No |

This is a browser security rule, not a limitation of the application: only
Chromium desktop browsers allow a web page to write to a chosen local file.

In an unsupported browser the app still runs and shows the starting stock, and
you can move data in and out with **Import .xlsx** and **Export .xlsx** in
Settings — but it will not save to the file automatically.

---

## Excel structure

Two sheets. Keep the names and the column headers as they are.

### Sheet: `Inventory`

| ID | Product | Category | Unit | Current Stock | Low Stock Threshold | Notes |
|----|---------|----------|------|--------------:|--------------------:|-------|
| P001 | Birra Moretti | Draught | Keg | 0 | 1 | |

### Sheet: `Transactions`

| Date | Time | Product ID | Product | Transaction Type | Quantity | Previous Stock | New Stock | Reason | Staff |
|------|------|------------|---------|------------------|---------:|---------------:|----------:|--------|-------|
| 11 Sep 2026 | 19:42 | P010 | Budweiser | Sale | -2 | 5 | 3 | | Bar staff |

Transaction types: `Sale`, `Stock Added`, `Adjustment`, `Product Added`,
`Product Deleted`. Quantity is signed — negative removes stock.

You can edit the workbook in Excel directly. The app picks the changes up
within a few seconds.

**Close it in Excel while the bar is trading.** Excel locks the file, and saves
will fail until it is closed — the app tells you rather than losing the change.

---

## Backups

The application does not make backups of its own; the workbook is a normal file,
so back it up the way you back up any other:

- Keep it in OneDrive, Dropbox or Google Drive — all three keep version history
  and can restore an earlier copy.
- Or use **Export .xlsx** in Settings to save a dated snapshot before a stock
  take.

---

## Current stock data

`data/inventory.xlsx` holds the list supplied on 11 Sep 2026 — 16 draught lines
and 66 spirits, wines, beers, soft drinks and mixers, 1,481 units in total.
Everything that was not on that list is set to 0.

Items with an amber ⚑ on their row need a decision:

- **Johnnie Walker Double Black** — no quantity was supplied; it shows 0 and is
  excluded from the out-of-stock count until someone counts it.
- **Crodino (Pink)** — could not be confirmed as an official variant (Crodino
  sells as Biondo and Rosso).
- **Schweppes** — no variant stated (tonic, slimline, lemonade, ginger ale).
- **Sambuca Apple / Black / White, Tequila Blanco, Tequila Oro, Dark Rum** — no
  brand stated.
- **Disaronno 111, Sambuca Black 112, Rosé Wine 1111** — entered as supplied,
  but the figures look like typing errors; please confirm.

Hennessy and Rémy Martin are recorded without an expression (VS, VSOP) because
none was given.

---

## Data safety

- Stock can never go negative — the movement is blocked and the user warned.
- Deleting a product asks for confirmation; its movement history is kept.
- A change that cannot be written to the file is refused and reported, not
  silently kept in the browser.
- A save is rejected if the file changed since it was read, so one computer
  cannot overwrite another's newer data.
- Duplicate products collapse automatically — same ID, or same name within the
  same category.

---

## Settings

- **Default low-stock threshold** — used for new products.
- **Staff name** — recorded against every movement in the Transactions sheet.
- **Restore starting stock list** — rewrites the inventory back to the supplied
  list and clears all movements. This overwrites the workbook. Export first.

---

## If you outgrow a single file

The application reads and writes through one small data layer. Replacing Excel
with SQLite or a hosted database later means changing where that layer reads
and writes — the interface, the screens and the workflow stay exactly as they
are.
