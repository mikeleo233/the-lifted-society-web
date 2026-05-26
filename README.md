# The Lifted Society — Complete Site

This folder contains the full website for The Lifted Society publisher: the homepage, every book's landing page, and every post-purchase thank you page.

The site currently holds **four books**:

1. *From Salary to Wealth*
2. *How Great Men Fall*
3. *The Sin That Ends Destinies*
4. *The Dream Killer*

Books 2, 3, and 4 are the **same book sold under three test titles**. They run as three separate landing pages so each Facebook ad can be matched to a page that echoes its title. Whichever title wins on cost per purchase becomes the main one; the others can be retired.

---

## File structure

```
the-lifted-society-web/
├── index.html                          ← Homepage (theliftedsociety.org)
├── from-salary-to-wealth/
│   ├── index.html                      ← Landing page
│   ├── cover.png                       ← Book cover
│   └── thank-you/
│       └── index.html                  ← Thank you page (after Paystack)
├── how-great-men-fall/
│   ├── index.html                      ← Landing page
│   ├── cover.png                       ← Book cover
│   └── thank-you/
│       └── index.html                  ← Thank you page (after Paystack)
├── the-sin-that-ends-destinies/
│   ├── index.html                      ← Landing page
│   ├── cover.png                       ← Book cover
│   └── thank-you/
│       └── index.html                  ← Thank you page (after Paystack)
├── the-dream-killer/
│   ├── index.html                      ← Landing page
│   ├── cover.png                       ← Book cover
│   └── thank-you/
│       └── index.html                  ← Thank you page (after Paystack)
├── shared/
│   ├── styles.css                      ← Shared base reset
│   └── cover-small.png                 ← Small cover for homepage
└── README.md                           ← This file
```

**Live URLs once deployed:**

| Page | URL |
|---|---|
| Homepage | https://theliftedsociety.org |
| From Salary to Wealth — landing | https://theliftedsociety.org/from-salary-to-wealth |
| From Salary to Wealth — thank you | https://theliftedsociety.org/from-salary-to-wealth/thank-you |
| How Great Men Fall — landing | https://theliftedsociety.org/how-great-men-fall |
| How Great Men Fall — thank you | https://theliftedsociety.org/how-great-men-fall/thank-you |
| The Sin That Ends Destinies — landing | https://theliftedsociety.org/the-sin-that-ends-destinies |
| The Sin That Ends Destinies — thank you | https://theliftedsociety.org/the-sin-that-ends-destinies/thank-you |
| The Dream Killer — landing | https://theliftedsociety.org/the-dream-killer |
| The Dream Killer — thank you | https://theliftedsociety.org/the-dream-killer/thank-you |

---

## STATUS — what is done and what is left

### From Salary to Wealth
- Facebook Pixel `2895366437482752`: set
- Paystack link: set (`https://paystack.shop/pay/from-salary-to-wealth-ebook`)
- R2 download links: still to be set (see "Common edits" if not yet done)

### How Great Men Fall / The Sin That Ends Destinies / The Dream Killer
- Facebook Pixel `2895366437482752`: set on all landing and thank you pages
- Paystack links: set, each page pointing to its own product:
  - `https://paystack.shop/pay/how-great-men-fall`
  - `https://paystack.shop/pay/the-sin-that-ends-destinies`
  - `https://paystack.shop/pay/the-dream-killer`
- R2 download links: set on all three thank you pages
- Price: GHS 45
- **Outstanding:** Paystack Success Redirect URLs (see "Paystack storefront setup"), homepage links (see "Adding the books to the homepage"), and confirming the 6 book files are uploaded to R2.

---

## FACEBOOK PIXEL

Pixel ID `2895366437482752` is embedded in every landing page and every thank you page. No action needed.

The homepage does not carry a Pixel and does not need one. Only landing pages and thank you pages track conversions:
- Landing pages fire `PageView` on load and `InitiateCheckout` when an order button is clicked.
- Thank you pages fire `PageView` and `Purchase` on load.

---

## PAYSTACK STOREFRONT SETUP

For each book, Paystack must redirect to that book's thank you page after a successful payment. This is what triggers the thank you page and fires the Pixel `Purchase` event.

For each of the four products:

1. Log into the Paystack Dashboard
2. Go to Products → the product
3. Edit the product
4. Find the Success Redirect URL field
5. Set it to that book's thank you page:

