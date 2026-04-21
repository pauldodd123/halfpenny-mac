# Halfpenny Mac

Landing page for a managed Apple silicon Mac mini rental business.

Static site. No build step. Hosted on GitHub Pages.

---

## What's in this repo

```
.
├── index.html           ← the landing page (single file)
├── docs/
│   ├── strategy.md      ← how to actually make money with this
│   └── lockdown.md      ← how to secure the rented Macs
├── README.md
└── LICENSE
```

---

## Deploy to GitHub Pages

1. Create a new repo on GitHub (public or private — Pages works with both on Pro).
2. Push this folder up:

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin git@github.com:YOUR_USERNAME/halfpenny-mac.git
   git push -u origin main
   ```

3. On GitHub: **Settings → Pages → Source: Deploy from a branch → `main` / root**.
4. Save. Wait 1–2 minutes. Your site will be at:
   `https://YOUR_USERNAME.github.io/halfpenny-mac/`

5. (Optional) To use a custom domain like `halfpennymac.com`:
   - Add a `CNAME` file at the repo root containing just: `halfpennymac.com`
   - Point the domain's DNS to GitHub Pages ([GitHub docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site))

---

## Rename the brand

"Halfpenny Mac" is a working name. To rename:

```bash
# macOS
grep -rl "Halfpenny Mac" . | xargs sed -i '' 's/Halfpenny Mac/Your New Name/g'
```

Then update the `<title>`, `og:title`, and logo mark styling in `index.html` if you want to swap colours.

---

## Local preview

Just open `index.html` in a browser. Or run any static server:

```bash
python3 -m http.server 8000
# visit http://localhost:8000
```

---

## Wiring up the waitlist form

The form currently just shows a thank-you message on submit. To actually capture emails, swap the `onsubmit` handler for one of:

- **Formspree** — drop-in, free tier, no backend
- **ConvertKit / Beehiiv / Mailchimp** — if you want to nurture the list
- **Your own endpoint** — if you're going to self-host the service anyway, might as well own the list

Minimal Formspree example:

```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
  <input type="email" name="email" required placeholder="you@company.com" />
  <button type="submit" class="btn">Request access →</button>
</form>
```

---

## Business docs

- **[docs/strategy.md](docs/strategy.md)** — how this makes money, who the real customers are, what to charge
- **[docs/lockdown.md](docs/lockdown.md)** — security posture for the rented Macs (important; don't skip)

---

## License

MIT — see [LICENSE](LICENSE).
