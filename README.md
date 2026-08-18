# FAZ Invoice — Google Apps Script Web App

FAZ Invoice is a Google Sheets-backed invoice system with:

- A polished blue-and-green web interface
- Separate **Administrator** and **User** experiences
- Automatic invoice IDs such as `FAZ-2026-0001`
- Customer address, item description, quantity, unit value, tax, and grand total
- Company details and logo on a print-ready invoice
- Account creation, hashed passwords, and active/inactive users

## Files to create in Apps Script

1. Make a new **Google Sheet**. This is your private database; do not share it with ordinary invoice users.
2. In the sheet, choose **Extensions → Apps Script**.
3. Replace the default `Code.gs` with [Code.gs](Code.gs).
4. Create an HTML file named `Index`, then paste in [Index.html](Index.html).
5. In **Project Settings**, set the timezone if needed. Optionally replace the manifest with [appsscript.json](appsscript.json).
6. Save the project, run `setupFAZInvoice` once in the Apps Script editor, and approve the requested Google permissions.

Running setup creates these protected database tabs:

| Tab | Purpose |
| --- | --- |
| `FAZ_Config` | Company information, logo URL, currency, invoice prefix |
| `FAZ_Accounts` | User records and one-way password hashes |
| `FAZ_Invoices` | Invoice headers and calculated totals |
| `FAZ_InvoiceItems` | Every invoice line item |

## First login

Use the initial administrator account:

| Username | Password |
| --- | --- |
| `admin` | `ChangeMe123!` |

Immediately open **Accounts**, edit `admin`, and set a new password. Administrators can then create user or administrator accounts. Users can create and edit only the invoices they created; administrators can edit every setting, account, and invoice.

## Add your logo

If a logo is inserted **over a cell** in the spreadsheet before setup, FAZ attempts to copy its URL into Settings. For the most reliable result, open **Settings** after first sign-in and paste a direct, publicly loadable image URL into **Logo URL**. It will be placed in the invoice header and the print/PDF view.

If the logo lives in Google Drive, make the file viewable to anyone with the link and use a direct image URL such as:

```
https://drive.google.com/uc?export=view&id=YOUR_FILE_ID
```

## Deploy the app

1. In Apps Script select **Deploy → New deployment**.
2. Choose **Web app**.
3. Set **Execute as** to **Me** (the spreadsheet owner).
4. Set **Who has access** to the audience that should be able to reach the login page. `Anyone` works for a custom username/password login, but use your organization setting if available.
5. Click **Deploy**, authorize it, and open the Web app URL.

The web app already checks FAZ credentials, even if the deployment link is shared. Keep the spreadsheet itself private, because it contains the database and password hashes.

## Generate an invoice PDF

Save an invoice and select the print prompt. In the browser print dialog, choose **Save to PDF**. The invoice includes your logo, business details, invoice number, customer address, all items, tax, and grand total.

## Notes

- Do not rename the four `FAZ_` database tabs or their header columns after setup.
- Invoice numbers are allocated under a Google Apps Script lock, so simultaneous saves do not create duplicate invoice numbers.
- This is a lightweight custom login for a private business tool. For a large public system, use Google Workspace / OAuth authentication rather than collecting passwords in a spreadsheet-backed app.
