# Handover to Claude Code: publish the Localized AI build map as a website

You are taking over a finished piece of work. Read this whole file before doing anything.

## What this is

`index.html` in this folder is a complete, working, single-file website called
"Localized AI: the accounting cycle build map". It was designed and tested elsewhere and
the owner, Dene, has signed it off exactly as it is.

It is one self-contained HTML file: all CSS and JavaScript are inline, there is no build
step, no framework and no server code. The only outside resource is Google Fonts
(IBM Plex Sans and IBM Plex Serif), loaded by a `<link>` in the `<head>`.

## The goal

Put this exact file into a new GitHub repository in Dene's account so he can share it
with his development partner, and (only if he says so) host it at a URL.

## Hard rules

1. `index.html` is the source of truth. Do NOT regenerate it, rewrite it, reformat it,
   minify it, lint-fix it, split it into several files, convert it to a framework, or
   "improve" it. It must be committed byte for byte.
2. Before committing, confirm the file is intact:
   `sha256sum index.html` must print
   `680fd5852d6113f08ccf32b7c265ea30f906da628936af81febe9a267408ed6f`
   and the size must be 121349 bytes.
   If it does not match, stop and tell Dene. Do not try to repair it.
3. Do not add analytics, tracking, cookies, extra scripts, a package.json, or any
   dependency.
4. Do not change wording, colours, fonts, spacing, data or behaviour.
5. The page contains notes from a private business conversation. Treat the content as
   confidential: default to a PRIVATE repository and do not publish it to a public URL
   unless Dene explicitly asks for that in this session.

## Ask Dene these three things first (use the defaults if he says "default")

1. Repository name. Default: `localized-ai-build-map`.
2. Visibility. Default: private.
3. Hosting. Default: none for now (the partner clones the repo and opens `index.html`).
   Other options he may choose are listed under "Hosting" below.
Also ask for his development partner's GitHub username, so you can invite them.

## Steps

1. Check you are in the folder that contains `index.html`, `README.md`, `.nojekyll`
   and this file. Verify the checksum (rule 2).
2. `git init -b main`, add all four files, and commit with the message
   `Localized AI build map: signed-off version`.
3. Check GitHub CLI access with `gh auth status`. If it is not logged in, ask Dene to
   run `gh auth login` himself; never ask him to paste a token into the chat.
4. Create the repository and push:
   `gh repo create <name> --private --source=. --remote=origin --push`
   (use `--public` only if Dene chose public).
5. Invite the partner: `gh api -X PUT repos/<owner>/<name>/collaborators/<username> -f permission=push`
   (or tell Dene to add them under Settings, Collaborators).
6. Tag the signed-off version: `git tag v1.0 && git push origin v1.0`.
7. Run the checks below and report the results to Dene, with the repository URL.

## Hosting (only if Dene asks)

- No hosting: the partner clones the repo and double-clicks `index.html`, or runs
  `python3 -m http.server 8000` in the folder and opens http://localhost:8000.
  Everything works from a local file.
- GitHub Pages: Settings, Pages, deploy from branch `main`, folder `/` (root). The
  `.nojekyll` file is already there so the file is served untouched. Check GitHub's
  current rules before enabling it: a Pages site is normally reachable by anyone with
  the link, and Pages on a private repository depends on the account's plan. Tell Dene
  plainly whether the resulting URL will be public before you turn it on.
- Any static host (Netlify, Vercel, Cloudflare Pages): no build command, publish
  directory is the repository root. Same warning about public links applies.

## Checks: what "exactly the same" means

Open `index.html` in a browser (a local file is fine) and confirm every line. If you can
drive a browser yourself, do so; otherwise ask Dene to look. Do not edit `index.html`
to make a check pass.

Header
- Top left: the title "The accounting cycle, end to end" in a serif typeface.
- Top right: the Localized AI logo (a green map pin with three connected dots, then
  "Localized" and a small green "AI").
- Browser tab title: "Localized AI: the accounting cycle build map".
- Right of the view switch: "41 pieces in 6 builds, 2 parked".

Views (four buttons: Ledger, Flow chart, Templates, Firm view)
- The address bar hash changes to `#ledger`, `#flow`, `#templates`, `#firm`, and
  loading the page with one of those hashes opens that view.

Build toggles
- "Everything" plus six toggles with these counts: 1 Capture and CSV upload 11,
  2 Requests and upkeep 8, 3 Bookkeeping 5, 4 Reporting 4, 5 AI advisory 6,
  6 Compliance 7.
- Selecting a build dims everything outside it and shows a one-line description.
  Several builds can be selected at once. "Everything" clears the selection.

Ledger
- Five stage columns with these row counts: Capture 11, Process 8, Report 4,
  Comply 7, Advise 5. Below them, "Underneath every stage" with 8 rows.
- Six rows carry a small document marker (📄).
- Clicking a row opens a panel from the right (from the bottom on a phone). Escape,
  the × button or clicking outside closes it. The Back and Next buttons step through
  the pieces. The hash becomes the piece id, for example `#account-register`.
- In the "Client and account list (Master List)" panel the list is split into
  "What build 1 includes", "Added by later builds" and further sections, and
  "See the request schedule" is a link that opens the Request schedule panel.

Flow chart
- 37 clickable bubbles joined by arrows (36 lines), top to bottom, with numbered build
  badges. Lines redraw correctly when the window is resized.
- Two lines run back up: one on the left into "Request documents", one on the right
  into the master list.

Templates
- Nine worked examples (tables, emails, a survey form, an approval card). With only
  build 1 selected, exactly one remains: "Master list: set-up view".

Firm view
- With "Everything": seven tabs (Statements, Today, Clients, Messages, Documents,
  Approvals, Audit log). With only build 1: the single tab "Statements".
- The buttons change the example data: for instance, on Approvals, approving the
  unmatched email also removes the drafted query and adds lines to the Audit log.
  "Reset the example" restores the starting data. A reload also resets it.

Look and feel
- Pale green paper background, dark ink text, green accents; dark mode follows the
  system setting.
- At a phone width (about 390px) nothing scrolls sideways except tables inside their
  own boxes.
- No errors in the browser console.

## After this handover

Future changes are made by editing the data arrays near the top of the `<script>` in
`index.html`; `README.md` explains them. Keep it a single file unless Dene asks for it
to be restructured, and if he does, the restructured version must pass every check
above before it replaces this one.
