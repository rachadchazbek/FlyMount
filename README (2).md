# ÉlasT1 pre-order page

One self-contained file: `index.html`. No build step.

## 1. Publish on GitHub Pages

1. Create a public repository (for example `elast1`) and add `index.html` at the root.
2. In the repository: Settings, then Pages. Under "Build and deployment", choose "Deploy from a branch", branch `main`, folder `/ (root)`. Save.
3. After a minute or two the page is live at `https://<your-username>.github.io/elast1/`.
4. English link to share: add `?lang=en` to the URL. French is the default.

## 2. Receive orders in a Google Sheet

Until this is done, the page still works: after placing an order, the buyer sees the Interac
instructions plus a button that emails you the order details.

1. Create a Google Sheet. Put these headers in row 1:
   `Date | Ref | Name | Email | Phone | Qty | Total | Delivery | Address | Gear | Lang | Paid?`
2. In the Sheet: Extensions, then Apps Script. Replace the code with:

   ```js
   function doPost(e) {
     const p = e.parameter;
     SpreadsheetApp.getActiveSpreadsheet().getSheets()[0].appendRow([
       new Date(), p.ref, p.name, p.email, p.phone, p.qty, p.total,
       p.delivery, p.address, p.gear, p.lang
     ]);
     return ContentService.createTextOutput("ok");
   }
   ```
3. Deploy, then New deployment, type "Web app". Execute as: Me. Who has access: Anyone. Deploy and copy the web app URL.
4. Paste that URL into `orderEndpoint` in the `CONFIG` block near the bottom of `index.html`, then commit.
5. Place a test order on the live page and check that a row appears.

## 3. Match payments to orders

Each order gets a reference such as `T1-K7M2Q`. The buyer puts it in the e-Transfer message.
When the Interac deposit email arrives, find the reference in the Sheet and mark the row as paid,
then email the buyer to confirm.

## 4. Things to edit

All in the `CONFIG` block of `index.html`:

| Setting | Current value |
| --- | --- |
| `brand` | `ÉlasT1` (working name) |
| `price` / `regularPrice` | 7.99 / 15.99 |
| `interacEmail` | rachadchazbek@gmail.com |
| `interacPhone` | empty; fill it in to show the phone number as a second option |
| `orderEndpoint` | empty until step 2 is done |

Text lives in the `I18N` block, in French and English side by side.

## 5. Add photos and the video

Each placeholder is a `<div class="slot">`. Put your files in an `img/` folder and replace the div:

```html
<img src="img/etape-1.jpg" alt="Languette collée au talon du soulier" style="width:100%;border-radius:10px">
```

For the video, replace `<div class="slot slot-video">` with a `<video controls src="img/demo.mp4">` tag or a YouTube embed.
