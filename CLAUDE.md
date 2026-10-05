# Caring with Grace website

The public website for Caring with Grace, an Aging Life Care (care management) practice in
Dallas, Texas. Plain static HTML, one file per page, one stylesheet, no build step. GitHub
Pages publishes the default branch about a minute after a change is merged.

## Who you are working with

Usually Melissa or Karen from the Caring with Grace team. They are not programmers and they
are editing the site by describing changes in plain English. Clay Thomas built the site and
handles anything technical.

- Explain what you did in everyday words. Say "the Our Team page", not `team.html`.
- Always show the before and after wording for every change.
- Change only what was asked. If you notice something else that looks wrong, mention it and
  leave it alone.
- If a request is ambiguous (which page, which sentence), ask before editing.
- Keep each session to one request or one short list, so the pull request is easy to review.

## House rules for copy

1. No dashes used as punctuation (no em-dashes or en-dashes). Use periods, commas or a
   middle dot.
2. Team bios are the team's own words. Change them only with the exact text you are given.
3. Write out "Aging Life Care Association". Never "ALCA" in visible copy.
4. "Care manager" and "care management" for what the company does. Never "caregiver" or
   "caregiving"; the company coordinates care and does not provide hands-on care.
5. The company does not give medical advice. Prefer "healthcare" to "medical" when
   describing its own work. "Medical" is fine for other people's work (a doctor's plan, a
   medical appointment) and inside client quotes.
6. Client reviews and quotes are never reworded.
7. Headings are sentence case. "Thomas's", not "Thomas'". Numerals for ages and years.
8. Warm, plain, confident voice. Short sentences, "you" and "we".
9. Never put client names, health details or any password into the site. This repository
   is public.

## How the site is put together

- Pages: `index.html` (home), `about.html` (Our Story), `team.html`,
  `care-management-services.html`, `caring-on-call.html` (shown in menus as "Care Management
  for Proactive Planners"), `thoughtful-engagement.html`, `for-professionals.html`,
  `aging-life-care.html`, `resources.html`, `newsletter.html`, `contact.html`, `privacy.html`, `404.html`.
- The header menu and the footer are repeated in every page file. A change to either one
  has to be made in all of them, identically.
- Styling is `assets/css/style.css`. Every page loads it as `style.css?v=NUMBER`. If you
  change the stylesheet, raise that number by one in every page, or visitors keep the old
  styles.
- Photos live in `assets/img/`. Team headshots are square, 800 x 800, in `assets/img/team/`.
  Match the size and crop of the image being replaced.
- A new team member is a new `team-card` block in `team.html`, copied from an existing one.
- `brand-guide.html` and `site-tutorial.html` are internal, password-protected pages. Their
  content is encrypted in `assets/brand/*.enc` and cannot be edited here.
- `404.html` is shown for any address that does not exist, at any depth, so every link and
  file path in it starts with `/` (`/about.html`, `/assets/...`). Keep the leading slash
  when copying a menu or footer change into it.
- Folders such as `about-us/`, `caringoncall/`, `blog/` and everything under `post/` each
  hold one small page that forwards an address from the previous website to the right page
  here, or to the same article on the newsletter site. The `CNAME` file tells GitHub which
  domain the site lives at.

## Go ahead without asking Clay

Wording on any page. Links, resources, books, reviews. Team members, bios, headshots.
Photos. Hours, address, service-area cities. A new section built in the style of an
existing one.

## Stop and tell the person to ask Clay

- The contact form: `workers/`, the form code and keys in `assets/js/main.js`, and the form
  markup on the Contact page.
- Anything about the domain, DNS, email delivery or analytics.
- The logo and brand files: `assets/img/brand-v6/` and `assets/brand/`. They are generated
  elsewhere and edits here would be overwritten.
- Deleting or renaming a page, or changing a page's address. People have saved links.
- The menu structure, the page layout, fonts and colors.
- `robots.txt`, `sitemap.xml`, `CNAME`, the forwarding folders, `MIGRATION.md`, this file,
  and repository settings.

If a request needs one of these, say so plainly, do not attempt it, and suggest sending
Clay a note that says what is wanted.

## Finishing a change

Summarize in two or three plain sentences: which page, what changed (before and after),
and anything you noticed but did not touch. Remind the person that nothing is live until
they create the pull request and merge it, and that the live page may need a hard refresh
(Ctrl or Cmd + Shift + R) to show the change.
