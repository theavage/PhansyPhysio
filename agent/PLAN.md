# PhansyPhysio website — build plan

This is the source of truth for getting this static site live on a custom domain via GitHub Pages. Read this file before doing any work in this repo. Update the checkboxes and the **Status log** at the bottom as steps complete — this doc should always reflect current reality, not just the original plan.

Starting point: informational static site for **Nesttuntorget Fysioterapi** (a physiotherapy clinic in Bergen, Norway), plain HTML/CSS/JS (no build step, no generator), hosted on GitHub Pages, repo is [theavage/PhansyPhysio](https://github.com/theavage/PhansyPhysio). You (the user) own a domain and have the registrar username/password — you do NOT have GitHub Pages or DNS configured yet.

## Ground rules
- Claude can edit files, commit, push, and manage the GitHub repo (it's already created and connected).
- Claude cannot log into the domain registrar or enter its credentials — any registrar/DNS-dashboard step is done by the user, with Claude providing the exact values to enter.
- Every step that changes DNS, repo settings, or goes live should be called out clearly when reached, not silently assumed done.
- **The site must be updatable by a non-technical person** (clinic staff, not a developer) — plain hand-edited HTML is not an acceptable end state for content that changes regularly (opening hours, staff, offered treatments, gallery photos). See Phase 1a below.

---

## Phase 1 — Site content & structure
Site: informational site for Nesttuntorget Fysioterapi, in Norwegian. Three pages:
- **Hjem** (`index.html`): who they are (centrally located at Nesttuntorget), prominent early mention that all therapists have avtale med Bergen kommune og HELFO, a "Vi tilbyr" section (Lymfødembehandling, Allmenn fysioterapi, treningsfasiliteter, +), and a photo gallery at the bottom (user uploads real photos later).
- **Våre fysioterapeuter** (`fysioterapeuter.html`): 3 sections, each with name, e-post, telefon, and a medium-length background/specialization description.
- **Kontakt oss** (`kontakt.html`): address, opening hours, phone number, embedded map.

Design: simple, stylish, calm/trustworthy palette suited to a physiotherapy clinic (not a copy of any reference site).

- [x] Site structure decided: 3 pages (see above), shared nav/footer duplicated per page (no build step, so simplest for non-technical edits later).
- [x] Scaffold files: `index.html`, `fysioterapeuter.html`, `kontakt.html`, `css/style.css`, `assets/` (images placeholders).
- [x] Keep it framework-free static HTML/CSS/JS — no npm build step, so GitHub Pages can serve the repo as-is.
- [x] Real address (Nesttunvegen 98, 5221 Nesttun) and phone (55 13 34 83) added from user + confirmed via public listings. Real "vi tilbyr"/fagområder list added: Lymfødembehandling, Allmenn fysioterapi, Nevrologisk fysioterapi, Treningsfasiliteter.
- [ ] Still placeholder, needs real info: e-post, åpningstider, the 3 therapists' names/emails/phones/bios, real photos (gallery, staff, hero) to replace the Unsplash stock photos currently in place.
- [x] Commit and push scaffold to `main`.

## Phase 1a — Non-technical content editing (required)
Plain hand-edited HTML/CSS files are not something clinic staff should have to touch directly.

**Decision (2026-08-14): skip a form-based CMS for now.** A git-based CMS (Decap CMS) was considered — it would give a proper `/admin` form editor, but its GitHub login requires a small server-side OAuth proxy, which means creating an account on an extra platform (Cloudflare/Netlify) purely to host that proxy. User chose to skip that extra infrastructure for now.

**Chosen approach: GitHub's built-in web file editor + a written guide.**
- [x] Wrote [`REDIGERING.md`](../REDIGERING.md) (Norwegian, staff-facing) — step-by-step for editing text directly on github.com, what's safe to touch vs. not, a lookup table of common edits, and how to recover from mistakes via commit history.
- [ ] User needs to invite each staff member as a GitHub **Collaborator** (repo Settings → Collaborators) so they have their own login rather than sharing the owner's credentials.
- [ ] Revisit Decap CMS later if the GitHub-editor approach proves too error-prone day-to-day (guide already flags this as an option).

## Phase 2 — Turn on GitHub Pages (get the `github.io` URL working first)
- [ ] In the repo on GitHub: **Settings → Pages**.
- [ ] Source: **Deploy from a branch** → branch `main`, folder `/ (root)`.
- [ ] Save, wait ~1 min for first build, then confirm the site loads at `https://theavage.github.io/PhansyPhysio/`.
- [ ] Do NOT touch the custom domain until this default URL works — it isolates DNS problems from Pages/build problems.

## Phase 3 — Point the custom domain at GitHub Pages (user action + Claude action)
- [ ] Decide canonical domain form: apex (`example.com`) or `www.example.com`. GitHub Pages supports either as primary with the other redirecting to it — recommend apex as primary, `www` redirecting, unless you have a reason to prefer `www`.
- [ ] Claude adds a `CNAME` file to the repo root containing the chosen domain (GitHub also auto-creates this if you type the domain into Settings → Pages → "Custom domain" — either path works, doing it via the UI is simplest since it also kicks off domain verification).
- [ ] **User step, at the registrar dashboard** — add DNS records:
  - **Apex domain** (`example.com`): four `A` records, all pointing to:
    - `185.199.108.153`
    - `185.199.109.153`
    - `185.199.110.153`
    - `185.199.111.153`
    - (optional, IPv6) `AAAA` records: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
  - **`www` subdomain**: one `CNAME` record → `theavage.github.io`
  - Remove/replace any conflicting existing `A`/`CNAME` records on the same host the registrar may have defaulted to (e.g. parking page records).
- [ ] **User step (recommended, prevents domain takeover)**: GitHub Pages → Settings → Pages → "Add a domain verification TXT record" flow; add the given `TXT` record at the registrar, then click verify on GitHub.
- [ ] Wait for DNS propagation (usually minutes, can take up to 24h). Check with `dig +short example.com` / `dig +short www.example.com` and compare against the values above.

## Phase 4 — Finalize on GitHub
- [ ] Settings → Pages should show the custom domain as verified, with a green check and no DNS warning.
- [ ] Enable **"Enforce HTTPS"** once the checkbox becomes available (GitHub auto-issues the cert after DNS is correctly pointed — this can take a while after DNS first resolves).
- [ ] Load the site at the real domain over `https://`, confirm the padlock/cert is valid and content renders.

## Phase 5 — Ongoing
- [ ] Future changes: edit files locally, commit, push to `main` — GitHub Pages rebuilds automatically, no extra deploy step needed.
- [ ] Keep this PLAN.md's checkboxes and status log current as work happens across sessions.

---

## Status log
_(most recent first — one line per session/change)_

- 2026-08-14 — Real content added: address (Nesttunvegen 98, 5221 Nesttun — confirmed via public business listings, incl. that the clinic dates back to 1983), phone (55 13 34 83), and fagområder (Lymfødem, Allmennfysioterapi, Nevrologisk fysioterapi, Treningsfasiliteter) from the user. Expanded "Om oss" copy on the homepage using these facts. Switched heading font to Plus Jakarta Sans (body stays Inter) for a more modern look. Added Unsplash stock photos (free license, no attribution required) throughout — hero image, 6-photo gallery, 3 staff portraits, kontakt page photo — all clearly marked in code comments as placeholders to swap for real photos later. Map on kontakt.html now geocodes the real address. Decided Phase 1a: skip Decap CMS (would need an extra OAuth-proxy account), wrote `REDIGERING.md` staff guide for editing via GitHub's web UI instead. NOTE: discovered Nesttuntorget Fysioterapi is a real, existing clinic with its own live site at nesttuntorgetfysioterapi.no — flagged to user, not yet addressed how this new site relates to it.
- 2026-08-14 — Phase 1 scaffold built: `index.html` (Hjem), `fysioterapeuter.html` (Våre fysioterapeuter), `kontakt.html` (Kontakt oss), `css/style.css`, `assets/gallery/`, `assets/staff/`. Verified visually in browser (desktop + mobile) via local static server. Content is placeholder pending real business details (address, phone, email, opening hours, therapist bios, photos). Added Phase 1a (non-technical editing) requirement to this plan. Not yet on GitHub Pages, no domain/DNS set up.
- 2026-08-14 — Repo `theavage/PhansyPhysio` created, initial commit (README) pushed. Plan file created.
