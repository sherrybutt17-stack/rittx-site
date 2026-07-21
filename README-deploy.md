# RITTX LLC — website

Static site. No build step, no dependencies, no server-side code. Every page is
plain HTML sharing one stylesheet.

## Files

| File | Purpose |
|---|---|
| `index.html` | Home |
| `about.html`, `how-it-works.html`, `contact.html` | Main pages |
| `terms-and-conditions.html`, `privacy-policy.html` | Legal pages |
| `thank-you.html` | Shown after the contact form is submitted (`noindex`) |
| `style.css` | Entire stylesheet, driven by CSS variables in `:root` |
| `assets/` | Logo wordmark and favicon |
| `CNAME` | Custom domain for GitHub Pages |
| `.nojekyll` | Stops GitHub running the files through Jekyll |

## Deploying to GitHub Pages

```bash
cd "/Users/shaheerbutt/Rittx LLC"
git init -b main
git add .
git commit -m "RITTX LLC website"
git remote add origin https://github.com/<your-user>/<your-repo>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch → `main` / `root`.**

### Pointing rittx.com at it

`CNAME` already contains `rittx.com`. At your DNS provider add:

| Type | Name | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `<your-user>.github.io` |

DNS takes up to an hour. Once it resolves, tick **Enforce HTTPS** in Settings → Pages.

**This replaces the current WordPress site.** Before switching DNS, be aware the
existing WordPress URLs (`/about-us/`, `/pricing/`, `/blog/`, `/lead-gen-strategies/`
and others) will stop existing — this site uses `.html` filenames. Anything
linking to the old URLs, including search results, will 404. Worth exporting
anything you want to keep from WordPress first.

## The contact form does not send anything

**This is deliberate but temporary.** The form on `contact.html` validates in the
browser and then redirects to `thank-you.html`. Nothing is transmitted, emailed,
or stored. A visitor who asks for a cash offer gets a thank-you page, and you
never find out they asked.

GitHub Pages cannot run server-side code, so a form needs an external handler.
When you're ready, the wiring is small — every field already has a correct `name`
attribute:

- **GoHighLevel** — build a form in GHL and swap in its embed iframe, or create a
  Workflow with an Inbound Webhook trigger and `fetch()` the payload to it. Leads
  land in your CRM with automations attached.
- **Formspree / Web3Forms** — set `method="post"` and `action="<endpoint>"` on the
  `<form>`, then delete the `window.location.href` line in the inline script at
  the bottom of `contact.html`. Submissions arrive by email.

Until one of those is connected, make sure the phone number and email address on
the contact page are being monitored, since they are the only working routes in.

## Editing

Colours live in one place — the `:root` block at the top of `style.css`:

```css
--navy:      #214a69;   /* headers, hero, footer */
--navy-mid:  #2b5f88;
--gold:      #faa21f;   /* buttons, accents */
--gold-text: #a3660a;   /* amber text on white, darkened for legibility */
--on-gold:   #14344a;   /* text on amber fills */
```

The header and footer are duplicated in each HTML file. Change one, change all
seven, or they will drift out of sync.
