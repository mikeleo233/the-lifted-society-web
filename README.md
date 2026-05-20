# The Lifted Society — Complete Site

This folder contains the full website for The Lifted Society publisher, the *From Salary to Wealth* landing page, and the post-purchase thank you page.

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
├── shared/
│   ├── styles.css                      ← Shared design system
│   └── cover-small.png                 ← Small cover for homepage
└── README.md                           ← This file
```

**Live URLs once deployed:**

| Page | URL |
|---|---|
| Homepage | https://theliftedsociety.org |
| Landing page | https://theliftedsociety.org/from-salary-to-wealth |
| Thank you page | https://theliftedsociety.org/from-salary-to-wealth/thank-you |

---

## BEFORE YOU DEPLOY — 3 placeholders to fill in

Use Find & Replace in any text editor (Notepad, TextEdit, VS Code). The three placeholders are clearly marked.

### 1. Facebook Pixel ID — ALREADY SET

Pixel ID `2895366437482752` is already embedded in the landing page and thank you page. No action needed.

(The homepage does not have a Pixel — it does not need to. Only the landing page and thank you page track conversions.)

### 2. Paystack order link — ALREADY SET

All 5 ORDER NOW buttons (4 in-page + 1 sticky mobile bar) already point to:
`https://paystack.shop/pay/from-salary-to-wealth-ebook`

No action needed unless you change your Paystack product URL.

### 3. Cloudflare R2 download links — STILL TO BE SET

The thank you page has two download buttons, one for PDF and one for EPUB. Both still need their R2 URLs:

Find: `https://files.theliftedsociety.org/YOUR-PDF-LINK-HERE`
Replace with: e.g. `https://files.theliftedsociety.org/From_Salary_to_Wealth.pdf`

Find: `https://files.theliftedsociety.org/YOUR-EPUB-LINK-HERE`
Replace with: e.g. `https://files.theliftedsociety.org/From_Salary_to_Wealth.epub`

Both are in: `from-salary-to-wealth/thank-you/index.html`

(See "CLOUDFLARE R2 SETUP" section below for how to set up the file hosting.)

---

## PAYSTACK STOREFRONT SETUP

To make the thank you page actually trigger after payment, configure Paystack to redirect there:

1. Log into Paystack Dashboard
2. Go to Products → your *From Salary to Wealth* product
3. Edit the product
4. Find the Success Redirect URL (or similar) field
5. Set it to: `https://theliftedsociety.org/from-salary-to-wealth/thank-you`
6. Save changes

Now after every successful payment, the buyer will be taken to your thank you page, where they can download the book and where the Facebook Pixel will fire a Purchase event.

---

## CLOUDFLARE R2 SETUP (file hosting for the book)

The book PDF is hosted on Cloudflare R2 (free for your traffic level). Setup takes about 20 minutes total.

### Part 1: Create the R2 bucket (5 minutes)

1. Cloudflare Dashboard → R2 (in left sidebar under Storage)
2. Click "Create bucket"
3. Name: `the-lifted-society-books`
4. Location hint: Eastern Europe or Asia (these route fastest to Ghana)
5. Click "Create bucket"

### Part 2: Upload both files (3 minutes)

1. Open your newly created bucket
2. Click "Upload"
3. Drag and drop BOTH files:
   - `From_Salary_to_Wealth.pdf` (the PDF version with cover)
   - `From_Salary_to_Wealth.epub` (the EPUB version for phones and e-readers)
4. Done — both files are now stored

### Part 3: Make the files publicly accessible via a custom domain (10 minutes)

The cleanest setup uses a subdomain like `files.theliftedsociety.org`:

1. In your bucket → Settings → Custom Domains → Connect Domain
2. Enter `files.theliftedsociety.org`
3. Cloudflare auto-configures DNS (since your domain is in Cloudflare)
4. Wait 1-5 minutes for the SSL certificate to provision
5. Your files are now accessible at:
   - `https://files.theliftedsociety.org/From_Salary_to_Wealth.pdf`
   - `https://files.theliftedsociety.org/From_Salary_to_Wealth.epub`

### Part 4: Update the two URLs in the thank-you page (2 minutes)

