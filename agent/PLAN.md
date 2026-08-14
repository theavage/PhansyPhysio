# PhansyPhysio website — build plan

This is the source of truth for getting this static site live on a custom domain via GitHub Pages. Read this file before doing any work in this repo. Update the checkboxes and the **Status log** at the bottom as steps complete — this doc should always reflect current reality, not just the original plan.

Starting point: informational static site for **Nesttuntorget Fysioterapi** (a physiotherapy clinic in Bergen, Norway), plain HTML/CSS/JS (no build step, no generator), hosted on GitHub Pages, repo is [theavage/PhansyPhysio](https://github.com/theavage/PhansyPhysio). You (the user) own a domain and have the registrar username/password — you do NOT have GitHub Pages or DNS configured yet. This new site replaces the clinic's old one (nesttuntorgetfysioterapi.no) — ownership has changed and the previous owner (Anne Kvamsdal) is no longer involved, so no owner name is used anywhere on the site.

**Content policy: no visible faces in stock imagery.** Photos used as placeholders must not show identifiable faces (crop to hands/backs/equipment instead); anywhere a specific person needs representing (e.g. staff avatars) use a faceless illustration instead of a stock photo, so nobody mistakes a random stranger's face for a real staff member.

## Ground rules
- Claude can edit files, commit, push, and manage the GitHub repo (it's already created and connected).
- Claude cannot log into the domain registrar or enter its credentials — any registrar/DNS-dashboard step is done by the user, with Claude providing the exact values to enter.
- Every step that changes DNS, repo settings, or goes live should be called out clearly when reached, not silently assumed done.
- **The site must be updatable by a non-technical person** (clinic staff, not a developer) — plain hand-edited HTML is not an acceptable end state for content that changes regularly (opening hours, staff, offered treatments, gallery photos). See Phase 1a below.

---

## Phase 1 — Site content & structure
Site: informational site for Nesttuntorget Fysioterapi, in Norwegian. Four pages:
- **Hjem** (`index.html`): who they are (centrally located at Nesttuntorget), a single trust badge ("Terapeuter med offentlig driftsavtale"), "Om oss", a separate "Hvordan komme deg til oss" section (parkering + kollektivtransport), a "Vi tilbyr" section (Lymfødembehandling, Allmenn fysioterapi, Nevrologisk fysioterapi, Treningsfasiliteter), and a photo gallery at the bottom.
- **Våre fysioterapeuter** (`fysioterapeuter.html`): 3 cards, each with name, e-post, telefon, and a medium-length background/specialization description. Avatars are a faceless illustrated placeholder (inline SVG), not a stock photo.
- **Spørsmål** (`sporsmal.html`): FAQ page (topic list inspired by rosenkrantz.no/spørsmål but original content) — henvisning, egenandel/frikort, avbestilling, pasientreiser, sykmelding, første time.
- **Kontakt oss** (`kontakt.html`): address, opening hours, phone number, embedded map.

Design: simple, stylish, calm/trustworthy palette suited to a physiotherapy clinic (not a copy of any reference site).

- [x] Site structure decided: 3 pages (see above), shared nav/footer duplicated per page (no build step, so simplest for non-technical edits later).
- [x] Scaffold files: `index.html`, `fysioterapeuter.html`, `kontakt.html`, `css/style.css`, `assets/` (images placeholders).
- [x] Keep it framework-free static HTML/CSS/JS — no npm build step, so GitHub Pages can serve the repo as-is.
- [x] Real address (Nesttunvegen 98, 5221 Nesttun) and phone (55 13 34 83) added from user + confirmed via public listings. Real "vi tilbyr"/fagområder list added: Lymfødembehandling, Allmenn fysioterapi, Nevrologisk fysioterapi, Treningsfasiliteter.
- [ ] Still placeholder, needs real info: e-post, åpningstider, the 3 therapists' names/emails/phones/bios, real photos (gallery, staff, hero), avbestillingsfrist/priser and sykmelding answer on `sporsmal.html`.
- [x] Commit and push scaffold to `main`.

## Phase 1a — Non-technical content editing (required)
Plain hand-edited HTML/CSS files are not something clinic staff should have to touch directly.

**Decision (2026-08-14): skip a form-based CMS for now.** A git-based CMS (Decap CMS) was considered — it would give a proper `/admin` form editor, but its GitHub login requires a small server-side OAuth proxy, which means creating an account on an extra platform (Cloudflare/Netlify) purely to host that proxy. User chose to skip that extra infrastructure for now.

**Chosen approach: GitHub's built-in web file editor + a written guide.**
- [x] Wrote [`REDIGERING.md`](../REDIGERING.md) (Norwegian, staff-facing) — step-by-step for editing text directly on github.com, what's safe to touch vs. not, a lookup table of common edits, and how to recover from mistakes via commit history.
- [ ] User needs to invite each staff member as a GitHub **Collaborator** (repo Settings → Collaborators) so they have their own login rather than sharing the owner's credentials.
- [ ] Revisit Decap CMS later if the GitHub-editor approach proves too error-prone day-to-day (guide already flags this as an option).

## Phase 2 — Turn on GitHub Pages (get the `github.io` URL working first)
- [x] Enabled via `gh api repos/theavage/PhansyPhysio/pages` (source: branch `main`, path `/`).
- [x] First build completed (status: `built`).
- [x] Confirmed live at `https://theavage.github.io/PhansyPhysio/` — homepage and `sporsmal.html` both verified rendering correctly.
- [x] Custom domain not touched yet — correctly isolating DNS problems from Pages/build problems.

## Phase 3 — Point the custom domain at GitHub Pages (user action + Claude action)
- [x] Domain confirmed: **nesttuntorgetfysioterapi.no** (apex, primary — no `www`).
- [x] `CNAME` file added to repo root containing `nesttuntorgetfysioterapi.no`.
- [x] Custom domain set via `gh api repos/theavage/PhansyPhysio/pages -X PUT -f cname=nesttuntorgetfysioterapi.no`. Status currently shows `"errored"` — this is expected/normal until DNS is pointed at GitHub (see next step), not a real problem.
- [ ] **User step, at the registrar dashboard** — add DNS records:
  - **Apex domain** (`nesttuntorgetfysioterapi.no`): four `A` records, all pointing to:
    - `185.199.108.153`
    - `185.199.109.153`
    - `185.199.110.153`
    - `185.199.111.153`
    - (optional, IPv6) `AAAA` records: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
  - **`www` subdomain** (optional, only if you also want `www.nesttuntorgetfysioterapi.no` to work and redirect to the apex): one `CNAME` record → `theavage.github.io`
  - Remove/replace any conflicting existing `A`/`CNAME` records on the same host (e.g. records currently pointing at the old WordPress install).
- [ ] Wait for DNS propagation (usually minutes, can take up to 24h). Check with `dig +short nesttuntorgetfysioterapi.no` and compare against the IPs above.
- [ ] Once DNS resolves, recheck `gh api repos/theavage/PhansyPhysio/pages` — status should move off `"errored"`, and GitHub Pages → Settings → Pages should show the domain verified (possibly with a one-time TXT-record domain-ownership-verification step surfaced there — that flow is web-UI-driven, so check there if prompted).

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

- 2026-08-14 — Phase 3 started: domain confirmed as nesttuntorgetfysioterapi.no (apex). Added `CNAME` file to repo, set custom domain via GitHub API. Pages status shows `"errored"` — expected, since DNS isn't pointed at GitHub yet. Gave user the exact `A`/`AAAA`/`CNAME` records to add at their registrar; that step is on them (we don't have/use registrar credentials).
- 2026-08-14 — More fixes before Phase 3: trust badge → "Fysioterapeuter med offentlig driftsavtale". Replaced the hero photo (was a clinical hands/foot close-up that read as massage-focused) with a resistance-band guided-training photo — better fits a clinic focused on opptrening/veiledning/forebygging rather than massage; still face-free. Reworked "Hvordan komme deg til oss": dropped the 🅿️/🚋 emoji cards, now leads with a prominent Bybanen callout ("Bybanestopp rett utenfor", styled like the hero trust badge) since the stop is right outside, with bus + parking mentioned as secondary plain text. Added real parking facts (Nesttun Parkering / Alti Nesttun, Østre Nesttunvei 16 — 2 hrs free evenings after 16:00 and weekends, paid ~15–16 kr/hr after that) — sourced from alti.no/nesttun, worth a periodic accuracy check since rates can change. Logo: checked the old domain (nesttuntorgetfysioterapi.no — it's just a default unconfigured WordPress install, no logo there) and public listings (helsesmart.no only shows a generic placeholder); the Google Images link the user sent hit a CAPTCHA wall which we don't attempt to bypass. No usable logo found — asked the user to send the file directly instead.
- 2026-08-14 — Phase 2 done: GitHub Pages enabled on `theavage/PhansyPhysio` (branch `main`, root), first build succeeded, live and verified at `https://theavage.github.io/PhansyPhysio/`. Next: Phase 3, custom domain — need the domain name and to decide apex vs. `www` before adding the `CNAME` file and DNS records.
- 2026-08-14 — Fixes + new content: confirmed this site replaces the old one (ownership changed, no owner name used). Removed all stock photos with visible faces (hero + one gallery photo) and replaced with face-free alternatives; staff avatars switched from stock photos to a generic faceless SVG illustration. Merged the two hero trust badges into one ("Terapeuter med offentlig driftsavtale"). Added a new "Hvordan komme deg til oss" section on the homepage (parkering + kollektivtransport, kept generic/safe since exact parking details weren't confirmed). Added `sporsmal.html` (Spørsmål/FAQ page, original content, topic list inspired by rosenkrantz.no/spørsmål) and wired it into nav/footer on all pages.
- 2026-08-14 — Real content added: address (Nesttunvegen 98, 5221 Nesttun — confirmed via public business listings, incl. that the clinic dates back to 1983), phone (55 13 34 83), and fagområder (Lymfødem, Allmennfysioterapi, Nevrologisk fysioterapi, Treningsfasiliteter) from the user. Expanded "Om oss" copy on the homepage using these facts. Switched heading font to Plus Jakarta Sans (body stays Inter) for a more modern look. Added Unsplash stock photos (free license, no attribution required) throughout — hero image, 6-photo gallery, 3 staff portraits, kontakt page photo — all clearly marked in code comments as placeholders to swap for real photos later. Map on kontakt.html now geocodes the real address. Decided Phase 1a: skip Decap CMS (would need an extra OAuth-proxy account), wrote `REDIGERING.md` staff guide for editing via GitHub's web UI instead. NOTE: discovered Nesttuntorget Fysioterapi is a real, existing clinic with its own live site at nesttuntorgetfysioterapi.no — flagged to user, not yet addressed how this new site relates to it.
- 2026-08-14 — Phase 1 scaffold built: `index.html` (Hjem), `fysioterapeuter.html` (Våre fysioterapeuter), `kontakt.html` (Kontakt oss), `css/style.css`, `assets/gallery/`, `assets/staff/`. Verified visually in browser (desktop + mobile) via local static server. Content is placeholder pending real business details (address, phone, email, opening hours, therapist bios, photos). Added Phase 1a (non-technical editing) requirement to this plan. Not yet on GitHub Pages, no domain/DNS set up.
- 2026-08-14 — Repo `theavage/PhansyPhysio` created, initial commit (README) pushed. Plan file created.
