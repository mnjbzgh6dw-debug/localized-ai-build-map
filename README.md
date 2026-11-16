# Localized AI: the accounting cycle build map

An interactive map of a product for bookkeeping and accounting firms in South Africa:
software that runs inside the firm's office, takes in client documents, and carries the
work through bookkeeping, reporting, compliance and advice. The map is a planning and
communication tool. It is not the product itself.

**Confidential.** It contains notes from a private conversation with a firm. Keep the
repository private unless the owner decides otherwise.

## Run it

Open `index.html` in a browser. That is all. No install and no build.
To serve it locally instead: `python3 -m http.server 8000`, then http://localhost:8000.

It is one self-contained file. The only outside request is Google Fonts
(IBM Plex Sans, IBM Plex Serif); without a connection it falls back to system fonts.

## What is in it

Four views, switched at the top and linkable by hash:

| View | Hash | What it shows |
|---|---|---|
| Ledger | `#ledger` | Every piece, by stage, in ruled columns |
| Flow chart | `#flow` | The same pieces as connected bubbles, top to bottom |
| Templates | `#templates` | Worked examples: master list, emails, survey, approval card |
| Firm view | `#firm` | A clickable sketch of the screen the firm would use |

Every piece opens a detail panel (hash = the piece id, e.g. `#statement-to-csv`).

The work is divided into six builds, each meant to be useful on its own. The toggles
under the view switch filter every view by build.

1. Capture and CSV upload: statements arrive by email, are filed, converted to a
   checked file and made ready for Sage.
2. Requests and upkeep: requests, reminders, check-ins, replies, document search.
3. Bookkeeping: bank rules, exceptions, journals, general ledger.
4. Reporting: profit and loss, balance sheet, cash flow, management accounts.
5. AI advisory: questions answered from the numbers, summaries, alerts.
6. Compliance: VAT, PAYE, provisional and income tax, CIPC.

Two pieces are parked ("Not scheduled", shown with a dash).

**Build 1 in one paragraph.** Match an incoming email to a client by sender address,
take the statement PDF, lift the text out with ordinary code (downloaded statements
contain real text), prove every line against the running balance, write a file in the
layout Sage Desktop imports, and show the result on one screen. As defined, build 1
needs no AI model. AI reading is only needed later, for scanned statements.

## What is real and what is a sketch

- The pieces, builds and flows are the plan.
- The Firm view is a prototype on made-up data held in memory. Nothing is sent or
  saved; a reload resets it.
- All clients, people, email addresses and account numbers are invented.
- Notes headed "Heard from the firm" come from a rough transcript and are approximate.
- Sections headed "Still to find out" and "To investigate first" are open questions,
  not decisions. The most important are the exact Sage product and its import layout.

## How the file is organised

Everything is in `index.html`, in this order:

1. `<style>`: design tokens on `:root` (light), repeated for dark mode, then styles
   grouped by comment: heading, flow (ledger), detail panel, flow chart view,
   templates view, firm view, builds.
2. `<body>`: the static shell. Almost all content is rendered by script.
3. `<script>`, top to bottom:

| Name | What it holds |
|---|---|
| `STAGES` | The five stages plus the foundation layer |
| `N` | Every piece: the main content of the map |
| `BUILDS`, `BMAP` | The six builds, and which build each piece belongs to |
| `TPL` | The example templates (HTML strings) and the piece each belongs to |
| `rowFor`, `stageEl`, `build` | Render the ledger |
| `FLOW` | Flow chart layout: nodes with column and row, outside parties, groups, edges |
| `bubble`, `buildChart`, `drawEdges` | Render the flow chart and draw its lines as SVG |
| `setView`, `route` | View switching and hash routing |
| `FV0`, `FTABS`, `fvRender`, `fvOk`, `fvNo` | Firm view data, tabs, rendering and actions |
| `applyFilter`, `buildToggles` | The build filter |
| `show`, `hide` | The detail panel |

### A piece (an entry in `N`)

```js
{id:'statement-checks',        // unique; also the URL hash
 s:'process',                  // stage id from STAGES
 g:'Reading the statement',    // optional group heading inside the stage (ledger)
 t:'Reconciliation checks',    // title
 small:'...',                  // optional "Simplest useful version"
 what:'...',                   // description
 build:['...','B2|...','X|...',['Link text','other-piece-id']],
 todo:['...'],                 // optional "To investigate first"
 extra:{h:'...',groups:[{b:1,items:[['text','for which piece']]}]},  // optional
 sched:[{when:'Every month',items:[['text',1,'Document']]}],          // optional
 inn:'...', out:'...',         // "In and out"
 check:'...',                  // "Human involvement"
 heard:'...', ask:'...'}       // optional notes and open questions
```

Conventions inside `build`:
- A plain string belongs to the piece's own build.
- `B2|text` (any build number) is listed under "Added by later builds".
- `X|text` is listed under "Parked for later".
- `['text','piece-id']` renders as a link to another piece.

Other flags: `flowOnly:1` shows the piece only in the flow chart; `open:[hash,label]`
adds a button to another view. The fields `p` and `v1` are left over from an earlier
phase system and have no visible effect.

### Common changes

- Move a piece to another build: change its number in `BMAP` (0 = not scheduled).
- Add a piece: add an entry to `N` (in the right stage block), add it to `BMAP`, and
  if it should appear in the flow chart add a node and edges in `FLOW`.
- Add an example: add an entry to `TPL` with `step` set to the piece it belongs to.
  A 📄 marker then appears on that piece automatically.
- Flow chart edges: `[from, to, kind, options]`. Kinds: default (down, with an elbow
  if the columns differ), `'h'` (sideways), `'upcol'` (straight back up),
  `'rlane'` (back up the right-hand margin). Options: `dash`, `both`, `label`.

### Design

- Type: IBM Plex Serif for titles, IBM Plex Sans for everything else.
- Colour: tokens `--paper`, `--sheet`, `--ink`, `--muted`, `--rule`, `--green`, and
  one colour per build `--b1` to `--b6`. Dark values are defined twice (system dark
  mode and an explicit `data-theme="dark"`).
- The ledger look (ruled rows, double rule under headers) is deliberate.

## Version

`v1.0`: the version signed off by the owner. `index.html` SHA-256:
`680fd5852d6113f08ccf32b7c265ea30f906da628936af81febe9a267408ed6f`