1. Open `from-salary-to-wealth/thank-you/index.html` on GitHub
2. Click the pencil icon to edit
3. Find: `https://files.theliftedsociety.org/YOUR-PDF-LINK-HERE`
4. Replace with: `https://files.theliftedsociety.org/From_Salary_to_Wealth.pdf`
5. Find: `https://files.theliftedsociety.org/YOUR-EPUB-LINK-HERE`
6. Replace with: `https://files.theliftedsociety.org/From_Salary_to_Wealth.epub`
7. Commit changes — Cloudflare Pages auto-redeploys in 30 seconds

### Why R2 instead of Google Drive

- Direct PDF download (no "sign in to Google" interruption)
- Branded URL on your own domain (more trust)
- Fast downloads via Cloudflare's Accra edge node
- Zero egress fees — you'll never pay for bandwidth even if 10,000 people download
- You can replace the file in place without breaking existing links (update the book later → same URL still works)

### Updating the book later

If you ever release v2 of the book:

1. Go to your R2 bucket
2. Upload the new PDF with the SAME filename
3. It overwrites in place
4. All existing buyers who bookmarked the link will download the new version next time
5. No changes needed to the website

---

## DEPLOYMENT TO CLOUDFLARE PAGES

### Step 1: Upload files to GitHub (10 minutes)

1. Go to github.com and sign in
2. Click + top right → New repository
3. Name it `the-lifted-society-web`
4. Set to Public
5. Click Create repository
6. On the next screen, click "uploading an existing file"
7. Drag the CONTENTS of this folder (not the folder itself) into the upload area:
   - You should see `index.html`, the `from-salary-to-wealth` folder, and the `shared` folder at the top
8. Click Commit changes

### Step 2: Sign up for Cloudflare (3 minutes)

1. Go to cloudflare.com → Sign Up → create free account
2. Verify your email

### Step 3: Add theliftedsociety.org to Cloudflare (5 minutes)

1. Cloudflare dashboard → Add a Site
2. Enter `theliftedsociety.org` → Continue
3. Choose the Free plan → Continue
4. Let Cloudflare scan your existing DNS records
5. Cloudflare gives you 2 nameservers like:
   - `xxx.ns.cloudflare.com`
   - `yyy.ns.cloudflare.com`
6. Copy these two nameservers

### Step 4: Update Namecheap nameservers (5 minutes)

1. namecheap.com → sign in → Domain List → Manage `theliftedsociety.org`
2. Nameservers section → change dropdown to Custom DNS
3. Paste Cloudflare's two nameservers
4. Click the green checkmark to save

DNS propagation usually takes 15 minutes to a few hours. Continue to Step 5 while you wait.

### Step 5: Create Cloudflare Pages project (5 minutes)

1. Cloudflare dashboard → Workers & Pages
2. Create application → Pages tab → Connect to Git
3. Click Connect GitHub and authorize
4. Select your `the-lifted-society-web` repository
5. Click Begin setup
6. Configure:
   - Project name: `the-lifted-society-web`
   - Production branch: `main`
   - Build settings: LEAVE EVERYTHING BLANK (static site, no build needed)
7. Click Save and Deploy

In about 60 seconds your site is live at: `the-lifted-society-web.pages.dev`

Open that URL on your phone and computer. Click through every page. Verify everything works before you connect the custom domain.

### Step 6: Connect your custom domain (5 minutes)

1. In your Cloudflare Pages project → Custom domains → Set up a custom domain
2. Enter `theliftedsociety.org` → Continue → Activate domain
3. Wait 1-5 minutes for SSL certificate to provision

Optional but recommended: also add `www.theliftedsociety.org`
1. Set up a custom domain again → enter `www.theliftedsociety.org` → activate

Your site is now live at all three URLs listed at the top of this README.

---

## TESTING BEFORE YOU RUN ADS

Three critical tests before spending any money on ads.

### Test 1: Pixel is firing

1. Install the Facebook Pixel Helper Chrome extension
2. Visit `theliftedsociety.org/from-salary-to-wealth`
3. Pixel Helper should show: 1 pixel found, PageView event fired
4. Click any ORDER NOW button → Pixel Helper should fire InitiateCheckout
5. Visit `theliftedsociety.org/from-salary-to-wealth/thank-you` directly
6. Pixel Helper should fire both PageView and Purchase

If any of these fail, you forgot to replace `YOUR_PIXEL_ID` in one or both files.

