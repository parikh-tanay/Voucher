# IEEE Nirma — Reimbursement Voucher Generator

A one-page web tool that recreates the IEEE Student Branch, Nirma University
**Expense Reimbursement Voucher** and outputs it as a ready-to-use `.docx`
file — including the student's signature and the bill / payment-screenshot /
reimbursement-screenshot attachments appended at the end of the document.

## How it works

Everything runs **entirely in the browser** (HTML + JavaScript, using the
[`docx`](https://www.npmjs.com/package/docx) library loaded from a CDN). There
is no backend, no database, and no server-side processing:

- Every visitor gets their own independent copy of the page running in their
  own browser tab.
- Any number of people can use the tool **at the same time** with zero
  conflict, because no state is shared or stored anywhere — the finished
  `.docx` is generated locally and downloaded straight to the user's device.
- No form data or uploaded images ever leave the user's computer.

This is why a static site (like GitHub Pages) is enough to host it — no
server code is required.

## What the tool does

1. **Event Details** — Event Name, Purpose, Date.
2. **Particulars** — an editable line-item table (add/remove rows) matching
   the original voucher's columns (Biller's Name, Bill Date, GST No., Items /
   Services, Amount, Paid in Cash/Online), with the total calculated
   automatically.
3. **Student Details** — Name, Roll No., Nirma email, phone, UPI/Account
   number.
4. **Student Signature** *(optional)* — upload a single image of the
   student's signature (photo, scan, or a signature drawn on a plain
   background). It's embedded small, right under the student details in the
   generated document. If nothing is uploaded, the document still prints a
   blank `_______________  Date: ____________` line so it can be signed by
   hand.
5. **Attachments** — drag-and-drop (or click to upload) for:
   - Bill(s) of purchase
   - Payment screenshot (individual → vendor)
   - Reimbursement screenshot (branch → individual)

   Each accepts multiple images. Every image is placed on its own page at
   the end of the generated Word document, scaled to fit, with a caption
   showing its filename.
6. **Generate .docx** — builds the full document client-side and triggers a
   download named `Reimbursement_Voucher_<Event>_<Student>.docx`.

## Customizing

- **Branding / colors**: edit the CSS variables at the top of the `<style>`
  block in `index.html` (`--ieee-blue`, `--ink`, etc).
- **Voucher layout**: the document structure is built in the `generateDocx()`
  JavaScript function — column widths, fonts, and section order are all
  defined there using the `docx` library's `Table`, `Paragraph`, and
  `ImageRun` building blocks.
- **Extra fields**: add an input to the relevant `<section class="card">` in
  the HTML, then reference its value inside `generateDocx()`.

## Local testing

Just open `index.html` directly in a browser — no server needed. (Some
browsers restrict `file://` module loading for certain features, so if
anything misbehaves locally, serve it with e.g. `python3 -m http.server` and
open `http://localhost:8000`.)
