# Off-Wix migration plan (ISS-009 / ISS-033)

Status: LAUNCHED 2026-10-03. Phases 0-2 done; Phase 3 remains. Revised 2026-09-21.

Wix does not allow nameserver changes on a domain registered with Wix, so the
earlier "move DNS to Cloudflare" phase is gone. The site launches by editing
two kinds of record inside Wix DNS (apex A, `www` CNAME). Email records are
never touched. The old Wix site stays published as a rollback target.

The one thing that must not break: Google Workspace MX records (company email).

## Phase 0 — Prep (zero risk)
- [x] Screenshot / export every DNS record in Wix's DNS manager (MX and
      priorities, SPF TXT, `_dmarc` CNAME, verification TXTs, A, CNAME).
      This is the rollback sheet. (2026-10-02)
- [x] CWG-owned accounts: GitHub organization, Cloudflare (Worker + Turnstile
      only, not DNS), Postmark. Two admins each.
- [x] Transfer this repo into the organization and rename it. (2026-10-02)

## Phase 1 — Contact form (can ship on the draft site)
- [x] Postmark: verify the caringwithgrace.com DOMAIN (DKIM TXT + custom
      Return-Path CNAME, both added in Wix DNS). Leave the apex SPF alone.
- [x] Cloudflare Turnstile widget; hostnames = draft host + production hosts.
- [x] From workers/contact-form/: `wrangler login`,
      `wrangler secret put POSTMARK_SERVER_TOKEN`,
      `wrangler secret put TURNSTILE_SECRET`, `wrangler deploy`.
      ALLOWED_ORIGINS must include the organization's github.io origin.
- [x] Set CONTACT_FORM_ENDPOINT and TURNSTILE_SITE_KEY in assets/js/main.js,
      bump the `?v=` on main.js, push.
- [x] Test with TO_EMAIL pointed at the implementer. In Gmail "Show original":
      DKIM, SPF and DMARC all PASS; Reply-To is the visitor.
- [x] Flip TO_EMAIL to the intake inbox; confirm inbox delivery with no filter.
      (2026-09-22)

## Phase 2 — Site cutover (needs team go-ahead; the public launch)
Done 2026-10-03, 8:00 to 8:45 am ET.
- [x] Repo launch checklist (built by a script from master, proofed locally):
      - remove draft banner from all pages
      - delete `noindex` metas; set robots.txt to Allow
      - canonicals, og:url, sitemap.xml, and the JSON-LD `url` on index.html
        -> https://www.caringwithgrace.com/
      - delete brand-review.html, brand-home.html, assets/js/palette.js
      - add redirect stubs for old Wix paths (about-us, caringoncall, blog,
        and the old resources sub-pages) plus 129 blog forwards under `post/`
      - retire the 20-years banner if past 2026 (not yet; still 2026)
- [x] GitHub Pages settings: custom domain `www.caringwithgrace.com`
      (set itself from the `CNAME` file when the launch commit was pushed).
- [x] Wix DNS: `www` CNAME -> `caringwithgrace.github.io`; apex A ->
      185.199.108.153 / .109.153 / .110.153 / .111.153 (replacing Wix's).
      Changed nothing else. Public resolvers had the new records within a
      minute.
- [x] Wait for GitHub's certificate, then tick Enforce HTTPS. GitHub handles
      apex -> www. NOTE: GitHub did not start issuing on its own; after 35
      minutes `https_certificate` was still absent from the Pages API. Clearing
      and re-setting the custom domain (`gh api -X PUT .../pages` with
      `cname` null, then the hostname again) got the certificate approved in
      seconds. GitHub commits a "Delete CNAME" / "Create CNAME" pair when you
      do this; harmless. HSTS gap was about 40 minutes.
- [x] Verify: every page on both hostnames, a real 404, the form from the
      production origin, GA Realtime, external email in and out. All checked
      by curl/dig; form test submitted by Clay, awaiting Melissa's receipt.
- Rollback: restore the Wix A records and `www` CNAME from the Phase 0 sheet.

## Phase 3 — After launch
- [ ] Search Console: verify domain, submit sitemap.
- [ ] Worker ALLOWED_ORIGINS and Turnstile hostnames: drop the draft origin.
- [ ] Wix: turn off auto-renew on the SITE plan only. The domain subscription
      is the registration and stays.

## Not planned: leaving Wix DNS
Requires transferring the registration to another registrar (locked for 60 days
after the September 2026 registrar move) and re-creating every record there
first. Only worth doing if something forces it.