### Test 2: The full purchase flow works end-to-end

1. Click ORDER NOW on the landing page → you should arrive at Paystack
2. Complete a small test purchase
3. You should be redirected to `theliftedsociety.org/from-salary-to-wealth/thank-you`
4. Click "Download the Book" → the book PDF should download

If the redirect doesn't happen, the Paystack Success Redirect URL is wrong. If the download fails, the R2 file URL is wrong or the bucket's custom domain hasn't fully provisioned yet.

### Test 3: The site looks right on phones

Most of your buyers will be on phones. Open all three pages on your phone:
- `theliftedsociety.org`
- `theliftedsociety.org/from-salary-to-wealth`
- `theliftedsociety.org/from-salary-to-wealth/thank-you`

Check:
- Cover image loads quickly
- Headlines are readable, no awkward line breaks
- Buttons are tappable and centered
- Sticky mobile order bar appears on the landing page after scrolling past hero
- Checkmark animation plays on the thank you page

---

## EDITING THE SITE AFTER LAUNCH

All edits happen on github.com via your browser. No terminal, no code editor needed.

1. Go to your `the-lifted-society-web` repo on github.com
2. Click the file you want to edit
3. Click the pencil icon (top right of file view)
4. Make changes in the browser
5. Scroll to bottom → Commit changes
6. Cloudflare auto-deploys in 30-60 seconds

### Common edits

Change the price: Find `GHS 45` in `from-salary-to-wealth/index.html`, replace with new price.

Change the Paystack link: Find your current Paystack URL across the file, find & replace with new URL.

Update the R2 download link: Edit `from-salary-to-wealth/thank-you/index.html`, find `files.theliftedsociety.org` line, update URL.

Add a new book: Duplicate the entire `from-salary-to-wealth/` folder, rename it, swap content, update the homepage to feature it.

---

## WHAT'S BUILT IN

### Landing page conversion features
- 4 ORDER NOW buttons at strategic decision moments
- Sticky mobile order bar that appears after scrolling past hero
- Trust badges in hero (Pay with MoMo · Instant download · Read anywhere)
- "Grounded in Reality" credibility callout
- "What happens after you order" 3-step reinforcement
- Line-art SVG icons for the chapter pillars
- Subtle pulse animation on primary buttons
- Hero shows cover image first on mobile (above headline)
- No escape links (no nav menu, no social links) — single-path conversion

### Thank you page features
- Animated checkmark on page load (emotional confirmation)
- Prominent download button
- Facebook Pixel Purchase event fires automatically on page load
- Hidden from search engines (noindex meta tag)
- "Save this page" reminder for repeat downloads
- Support email for download issues
- Soft pitch to follow socials for future books

### Design
- Cohesive brand across all pages (deep forest green, warm gold, cream)
- Cormorant Garamond serif headlines + Inter body
- Fully responsive (mobile-first)
- Editorial/publisher feel

### SEO
- Proper meta titles and descriptions
- Open Graph tags for Facebook/WhatsApp sharing
- Clean semantic HTML
- Thank you page hidden from search engines

---

## TROUBLESHOOTING

The site isn't loading at theliftedsociety.org
- DNS propagation can take up to 24 hours. Test again in 1-2 hours.
- Check Cloudflare → DNS to confirm nameservers are active.

Order Now button doesn't go anywhere
- You forgot to replace `https://paystack.shop/YOUR-LINK-HERE`.

Pixel isn't firing
- You forgot to replace `YOUR_PIXEL_ID` in one or more files.

Buyer paid but landed on Paystack's default page, not my thank you page
- Paystack Success Redirect URL is not set. Configure it in Paystack Dashboard → your product → Settings.

Download button gives an error or shows a blank page
- Your R2 bucket's custom domain may not be fully provisioned yet (wait 5-10 minutes after setup).
- The file URL in the thank-you page doesn't match the actual file name in R2 (filenames are case-sensitive).
- The bucket isn't connected to a custom domain. Re-check: R2 bucket → Settings → Custom Domains.

Changes I made aren't showing up
- Wait 60 seconds for Cloudflare auto-deploy.
- Hard refresh (Ctrl+Shift+R / Cmd+Shift+R).

---

Built with care for The Lifted Society.
