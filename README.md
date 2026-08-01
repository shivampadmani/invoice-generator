# Swami Churna — Invoice Generator

A simple, free, static web tool that fills your existing Excel invoice
template in the browser and downloads a ready `.xlsx` invoice — no backend,
no server, no data ever leaves your browser.

## How it works

- `invoice-template.xlsx` is your original Google-Sheet-style invoice,
  kept exactly as-is (same colors, borders, GST formulas, bank details).
- `index.html` is the tool. When you click **Download**, it loads that
  template in memory using a library called ExcelJS, writes your typed
  details into the correct cells (buyer info, invoice no., date, product
  rows), and hands you back a filled `.xlsx` file — the original formulas
  (line totals, GST, grand total) recalculate automatically when you open
  it in Excel/Google Sheets/LibreOffice.
- Nothing is uploaded anywhere. It all happens locally in the visitor's browser.

## Files

```
invoice-template.xlsx   ← your Excel template (do not rename)
index.html              ← the tool (do not rename)
```

Both files must sit in the **same folder**, because `index.html` fetches
`invoice-template.xlsx` by that exact relative filename.

## Run it locally (before publishing)

Opening `index.html` directly by double-clicking it (a `file://` URL) will
usually fail to load the template, because browsers block that kind of
local file loading for security. Instead, serve the folder over a local
address:

```bash
cd svami-invoice-tool
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

(Any static server works — `npx serve`, VS Code's "Live Server" extension,
etc. This step is only for testing; it isn't needed once it's on GitHub Pages.)

## Publish for free with GitHub Pages

1. Create a new GitHub repository (e.g. `svami-invoice-tool`).
2. Upload `index.html` and `invoice-template.xlsx` to the repo root
   (drag-and-drop on github.com works fine, or `git push`).
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**,
   pick the `main` branch and `/ (root)` folder, then **Save**.
5. GitHub gives you a live link within a minute or two, typically:
   `https://<your-username>.github.io/svami-invoice-tool/`

Share that link — anyone who opens it can fill the form and download an
invoice; you don't need to keep any server running.

## Using the tool

1. Fill **Buyer Details** — customer name, address, mobile number.
   Invoice number auto-fills (`SC-0001`, `SC-0002`, …, remembered in that
   browser) and date defaults to today — both editable.
2. Add each product with quantity and rate; use **+ Add Product** for more
   rows (up to 15, matching the template's line-item area).
3. Optional: open **GST & discount settings** to change the discount or
   tax rates, or switch to IGST for an out-of-state buyer. Open
   **Advanced buyer / dispatch details** for PAN, challan, or transporter
   info if you need them.
4. Click **Download Invoice (.xlsx)**. The file downloads as
   `Invoice_<CustomerName>_<InvoiceNo>.xlsx`.

## Customizing

- **Seller details** (name, address, GST no., bank details, terms) live
  directly in `invoice-template.xlsx` — edit that file in Excel/Sheets the
  normal way if any of that ever changes; the tool always reads whatever
  is currently in that file.
- **Styling** of the web form (colors, layout) is plain CSS at the top of
  `index.html` — safe to tweak without touching the logic below it.
- **Row limit**: if you need more than 15 product lines, add more numbered
  rows to the template (copy row 16's formatting down) and update the
  `MAX_ROWS` constant near the top of the `<script>` block in `index.html`
  to match.

## Notes

- The invoice number counter is stored in the visitor's browser
  (`localStorage`), not shared across devices or visitors — treat it as a
  helpful default, not an official sequential record.
- "Amount in words" is generated automatically from the total using the
  Indian numbering system (lakh/crore).