| Product | Success Redirect URL |
|---|---|
| From Salary to Wealth | https://theliftedsociety.org/from-salary-to-wealth/thank-you |
| How Great Men Fall | https://theliftedsociety.org/how-great-men-fall/thank-you |
| The Sin That Ends Destinies | https://theliftedsociety.org/the-sin-that-ends-destinies/thank-you |
| The Dream Killer | https://theliftedsociety.org/the-dream-killer/thank-you |

6. Save changes

If a buyer pays but lands on Paystack's default page instead of your thank you page, this URL is missing or wrong.

---

## CLOUDFLARE R2 SETUP (file hosting for the books)

All book files are hosted on Cloudflare R2 and served from `files.theliftedsociety.org`.

### The files

There are **eight** files in the bucket: a PDF and an EPUB for each of the four books.

| Book | PDF | EPUB |
|---|---|---|
| From Salary to Wealth | From_Salary_to_Wealth.pdf | From_Salary_to_Wealth.epub |
| How Great Men Fall | How Great Men Fall - The Lifted Society.pdf | How Great Men Fall - The Lifted Society.epub |
| The Sin That Ends Destinies | The Sin That Ends Destinies - The Lifted Society.pdf | The Sin That Ends Destinies - The Lifted Society.epub |
| The Dream Killer | The Dream Killer - The Lifted Society.pdf | The Dream Killer - The Lifted Society.epub |

Filenames are case-sensitive and spaces matter. The thank you pages link to these exact names (spaces encoded as `%20`). If a download 404s, a filename mismatch is the first thing to check.

### Bucket setup (one-time)

1. Cloudflare Dashboard → R2
2. Open the `the-lifted-society-books` bucket (or create it)
3. Click Upload, drag in all eight files
4. Bucket → Settings → Custom Domains → connect `files.theliftedsociety.org`
5. Wait 1-5 minutes for the SSL certificate

### Why R2

- Direct download, no "sign in to Google" interruption
- Branded URL on your own domain
- Fast downloads via Cloudflare's edge
- Zero egress fees regardless of how many people download
- Replace a file in place (same name) to push a book update without changing the site

### Updating a book later

Upload the new file to R2 with the **same filename**. It overwrites in place and every existing link keeps working. No website change needed.

---

## DEPLOYMENT TO CLOUDFLARE PAGES

If the site is already live (the wealth book is deployed), you are only **adding three folders**, see "Adding a new book set" below. The full first-time deployment steps are kept here for reference.

### First-time deployment

1. **GitHub:** create the `the-lifted-society-web` repository, upload the contents of this folder (`index.html`, the book folders, `shared/`), commit.
2. **Cloudflare account:** sign up at cloudflare.com, verify email.
3. **Add the domain:** Cloudflare dashboard → Add a Site → `theliftedsociety.org` → Free plan → let it scan DNS → copy the two nameservers.
4. **Namecheap nameservers:** Namecheap → Domain List → Manage `theliftedsociety.org` → Nameservers → Custom DNS → paste Cloudflare's two nameservers → save.
5. **Cloudflare Pages project:** Workers & Pages → Create application → Pages → Connect to Git → select the repo → Begin setup → project name `the-lifted-society-web`, production branch `main`, leave all build settings blank → Save and Deploy.
6. **Custom domain:** in the Pages project → Custom domains → add `theliftedsociety.org` and `www.theliftedsociety.org`, wait for SSL.

### Adding a new book set (when the site is already live)

The three new books are added without disturbing the wealth book:

1. Go to the `the-lifted-society-web` repo on github.com
2. Add file → Upload files
3. Drag in the three folders: `how-great-men-fall`, `the-sin-that-ends-destinies`, `the-dream-killer`. (Do not re-upload `shared/` — leave the existing one as is.)
4. Commit changes
5. Cloudflare Pages auto-redeploys in 30-60 seconds
6. Set the three Paystack Success Redirect URLs (see "Paystack storefront setup")
7. Leave the homepage as is for now. The three titles are an ad-only test; once a winner emerges, add only that one to the homepage (see "Adding the books to the homepage")

---

## ADDING THE BOOKS TO THE HOMEPAGE

`index.html` is the homepage.

**The three test titles are deliberately NOT on the homepage.** *How Great Men Fall*, *The Sin That Ends Destinies*, and *The Dream Killer* are the same book being tested under three titles. Putting all three on the homepage would confuse visitors and split attention. They run as ad-only landing pages, reachable by direct link, which is all the Facebook ads need.

