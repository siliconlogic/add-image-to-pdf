# Add Image to PDF — Website

Marketing, welcome, uninstall, and privacy pages for the **Add Image to PDF** Chrome extension. Hosted on GitHub Pages at **[add-image-to-pdf.xyz](https://add-image-to-pdf.xyz)**.

## Structure

```
/
├── index.html          ← public home page (Install the extension)
├── welcome.html        ← shown after install — pin & launch guide
├── uninstall.html      ← shown when the user uninstalls (feedback form)
├── privacy.html        ← privacy policy
├── 404.html
├── CNAME               ← custom domain (add-image-to-pdf.xyz)
├── .nojekyll           ← disables Jekyll processing on GitHub Pages
├── assets/
│   └── style.css
└── images/
    ├── icon-64.png
    ├── favicon-{16,32,180}.png
    └── screeshot{1,2}.png    ← used on welcome.html
```

## Before going live — checklist

1. **Replace remaining placeholders** (search the repo for these strings):
   - `https://chromewebstore.google.com/` → your actual Chrome Web Store listing URL
   - `YOUR_FORM_ID` in `uninstall.html` → your Formspree form ID

2. **Set up Formspree** (powers the uninstall feedback form):
   - Sign up at [formspree.io](https://formspree.io) (free tier covers 50 submissions/month)
   - Create a form, copy its endpoint
   - Replace `FORMSPREE_ENDPOINT` in `uninstall.html`

3. **Push to GitHub & enable Pages:**
   - Create a public repo and push this folder
   - Repo Settings → Pages → Source: `main` branch, `/` root
   - The `CNAME` file pre-fills the custom domain

4. **DNS records** at your registrar for `add-image-to-pdf.xyz`:

   Apex (A records → GitHub Pages):
   ```
   A  @  185.199.108.153
   A  @  185.199.109.153
   A  @  185.199.110.153
   A  @  185.199.111.153
   ```

   Optional IPv6 (AAAA records):
   ```
   AAAA  @  2606:50c0:8000::153
   AAAA  @  2606:50c0:8001::153
   AAAA  @  2606:50c0:8002::153
   AAAA  @  2606:50c0:8003::153
   ```

   www subdomain (CNAME):
   ```
   CNAME  www  <your-gh-username>.github.io
   ```

5. **After DNS propagates**, in Settings → Pages tick **Enforce HTTPS**.

## Extension integration

In your extension's background service worker:

```js
// background.js (Manifest V3)

chrome.runtime.onInstalled.addListener((details) => {
  if (details.reason === 'install') {
    chrome.tabs.create({ url: 'https://add-image-to-pdf.xyz/welcome.html' });
  }
});

chrome.runtime.setUninstallURL('https://add-image-to-pdf.xyz/uninstall.html');
```

And in `manifest.json`:

```json
{
  "manifest_version": 3,
  "homepage_url": "https://add-image-to-pdf.xyz/",
  "background": { "service_worker": "background.js" }
}
```

## Local preview

```bash
# Any static server will do
python3 -m http.server 8080
# Then open http://localhost:8080
```

## License

© 2026 Add Image to PDF
