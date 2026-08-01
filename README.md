# Swami Churna — Invoice Generator

A simple, free, static web tool: fill in customer + product details in the
browser and download a ready-made GST invoice as a `.pdf` — no backend, no
server, no data ever leaves your browser.

## How it works

- `index.html` is the whole tool — one file, no build step.
- When you click **Download**, it draws the invoice directly in the browser
  using a library called jsPDF (same layout/colors as the original Excel
  design: navy header bar, buyer/invoice details, product table, GST totals,
  amount in words, bank details, signature block) and hands you back a `.pdf`.
- Nothing is uploaded anywhere. It all happens locally in the visitor's browser.

## Files

```
index.html   ← the entire tool (do not rename)
README.md    ← this file
```

That's it — a single HTML file is all you need to host.

## Run it locally (optional, for testing)

You can just double-click `index.html` and it will work, since it no longer
depends on fetching a separate template file. If you'd rather serve it over
a local address (e.g. to test on your phone on the same network):

```bash
cd swami-invoice-tool
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Publish for free with GitHub Pages

1. Create a new GitHub repository (e.g. `swami-invoice-tool`).
2. Upload `index.html` to the repo root (drag-and-drop on github.com works
   fine, or `git push`).
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**,
   pick the `main` branch and `/ (root)` folder, then **Save**.
5. GitHub gives you a live link within a minute or two, typically:
   `https://<your-username>.github.io/swami-invoice-tool/`

Share that link — anyone who opens it can fill the form and download an
invoice; you don't need to keep any server running.

## Using the tool

1. Fill **Buyer Details** — customer name, address, mobile number.
   Invoice number auto-fills (`SC-0001`, `SC-0002`, …, remembered in that
   browser) and date defaults to today — both editable.
2. Add each product with quantity and rate; use **+ Add Product** for more
   rows (up to 15 per invoice).
3. Optional: open **GST & discount settings** to change the discount or
   tax rates, or switch to IGST for an out-of-state buyer. Open
   **Advanced buyer / dispatch details** for PAN, challan, or transporter
   info if you need them.
4. Optional: fill in **Bank Details** if you want them printed on this
   particular invoice (see note below on why this isn't pre-filled).
5. Click **Download Invoice (.pdf)**. The file downloads as
   `Invoice_<CustomerName>_<InvoiceNo>.pdf`.

## Customizing

- **Seller details** (business name, address, mobile, GST no., the
  signature block text, the footer note) are constants right near the top
  of the `<script>` block in `index.html`, in an object called `SELLER` —
  edit the strings there if any of that ever changes.
- **Styling** of the web form (colors, layout) is plain CSS at the top of
  `index.html` — safe to tweak without touching the logic below it. The
  PDF's own look (colors, spacing, fonts) is controlled separately inside
  the `buildInvoicePdf()` function, further down the same `<script>` block.
- **Row limit**: change the `MAX_ROWS` constant near the top of the
  `<script>` block in `index.html` if you need more than 15 product lines
  — the PDF table paginates automatically if content runs past one page.

## Notes

- The invoice number counter is stored in the visitor's browser
  (`localStorage`), not shared across devices or visitors — treat it as a
  helpful default, not an official sequential record.
- "Amount in words" is generated automatically from the total using the
  Indian numbering system (lakh/crore).
- **Bank details are intentionally not hardcoded anywhere in this repo**,
  since it's public. The **Bank Details** section on the form is optional —
  leave it blank to omit that section from the PDF, or fill it in per
  invoice if you want it printed. Nothing is saved between visits; you'd
  re-enter it each time (or edit the `SELLER` object in `index.html` if
  you'd rather it always appear automatically — just be aware that then
  lives in your public repo).