Once the title test has a clear winner (lowest cost per purchase), add **only that one winning title** to the homepage as the next book, alongside *From Salary to Wealth*. The two losing pages can then be retired.

To add the winning book to the homepage, duplicate the existing book card/link block in `index.html` and update three things: the link (e.g. `/the-dream-killer/`), the cover image, and the title. The winning book also needs a small cover image in `shared/` for the homepage, matching how the wealth book uses `shared/cover-small.png`.

---

## TESTING BEFORE YOU RUN ADS

Run these for **each** book before spending on ads for it.

### Test 1: Pixel is firing
1. Install the Facebook Pixel Helper Chrome extension
2. Visit the book's landing page → should show 1 pixel, `PageView` fired
3. Click an order button → should fire `InitiateCheckout`
4. Visit the book's thank you page directly → should fire `PageView` and `Purchase`

### Test 2: The full purchase flow works end-to-end
1. Click an order button → you should arrive at the correct Paystack product
2. Complete a small test purchase (refundable from the Paystack dashboard)
3. You should be redirected to that book's thank you page
4. Click both download buttons → the correct PDF and EPUB should download

### Test 3: The site looks right on phones
Most buyers are on phones. Open the landing page and thank you page on a phone and check:
- Cover image loads quickly
- Headlines are readable, no awkward line breaks
- Buttons are tappable, centered, and the shine animation runs
- Sticky mobile order bar appears after scrolling past the hero
- Checkmark animation plays on the thank you page

---

## EDITING THE SITE AFTER LAUNCH

All edits happen on github.com in the browser. No terminal needed.

1. Open the repo on github.com
2. Click the file to edit
3. Click the pencil icon
4. Make changes
5. Commit changes at the bottom
6. Cloudflare auto-deploys in 30-60 seconds

### Common edits

**Change a book's price:** find `GHS 45` (or `GHS&nbsp;45`) in that book's `index.html` and replace it. It appears in several places per page.

**Change a Paystack link:** find the Paystack URL in that book's `index.html` and replace every instance.

**Update an R2 download link:** edit that book's `thank-you/index.html`, update the `files.theliftedsociety.org` URLs.

**Add another book:** duplicate a book folder, rename it, swap the content, cover, links and price, then link it from the homepage.

---

## WHAT'S BUILT IN

### Landing page conversion features
- Multiple order buttons at strategic decision moments, plus a sticky mobile order bar that appears after the hero
- Rounded, glowing primary buttons with a periodic shine sweep and a subtle pulse
- "Who this book is for" section covering each reader the book speaks to
- "Read this one together" section for couples and married readers
- Honest cost breakdown and a clear "what's inside" section
- "What happens after you order" 3-step reinforcement
- Cover image leads the hero on mobile
- Single-path conversion: no nav menu, minimal footer

### Thank you page features
- Animated checkmark on load
- Prominent PDF and EPUB download buttons with file guidance
- Facebook Pixel `Purchase` event fires on load
- Hidden from search engines (`noindex, nofollow`)
- Reading guidance and a support email for download issues

### Design
- Each book carries its own colour world and layout, chosen for its subject (no shared house "look" beyond a base reset). The three sexual-integrity books use a deep navy / dawn-light / terracotta palette.
- *From Salary to Wealth* uses its own deep green / gold / cream world.
- Fully responsive, mobile-first, editorial publisher feel.

### SEO
- Proper meta titles and descriptions
- Clean semantic HTML
- Thank you pages hidden from search engines

---

## TROUBLESHOOTING

**The site isn't loading at theliftedsociety.org**
DNS propagation can take up to 24 hours. Check Cloudflare → DNS that nameservers are active.

**An order button doesn't go anywhere**
The Paystack link in that book's `index.html` is missing or wrong.

**Pixel isn't firing**
Re-check the Pixel block in the affected page's `<head>`.

**Buyer paid but landed on Paystack's default page, not the thank you page**
That product's Success Redirect URL is not set in the Paystack dashboard.

**Download button gives an error or a blank page**
- The R2 bucket's custom domain may not be fully provisioned yet (wait 5-10 minutes).
- The file URL in the thank you page does not match the actual filename in R2 (case-sensitive, spaces matter).
- The bucket is not connected to its custom domain.

**Changes aren't showing up**
Wait 60 seconds for Cloudflare auto-deploy, then hard refresh (Ctrl+Shift+R / Cmd+Shift+R).

---

Built with care for The Lifted Society.
