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

## Launch checklist — what's left before this goes live for real
_(Quick-reference summary — the phases below have the full detail on each item.)_

**Blocking — must happen before the real domain points here:**
- [ ] **User:** update DNS at Domeneshop (see Phase 3) — swap the `A` records to GitHub's IPs, leave `MX` alone.
- [ ] **Claude, once DNS resolves:** re-add `CNAME`, reconfigure the custom domain on GitHub Pages, verify it resolves, enable "Enforce HTTPS" (Phase 4).

**Should fill in before launch** (these currently show as bracketed `[...]` placeholders on the live site — fine for review, not for real visitors):
- [ ] E-post-adresse (shown as `[post@nesttuntorgetfysioterapi.no]` in every footer + `kontakt.html`)
- [ ] Åpningstider on `kontakt.html` (placeholder hours)
- [ ] The 3 therapists' navn, e-post, telefon og bio on `fysioterapeuter.html`
- [ ] Avbestillingsfrist/gebyr and the sykmelding answer on `sporsmal.html` (bracketed TODOs)
- [ ] Logo — still not found/sent (see status log)

**Recommended, not blocking:**
- [ ] Real photos to replace the Unsplash stock placeholders (gallery, hero, `kontakt.html`, and the therapist avatars which are currently a generic illustration)
- [ ] Invite staff as GitHub Collaborators so `REDIGERING.md` actually works for them (Phase 1a)
- [ ] Favicon — none set yet
- [ ] Optional SEO basics (sitemap.xml, robots.txt, social share/og: image tags) — not required for a small local-business site but easy to add later

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

**Q: What's the simplest way for the therapists to update the site themselves, with zero coding?**
**A: A git-based CMS (Decap CMS).** This is the actual simplest option *for the therapists* — simpler than the GitHub-editor approach currently in place, which still means carefully touching raw HTML tags. With Decap CMS set up:
- A therapist goes to `nesttuntorgetfysioterapi.no/admin`, logs in with their own GitHub account (one click, no password to manage separately).
- They see a plain form — labeled fields like "Navn", "Telefon", "Bio", and a button to upload a photo. No HTML, no tags, nothing that can visually break the page.
- Saving the form commits the change and the live site updates automatically, same as any other push — no extra step for them.

**The catch (why this isn't already built): the one-time setup, not the day-to-day use.** Decap's GitHub login needs a small server-side "OAuth proxy" — GitHub Pages only serves static files, so it can't run that login handshake itself. Setting that up means:
1. Creating a GitHub OAuth App under the `theavage` account (a few clicks, no cost).
2. Deploying a small proxy to a platform with serverless functions — e.g. a free Cloudflare account + a one-file Worker (~15–30 min, one-time). This is the step declined earlier (2026-08-14) to avoid extra accounts — flagging again now since "simplest for therapists" specifically favors this path.

Once that one-time setup is done, the therapists never see or deal with any of that — just the plain form.

**Current fallback (already built, in place today): GitHub's built-in web file editor + a written guide.**
- [x] Wrote [`REDIGERING.md`](../REDIGERING.md) (Norwegian, staff-facing) — step-by-step for editing text directly on github.com, what's safe to touch vs. not, a lookup table of common edits, and how to recover from mistakes via commit history.
- [ ] User needs to invite each staff member as a GitHub **Collaborator** (repo Settings → Collaborators) so they have their own login rather than sharing the owner's credentials.
- [ ] **Decision pending:** set up Decap CMS (simplest for therapists, needs ~15–30 min one-time setup + a free Cloudflare account) vs. stick with the GitHub-editor + guide (zero extra setup, but therapists touch raw HTML). Let Claude know which to proceed with.

## Phase 2 — Turn on GitHub Pages (get the `github.io` URL working first)
- [x] Enabled via `gh api repos/theavage/PhansyPhysio/pages` (source: branch `main`, path `/`).
- [x] First build completed (status: `built`).
- [x] Confirmed live at `https://theavage.github.io/PhansyPhysio/` — homepage and `sporsmal.html` both verified rendering correctly.
- [x] Custom domain not touched yet — correctly isolating DNS problems from Pages/build problems.

## Phase 3 — Point the custom domain at GitHub Pages (user action + Claude action)
- [x] Domain confirmed: **nesttuntorgetfysioterapi.no** (apex, primary — no `www`).
- [x] Registrar confirmed: **domeneshop.no** ("Domeneshop"), currently on their "Epost + WebStandard" package (bundled email + basic web hosting for the domain).
- [ ] **User step, Domeneshop-specific** — log in at domeneshop.no ("Mitt Domeneshop") → **Mine domener** → click `nesttuntorgetfysioterapi.no` → **DNS-innstillinger**. In the record table:
  - Find the existing `A`-record(s) for host `@` (root domain) — these currently point at Domeneshop's own WebStandard hosting server, which is why the domain shows the old WordPress site. Replace them with the 4 GitHub Pages `A` records below. If Domeneshop's UI ties the `@` `A`-record to the WebStandard package (e.g. a "bruk webhotell" toggle) and won't let you edit it directly, look for an option to switch that host to custom/manual DNS, or just add the new `A` records — Domeneshop's advanced DNS editor generally allows overriding this.
  - **Leave the `MX` record(s) alone** — that's what routes your email, and it's independent of the `A` record. Changing only the `A` record does not affect the "Epost" part of the package.
  - Click **Lagre** (Save) when done.
- [x] ~~`CNAME` file added, custom domain set via API~~ — **reverted.** Learned the hard way: as soon as GitHub Pages has a custom domain configured, it force-redirects the default `github.io` URL to that domain — even before DNS points there. Since `nesttuntorgetfysioterapi.no` still resolves to the old site (a default WordPress install) until DNS is updated, this made the new site briefly unreachable/unreviewable. Removed `CNAME` and unset `cname` via API so `https://theavage.github.io/PhansyPhysio/` works for review again in the meantime.
- [ ] **Do not re-add `CNAME`/set the custom domain until DNS is updated** (or coordinate the two closely) — that's the actual next step once the user has updated DNS.
- [ ] **User step, at the registrar dashboard** — add DNS records:
  - The 4 target `A` records (also listed below for reference):
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

- 2026-08-14 — Added a "Fjerne den oransje/kursiverte plassholder-stilen" section to `REDIGERING.md`, explaining how to drop `class="placeholder"` (as a wrapping `<span>` vs. an extra class on an existing tag) when replacing bracketed TODO content with real text. Also added `sporsmal.html` to the file list in "Slik endrer du tekst" — it was missing.
- 2026-08-14 — Answered "what's the simplest way for therapists to edit with zero coding" in Phase 1a: Decap CMS remains the actual answer (plain form, no HTML) — the earlier objection was about one-time setup cost (OAuth proxy + a free Cloudflare/Netlify account), not ongoing complexity for therapists. Documented clearly and left as a pending decision for the user.
- 2026-08-14 — Added a top-level "Launch checklist" section consolidating everything still outstanding (DNS, placeholder content, logo, nice-to-haves) in one scannable list. Fixed two small inconsistencies found while reviewing: homepage meta description still said the old badge text ("Terapeuter..." vs the actual "Fysioterapeuter..."), and therapist email placeholders in `fysioterapeuter.html` used a dash (`nesttuntorget-fysioterapi.no`) that didn't match the real domain.
- 2026-08-14 — Hero image fade pushed further per follow-up feedback: reduced opacity (0.6) and widened/softened the mask gradient so it reads as an ambient background element on the right side rather than a distinct framed photo.
- 2026-08-14 — Moved "Timebestilling" text on `kontakt.html` out of the `contact-info-card` box, now sits directly under the "Kontakt oss" heading/lead in the hero. Removed a stray comma in the parking sentence on `index.html` ("...Nesttunvei 16) mot betaling."). Domain registrar confirmed as domeneshop.no (package includes "Epost" + "WebStandard" hosting) — see Phase 3 notes for Domeneshop-specific DNS steps.
- 2026-08-14 — Small refinements: phone number reverted back to 55 13 34 83 everywhere (the 55 13 39 06 swap from the previous round was undone — old number is correct after all). Homepage hero image now has a soft radial fade (CSS `mask-image`, `.hero-image--fade` — scoped to the homepage only, so `kontakt.html`'s hero photo keeps its normal sharp card look) instead of a hard-edged photo card, per request ("fader mer inn i bakgrunnen"). Parking text on `index.html` simplified: dropped the free-evenings/weekends detail (irrelevant — that's outside opening hours) and "rett ved siden av oss", now just states parking is paid ("mot betaling").
- 2026-08-14 — Content round + Phase 3 course-correction: reverted the custom domain (see Phase 3 note above) so `theavage.github.io/PhansyPhysio` works for review again. "Hvordan komme deg til oss" renamed to "Slik finner du oss"; Bybane callout now reads "200 meter fra Nesttun sentrum bybanestopp" instead of "rett utenfor". Hero photo replaced again (2nd swap — resistance-band photo also didn't land) with a face-free, people-free shot of neatly organized training equipment on a wall; flagged to user as still-a-guess given two prior misses. Removed emoji icons + the "Våre fagområder og fasiliteter" and "Et lite innblikk..." subtitles per request. Added a "Timebestilling" block on `kontakt.html` (booking goes via calling the therapist directly, links to `fysioterapeuter.html`) and a "Tilgjengelighet" block (building has a lift, use the westmost entrance away from the Bybane track). **Phone number changed site-wide**: old 55 13 34 83 (sourced from public directories) replaced everywhere with 55 13 39 06 (given directly by the user as the "øvrige henvendelser" line) — flagged to user in case the old number should've stayed for some other purpose. E-post still placeholder.
- 2026-08-14 — Phase 3 started: domain confirmed as nesttuntorgetfysioterapi.no (apex). Added `CNAME` file to repo, set custom domain via GitHub API. Pages status shows `"errored"` — expected, since DNS isn't pointed at GitHub yet. Gave user the exact `A`/`AAAA`/`CNAME` records to add at their registrar; that step is on them (we don't have/use registrar credentials).
- 2026-08-14 — More fixes before Phase 3: trust badge → "Fysioterapeuter med offentlig driftsavtale". Replaced the hero photo (was a clinical hands/foot close-up that read as massage-focused) with a resistance-band guided-training photo — better fits a clinic focused on opptrening/veiledning/forebygging rather than massage; still face-free. Reworked "Hvordan komme deg til oss": dropped the 🅿️/🚋 emoji cards, now leads with a prominent Bybanen callout ("Bybanestopp rett utenfor", styled like the hero trust badge) since the stop is right outside, with bus + parking mentioned as secondary plain text. Added real parking facts (Nesttun Parkering / Alti Nesttun, Østre Nesttunvei 16 — 2 hrs free evenings after 16:00 and weekends, paid ~15–16 kr/hr after that) — sourced from alti.no/nesttun, worth a periodic accuracy check since rates can change. Logo: checked the old domain (nesttuntorgetfysioterapi.no — it's just a default unconfigured WordPress install, no logo there) and public listings (helsesmart.no only shows a generic placeholder); the Google Images link the user sent hit a CAPTCHA wall which we don't attempt to bypass. No usable logo found — asked the user to send the file directly instead.
- 2026-08-14 — Phase 2 done: GitHub Pages enabled on `theavage/PhansyPhysio` (branch `main`, root), first build succeeded, live and verified at `https://theavage.github.io/PhansyPhysio/`. Next: Phase 3, custom domain — need the domain name and to decide apex vs. `www` before adding the `CNAME` file and DNS records.
- 2026-08-14 — Fixes + new content: confirmed this site replaces the old one (ownership changed, no owner name used). Removed all stock photos with visible faces (hero + one gallery photo) and replaced with face-free alternatives; staff avatars switched from stock photos to a generic faceless SVG illustration. Merged the two hero trust badges into one ("Terapeuter med offentlig driftsavtale"). Added a new "Hvordan komme deg til oss" section on the homepage (parkering + kollektivtransport, kept generic/safe since exact parking details weren't confirmed). Added `sporsmal.html` (Spørsmål/FAQ page, original content, topic list inspired by rosenkrantz.no/spørsmål) and wired it into nav/footer on all pages.
- 2026-08-14 — Real content added: address (Nesttunvegen 98, 5221 Nesttun — confirmed via public business listings, incl. that the clinic dates back to 1983), phone (55 13 34 83), and fagområder (Lymfødem, Allmennfysioterapi, Nevrologisk fysioterapi, Treningsfasiliteter) from the user. Expanded "Om oss" copy on the homepage using these facts. Switched heading font to Plus Jakarta Sans (body stays Inter) for a more modern look. Added Unsplash stock photos (free license, no attribution required) throughout — hero image, 6-photo gallery, 3 staff portraits, kontakt page photo — all clearly marked in code comments as placeholders to swap for real photos later. Map on kontakt.html now geocodes the real address. Decided Phase 1a: skip Decap CMS (would need an extra OAuth-proxy account), wrote `REDIGERING.md` staff guide for editing via GitHub's web UI instead. NOTE: discovered Nesttuntorget Fysioterapi is a real, existing clinic with its own live site at nesttuntorgetfysioterapi.no — flagged to user, not yet addressed how this new site relates to it.
- 2026-08-14 — Phase 1 scaffold built: `index.html` (Hjem), `fysioterapeuter.html` (Våre fysioterapeuter), `kontakt.html` (Kontakt oss), `css/style.css`, `assets/gallery/`, `assets/staff/`. Verified visually in browser (desktop + mobile) via local static server. Content is placeholder pending real business details (address, phone, email, opening hours, therapist bios, photos). Added Phase 1a (non-technical editing) requirement to this plan. Not yet on GitHub Pages, no domain/DNS set up.
- 2026-08-14 — Repo `theavage/PhansyPhysio` created, initial commit (README) pushed. Plan file created.
